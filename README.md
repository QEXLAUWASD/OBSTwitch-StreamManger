# Twitch OBS Title Updater

An OBS Studio plugin that automatically updates your Twitch stream title and category based on the game process currently running on your PC.

## Features

- Detects running game processes and maps them to Twitch game categories
- Auto-updates stream title and category via the Twitch Helix API
- Configurable title template with `%game%` and `%date%` placeholders
- Optional custom text appended to every title
- "Keep last title" mode — when no game is detected, keeps the current title instead of switching to Just Chatting
- Process exclusion list (by exact name or prefix) to ignore system/background processes
- Hot-reload of `config.json` when edited externally
- Dark mode UI
- Automatic update check against the GitHub releases page

## Requirements

- OBS Studio 31.1.1 or newer
- Windows x64
- A Twitch developer application (Client ID + OAuth access token with `channel:manage:broadcast` scope)

## Installation

1. Download the latest release `.dll` from the [Releases](../../releases) page.
2. Copy `TwitchOBSTitleUpdater.dll` into your OBS plugins folder:
   ```
   %ProgramFiles%\obs-studio\obs-plugins\64bit\
   ```
3. Restart OBS Studio.
4. Open **Tools → Twitch Auto-Title** and enter your Twitch credentials when prompted.

## Twitch Credentials

You need three values:

| Field | Where to get it |
|---|---|
| **Client ID** | [Twitch Developer Console](https://dev.twitch.tv/console) → your app |
| **Access Token** | OAuth token with `channel:manage:broadcast` scope |
| **Streamer ID** | Your numeric Twitch user ID |

Credentials are stored locally in `%AppData%\obs-studio\plugin_config\twitch-auto-title\config.ini`.

## Usage

1. Open **Tools → Twitch Auto-Title**.
2. Select a running process from the process list, fill in the **Game Name** and **Twitch Category**, then click **Add / Update Mapping**.
3. The plugin polls running processes every 5 seconds. When a mapped process is detected, it automatically PATCHes your Twitch channel title and category.
4. Use **Edit Exclusions** to prevent background apps from being matched.
5. Use **Manual Update Title/Category** to trigger an immediate update.

### Title Template

The default template is ` %game% %date%`. You can change it by editing `config.json` directly (or reloading with the **Reload config.json** button):

```json
{
  "base": "Playing %game% — %date%"
}
```

| Placeholder | Replaced with |
|---|---|
| `%game%` | Detected game name |
| `%date%` | Current date (`YYYY-MM-DD`) |

## Building from Source

### Prerequisites

- CMake 3.28+
- Visual Studio 2022 (Build Tools)
- Windows SDK 10.0.26100.0 or newer

### Steps

```powershell
git clone https://github.com/QEXLAUWASD/Twitch-StreamManger
cd Twitch-StreamManger

cmake -S . -B build_x64 `
  -G "Visual Studio 17 2022" -A x64 `
  "-DCMAKE_SYSTEM_VERSION=10.0.26100.0" `
  -DENABLE_FRONTEND_API=ON -DENABLE_QT=ON

cmake --build build_x64 --config RelWithDebInfo --parallel
```

The plugin DLL will be at `build_x64\RelWithDebInfo\TwitchOBSTitleUpdater.dll`.

Dependencies (OBS sources, Qt6, obs-deps) are downloaded automatically during CMake configure.

## License

[MIT](LICENSE)
