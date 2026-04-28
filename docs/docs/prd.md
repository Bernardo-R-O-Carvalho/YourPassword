# YourPassword — Product Requirements Document (PRD)

## Overview

YourPassword is a browser-based password security toolkit for non-technical users. It requires no installation, no account, and no internet connection for most features. The goal is to educate users while solving a real problem.

---

## User Stories & Acceptance Criteria

### 1. Password Strength Checker

**As a user, I want to type a password and immediately see how strong it is, so I can improve it before using it.**

Acceptance criteria:
- Strength bar updates in real time as the user types
- Strength is labeled as Weak, Medium, or Strong with a matching color (red / orange / green)
- Feedback list shows per-criterion results: length, uppercase, lowercase, numbers, symbols, repetition, common patterns
- Each criterion shows a green/orange/red indicator dot
- Estimated crack time is shown with entropy in bits
- Show/Hide toggle works correctly

---

### 2. Password Generator

**As a user, I want to generate a strong random password with options I can control, so I don't have to think one up myself.**

Acceptance criteria:
- Length slider goes from 8 to 64 characters and updates the output in real time
- Checkboxes for uppercase, lowercase, numbers, and symbols all work independently
- If all checkboxes are unchecked, lowercase is used as fallback (no empty output)
- Generated password is shown in a monospace output area
- Regenerate button creates a new password with the same settings
- Copy button copies the password to clipboard and shows "Copied!" confirmation
- Estimated crack time and entropy are shown below the output

---

### 3. Passphrase Generator

**As a user, I want to generate a memorable passphrase made of random words, so I have a strong password I can actually remember.**

Acceptance criteria:
- Word count slider goes from 3 to 8 words
- Separator dropdown supports hyphen, dot, underscore, and space
- Optional checkbox to append a random number
- Regenerate and Copy buttons work the same as in the generator
- Estimated crack time and entropy are shown
- A note explains why passphrases are strong

---

### 4. Breach Check

**As a user, I want to check if a password has appeared in a known data breach, so I know whether it's safe to use.**

Acceptance criteria:
- Input accepts a password with Show/Hide toggle
- On submit, the SHA-1 hash is computed client-side
- Only the first 5 characters (prefix) are sent to the HaveIBeenPwned API
- Response is parsed to check if the full hash suffix appears in the results
- If found: shows breach count and a warning not to use the password
- If not found: shows a safe confirmation with a note about reuse
- If the API is unreachable: shows a clear error message
- A note explains the k-anonymity method so users understand their privacy is protected

---

### 5. Education Tab

**As a user, I want to understand why certain passwords are weak or strong, so I can make better decisions on my own.**

Acceptance criteria:
- Six educational sections are present: entropy, length vs complexity, why P@ssw0rd is weak, dictionary attacks, passphrases, and 2FA
- Content is written in plain English, accessible to non-technical users
- Code-style examples use a monospace font
- Badges (Weak / Recommended) appear where relevant

---

## Non-Functional Requirements

- All features work with no internet connection except the breach check
- No data is sent to any server except the 5-character hash prefix to HaveIBeenPwned
- Page loads in under 2 seconds on a standard connection
- No external dependencies — single HTML file, no CDN, no frameworks
- Works on Chrome, Firefox, Safari, and Edge (latest versions)
- Responsive layout works on screens 375px wide and above
