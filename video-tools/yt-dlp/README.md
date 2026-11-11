# yt-dlp for video editing

Pull footage from YouTube, Vimeo, X, TikTok and over 1,000 other sites straight into an editor-ready MP4.

The config in this folder handles the annoying part. Every download comes out as H.264 video with AAC audio, which Premiere, Final Cut, DaVinci and CapCut all import without converting.

## 1. Install (one time)

You need three tools. yt-dlp does the downloading, ffmpeg joins video and audio, and Deno lets yt-dlp read YouTube's player.

**Mac** (needs [Homebrew](https://brew.sh))

```bash
brew install yt-dlp ffmpeg deno
```

**Windows** (PowerShell)

```powershell
winget install yt-dlp.yt-dlp
winget install Gyan.FFmpeg
winget install DenoLand.Deno
```

Close and reopen the terminal, then confirm all three answer:

```bash
yt-dlp --version
ffmpeg -version
deno --version
```

## 2. Drop in the config (one time)

Copy `yt-dlp.conf` from this folder into your home folder.

```bash
# Mac
cp yt-dlp.conf ~/yt-dlp.conf
```

```powershell
# Windows
Copy-Item yt-dlp.conf $HOME\yt-dlp.conf
```

yt-dlp reads it automatically from then on. Downloads land in `Downloads/yt-dlp`.

## 3. Everyday commands

Full video, editor-ready:

```bash
yt-dlp "URL"
```

Just one section (frame-accurate, named by its start and end seconds):

```bash
yt-dlp --download-sections "*1:30-2:45" --force-keyframes-at-cuts -o "%(title).90B [%(id)s] %(section_start)s-%(section_end)s.%(ext)s" "URL"
```

Audio only, as WAV (saved to its own `audio` folder so it never touches your video files):

```bash
yt-dlp -f ba -x --audio-format wav -P "~/Downloads/yt-dlp/audio" "URL"
```

Captions as an SRT file for your timeline:

```bash
yt-dlp --skip-download --write-subs --write-auto-subs --sub-langs en --convert-subs srt "URL"
```

4K. YouTube only offers H.264 up to 1080p, so this grabs the best quality and re-encodes it. Expect it to take a while.

```bash
yt-dlp -f "bv*+ba/b" --merge-output-format mkv --recode-video mp4 "URL"
```

## 4. When something breaks

YouTube changes its site often. When downloads suddenly fail, update first. It fixes most problems.

```bash
brew upgrade yt-dlp               # Mac
winget upgrade yt-dlp.yt-dlp      # Windows
```

If a file won't import into your editor, convert it:

```bash
ffmpeg -i "input.mp4" -c:v libx264 -crf 18 -c:a aac "fixed.mp4"
```

## Use it clean

Download your own videos, footage you've licensed, Creative Commons or public domain material, or anything you have written permission to use. YouTube's terms bar downloading other people's videos without permission, and publishing their footage in your edits can get your channel struck or your client's ad pulled.
