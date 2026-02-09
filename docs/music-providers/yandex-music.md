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
| Lyrics Supported | No |
| [Radio Mode](../ui.md#track-menu) | Yes |
| Maximum Stream Quality | Lossless FLAC (with Plus subscription) |
| Login Method | Token |

### Other

- Searching the Yandex Music catalogue is possible
- Items in a users Yandex Music library will be synced to Music Assistant
- Adding/removing items to/from the Music Assistant library will sync back to Yandex Music
- Browse is available to explore the Yandex Music catalogue
- **My Wave (Моя волна)** — personalized track feed, recommendations in Discover, and radio mode (similar tracks via Rotor)

## My Wave (Моя волна)

My Wave is Yandex Music’s personalized track feed, powered by the Rotor API. It appears in several places in Music Assistant.

### Sync and where it appears

- **Browse:** A root folder “Моя волна” / “My Wave” (name depends on locale: Russian for `ru_*`, English otherwise). Opening it loads up to three batches of tracks so that starting playback adds more to the queue. A “Load more” / “Ещё” folder loads the next batch (pagination by first track in the chain in the API).
- **Recommendations (Discover):** A single “Моя волна” section with the first batch of tracks.
- **Library playlists:** A virtual playlist “Моя волна” is shown first in the playlists list; tracks are loaded in pages via `get_playlist_tracks` with pagination.

### What you can do

- Play from Browse (folder), from Discover, or from the “Моя волна” playlist in your library.
- **Radio mode:** When you start radio from a Yandex Music track, similar tracks are added to the queue using the Rotor station `track:{id}`.
- **Rotor feedback:** While playing My Wave, the provider sends `radioStarted`, `trackStarted`, `trackFinished` / skip events to the API to improve recommendations. On connection errors (e.g. “Server disconnected”), it reconnects and retries the request.

## Configuration

Configuration requires obtaining an OAuth token from Yandex Music.

### Obtaining the Token

1. Open your browser and navigate to [Yandex OAuth](https://oauth.yandex.ru/authorize?response_type=token&client_id=23cabbbdc6cd418abb4b39c32c41195d)
2. Log in with your Yandex account if prompted
3. After authorization, you will be redirected to a URL containing `access_token=YOUR_TOKEN`
4. Copy the token value (the part after `access_token=` and before `&`)
5. Paste this token into the Music Assistant Yandex Music provider configuration

### Settings

- **Audio quality**: Select preferred audio quality. The default is `High (320 kbps)` which is available for all accounts. The other option is `Lossless (FLAC)` which requires a Yandex Music Plus subscription

## Known Issues / Notes

- The token may expire and need to be refreshed periodically
- During long My Wave sessions the API may occasionally disconnect; the provider will reconnect and retry automatically

## Not yet supported
- Multiple Yandex Music accounts cannot be added as yet
