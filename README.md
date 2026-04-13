# fvtt-player-client

A simple, lightweight Chromium-based desktop client for Foundry VTT and similar browser-hosted games.

## Maintainer

This project is actively maintained by **Cranky_Coyote** of **Kanh Productions**.

- Name: `Cranky_Coyote`
- Email: `coyote@kanhproductions.com`
- URL: `https://www.kanhproductions.com`
- Discord: `Cranky Coyote`

## Project status

- Community maintained and actively updated.
- Focused on low overhead and reliability for game sessions.
- Issues and feature requests should be opened in this repository.

## Performance philosophy

This client is still Chromium-based, so raw performance depends on your hardware and world/module load.
The main gains come from running a dedicated game client with minimal background browser activity.

- Best case: better consistency and lower overhead than a heavily extended browser profile.
- Worst case: similar performance to a clean Chromium/Edge profile with one tab.
- Goal: keep UI overhead low so more resources go to the VTT.

## Features

- Preconfigured game list from `config.json`.
- Optional per-game saved login values (user/password/admin password).
- Built-in popout support (including nested popouts).
- Optional unpacked extension loading (for example Rivet).
- Lightweight defaults:
  - `checkForUpdates` disabled by default
  - spellcheck disabled
  - production devtools disabled
- Windows installer support with shortcut handling.

## Installation and launch

- **Windows installer**: run the generated `Setup.exe`.
- **Portable/unpacked build**: run `vtt-webclient.exe` from the unpacked folder.

## Uninstall

Use either of the following:

- **Windows Apps & Features** (`VTT WebClient`)
- `C:/Users/<your-user>/AppData/Local/vtt_webclient/Update.exe --uninstall`

For a fully clean reset, uninstall first, then remove remaining cache/user data only if needed.

### Config file locations

- Installed app:
  - `C:/Users/<your-user>/AppData/Local/vtt_webclient/app-<version>/resources/app/config.json`
- Unpacked/portable build:
  - `<build-folder>/resources/app/config.json`

## Configuration

Edit `config.json` with any text editor.  
Important: JSON does not allow trailing commas.

### Example

```json
{
  "games": [
    {
      "name": "My Game Server",
      "url": "https://example.com/join"
    }
  ],
  "background": "",
  "backgrounds": [],
  "backgroundColor": "#003049ff",
  "textColor": "#eae2b7ff",
  "accentColor": "#f77f00ff",
  "customCSS": "",
  "cachePath": "",
  "autoCacheClear": false,
  "ignoreCertificateErrors": false,
  "checkForUpdates": false,
  "extensionPaths": []
}
```

### Config options

- `games`: preconfigured game buttons (`name` + `url`).
- `background`: single background image URL.
- `backgrounds`: optional array of image URLs; one is selected randomly.
- `backgroundColor`, `textColor`, `accentColor`: UI color customization.
- `customCSS`: custom CSS string injected into the launcher UI.
- `cachePath`: advanced override for Chromium session cache directory.
- Cache location can be changed from Settings using the `Browse...` button.
- `autoCacheClear`: clear cache on close when enabled.
- `ignoreCertificateErrors`: certificate bypass flag; intentionally restricted for safety in packaged builds.
- `checkForUpdates`: enable/disable release check API call.
- `extensionPaths`: absolute paths to unpacked Chromium extension folders.

## Extension support

To use extensions such as Rivet:

- Provide one or more absolute folder paths in `extensionPaths`.
- Use unpacked extension folders (not `.crx` files).
- Extensions load per session at startup.

## Popouts

Popouts are supported by default. No PopOut! module source edits or user-agent workaround macros are required in this client.

## Keyboard shortcuts

| Control | Action |
|---|---|
| `Ctrl + R` or `F5` | Reload |
| `Ctrl + Shift + R` or `Ctrl + F5` | Hard reload |
| `Ctrl + +` | Zoom in |
| `Ctrl + -` | Zoom out |
| `Ctrl + 0` | Reset zoom |
| `F11` | Fullscreen |
| `Ctrl + Shift + I` / `F12` | Developer tools (dev mode only) |

## Compatibility notes

- Direct world URLs (such as `/join`, `/auth`, `/setup`) are the primary supported flow.
- Third-party SSO/hosted auth flows may vary by provider.

## Development checks

Run these before release:

```bash
corepack yarn lint
corepack yarn typecheck
corepack yarn test
```

## Build artifacts (CI)

The installer workflow uploads direct download artifacts similar to classic release bundles:

- `windows-setup-exe`
- `windows-portable-zip`
- `linux-deb-package`
- `linux-rpm-package`
- `linux-portable-zip`
- `macos-dmg-installer`
- `macos-portable-zip`

## Migration helper (localStorage -> config.json)

```js
JSON.stringify({
  ...JSON.parse(window.localStorage.getItem("appConfig") || "{}"),
  games: JSON.parse(window.localStorage.getItem("gameList") || "[]")
})
```

## Lineage and attribution

This project originated from community work by prior maintainers and developers.  
Current stewardship, releases, and redevelopment standards are managed by Kanh Productions.