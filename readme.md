# yt2phone

Download any YouTube URL as MP3 and sync it to your phone.

- Primary sync: **Syncthing** (drop into watched folder, phone gets it automatically)
- Fallback sync: **SSH/rsync** (for when Tailscale/VPN conflicts on the phone)

## Dependencies

- [`yt-dlp`](https://github.com/yt-dlp/yt-dlp)
- `ffmpeg`
- `rsync` (only for SSH fallback)

## Install

```bash
# Symlink the script somewhere on your PATH
ln -sf /path/to/yt2phone ~/.local/bin/yt2phone
chmod +x ~/.local/bin/yt2phone
```

## Usage

```bash
yt2phone <url>                   # download + drop into Syncthing folder
yt2phone <url1> <url2>           # batch
yt2phone --ssh <url>             # force SSH/rsync to phone
yt2phone --dry-run <url>         # preview without downloading
yt2phone --dry-run --ssh <url>   # preview SSH path
```

## Config

Set via environment variables (add to `.bashrc` / `.zshrc`):

| Variable        | Default                        | Description                        |
|-----------------|--------------------------------|------------------------------------|
| `MUSIC_DIR`     | `/home/$USER/Syncthing/Music`  | Local Syncthing music folder       |
| `PHONE_SSH`     | *(empty)*                      | SSH target e.g. `user@192.168.1.x` |
| `PHONE_SSH_PATH`| `/sdcard/Music`                | Remote music path on phone         |

```bash
export PHONE_SSH=user@192.168.1.x
```

## How it works

1. `yt-dlp` downloads the video and converts to MP3 (best quality, embedded thumbnail + metadata)
2. **Syncthing mode**: file is copied into `MUSIC_DIR` — Syncthing pushes it to the phone
3. **SSH mode** (`--ssh`): file is transferred directly via `rsync` over SSH
