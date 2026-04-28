# YourPassword — Technical Specification

## Architecture Overview

YourPassword is a single-file static web application. There is no build step, no framework, no backend, and no external dependencies. Everything runs in the user's browser.

```
yourpassword.html
├── HTML structure (tabs, panels, inputs, outputs)
├── CSS (embedded in <style> tag)
└── JavaScript (embedded in <script> tag)
    ├── Tab navigation
    ├── Password analysis engine
    ├── Password generator
    ├── Passphrase generator
    ├── HaveIBeenPwned breach checker
    └── Clipboard utility
```

## Technology Decisions

| Decision | Choice | Reason |
|---|---|---|
| Framework | None (vanilla HTML/CSS/JS) | Zero dependencies, instant load, no build step required |
| Hosting | GitHub Pages | Free, deploys directly from repo, no CI needed |
| Styling | Plain CSS with system font stack | No CDN needed, fast, accessible |
| Crypto | Web Crypto API (`crypto.subtle.digest`) | Native browser API, no library needed for SHA-1 |
| Clipboard | `navigator.clipboard.writeText` | Modern standard, works in all target browsers |

## Core Logic

### Entropy Calculation

```javascript
function calcEntropy(pwd) {
  let pool = 0;
  if (/[a-z]/.test(pwd)) pool += 26;
  if (/[A-Z]/.test(pwd)) pool += 26;
  if (/[0-9]/.test(pwd)) pool += 10;
  if (/[^A-Za-z0-9]/.test(pwd)) pool += 32;
  return pool > 0 ? pwd.length * Math.log2(pool) : 0;
}
```

Entropy = `length × log₂(pool size)`. Pool size is the number of possible characters based on which character sets are present.

### Crack Time Estimate

Assumes 10 billion attempts per second (modern GPU attack baseline):

```javascript
function crackTime(entropy) {
  const seconds = Math.pow(2, entropy) / 1e10;
  // returns human-readable string
}
```

### HaveIBeenPwned k-Anonymity

1. Compute SHA-1 hash of the password using `crypto.subtle.digest`
2. Take the first 5 hex characters (prefix)
3. Send `GET https://api.pwnedpasswords.com/range/{prefix}`
4. API returns all hash suffixes that match the prefix
5. Check if our full hash suffix appears in the list
6. The password itself — and even the full hash — is never transmitted

### Password Generator

Uses `Math.random()` to pick characters from a concatenated character pool built from enabled checkbox options. If no options are selected, falls back to lowercase to prevent empty output.

### Passphrase Generator

Uses a hardcoded wordlist of ~120 common English words. Each word is selected using `Math.random()`. Words are joined with the selected separator. Entropy is estimated at ~11 bits per word (approximating a 2048-word list).

## File Structure

```
YourPassword/
├── yourpassword.html       # The entire application
└── docs/
    ├── scope.md
    ├── prd.md
    ├── technical-spec.md
    └── build-checklist.md
```

## Deployment

- **Hosting:** GitHub Pages
- **URL:** `https://bernardo-r-o-carvalho.github.io/YourPassword/yourpassword.html`
- **Deploy process:** Push to `main` branch → GitHub Pages auto-deploys
- **No build step required**

## Browser Compatibility

| Feature | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| Web Crypto API | ✅ | ✅ | ✅ | ✅ |
| Clipboard API | ✅ | ✅ | ✅ | ✅ |
| CSS custom properties | ✅ | ✅ | ✅ | ✅ |
| Fetch API | ✅ | ✅ | ✅ | ✅ |

## Privacy

- No analytics, no tracking, no cookies
- No data stored anywhere (no localStorage, no sessionStorage)
- Only network request: `api.pwnedpasswords.com/range/{5-char-prefix}` — triggered only when user clicks "Check breach"
