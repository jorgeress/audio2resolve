# audio2resolve

*[Versión en español](README.es.md)*

Converts audio to PCM WAV so **DaVinci Resolve on Linux** imports it without a fuss.

Resolve on Linux doesn't ship AAC decoders, so the `.m4a` and `.mp3` files
that come off a phone or Google Drive import silent, with the wrong duration,
or fail outright. This script converts them to PCM WAV, which Resolve reads
natively: no decoding, instant scrubbing and exact sync.

## Requirements

- `ffmpeg`
- `unzip` (only if you pass it `.zip` files)

## Installation

```bash
curl -o ~/.local/bin/audio2resolve \
  https://raw.githubusercontent.com/jorgeress/audio2resolve/main/audio2resolve
chmod +x ~/.local/bin/audio2resolve
```

Make sure `~/.local/bin` is on your `PATH`.

## Usage

```bash
audio2resolve recording.m4a          # a single file
audio2resolve ~/Downloads/audio/     # all the audio in a folder
audio2resolve download.zip           # unzips and converts whatever is inside
```

Without `-o`, the WAVs go into a `wav_resolve/` subfolder next to the source.
The originals are never touched.

The exit code is 0 if everything converted and 1 if any file failed, so you
can use it in other scripts.

## Options

| Option | What it does |
|---|---|
| `-o DIR` | Output folder |
| `-b 16\|24\|32` | Bit depth (default 24) |
| `-r HZ` | Sample rate (default: the original's) |
| `-s` | Force stereo (default: keep the original channels) |
| `-n` | Normalize to -16 LUFS with `loudnorm`, handy for voice recorded at uneven levels |
| `-f` | Redo WAVs that already exist (skipped by default) |
| `-h` | Help |

Since it skips what's already converted by default, you can rerun it on the
same folder every time you add material and it will only process what's new.

## Input formats

`m4a`, `mp3`, `aac`, `opus`, `ogg`, `oga`, `wma`, `flac`, `wav`, `aiff`, `aif`
and video (`mp4`, `mov`, `mkv`, `webm`), from which it extracts the audio
track.

## Why 24 bits

It's the post-production standard and gives the mix headroom without a
noticeable size cost for short tracks. If space matters to you, `-b 16` is
plenty for voice.

## License

MIT, in the [LICENSE](LICENSE) file.
