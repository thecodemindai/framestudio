# Third-party software

FrameStudio is licensed under the MIT License (see [LICENSE](./LICENSE)).
This project depends on or interoperates with the following third-party software,
each of which retains its own license. None of these are bundled in this
repository — they are installed separately at build time (`npm install`) or by
the end user (system tools).

## npm dependencies

| Package | License |
| --- | --- |
| react, react-dom | MIT |
| vite, @vitejs/plugin-react | MIT |
| @tailwindcss/vite, tailwindcss, autoprefixer | MIT |
| express, dotenv | MIT |
| jszip | MIT or GPL-3.0 (dual) |
| lucide-react | ISC |
| motion (formerly framer-motion) | MIT |
| electron, electron-builder | MIT |
| typescript, tsx, concurrently | Apache-2.0 / MIT |
| @types/* | MIT |

Run `npm ls --json` after installing to inspect the full transitive dependency
tree. Each package's license is included under `node_modules/<pkg>/LICENSE` once
installed.

## External tools (NOT redistributed in this repository)

These are runtime dependencies you install on your own machine:

- **[yt-dlp](https://github.com/yt-dlp/yt-dlp)** — Unlicense (public domain).
  Used to fetch videos from URLs. Install via `winget`, `brew`, `pipx`, or your
  distribution package manager.
- **[ffmpeg](https://ffmpeg.org/)** — LGPL or GPL depending on the build.
  Used by yt-dlp to merge separate video and audio streams. Install via your
  package manager.

We deliberately do not redistribute compiled `yt-dlp` or `ffmpeg` binaries in
this repository because their packaging requires careful license compliance
(notably, GPL builds of ffmpeg require source-code distribution and an explicit
GPL relicensing of the bundle). The application detects them on `PATH` at
runtime and prompts the user to install them if missing.

## Electron runtime

Built artifacts produced by `npm run build:electron` include a copy of the
[Electron](https://www.electronjs.org/) binary (MIT) and its embedded Chromium
(BSD-style + many other licenses, see `LICENSES.chromium.html` in the build
output). These artifacts are intentionally git-ignored.
