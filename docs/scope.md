# YourPassword — Scope Document

## What We're Building

YourPassword is a client-side web application that helps everyday users understand and improve their password security. It combines a strength checker, a password generator, a passphrase generator, a breach checker, and an educational section — all in a single HTML file that runs entirely in the browser.

## Problem Statement

Most people use weak or reused passwords because they don't understand what makes a password strong, and the tools available to them either give unhelpful feedback ("strength: medium") or require creating an account. YourPassword solves this with honest, educational feedback and no server, no login, no tracking.

## In Scope

- **Password Strength Checker** — real-time analysis with a visual strength bar, per-criterion feedback (length, character types, repetition, common patterns), entropy calculation, and estimated crack time
- **Password Generator** — configurable length (8–64 characters) and character sets (uppercase, lowercase, numbers, symbols), with crack time estimate
- **Passphrase Generator** — random word combinations with configurable word count (3–8), separator, and optional number suffix
- **Breach Check** — integration with HaveIBeenPwned API using k-anonymity (SHA-1 prefix method); password never sent in plain text
- **Education Tab** — plain-language explanations of entropy, why length beats complexity, how dictionary attacks work, why passphrases are strong, and the importance of 2FA
- **Responsive design** — works on desktop and mobile browsers
- **Copy to clipboard** — one-click copy for generated passwords and passphrases

## Out of Scope

- User accounts or login
- Saving or storing passwords (no localStorage, no database)
- Browser extension
- Password manager integration
- Mobile native app (iOS/Android)
- Backend server or API of our own
- Multi-language support (English only for v1)
- Dark mode toggle (uses system default)
