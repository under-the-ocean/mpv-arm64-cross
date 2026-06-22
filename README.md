# ffmpeg-arm64-v4l2-request

Build [FFmpeg](https://ffmpeg.org/) with **V4L2 request API** hardware video decoding support for **ARM64 (aarch64)** — designed for Allwinner H618 / H616 / H6 / H3 chips with the `cedrus` kernel driver (Orange Pi Zero 3, Orange Pi 3B, etc.).

## Why?

The `cedrus` V4L2 M2M stateless decoder in modern Linux kernels (6.x) requires the **V4L2 request API** (`--enable-v4l2-request`), which most pre-built FFmpeg packages don't include.

This workflow compiles FFmpeg **statically** with the right flags so you can just download and run it on your board.

## Usage

1. Go to **Actions** → **Build FFmpeg 8.1.1 (arm64 + V4L2 request)** → **Run workflow**
2. Wait ~15-20 min for the build
3. Download `ffmpeg-8.1.1-arm64.zip` from the artifact
4. On your Orange Pi:

```bash
unzip ffmpeg-8.1.1-arm64.zip
chmod +x ffmpeg ffplay ffprobe

# Hardware-accelerated playback
./ffplay -hwaccel v4l2request -i video.mp4

# Or with explicit decoder
./ffplay -codec:v h264_v4l2m2m -i video.mp4

# Transcode with hardware decode
./ffmpeg -hwaccel v4l2request -i input.mp4 -c:v libx264 output.mp4
```

## How it works

- Runs on GitHub's native **ARM64 runner** (`ubuntu-24.04-arm`)
- Compiles FFmpeg 8.1.1 from source with `--enable-v4l2-request`
- Statically linked — no extra libraries needed on your board
- Includes `ffmpeg`, `ffplay`, `ffprobe`
