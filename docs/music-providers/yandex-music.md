# Yandex Music Provider ![Preview image](../assets/icons/yandex-music-icon.svg){ width=50 align=right }

Music Assistant has support for [Yandex Music](https://music.yandex.ru). Contributed and maintained by [TrudenBoy](https://github.com/TrudenBoy).

This provider is built on top of the [yandex-music-api](https://github.com/MarshalX/yandex-music-api) library.

!!! note
    A Yandex Music Plus subscription is required for lossless (FLAC) quality. Standard accounts can stream up to high quality (320 kbps MP3).

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
- **My Wave** — personalized infinite radio powered by Yandex's AI
- **Picks & Mixes** — curated playlists organized by mood, activity, era, genre, and season
- **Extended recommendations** — Made for You, Chart, New Releases, New Playlists, and more on the home page
- **Lyrics** — synced (LRC) and plain text lyrics

## Audio Quality

Four quality tiers are available:

| Quality | Format | Bitrate | Requirements |
|---------|--------|---------|--------------|
| Efficient | AAC | ~64 kbps | Free account |
| Balanced (default) | AAC | ~192 kbps | Free account |
| High | MP3 | ~320 kbps | Free account |
| Superb | FLAC | Lossless | Plus subscription |

You can change the quality at any time in the provider settings. The actual codec and bitrate are selected automatically when playback starts, based on what Yandex Music offers for each track.

### FLAC Streaming Modes

When streaming encrypted FLAC tracks (Superb quality), you can choose how decryption is handled:

- **Direct** — Decrypts on-the-fly as data arrives. Uses the least memory but requires a fast device.
- **Buffered** (default, recommended) — Separates the download and decryption into an async queue. Works well on most hardware.
- **Preload** — Downloads and decrypts the entire file before playback starts. This takes longer to begin playing, but enables seeking and shows an accurate progress bar. If the file is larger than the configured limit (default: 100 MB), Preload automatically falls back to Buffered mode.

!!! tip
    Most users should leave the streaming mode at **Buffered**. Only change it if you experience playback issues or specifically need seek support in lossless tracks.

## My Wave

My Wave is Yandex Music's personalized infinite radio. It uses Yandex's Rotor recommendation engine to generate a continuous stream of tracks tailored to your listening habits.

### How it works

1. When you open My Wave, the provider requests several batches of tracks from Yandex's Rotor API (the same engine behind "My Wave" in the official Yandex Music app).
2. Each batch contains a few tracks. The provider fetches multiple batches at once to give you a longer playlist to start with.
3. Duplicate tracks are automatically filtered out — if a track appeared in a previous batch, it won't show up again.
4. In Browse, a **"Load more"** button appears at the bottom. Tapping it fetches the next batch and adds more tracks.
5. The maximum number of tracks is configurable (default: 150). Once the limit is reached, no more tracks are loaded.

### Improving your recommendations

The provider sends playback feedback to Yandex, similar to what the official app does:

- When you **start playing** a track, Yandex is notified.
- When you **finish** a track (listen to nearly the end), Yandex counts it as "liked."
- When you **skip** a track (stop before the end), Yandex adjusts future recommendations accordingly.

This feedback happens automatically — you don't need to do anything. Over time, Yandex learns your preferences and My Wave becomes more personalized.

### Where My Wave appears

- **Browse** — A "My Wave" folder at the root of Yandex Music. You can play it directly or browse individual tracks.
- **Home page (Discover)** — A "My Wave" recommendation section with a selection of personalized tracks.
- **Library playlists** — A virtual "My Wave" playlist always appears in your playlist list for quick access.

### Similar tracks (Radio mode)

When you start radio mode from any Yandex Music track, the provider uses Yandex's Rotor engine to find similar tracks. This works for any track, not just My Wave — it creates a station based on that specific track's style and genre.

## Liked Tracks

Your liked (favorited) tracks are available as a virtual playlist:

- Sorted in **reverse chronological order** — your most recently liked tracks appear first
- Always visible in your library playlists and Browse section
- The maximum number of tracks is configurable (default: 500)
- Full track details (including album art) are fetched automatically

## Picks & Mixes

Picks and Mixes let you browse curated playlists organized by theme, available in the Browse section.

### Browse structure

```
Yandex Music
├── Picks
│   ├── Mood — Chill, Sad, Romantic, Party, Relax
│   ├── Activity — Workout, Focus, Morning, Evening, Driving
│   ├── Era — 80s, 90s, 2000s, Retro
│   └── Genres — Rock, Jazz, Classical, Electronic, R&B, Hip-Hop
└── Mixes
    └── Seasonal — Winter, Summer, Autumn, New Year
```

The categories under Picks contain fixed tags, but additional tags may be discovered dynamically from Yandex Music's catalog (refreshed hourly). These extra tags appear in the Mood category.

Each tag opens a list of curated playlists you can play directly.

## Home Page Recommendations

The home page shows up to 9 recommendation sections. All sections are always enabled — there are no toggles to show or hide individual sections.

| Section | What it shows | How often it refreshes |
|---------|--------------|----------------------|
| **My Wave** | Personalized tracks from Rotor AI | Every 10 minutes |
| **Made for You** | Playlist of the Day, DejaVu, Premiere, and other auto-generated playlists | Every 30 minutes |
| **Chart** | Current top tracks | Every hour |
| **New Releases** | Latest album releases | Every hour |
| **New Playlists** | Fresh editorial playlists | Every hour |
| **Top Picks** | Top curated playlists (tag: "top") | Every hour |
| **Mood Mix** | Playlists for a random mood (rotates between chill, sad, romantic, party, relax) | Every 30 minutes |
| **Activity Mix** | Playlists for a random activity (rotates between workout, focus, morning, evening, driving) | Every 30 minutes |
| **Seasonal Mix** | Season-appropriate playlists based on the current month (winter, summer, autumn) | Every 6 hours |

The Mood Mix and Activity Mix sections show a different mood/activity each time they refresh, so you'll see variety throughout the day. The Seasonal Mix picks the season based on the current month automatically.

## Lyrics

Lyrics are fetched directly from Yandex Music when viewing track details:

- **Synced lyrics (LRC)** — With timestamps for karaoke-style synchronized display, when available
- **Plain text lyrics** — Used as fallback when synced lyrics are not available
- **Automatic caching** — Lyrics are cached together with track data for 30 days

!!! note
    Lyrics availability depends on the track and may vary by region due to licensing restrictions. If lyrics are unavailable for your region, the provider handles this silently without errors.

## Configuration

### Obtaining the Token

1. Open your browser and navigate to [Yandex OAuth](https://oauth.yandex.ru/authorize?response_type=token&client_id=23cabbbdc6cd418abb4b39c32c41195d)
2. Log in with your Yandex account if prompted
3. After authorization, you will be redirected to a URL containing `access_token=YOUR_TOKEN`
4. Copy the token value (the part after `access_token=` and before `&`)
5. Paste this token into the Music Assistant Yandex Music provider configuration

### Settings

The provider has 8 settings. The first 3 are shown by default; the rest are under "Show advanced settings."

| Setting | Default | Description |
|---------|---------|-------------|
| **Yandex Music Token** | — | Your OAuth token (see above) |
| **Reset authentication** | — | Clears the saved token so you can re-authenticate |
| **Audio quality** | Balanced | Choose between Efficient, Balanced, High, or Superb (FLAC) |

**Advanced settings** (click "Show advanced settings" to see these):

| Setting | Default | Description |
|---------|---------|-------------|
| **FLAC streaming mode** | Buffered | How encrypted FLAC streams are decoded (Direct, Buffered, or Preload) |
| **Preload max file size (MB)** | 100 | Only visible when Preload mode is selected. Files larger than this use Buffered mode instead |
| **My Wave maximum tracks** | 150 | How many tracks to load for My Wave. Lower = faster loading |
| **Liked Tracks maximum tracks** | 500 | How many liked tracks to show. Lower = faster loading |
| **API Base URL** | api.music.yandex.net | Only change if Yandex changes their API endpoint |

## Localization

All folder and section names automatically adapt to your Music Assistant language:

- **Russian** (`ru_*` locales): "Моя волна", "Мне нравится", "Подборки", "Миксы", etc.
- **Other locales**: "My Wave", "My Favorites", "Picks", "Mixes", etc.

## Known Issues / Notes

- The OAuth token may expire and need to be refreshed periodically
- During long sessions the API may occasionally disconnect; the provider reconnects automatically
- Some curated playlists may be unavailable in certain regions due to geo-restrictions
- Multiple Yandex Music accounts are not yet supported
