# Contributing to FrameStudio

Thanks for your interest in contributing. This project welcomes pull requests
for bug fixes, new features, documentation, and tests.

## Development setup

```bash
git clone https://github.com/<your-fork>/framestudio.git
cd framestudio
npm install
```

For URL-import features, install yt-dlp and ffmpeg locally — see the [README](./README.md#optional-install-yt-dlp--ffmpeg-for-url-downloads).

## Running the app

```bash
npm run dev:all       # web + API only
npm run dev:electron  # web + API + Electron shell
```

## Code style

- TypeScript with strict types where practical
- React functional components with hooks
- Tailwind utility classes for styling
- Match existing formatting; no extra linter config is enforced beyond `tsc --noEmit`

Run the type-checker before submitting:

```bash
npm run lint
```

## Project layout

```
electron/   # Electron main + preload + bundled production server
server/     # tsx-driven dev server (Express wrapper around yt-dlp)
src/        # React app (Vite)
```

## Pull requests

- Keep PRs focused — one feature or fix per PR.
- Describe the user-visible change in the PR body.
- Include screenshots or screen recordings for UI changes when possible.
- Do not commit any of the items listed in [`.gitignore`](./.gitignore) — in
  particular, never commit `.env`, compiled binaries (`yt-dlp.exe`, `ffmpeg.exe`),
  or anything from `release*/` or `dist/`.

## Reporting bugs

Open an issue with:
- OS + version
- Node and npm versions
- Steps to reproduce
- yt-dlp / ffmpeg versions if the bug is in the URL importer
