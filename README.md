# Phosphor

A minimalist particle-catching browser game. Glowing particles rise from the
deep — click them before they fade.

## How to play

Luminous particles spawn at the bottom of the screen and float upward, fading
as they rise. Click a particle to catch it and earn its point value.

**Scoring:** Each particle's brightness determines its value (up to 100
points). Brighter particles are worth more but fade faster, creating a
risk/reward tradeoff. Dimmer particles linger longer but yield fewer points.
Your final score is the sum of all particles you caught in 30 seconds.

**Controls:** Click or tap particles to catch them. The game runs for 30
seconds, then you can save your score to the persistent high-score table.

## Setup and run

Requires Node.js 22.

```bash
npm install
npm start
```

The server listens on `127.0.0.1` at the port set by the `PORT` environment
variable (default `3000`):

```bash
PORT=8080 npm start
```

Open the URL printed at startup in a browser to play.

## API

| Method | Path     | Description            |
|--------|----------|------------------------|
| GET    | `scores` | Top 20 scores (JSON)   |
| POST   | `scores` | Submit `{name, score}` |

All URLs are relative. The game works behind a reverse proxy at any path
prefix, and inside a CSP sandbox with an opaque origin. No cookies,
localStorage, or external resources are used.

Scores are stored in a SQLite database (`scores.db`) that persists across
server restarts.

### Validation

- `name`: non-empty string, max 20 characters
- `score`: integer 0–99999

Invalid submissions return `400` with `{"error": "..."}`.

## Hosting notes

- All asset and API URLs are relative (no root-absolute paths).
- No CDN, web fonts, or third-party scripts.
- No cookies or browser storage required.
- CORS enabled for cross-origin requests including JSON preflight.
- Form submission handled via `fetch` with `preventDefault`.
- Works in the workshop's CSP sandbox (`allow-scripts`, `allow-forms`,
  opaque origin).
