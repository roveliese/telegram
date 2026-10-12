# Telegram theme format guide

Roveliese releases native Telegram theme files in
[`dist/`](../dist/). Each platform has its own format and its own set of
themeable elements, so a colour decision should always be checked in the
native client that will use it.

## Choose the right download

| Telegram client | File | How to apply it |
| --- | --- | --- |
| Telegram Desktop (Windows, Linux, or the Desktop app on macOS) | `.tdesktop-theme` | Download and open the file in Telegram Desktop, then confirm the preview. |
| Telegram for Android | `.attheme` | Open the downloaded file in Telegram, preview it, and choose **Apply**. |
| Telegram for iOS | `.tgios-theme` | Share the downloaded file to Telegram from Files, then apply it from the preview. |
| Telegram for macOS | `.palette` | Open the file in the native Telegram for macOS app and apply it from the preview. |

Telegram Desktop and Telegram for macOS are different apps with different
formats. A Mac running Telegram Desktop needs the `.tdesktop-theme` file; the
`.palette` file is only for Telegram for macOS.

## Format notes

### Telegram Desktop

Desktop themes are archives. Their colour file is named
`colors.tdesktop-theme`; an optional wallpaper is stored as `background.*` or
`tiled.*`. Roveliese packages `colors.tdesktop-theme` and `background.jpg`
(the background contains lossless PNG image data). Colours can be literal `#RRGGBB` or `#RRGGBBAA` values, or aliases
of another colour.

Desktop has the richest public theme tooling. Its in-app editor is useful for
identifying a visible element, but a change still needs checking in ordinary
chats, replies and forwarded messages, media, and selected states.

### Android

Android uses a flat `.attheme` key/value file. The importer requires `key=value`
with no whitespace surrounding the key; `key: value` is ignored. Six-digit hex values are RGB;
eight-digit hex values are **ARGB**, unlike Desktop RGBA. Roveliese currently
uses opaque colors for the newly cataloged reaction and story keys. The advanced Android editor is
the most reliable way to discover which elements a current client exposes.
Theme keys and wallpaper handling can change between app releases, so an
Android theme should be previewed on the intended Telegram version.

### iOS

iOS imports `.tgios-theme` documents. Telegram's appearance tools support
importing and applying themes, but Telegram does not publish an exhaustive
token glossary for this format. Treat an unexposed element as a client limit
rather than assuming that a similarly named Desktop or Android key will work.

Colors are bare RGB or ARGB hex strings (without `#`), or `clear`. Outgoing
bubble `bgList` contains gradient colors; `highlightedBg` is a separate state
and must not become another gradient stop.

Check both wallpaper modes as well as incoming and outgoing messages, media,
forwarded/replied content, settings switches, and pressed states.

### Telegram for macOS

The native macOS client uses a `.palette` text file, with metadata such as
`name`, `shortname`, `isDark`, `tinted`, and `parent`, followed by
`key = value` entries. Alpha uses `#RRGGBB: opacity`, rather than
Desktop RGBA or iOS ARGB. It is not compatible with the Desktop archive format.

Telegram does not maintain a complete public macOS key reference. Test any
changed palette in the native app, especially selections, replies, links,
media overlays, and semantic status colours.

## Publishing and sharing

Telegram's online theme editor can import an existing theme, choose a
platform, and publish a Cloud Theme. Only add a `t.me/addtheme/...` link after
it resolves to a real theme published from Telegram; a download from this
repository remains the dependable installation path.

## Before sharing a theme

- Preview the exact downloaded file in its native client.
- Check chat list, incoming and outgoing messages, replies and forwards,
  media captions, selected/pressed elements, and settings controls.
- Check text and link readability on every altered surface in both light and
  dark environments.
- Re-test after a Telegram update when a client changes its theming editor or
  visual components.

## Source review: 2026-10-12

The review used the branch heads below, pinned to commits for reproducibility.
These are source snapshots, not a claim that all installed stable clients use
these revisions. The public guide describes workflows; client source defines
literal keys and decoding behavior.

- Desktop: [built-in night archive, `63a1831`](https://github.com/telegramdesktop/tdesktop/blob/63a1831728147e941ad2dc8939879cf5afe419c7/Telegram/Resources/night.tdesktop-theme).
- Android: [Theme.java, `f2908b1`](https://github.com/DrKLO/Telegram/blob/f2908b14133bbffbf7ab04f641ecb5bfaf533242/TMessagesProj/src/main/java/org/telegram/ui/ActionBar/Theme.java).
- iOS: [PresentationThemeCodable.swift, `f1dd7a2`](https://github.com/TelegramMessenger/Telegram-iOS/blob/f1dd7a2dbd02cbbf513e75d5695d8d36d1cf5838/submodules/TelegramPresentationData/Sources/PresentationThemeCodable.swift).
- macOS: [ColorPalette.swift, `579cebb`](https://github.com/overtake/TelegramSwift/blob/579cebbf0c01fd41b712eff3647fa7f69db9665d/packages/ColorPalette/Sources/ColorPalette/ColorPalette.swift).

This update fixes Android exports to use native `key=value` syntax and ARGB
alpha, and explicitly colors reaction controls and story rings. Rose
marks regular unseen stories and selected reactions; green marks close-friend
stories and live streams. Selected reaction text has a dedicated foreground.
iOS outgoing gradients retain their two fill colors, with highlight stored
separately. Desktop and macOS outputs were regenerated and checked. Desktop Storm also
picks up the existing generator correction for active-area foreground aliases;
macOS output remains unchanged.

Automated checks pass. The existing 16 surface-state contrast warnings and two
switch coverage gaps remain. Native-client import and visual previews still
need testing on the intended client versions.

## References

- [Creating Custom Cloud Themes](https://core.telegram.org/themes) ? official editor, import, publishing, and wallpaper workflow.
- [Telegram themes API](https://core.telegram.org/api/themes) ? cloud theme files, settings, and updates; it is not a cross-platform color-key glossary.
- [Creating Android Themes](https://telegra.ph/Create-Theme-Android-FAQ) ? official in-app editor workflow.
