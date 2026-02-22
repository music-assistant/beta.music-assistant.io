# MSX Bridge

The **MSX Bridge** player provider lets you play music from Music Assistant on your Smart TV or any device with a web browser. It works through the free [Media Station X (MSX)](https://msx.benzac.de/) app — a universal media app available on most Smart TV platforms. The provider runs inside the Music Assistant server and requires no separate containers, add-ons, or cloud services.

!!! tip "Two ways to listen"
    📺 **On a Smart TV** — install the free MSX app and point it at your Music Assistant server.
    🌐 **In a browser** — open `http://<YOUR_SERVER_IP>:8099/web` on a tablet, laptop, phone, or Raspberry Pi.

## Features

### Browse Your Library on TV

Use your TV remote to explore your full Music Assistant library:

- **Albums** — browse all albums, open any album to see its tracks
- **Artists** — browse artists, open an artist to see their albums
- **Playlists** — browse playlists, open any playlist to see its tracks
- **Tracks** — browse all tracks in your library
- **Recently Played** — quick access to tracks you listened to recently
- **Search** — search across your entire library using the on-screen MSX keyboard

All content is presented as native MSX pages with artwork, optimized for TV remote navigation (arrow keys, OK, back).

### Play Music

Select any track in a list to start playing from that point — the rest of the tracks in the list play automatically after it, just like a regular playlist.

- Supports **MP3**, **AAC**, and **FLAC** output formats (configurable)
- **Play / Pause / Stop / Next / Previous** — from the TV remote, the web player, or the Music Assistant interface
- **Progress bar** — the TV shows the current position and total duration of each track
- Music continues playing as you browse other pages — playback runs independently

### Control from Anywhere

Playback is fully synchronized between the TV and Music Assistant:

- **Pause on the TV** → Music Assistant UI shows paused
- **Skip a track in Music Assistant** → the TV jumps to the next track
- **Stop from Music Assistant** → the TV stops immediately

This works in both directions and in real time. You never need to worry about which device you're controlling from — they all stay in sync.

### Multiple TVs and Browsers

Each TV or browser tab registers as a separate player in Music Assistant:

- Connect as many TVs and browsers as you want — they all work independently
- Players appear automatically when a device connects (no manual setup needed)
- In Music Assistant, TV players appear as **"MSX TV (device_id)"**
- Players that have been idle for more than 30 minutes (configurable) are automatically cleaned up

### Web Player

A built-in browser-based player is available at `/web` — no MSX app needed:

- Works on tablets, laptops, phones, Raspberry Pi, or any device with a modern browser
- Sidebar navigation: Albums, Artists, Playlists, Tracks, Search — same content as the TV
- HTML5 audio player with controls, progress bar, and seek
- **Queue panel** — view and navigate the current playback queue alongside the player
- Each browser tab registers as an independent player in Music Assistant

!!! example "Use case: kitchen tablet"
    Mount a tablet on the wall, open `http://192.168.1.100:8099/web` in Chrome, and you have a dedicated music player controlled from anywhere in your home.

#### Kiosk Mode

Add `?kiosk=1` to the web player URL for an immersive full-screen experience:

```
http://<YOUR_SERVER_IP>:8099/web?kiosk=1
```

Kiosk mode features:

- **Full-screen album art** with blur and vignette overlay as background
- **Auto-hiding controls** — playback controls slide in when you move the mouse/touch the screen, and hide after a few seconds of inactivity
- **Karaoke lyrics overlay** — synchronized lyrics display on screen (when lyrics are available for the track)
- **Animated equalizer** — CSS equalizer animation while music is playing
- **Three-column layout** — album art | lyrics/equalizer | playback queue
- Mouse cursor hides automatically during idle playback

!!! tip "Digital signage / wall-mounted tablet"
    Launch the browser in kiosk mode for a dedicated music display:
    ```
    chromium --kiosk http://192.168.1.100:8099/web?kiosk=1
    ```

### Player Grouping (Experimental)

You can group multiple MSX TVs to play the same music simultaneously:

- Enable or disable via the **"Enable player grouping"** setting
- Two stream modes:
    - **Independent** (default) — each TV gets its own audio stream
    - **Shared Buffer** — one audio stream shared across all grouped TVs (uses less CPU)

!!! warning "Experimental"
    Player grouping is experimental. If you experience issues with a multi-TV setup, try disabling it in the provider settings.

## Setup

### Smart TV (MSX App)

1. Install the **Media Station X** app on your Smart TV:
    - [Samsung Tizen (Smart Hub)](https://msx.benzac.de/info/more.html?tab=Samsung)
    - [LG webOS (LG Content Store)](https://msx.benzac.de/info/more.html?tab=LG)
    - [Android TV / Google TV (Google Play)](https://msx.benzac.de/info/more.html?tab=Android)
    - [Amazon Fire TV (Amazon Appstore)](https://msx.benzac.de/info/more.html?tab=Amazon)
    - [Apple TV (App Store)](https://msx.benzac.de/info/more.html?tab=Apple)
    - [Xbox (Microsoft Store)](https://msx.benzac.de/info/more.html?tab=Xbox)
    - Or open [msx.benzac.de](https://msx.benzac.de/info/more.html?tab=Browser) in any web browser

2. Open the MSX app and go to **Settings → Start Parameter**

3. Enter the start URL:
    ```
    http://<YOUR_SERVER_IP>:8099/msx/start.json
    ```
    Replace `<YOUR_SERVER_IP>` with the IP address of your Music Assistant server.

4. Restart the MSX app. You should see the Music Assistant menu: Recently Played, Albums, Artists, Playlists, Tracks, and Search.

!!! tip "Finding the right URL"
    Open `http://<YOUR_SERVER_IP>:8099/` in any browser — the status dashboard shows the exact URL to copy into MSX.

### Web Browser

Open the following URL in any modern browser:

```
http://<YOUR_SERVER_IP>:8099/web
```

No installation needed. Each browser tab becomes a separate player in Music Assistant.

### Status Dashboard

Open `http://<YOUR_SERVER_IP>:8099/` in a browser to see:

- The MSX setup URL (ready to copy into the TV)
- Links to the web player and kiosk mode
- All registered players with their current playback state
- A **Quick stop** button for each player

## Settings

Add the MSX Bridge provider under **Settings → Player Providers**.

| Setting | Description | Default |
|---------|-------------|---------|
| **HTTP Server Port** | The port the HTTP server listens on. MSX, the web player, and the REST API all use this port. Change if 8099 is occupied. | `8099` |
| **Audio Output Format** | Format for audio streaming. **MP3** works on all devices. Use **AAC** for better quality at the same bitrate, or **FLAC** for lossless audio (requires a fast network and a TV that supports FLAC). | `MP3` |
| **Player Idle Timeout** | Minutes of inactivity (no playback, no browsing) before a player is automatically removed. The player reappears instantly when the TV or browser reconnects. | `30` |
| **Show notification before closing player** | When stopping playback from Music Assistant, show a brief notification on the TV screen before closing the player. | Off |
| **Abort stream before broadcast stop** | When stopping playback, cut the audio stream before sending the stop command to the TV (instead of after). Try enabling this if the TV doesn't stop cleanly. | Off |
| **Enable player grouping** | Allow grouping multiple MSX TVs to play the same music simultaneously. Disable if you experience issues. | On |
| **Group Stream Mode** | How audio is delivered to grouped players. **Independent**: each TV gets its own stream (more reliable). **Shared Buffer**: one stream shared across all TVs in the group (less CPU, better sync). | Independent |

Standard [player settings](../settings/player-provider.md) apply to MSX Bridge players once they appear.

## How It Works

### On a Smart TV

1. The MSX app loads the start URL and receives a menu with your library sections
2. You browse content using the TV remote — each page loads instantly from the Music Assistant server
3. When you select a track, the provider creates a playlist from the current list and sends it to the TV
4. The TV fetches audio from the provider, which streams it in real time (Music Assistant → audio encoding → TV)
5. A WebSocket connection keeps the TV and Music Assistant in sync — play, pause, stop, next, previous all work from both sides

### In a Web Browser

1. The browser opens the web player page and generates a unique device ID
2. You browse the same library content as the TV — albums, artists, playlists, tracks, search
3. Clicking a track starts playback through the browser's built-in audio player
4. Position, pause, and resume are synchronized with Music Assistant via WebSocket — same as the TV

### Network

All traffic stays on your local network. The TV or browser communicates with the Music Assistant server over HTTP and WebSocket on the configured port (default: 8099). No cloud services, no external accounts, no internet connection required (beyond what your music sources need).

## REST API

The provider exposes a REST API on the same port that external clients or home automation systems can use:

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/albums` | List albums (supports `limit` and `offset` parameters) |
| GET | `/api/artists` | List artists |
| GET | `/api/playlists` | List playlists |
| GET | `/api/tracks` | List tracks |
| GET | `/api/albums/{id}/tracks` | Tracks for an album |
| GET | `/api/artists/{id}/albums` | Albums for an artist |
| GET | `/api/playlists/{id}/tracks` | Tracks for a playlist |
| GET | `/api/search?q=...` | Search the library |
| GET | `/api/recently-played` | Recently played tracks |
| GET | `/api/lyrics/{player_id}` | Lyrics for the currently playing track |
| GET | `/api/queue/{player_id}` | Current playback queue |
| POST | `/api/play` | Start playback (JSON body: `track_uri`, `player_id`) |
| ANY | `/api/pause/{player_id}` | Pause playback |
| ANY | `/api/stop/{player_id}` | Stop playback |
| ANY | `/api/quick-stop/{player_id}` | Immediate stop (abort stream + stop notification) |
| ANY | `/api/next/{player_id}` | Skip to next track |
| ANY | `/api/previous/{player_id}` | Skip to previous track |
| GET | `/health` | Health check |

!!! example "Example: play a track via curl"
    ```bash
    curl -X POST http://192.168.1.100:8099/api/play \
      -H "Content-Type: application/json" \
      -d '{"track_uri": "library://track/42", "player_id": "msx_my_tv"}'
    ```

!!! example "Example: pause a player"
    ```bash
    curl http://192.168.1.100:8099/api/pause/msx_my_tv
    ```

!!! example "Example: get lyrics for current track"
    ```bash
    curl http://192.168.1.100:8099/api/lyrics/msx_my_tv
    ```

## Known Issues and Limitations

- **Local network required** — the TV or browser must be able to reach the Music Assistant server over HTTP. They should be on the same network or routable to each other.
- **One provider instance** — only one MSX Bridge provider per Music Assistant server. Multiple TVs and browsers connect to the same instance.
- **MSX app required on TV** — the TV interface requires the free MSX app. Use the web player (`/web`) on devices where the MSX app is not available.
- **Audio format compatibility** — some older Smart TVs may not support AAC or FLAC. If audio doesn't play, switch to MP3 (the default) which has the widest compatibility.
- **Browser autoplay** — some browsers block audio autoplay until you interact with the page. If audio doesn't start, click the play button.
- **Long pause on TV** — pausing for more than a few minutes may cause the audio stream to expire. If this happens, select the track again to restart playback.
- **Port conflict** — if port 8099 is already used by another service, change the **HTTP Server Port** in the provider settings.

## Links

- [Media Station X](https://msx.benzac.de/) — MSX app homepage, download links, and documentation
- [MSX Music Assistant Bridge](https://github.com/trudenboy/msx-music-assistant) — provider source code
- [Music Assistant Server PR](https://github.com/music-assistant/server/pull/3123) — server integration pull request
