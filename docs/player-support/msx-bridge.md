# MSX Bridge

The **MSX Bridge** player provider streams your Music Assistant library to Smart TVs through the [Media Station X (MSX)](https://msx.benzac.de/) app. It runs inside the Music Assistant server and exposes an HTTP interface that MSX uses for a native TV-optimized menu and audio playback.

## Features

- **MSX native UI**: Browse albums, artists, playlists, and tracks with remote-friendly navigation
- **Detail pages**: Drill into album tracks, artist albums, and playlist tracks
- **Search**: Search the library with the MSX Input Plugin keyboard
- **Audio playback**: Stream audio to the TV via MA's queue with PCM→ffmpeg encoding (MP3, AAC, or FLAC)
- **Dynamic player registration**: Each TV registers as an MA player on demand (by device ID or IP), with automatic idle timeout cleanup
- **Multi-TV support**: Each TV gets its own unique player
- **WebSocket push**: Play and stop events from Music Assistant are pushed to connected TVs in real time
- **Library REST API**: External clients can use `/api/albums`, `/api/artists`, `/api/playlists`, `/api/tracks`, `/api/search`, `/api/recently-played`
- **Playback control**: Play, pause, stop, next, previous via API or MA UI

## Configuration

Add the MSX Bridge provider under **Settings → Player Provider Settings**. The following options are available:

| Option | Description | Default |
|--------|-------------|---------|
| **HTTP port** | Port for the MSX HTTP server (bootstrap, content pages, stream). | 8099 |
| **Output format** | Audio format sent to the TV: MP3, AAC, or FLAC. | MP3 |
| **Player idle timeout** | Minutes of inactivity after which a TV player is automatically unregistered. | 30 |
| **Show stop notification** | When stopping playback, show a notification on the TV (if supported by the MSX client). | Off |

Standard [player settings](../settings/player-provider.md) apply to MSX players once they appear.

## Setup on TV

1. Install the [Media Station X](https://msx.benzac.de/) app on your Smart TV (Samsung Tizen, LG webOS, Android TV, Fire TV, Apple TV, or web browser).
2. In the MSX app, go to **Settings → Start Parameter**.
3. Enter the start URL: `http://<YOUR_SERVER_IP>:8099/msx/start.json`  
   Replace `<YOUR_SERVER_IP>` with the IP address of the machine running Music Assistant (or use hostname if your TV can resolve it).
4. Restart MSX. The Music Assistant menu (Albums, Artists, Playlists, Tracks, Search) should appear.

You can also open `http://<YOUR_SERVER_IP>:8099/` in a browser for a simple status page with the setup URL and list of registered players.

## How It Works

1. **Bootstrap**: The TV loads `/msx/start.json`, which points to an interaction plugin. The plugin loads the main menu (Albums, Artists, Playlists, Tracks, Search).
2. **Navigation**: Selecting a menu item loads a content page (e.g. `/msx/albums.json`). Items have actions to drill down (e.g. album tracks) or to play a track.
3. **Playback**: When you select a track, MSX calls the audio endpoint; the provider enqueues the track via MA's queue system and streams encoded audio to the TV.
4. **WebSocket**: If the MSX client connects to the provider's WebSocket, it receives play/stop commands when you control playback from Music Assistant.

All traffic stays on your local network; no cloud services are required.

## Known Issues / Limitations

- **Stop delay**: Pressing Stop in Music Assistant may take some time (e.g. up to ~30 seconds) before playback actually stops on the TV. Disabling (power off) the player stops immediately. This is under investigation (MA stream buffer / MSX client behavior).
- **Local network**: The TV and the Music Assistant server must be on the same network (or reachable) for the HTTP and WebSocket connections to work.
- **Single server**: One MSX Bridge instance per Music Assistant server; multiple TVs connect to the same server and each gets its own player.

## Links

- [MSX Music Assistant Bridge](https://github.com/trudenboy/msx-music-assistant) — provider source code and documentation (by [trudenboy](https://github.com/trudenboy))
- [Media Station X](https://msx.benzac.de/) — MSX app and documentation
- [Music Assistant Server](https://github.com/music-assistant/server) — upstream server repository (MSX Bridge provider is included when the PR is merged)
