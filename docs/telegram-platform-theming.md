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
`tiled.*`. Colours can be literal `#RRGGBB` or `#RRGGBBAA` values, or aliases
of another colour.

Desktop has the richest public theme tooling. Its in-app editor is useful for
identifying a visible element, but a change still needs checking in ordinary
chats, replies and forwarded messages, media, and selected states.

### Android

Android uses a flat `.attheme` key/value file. The advanced Android editor is
the most reliable way to discover which elements a current client exposes.
Theme keys and wallpaper handling can change between app releases, so an
Android theme should be previewed on the intended Telegram version.

### iOS

iOS imports `.tgios-theme` documents. Telegram's appearance tools support
importing and applying themes, but Telegram does not publish an exhaustive
token glossary for this format. Treat an unexposed element as a client limit
rather than assuming that a similarly named Desktop or Android key will work.

Check both wallpaper modes as well as incoming and outgoing messages, media,
forwarded/replied content, settings switches, and pressed states.

### Telegram for macOS

The native macOS client uses a `.palette` text file, with metadata such as
`name`, `shortname`, `isDark`, `tinted`, and `parent`, followed by
`key = value` entries. It is not compatible with the Desktop archive format.

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

## References

- [Telegram: Creating Custom Cloud Themes](https://core.telegram.org/themes) — official import, editor, publishing, and wallpaper workflow.
- [Telegram Desktop Theme Reference](https://github-wiki-see.page/m/telegramdesktop/tdesktop/wiki/Theme-Reference) — detailed Desktop syntax and archive layout; it is community-maintained, so validate new keys in the current client.
- [Telegram Desktop built-in night theme](https://github.com/telegramdesktop/tdesktop/blob/dev/Telegram/Resources/night.tdesktop-theme) — current upstream reference for Desktop token names and defaults.
- [Telegram: Creating Android Themes](https://telegra.ph/Create-Theme-Android-FAQ) — official Android editor workflow.
- [Telegram: Theme Editor 2.0](https://telegram.org/blog/verifiable-apps-and-more?setln=en) — official editor, gradient, and background capabilities.
