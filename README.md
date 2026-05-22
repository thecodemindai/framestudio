# FrameStudio

> A focused video frame extraction and grid composition tool with built-in URL
> import. Available as a web app or a packaged Windows desktop app.

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)
[![Node](https://img.shields.io/badge/node-%3E%3D18-43853d.svg)](#requirements)
[![Electron](https://img.shields.io/badge/electron-42.x-9feaf9.svg)](https://www.electronjs.org/)

FrameStudio (formerly FrameCap) helps you extract precise frames from any video,
arrange them into grids, annotate, and export — all locally, with zero uploads
to a third-party service.


## Features

- **Drop or open** any local video — no uploading.
- **Import from URL** — paste a YouTube, Vimeo, TikTok, Twitch, or any
  [yt-dlp-supported](https://github.com/yt-dlp/yt-dlp/blob/master/supportedsites.md)
  link. Pick a quality preset or an exact stream, optionally trim a section.
- **Manual + automatic frame capture** — by interval, count, FPS, or just the
  in/out endpoints.
- **Timeline scrubbing** with zoom, in/out points, and keyboard shortcuts.
- **Per-frame transforms** — flip, rotate, watermark, notes, tags, favorites.
- **Grid composer** — turn captured frames into shareable contact sheets.
- **Watermark remover** mode for region-based masking.
- **yt-dlp console** for power users who want raw command access.
- **Custom save folder** in the desktop build (frames + downloaded videos go
  straight to your folder of choice).
- **Window resize presets** + **screen zoom** for accessibility on high-DPI
  displays.
- **Video history** — last 20 videos with thumbnails.
- **Three themes** — light, dark, system; chosen during first-run setup.
- **First-run setup wizard** with animated steps for theme, yt-dlp install
  guidance, and folder selection.

## Screenshots

> Add screenshots / GIFs here when you publish.

## Requirements

- **Node.js** ≥ 18
- **npm** ≥ 9 (or pnpm / yarn equivalent)
- For URL imports: **[yt-dlp](https://github.com/yt-dlp/yt-dlp)** and
  **[ffmpeg](https://ffmpeg.org/)** on `PATH` (or pointed at via env vars)

## Quick start

```bash
git clone https://github.com/<you>/framestudio.git
cd framestudio
npm install
npm run dev:all
```

Then open <http://localhost:3000>.

### Optional: install yt-dlp + ffmpeg for URL downloads

| Platform | Command |
| --- | --- |
| Windows  | `winget install yt-dlp.yt-dlp` (bundles ffmpeg) |
| macOS    | `brew install yt-dlp ffmpeg` |
| Linux    | `pipx install yt-dlp` plus ffmpeg from your package manager |

If the binaries aren't on `PATH`, copy [`.env.example`](./.env.example) to
`.env` and set `YT_DLP_PATH` / `FFMPEG_PATH` to the absolute paths.

## Scripts

| Script | What it does |
| --- | --- |
| `npm run dev` | Vite dev server only (port 3000) |
| `npm run dev:server` | yt-dlp Express backend only (port 5174) |
| `npm run dev:all` | Both of the above, with combined logs |
| `npm run dev:electron` | Above + an Electron shell |
| `npm run build` | Vite production build to `dist/` |
| `npm run build:installer` | Builds the Windows NSIS installer to `release2/` |
| `npm run lint` | `tsc --noEmit` (type-check) |
| `npm run preview` | Preview the production build |

## Architecture

```
.
├── electron/        Electron main process, preload bridge, embedded server
│   ├── main.cjs     Window lifecycle, IPC handlers (window resize, save folder)
│   ├── preload.cjs  contextBridge surface exposed to the renderer
│   └── server.cjs   Production-mode Express server (yt-dlp wrapper)
├── server/
│   └── index.ts     tsx-driven dev server (same routes as server.cjs)
├── src/             React + Vite front-end
│   ├── App.tsx               Main shell, capture/grid logic
│   ├── Setup.tsx             First-run setup wizard
│   ├── GridsImage.tsx        Grid composer view
│   ├── WatermarkRemover.tsx  Region-mask watermark remover
│   └── YtDlpConsole.tsx      Raw yt-dlp console
├── electron-builder.json     Windows installer config
├── vite.config.ts            Vite config (proxies /api → :5174)
└── tsconfig.json
```

The renderer never invokes yt-dlp directly. It posts to `POST /api/info` to
inspect a URL, then `POST /api/download/start` to kick off a download. Progress
is streamed over Server-Sent Events; the finished file is fetched from
`GET /api/download/file/:id` and handed to the editor as a `File` object — no
re-upload required.

## Configuration

`.env` (optional, only if auto-detection fails):

```env
# Absolute path to the yt-dlp executable
YT_DLP_PATH=/usr/local/bin/yt-dlp

# Absolute path to ffmpeg
FFMPEG_PATH=/usr/local/bin/ffmpeg

# Backend port (default 5174)
PORT=5174
```

Persistent app preferences (theme, save folder, video history) are stored in
your browser's `localStorage` for the web build and in
`%APPDATA%/<app-name>/framecup-settings.json` for the desktop build.

## Building the desktop app

```bash
npm run build:installer
```

Output:
- `release2/FrameStudio-Setup-1.0.0.exe` — Windows NSIS installer
- `release2/win-unpacked/` — portable Windows build

The desktop build expects `binaries/yt-dlp.exe` and `binaries/ffmpeg.exe` next
to `package.json` if you want to bundle them. We do not ship pre-compiled
binaries in this repository (see [NOTICE.md](./NOTICE.md) for licensing
reasons); install them via your package manager and copy them in if you want a
self-contained installer.

## Privacy

FrameStudio runs entirely on your machine. Videos you load — whether from disk
or downloaded via yt-dlp — never leave your computer. No telemetry, analytics,
or remote logging is sent. The download server is bound to `localhost` only.

## License

[MIT](./LICENSE) — © 2026 FrameStudio Contributors. See [NOTICE.md](./NOTICE.md)
for third-party software acknowledgements.

## Contributing

Pull requests are welcome. See [CONTRIBUTING.md](./CONTRIBUTING.md) for the dev
loop and code-style conventions.
