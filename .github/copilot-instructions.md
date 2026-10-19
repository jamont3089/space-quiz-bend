# Orbit quiz

## Running and checking

Open `index.html` directly in a browser (`file://` works), including the integrated browser. In PowerShell, `Start-Process .\index.html` opens the default browser. Keep the app usable without a server, network access, dependencies, or a build step.

Validation is browser-based; there are no configured build, test-runner, or lint commands. For a single-case smoke check, reload the page, choose Mars on question 1, and verify score 1, green feedback, 10% progress, and focus on Next. On question 2, choose Yuri Gagarin: score stays 1, the selected answer shakes red, and Neil Armstrong is revealed as correct.

For a full smoke pass, complete all 10 questions and check final score, accuracy, emoji, 100% telemetry, and replay reset. Exercise actual Tab/Enter/Space navigation when the browser tooling supports keystrokes; programmatic focus checks alone do not verify keyboard navigation. Check light/dark themes, narrow screens, reduced motion, and Pause animations.

On the results screen, Copy result shares the displayed score, accuracy, and emoji as plain text. Check successful clipboard confirmation without a focus change and unavailable/denied clipboard fallback with a labeled, read-only summary that can be selected and copied under `file://`. Both replay controls must clear copy feedback and the summary; pending clipboard requests must not update a later run.

## Architecture

The application is entirely in `index.html`: inline CSS, semantic page markup and SVG artwork, followed by a classic inline JavaScript script. There are no external assets or application modules. Preserve this single-file arrangement.

The `questions` array contains `category`, `question`, four `answers`, a zero-based `correct` index, and an explanatory `fact`. State lives in `current` (zero-based question index), `score`, and `answered`; it is in memory and resets on reload.

`renderQuestion()` rebuilds answer buttons and resets per-question feedback. `selectAnswer()` locks all answers, prevents duplicate scoring, reveals feedback, and enables Next. `updateProgress()` synchronizes the native progress element, accessible value text, and visual percentage. Next advances to another question or `showResults()`; restart resets state and renders the first question.

## Project conventions

- Keep the quiz at 10 questions unless explicitly changing its scope. Some totals and accuracy calculations are hard-coded in markup and `showResults()`; updating the array alone does not update every surface. Emoji tiers are perfect score, 7+, 4+, and below 4.
- DOM IDs are the JavaScript interface, accessed through `$()` (`getElementById`). Dynamic answer and explanation text uses `textContent` and `replaceChildren`; the question counter uses an internal numeric HTML template.
- Maintain focus handoffs: answer selection to Next, question transitions/replay to the question heading, results to the results heading. Headings use `tabindex="-1"`; answers are native buttons. Feedback is a polite status live region, and correct/incorrect states include symbols and text, not color alone.
- Theme colors come from `:root` custom properties with `prefers-color-scheme: dark` overrides. Use `--control-border` for interactive boundaries rather than the decorative `--line`. Preserve at least 4.5:1 normal-text contrast and 3:1 UI/focus contrast in both themes, including hover and feedback states. Do not restore opacity dimming on `.answer.dimmed`.
- The space scene uses fixed, non-interactive layers behind the content: masked hex grids, inline SVG meshes/objects, and stars. Keep decorative content `aria-hidden`, with `pointer-events: none`. Solid backgrounds on the topbar, intro, footer, and quiz protect text contrast from the animated artwork.
- Motion is controlled both by `prefers-reduced-motion` and the footer toggle (`aria-pressed`, `body[data-motion="paused"]`). New animations must respect both. Forced-colors rules hide the decorative scene and restore native progress rendering.
- Preserve the centered, maximum-490px column, the 420px mobile breakpoint, system sans-serif body typography, and monospace mission telemetry.
