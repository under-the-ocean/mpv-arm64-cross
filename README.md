# ffmpeg-arm64-cross

Build [FFmpeg](https://ffmpeg.org/) for **ARM64 (aarch64)** — statically linked, includes `ffmpeg`, `ffplay`, `ffprobe`.

## Important note about Allwinner / cedrus hardware decoding

**FFmpeg upstream does not support the V4L2 stateless decoder API** used by the `cedrus` kernel driver (Allwinner H618/H616/H3).

The `h264_v4l2m2m` decoder in FFmpeg only works with **stateful** V4L2 M2M decoders. The `cedrus` driver on modern kernels (6.x) uses the **stateless** decoder API, which requires `V4L2_PIX_FMT_*_SLICE` formats — these are not supported by FFmpeg's V4L2 M2M wrapper.

### What works for hardware decoding on Orange Pi

| Method | Works? | How |
|--------|--------|-----|
| FFmpeg `h264_v4l2m2m` | ❌ | Not compatible with cedrus stateless API |
| MPV `--hwdec=v4l2m2m` | ❌ | Same limitation — relies on FFmpeg |
| **GStreamer** `v4l2slh264dec` | **✅** | Native support for V4L2 stateless decoders |

### Play with GStreamer (recommended)

```bash
# Hardware-accelerated playback via GStreamer
gst-launch-1.0 souphttpsrc location="video.mp4" ! qtdemux ! h264parse \
  ! v4l2slh264dec ! videoconvert ! autovideosink
```

This FFmpeg build can still be used for **software** decoding, encoding, transcoding, and all other FFmpeg features.

## Usage

1. Go to **Actions** → **Build FFmpeg 8.1.1 (arm64)** → **Run workflow**
2. Download `ffmpeg-8.1.1-arm64.zip` from the artifact
3. On your Orange Pi:

```bash
unzip ffmpeg-8.1.1-arm64.zip
chmod +x ffmpeg ffplay ffprobe

# Software decode (always works)
./ffplay -i video.mp4
```
