# TimeTrack

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-Single%20File-E34F26?logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/Vanilla%20JS-ES2020+-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![PWA](https://img.shields.io/badge/PWA-Installable-5A0FC8?logo=pwa&logoColor=white)](https://web.dev/progressive-web-apps/)
[![Built with AI](https://img.shields.io/badge/Built%20with-Claude%20AI-D4A574?logo=anthropic&logoColor=white)](https://claude.ai)

Minimalist time tracking as a Progressive Web App — a single HTML file, no backend, no build step.

## Live Demo

**[https://runter-vom-mattenwagen.github.io/timetrack/](https://runter-vom-mattenwagen.github.io/timetrack/)**

## Features

- **Project Timer** — Start/stop with a single tap, automatic switch between projects (single-timer mode)
- **Favorites** — Pin frequently used projects as prominent quick-access cards on the home screen
- **Day / Week / Month View** — Time entries grouped by project with subtotals and grand totals
- **Manual Entries** — Add time entries after the fact
- **Dark / Light Mode** — Follows browser preference automatically, or set manually
- **Import / Export** — JSON-based, with merge option for multi-device usage
- **Daily Export Reminder** — Unobtrusive banner reminds you to back up your data
- **Installable** — PWA with Service Worker, works fully offline as a standalone app
- **Midnight Split** — Timers running past midnight are cleanly split across both days

## Tech Stack

- Vanilla JavaScript (ES2020+), zero dependencies
- Single `index.html` file — CSS and JS inline
- Data stored in browser `localStorage`
- PWA manifest and Service Worker generated dynamically
- XSS-safe: no `innerHTML` for user input

## Usage

### In the Browser

Just open `index.html` in any modern browser. That's it.

### As an Installed App

1. Open in browser
2. "Add to Home Screen" (mobile) or "Install App" (desktop Chrome/Edge)
3. Runs as a standalone app, fully offline

### Data Backup

All data lives in `localStorage`. For regular backups:

- **Export:** Settings → Export JSON
- **Import:** Settings → Import JSON (replace or merge)

The daily export reminder appears automatically when the last export is more than 24 hours ago.

## Deployment

The repo is configured for GitHub Pages. The `index.html` in the root is served directly.

## Built with AI

This project was built entirely with [Claude](https://claude.ai) by Anthropic — from concept and architecture through implementation and iteration. Prompt-driven development at its finest.

## License

MIT — see [LICENSE](LICENSE)
