<p align="center">
  <img src="icon.png" alt="QbitPull" width="128">
</p>

<h1 align="center">QbitPull</h1>


QbitPull automatically downloads completed torrents from a remote qBittorrent instance via SFTP, using **lftp** for fast parallel transfers. It includes a live web dashboard with real-time download progress, ETA, pause/resume controls, and download history.

## How It Works

QbitPull polls your remote qBittorrent instance for torrents tagged with a specific tag (default: `ftp`). When it finds completed torrents, it downloads them to your local server via SFTP using lftp's parallel transfer capability. Once the download is complete, the torrent is re-tagged (default: `done`) so it won't be downloaded again.

## Features

- **Fast parallel SFTP downloads** — Uses lftp with configurable parallel connections (2–8) for maximum transfer speed.
- **Live web dashboard** — Real-time progress display with download speed, ETA, file size, and percentage complete.
- **Pause / Resume** — Pause and resume active downloads directly from the dashboard.
- **Download history** — View past downloads with status (completed, skipped, failed), speed, duration, and timestamps.
- **Skip if exists** — Automatically skips torrents that have already been downloaded locally.
- **Partial download resume** — Detects interrupted downloads (.part files) and resumes them instead of starting over.
- **Auto-reconnect** — Reconnects to qBittorrent and SFTP automatically if the connection drops.
- **Tag-based workflow** — Uses qBittorrent tags to track download state.
- **Unraid compatible** — Designed for Unraid with proper file permissions (umask 0000).

## Installation

### Unraid (Community Applications)

[Soon...] Search for **qbitpull** in the Unraid Community Applications store and click Install.

### Unraid (Manual)

Pull the image from GitHub Container Registry:

```
docker pull ghcr.io/coppiman11/qbitpull:latest
```

### Docker Run

```bash
docker run -d \
  --name qbitpull \
  -p 8686:8686 \
  -v /path/to/downloads:/data \
  -e QBIT_HOST="https://your-qbit-host/qbittorrent/" \
  -e QBIT_USER="username" \
  -e QBIT_PASS="password" \
  -e SFTP_HOST="your-sftp-host" \
  -e SFTP_PORT="22" \
  -e SFTP_USER="username" \
  -e SFTP_PASS="password" \
  -e SOURCE_TAG="ftp" \
  -e DONE_TAG="done" \
  ghcr.io/coppiman11/qbitpull:latest
```

## Configuration

| Variable | Default | Description |
|---|---|---|
| `QBIT_HOST` | — | qBittorrent Web API URL |
| `QBIT_USER` | — | qBittorrent username |
| `QBIT_PASS` | — | qBittorrent password |
| `SFTP_HOST` | — | Remote SFTP server hostname or IP |
| `SFTP_PORT` | `22` | Remote SFTP server port |
| `SFTP_USER` | — | SFTP username |
| `SFTP_PASS` | — | SFTP password |
| `SOURCE_TAG` | `ftp` | Tag that marks torrents ready for download |
| `DONE_TAG` | `done` | Tag applied after successful download |
| `ERROR_TAG` | `error` | Tag applied when download fails |
| `LFTP_PARALLEL` | `4` | Number of parallel lftp connections (2–8) |
| `POLL_SECONDS` | `30` | Seconds between polling for new torrents |
| `SKIP_IF_EXISTS` | `1` | Skip if local file already exists (1=yes, 0=no) |
| `SSH_KEEPALIVE` | `15` | SSH keepalive interval in seconds |
| `DASHBOARD_PORT` | `8686` | Web dashboard port |

## Web Dashboard

Access the dashboard at `http://<your-server-ip>:8686`. It shows:

- **Active download** — File name, progress bar, speed, ETA, and size.
- **Pause / Resume** — Control the active download with a single click.
- **Download history** — Table of all past downloads with status, speed, duration, and relative timestamps.

## Volumes

| Container Path | Description |
|---|---|
| `/data` | Local directory where downloaded torrents are saved |

## Ports

| Port | Description |
|---|---|
| `8686` | Web dashboard |

## Support

For questions, issues, or feature requests, please open an issue on [GitHub](https://github.com/coppiman11/qbitpull/issues).
