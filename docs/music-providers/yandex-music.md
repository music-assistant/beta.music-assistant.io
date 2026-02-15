# Yandex Music Provider ![Preview image](../assets/icons/yandex-music-icon.svg){ width=50 align=right }

Music Assistant has support for [Yandex Music](https://music.yandex.ru). Contributed and maintained by [TrudenBoy](https://github.com/TrudenBoy).

This provider is built on top of the [yandex-music-api](https://github.com/MarshalX/yandex-music-api) library.

!!! note
    A Yandex Music Plus subscription is required for lossless (FLAC) quality. Standard accounts can stream at high quality (320 kbps).

## Features

|           |                     |
|:-----------------------|:---------------------:|
| Subscription FREE | Yes (with limitations) |
| Self-Hosted Local Media | No |
| Media Types Supported | Artists, Albums, Tracks, Playlists |
| [Recommendations](../ui.md#view-home) Supported | Yes |
| Lyrics Supported | Yes |
| [Radio Mode](../ui.md#track-menu) | Yes |
| Maximum Stream Quality | Lossless FLAC (with Plus subscription) |
| Login Method | Token |

### Other

- Searching the Yandex Music catalogue is possible
- Items in a users Yandex Music library will be synced to Music Assistant
- Adding/removing items to/from the Music Assistant library will sync back to Yandex Music
- Browse is available to explore the Yandex Music catalogue
- **My Wave (Моя волна)** — personalized track feed, recommendations in Discover, and radio mode
- **Picks & Mixes** — curated playlists by mood, activity, era, genre, and season
- **Extended recommendations** — Made for You, Chart, New Releases, New Playlists on home page
- **Lyrics** — synced (LRC) and plain text lyrics from Yandex Music

## Audio Quality

Four quality tiers are available:

| Quality | Format | Bitrate | Requirements |
|---------|--------|---------|--------------|
| Efficient | AAC | ~64 kbps | Free account |
| Balanced | AAC | ~192 kbps | Free account |
| High | MP3 | ~320 kbps | Free account |
| Superb | FLAC | Lossless | Plus subscription |

### FLAC Streaming Modes

For encrypted FLAC streams, three streaming modes are available:

- **Direct** — On-the-fly decryption, best for fast devices
- **Buffered** — Async queue decoupling download from decryption (recommended)
- **Preload** — Full file download before playback, enables seek and accurate progress bar

## My Wave (Моя волна)

My Wave is Yandex Music's personalized track feed, powered by the Rotor API. It appears in several places in Music Assistant.

### Where it appears

- **Browse:** A root folder "Моя волна" / "My Wave" (locale-dependent). Opening it loads multiple batches of tracks. A "Load more" folder loads the next batch.
- **Recommendations (Discover):** A "Моя волна" section with personalized tracks.
- **Library playlists:** A virtual playlist "Моя волна" in your playlists list.

### What you can do

- Play from Browse, Discover, or the playlist in your library
- **Radio mode:** Start radio from any Yandex Music track to get similar tracks via Rotor
- **Rotor feedback:** The provider sends play events to Yandex to improve recommendations

### Configuration options

- Enable/disable My Wave in Browse, Discover, and library playlists
- Maximum tracks limit (default: 150)
- Batch count for loading (default: 3)
- Radio mode feedback toggle

## Liked Tracks

Your liked/favorited tracks are available as a virtual playlist with these features:

- **Reverse chronological sorting** — most recent likes first
- **Browse folder** — optional folder in Browse section
- **Virtual playlist** — appears in your library playlists
- **Configurable limit** — default 500 tracks

## Picks & Mixes

Curated playlists organized by category, available in Browse and on the home page.

### Browse structure

```
Yandex Music
├── Picks (Подборки)
│   ├── Mood — Chill, Sad, Romantic, Party, Relax
│   ├── Activity — Workout, Focus, Morning, Evening, Driving
│   ├── Era — 80s, 90s, 2000s, Retro
│   └── Genres — Rock, Jazz, Classical, Electronic, R&B, Hip-Hop
└── Mixes (Миксы)
    └── Seasonal — Winter, Summer, Autumn, New Year
```

### Discovery sections

| Section | Description | Rotation |
|---------|-------------|----------|
| Top Picks | Top curated playlists | Hourly |
| Mood Mix | Rotating mood playlists (chill, sad, romantic...) | Every 30 min |
| Activity Mix | Rotating activity playlists (workout, focus...) | Every 30 min |
| Seasonal Mix | Season-based playlists | Every 6 hours |

## Extended Recommendations

Additional sections on the home page:

- **Made for You** — Playlist of the Day, DejaVu, Premiere, and other personalized playlists
- **Chart** — Top chart tracks
- **New Releases** — Latest album releases
- **New Playlists** — Fresh editorial playlists

Each section can be enabled or disabled independently in settings.

## Lyrics

Lyrics are fetched directly from Yandex Music when viewing track details.

- **Synced lyrics (LRC)** — With timestamps for karaoke-style synchronized display
- **Plain text lyrics** — When synced lyrics are not available
- **Automatic caching** — Lyrics are cached for 30 days

!!! note
    Lyrics availability depends on the track and may vary by region due to licensing restrictions.

## Configuration

Configuration requires obtaining an OAuth token from Yandex Music.

### Obtaining the Token

1. Open your browser and navigate to [Yandex OAuth](https://oauth.yandex.ru/authorize?response_type=token&client_id=23cabbbdc6cd418abb4b39c32c41195d)
2. Log in with your Yandex account if prompted
3. After authorization, you will be redirected to a URL containing `access_token=YOUR_TOKEN`
4. Copy the token value (the part after `access_token=` and before `&`)
5. Paste this token into the Music Assistant Yandex Music provider configuration

### Settings Categories

Settings are organized into categories for easier navigation:

| Category | Settings |
|----------|----------|
| **My Wave** | Enable in Browse/Discover/Playlists, max tracks, batch count, radio feedback |
| **Liked Tracks** | Enable in Browse/Playlists, max tracks |
| **Discovery** | Made for You, Chart, New Releases, New Playlists toggles |
| **Picks & Mixes** | Enable Picks/Mixes in Browse, Top Picks, Mood/Activity/Seasonal Mix on home |
| **Streaming** | FLAC streaming mode, Preload max file size |
| **Advanced** | Track batch size, initial tracks limits, API base URL |

## Localization

All folder names and labels are available in Russian and English. The language is determined automatically based on Music Assistant locale settings:

- Russian (`ru_*` locales): "Моя волна", "Подборки", "Миксы", etc.
- Other locales: "My Wave", "Picks", "Mixes", etc.

## Known Issues / Notes

- The token may expire and need to be refreshed periodically
- During long sessions the API may occasionally disconnect; the provider reconnects automatically
- Some curated playlists may be unavailable in certain regions due to geo-restrictions

## Not yet supported

- Multiple Yandex Music accounts cannot be added as yet
