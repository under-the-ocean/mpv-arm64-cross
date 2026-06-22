# mpv-arm64-cross

Cross-compile mpv with v4l2-m2m hardware video decoding support for ARM64 (aarch64).

## Usage

1. Fork or push this repo to GitHub
2. Go to Actions → "Build mpv with v4l2-m2m for arm64" → Run workflow
3. Wait ~20-30 min for the build to finish
4. Download the `mpv-arm64` artifact
5. On your Orange Pi:
   ```bash
   tar xzf mpv-arm64.tar.gz
   cp bin/mpv /usr/local/bin/
   cp lib/*.so* /usr/local/lib/
   ldconfig
   ```

## Playback

```bash
# Hardware decode (auto)
mpv --hwdec=auto video.mp4

# Force v4l2-m2m
mpv --hwdec=v4l2m2m video.mp4

# With DRM display (no X11 needed)
mpv --vo=drm video.mp4
```
