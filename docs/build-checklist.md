# YourPassword — Build Checklist

## Planning

- [x] Defined app name: YourPassword
- [x] Identified target user: non-technical users who want to improve their password habits
- [x] Defined problem: weak/reused passwords due to lack of education and accessible tooling
- [x] Decided on 5 features: Checker, Generator, Passphrase, Breach Check, Education
- [x] Decided on tech stack: single HTML file, vanilla JS, GitHub Pages
- [x] Wrote scope document
- [x] Wrote PRD with user stories and acceptance criteria
- [x] Wrote technical specification

## Core Features

### Password Strength Checker
- [x] Password input with show/hide toggle
- [x] Real-time analysis on input event
- [x] Animated strength bar with color (red/orange/green)
- [x] Score label (Weak / Medium / Strong)
- [x] Per-criterion feedback list with colored dots
  - [x] Length check (12+ green, 8+ orange, below red)
  - [x] Uppercase letters
  - [x] Lowercase letters
  - [x] Numbers
  - [x] Special characters
  - [x] No repeated characters (3+)
  - [x] No common patterns (123, abc, qwerty, password)
- [x] Entropy calculation
- [x] Estimated crack time display

### Password Generator
- [x] Length slider (8–64) with live label
- [x] Checkboxes: uppercase, lowercase, numbers, symbols
- [x] Fallback to lowercase if all unchecked
- [x] Monospace output area
- [x] Regenerate button
- [x] Copy button with "Copied!" confirmation
- [x] Crack time and entropy shown below output
- [x] Auto-generates on tab open

### Passphrase Generator
- [x] Word count slider (3–8) with live label
- [x] Separator selector (hyphen, dot, underscore, space)
- [x] Add number checkbox
- [x] Monospace output area
- [x] Regenerate and Copy buttons
- [x] Crack time and entropy estimate
- [x] Explanatory note about passphrase strength
- [x] Auto-generates on tab open

### Breach Check
- [x] Password input with show/hide toggle
- [x] SHA-1 hash computed with Web Crypto API
- [x] Only prefix (5 chars) sent to HaveIBeenPwned
- [x] API response parsed for suffix match
- [x] Found: danger result with breach count
- [x] Not found: safe result with reuse warning
- [x] Error: friendly message if API unreachable
- [x] Privacy note explaining k-anonymity

### Education Tab
- [x] Why length matters more than complexity
- [x] What is entropy
- [x] Why P@ssw0rd is weak
- [x] How dictionary attacks work
- [x] The case for passphrases
- [x] Always use 2FA

## UI & Polish
- [x] Tab navigation with active state
- [x] Responsive layout (works on mobile)
- [x] System font stack (no external fonts)
- [x] Consistent card-based layout
- [x] No external dependencies

## Deployment
- [x] Code pushed to public GitHub repository
- [x] GitHub Pages enabled on main branch
- [x] App accessible at public URL
- [x] docs/ folder created with all required documents
