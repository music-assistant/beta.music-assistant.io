---
title: AriaCast Receiver Plugin
description: Features and Notes for the AriaCast Receiver Plugin
---

# AriaCast Receiver ![AriaCast Icon](https://raw.githubusercontent.com/AirPlr/beta.music-assistant.io/refs/heads/main/docs/assets/icons/ariacast-receiver-icon.svg){ width=70 align=right }

The **AriaCast Receiver** plugin allows you to stream high-quality audio wirelessly from Android devices to any Music Assistant player.

!!! info "Status: Active Development"
    This plugin is under active development.

--- 

### Configuration
1. Enable **AriaCast Receiver** in Music Assistant settings.
2. Configure basic settings:
   - **Server Name**: How it appears in discovery (default: "Music Assistant").
   - **Target Player**: Select a specific player or set to "Auto".
   - **Ports**: 12888 (discovery), 12889 (streaming).

### Usage
1. Install the [AriaCast Android app](https://github.com/AirPlr/AriaCast-app).
2. Open the app — it will automatically discover servers on your network.
3. Select your Music Assistant server and select an app
4. Enjoy!

## Configuration Options

| Setting | Default | Description |
|:---|:---|:---|
| **Server Name** | AriaCast Speaker | Name shown in client discovery |
| **Connected Player** | Auto | Target Music Assistant player |
| **Streaming Port** | 12889 | WebSocket/HTTP port for all endpoints |
| **Discovery Port** | 12888 | UDP discovery port |
| **Allow Player Switching** | Yes | Enable manual source selection |


## Troubleshooting

- **Server Not Found**: Ensure both devices are on the same network. Check that your firewall allows UDP port 12888 and TCP port 12889.
- **No Audio Playback**: Verify the target Music Assistant player is not already in use.
