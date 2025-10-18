# Simple Stopwatch

## Summary
This project is a minimal, production-ready stopwatch web application. It focuses on correctness and reliability over visual polish, delivering precise timing with Start, Pause, Resume, and Reset controls. The implementation uses high-resolution time (performance.now) and requestAnimationFrame to avoid drift and ensure accuracy. It is dependency-free, self-contained in a single HTML file, and ready for deployment to GitHub Pages.

Additionally, the app safely handles an optional `?url=` query parameter by showing a secure, external link if provided.

## Setup
No build tools or dependencies are required.

Options:
- Open `index.html` directly in any modern browser; or
- Host it via any static server; or
- Deploy to GitHub Pages by committing this repository and enabling Pages on the repository settings (source: main branch, root).

The page is fully self-contained (HTML, CSS, and JavaScript embedded).

## Usage
- Start: Click “Start” (or press S/Space).
- Pause: Click “Pause” (or press P/Space).
- Resume: Click “Resume” (or press R/Space).
- Reset: Click “Reset” (or press X).

Display format: HH:MM:SS.CC (hours, minutes, seconds, centiseconds).

Keyboard shortcuts:
- Space toggles start/pause/resume depending on state
- S = Start
- P = Pause
- R = Resume
- X (or Delete) = Reset

Optional query parameter:
- You can pass a URL in the address bar with `?url=...` (e.g., `https://your-name.github.io/stopwatch/?url=https%3A%2F%2Fexample.com`).
- If the URL is valid and uses http/https, a small banner appears with a safe link to open it in a new tab.
- Invalid or non-http(s) values are ignored for security.

## Code Explanation
The entire application is in `index.html`, structured as follows:

- Head
  - Meta tags and responsive viewport.
  - Embedded CSS with a modern, minimal design using system fonts, CSS variables, and accessible color contrast.

- Body
  - Header with title and the optional URL banner (populated when `?url=...` is supplied and valid).
  - Main card containing:
    - A large, monospace time display.
    - Four buttons: Start, Pause, Resume, Reset.
    - A screen-reader-only live region for state announcements (e.g., “Started”, “Paused”).
  - Footer showing keyboard hints.

- JavaScript
  - URL parameter handling:
    - Parses `?url` using `URLSearchParams`.
    - Validates with the `URL` constructor and only allows http/https protocols.
    - If valid, exposes a safe external link (`target="_blank" rel="noopener noreferrer"`).
  - Stopwatch core:
    - State:
      - `running` (boolean), `startTime` (baseline from `performance.now()`), `elapsed` (accumulated milliseconds), `rafId` (current animation frame id).
    - Timekeeping:
      - On start/resume, baseline is set to `performance.now() - elapsed`.
      - A `tick()` loop driven by `requestAnimationFrame` updates `elapsed` using current `performance.now() - startTime`.
      - This approach prevents drift that typical `setInterval` solutions suffer from.
      - Display is formatted as HH:MM:SS.CC (centiseconds) to keep it readable and stable.
    - Controls and states:
      - Start: begins timing or is disabled if resuming is more appropriate.
      - Pause: freezes time and stops the animation loop.
      - Resume: restarts the loop with correct baseline.
      - Reset: stops and zeroes out the timer, re-enabling Start.
      - Button enable/disable logic prevents invalid actions and duplicate loops.
    - Accessibility:
      - The timer text uses `font-variant-numeric: tabular-nums` for predictable layout.
      - ARIA live region announces state changes.
      - Buttons have clear titles; keyboard shortcuts are available.

No external libraries or assets are used, ensuring quick loads and straightforward deployment.

## License
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.