# MSX Bridge

The **MSX Bridge** player provider streams your Music Assistant library to Smart TVs through the [Media Station X (MSX)](https://msx.benzac.de/) app and to any device with a web browser via the built-in kiosk web player. It runs inside the Music Assistant server — no separate containers, add-ons, or cloud services are required.

!!! tip "Two ways to use it"
    **On a Smart TV**: Install the free MSX app and point it at your Music Assistant server.
    **In a browser**: Open `http://<SERVER_IP>:8099/web` on a tablet, laptop, or Raspberry Pi — no app install needed.

## Features

### Library Browsing

- Browse **Albums**, **Artists**, **Playlists**, and **Tracks** with TV remote-friendly navigation
- **Recently Played** section for quick access to recent music
- **Detail pages**: Drill into album tracks, artist albums, and playlist tracks
- **Search**: Full-text search across the entire library (MSX keyboard on TV, standard input in web player)

### Audio Playback

- Stream audio via Music Assistant's queue system with real-time PCM → ffmpeg encoding (MP3, AAC, or FLAC)
- **Playlist mode**: Select any track in a list to start playback from that point — subsequent tracks play automatically
- **Playback controls**: Play, pause, stop, next, previous — from the TV remote, the web player, or the Music Assistant UI
- **Bidirectional sync**: Control playback from any source — actions taken on the TV reflect in MA and vice versa
- **Progress tracking**: Current position is reported back to Music Assistant in real time

### Multi-Device

- **Dynamic player registration**: Each TV or browser registers as a Music Assistant player on demand (by device ID or IP)
- **Multi-TV support**: Connect multiple TVs and browsers simultaneously — each gets its own independent player
- **Automatic cleanup**: Idle players are automatically unregistered after a configurable timeout (default: 30 minutes)
- **Distinct naming**: MSX app players appear as "MSX TV", web players appear as "WEB TV" in Music Assistant

### Web Kiosk Player

- **Browser-based player** at `/web` — works on any device with a modern browser
- Sidebar navigation matching the MSX TV layout (Albums, Artists, Playlists, Tracks, Search)
- Built-in HTML5 audio player with controls, progress bar, and full-screen player mode
- Ideal for wall-mounted tablets, old laptops, Raspberry Pi with Chromium, or digital signage

### REST API

External clients and home automation systems can integrate with the provider's HTTP API:

| Endpoint | Description |
|----------|-------------|
| `GET /api/albums` | List albums (supports `limit` and `offset`) |
| `GET /api/artists` | List artists |
| `GET /api/playlists` | List playlists |
| `GET /api/tracks` | List tracks |
| `GET /api/albums/{id}/tracks` | Tracks for an album |
| `GET /api/artists/{id}/albums` | Albums for an artist |
| `GET /api/playlists/{id}/tracks` | Tracks for a playlist |
| `GET /api/search?q=...` | Search the library |
| `GET /api/recently-played` | Recently played tracks |
| `POST /api/play` | Start playback (JSON body: `track_uri`, `player_id`) |
| `POST /api/pause/{player_id}` | Pause playback |
| `POST /api/stop/{player_id}` | Stop playback |
| `POST /api/next/{player_id}` | Skip to next track |
| `POST /api/previous/{player_id}` | Skip to previous track |
| `GET /health` | Health check |

## Configuration

Add the MSX Bridge provider under **Settings → Player Provider Settings**. The following options are available:

| Option | Description | Default |
|--------|-------------|---------|
| **HTTP port** | Port for the HTTP server (MSX bootstrap, content pages, audio streams, web player). | 8099 |
| **Output format** | Audio format sent to clients: MP3, AAC, or FLAC. | MP3 |
| **Player idle timeout** | Minutes of inactivity after which a player is automatically unregistered. | 30 |
| **Show stop notification** | Show a notification on the TV when stopping playback (if supported by the MSX client). | Off |
| **Abort stream first** | Abort the audio stream before sending stop to MSX. May help on some TVs where playback doesn't stop cleanly. | Off |

Standard [player settings](../settings/player-provider.md) apply to MSX Bridge players once they appear.

## Setup

### Option 1: Smart TV (MSX App)

1. Install the [Media Station X](https://msx.benzac.de/) app on your Smart TV.
   Supported platforms: Samsung Tizen, LG webOS, Android TV, Fire TV, Apple TV, Xbox, or any web browser.
2. Open the MSX app and go to **Settings → Start Parameter**.
3. Enter the start URL:
   ```
   http://<YOUR_SERVER_IP>:8099/msx/start.json
   ```
   Replace `<YOUR_SERVER_IP>` with the IP address or hostname of your Music Assistant server.
4. Restart MSX. The Music Assistant menu should appear with: Recently Played, Albums, Artists, Playlists, Tracks, and Search.

### Option 2: Web Browser (Kiosk Player)

Open the following URL in any modern browser:

```
http://<YOUR_SERVER_IP>:8099/web
```

No installation required. The web player works on tablets, laptops, phones, Raspberry Pi, or any device with a browser. Each browser tab registers as an independent "WEB TV" player in Music Assistant.

!!! tip "Kiosk mode"
    For a wall-mounted tablet or digital signage, use your browser's fullscreen or kiosk mode (e.g. `chromium --kiosk http://192.168.1.100:8099/web`) for a distraction-free experience.

### Status Dashboard

Open `http://<YOUR_SERVER_IP>:8099/` in a browser to see:

- The MSX setup URL (for copy-pasting into the TV)
- A link to the web player
- A list of all registered players with their current state
- A **Quick stop** button per player for immediate stop

## How It Works

### MSX TV Flow

1. **Bootstrap**: The TV loads `/msx/start.json` → an interaction plugin detects the device ID and opens a WebSocket connection for push notifications.
2. **Menu**: The plugin requests the main menu, which lists library sections (Albums, Artists, Playlists, Tracks, Search).
3. **Navigation**: Selecting a menu item loads a content page (e.g. `/msx/albums.json`). Album items drill into track lists; track items start playlist playback.
4. **Playback**: When a track is selected, MSX fetches the audio endpoint. The provider enqueues the track via Music Assistant's queue and streams PCM → ffmpeg → encoded audio (MP3/AAC/FLAC) to the TV.
5. **Push notifications**: When playback is controlled from Music Assistant (play, pause, stop, next, previous), the WebSocket pushes the event to the TV so the MSX player updates immediately.

### Web Player Flow

1. **Page load**: The browser opens `/web`, generates a unique device ID (stored in `localStorage`), and connects to the WebSocket.
2. **Sidebar menu**: Fetched from `/msx/menu.json` — the same endpoint the MSX app uses.
3. **Content**: Clicking a menu item fetches the corresponding MSX JSON content page and renders it as HTML (grid cards for albums/artists, list rows for tracks).
4. **Playback**: Clicking a track loads the MSX playlist JSON and plays audio via the HTML5 `<audio>` element. Next/previous, progress, and seek all work natively.
5. **Sync**: The web player sends position updates and pause/resume events via WebSocket, and receives push notifications from Music Assistant — fully bidirectional, just like the MSX app.

### Network

All traffic stays on your local network. The TV/browser communicates with the Music Assistant server over HTTP and WebSocket on the configured port (default: 8099). No cloud services, no external dependencies.

## Known Issues / Limitations

- **Local network**: The TV/browser and the Music Assistant server must be on the same network (or otherwise reachable) for HTTP and WebSocket connections.
- **Single instance**: One MSX Bridge provider per Music Assistant server. Multiple TVs and browsers connect to the same instance.
- **MSX app required on TV**: The TV interface requires the free MSX app. The web player (`/web`) is the alternative for devices without MSX.
- **Audio format**: Some older Smart TVs may not support AAC or FLAC. MP3 (default) has the widest compatibility.
- **Browser autoplay**: Some browsers block audio autoplay until the user interacts with the page. Click the play button if audio doesn't start automatically.

## Links

- [MSX Music Assistant Bridge](https://github.com/trudenboy/msx-music-assistant) — provider source code (by [trudenboy](https://github.com/trudenboy))
- [Media Station X](https://msx.benzac.de/) — MSX app and documentation
- [Music Assistant Server](https://github.com/music-assistant/server) — upstream server repository
