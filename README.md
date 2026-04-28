# YourPassword

A client-side password security toolkit that helps everyday users understand, evaluate, and improve their passwords — with no account, no server, and no tracking.

**[Try it live →](https://bernardo-r-o-carvalho.github.io/YourPassword/yourpassword.html)**

---

## What it does

Most password tools give you a colored bar and call it a day. YourPassword goes further: it explains *why* a password is weak, teaches the concepts behind password security, and gives you better alternatives — all without sending your password anywhere.

| Feature | Description |
|---|---|
| **Strength Checker** | Real-time analysis with per-criterion feedback, entropy calculation, and estimated crack time |
| **Password Generator** | Configurable length (8–64) and character sets, with one-click copy |
| **Passphrase Generator** | Random word combinations — easier to remember, harder to crack |
| **Breach Check** | Checks against HaveIBeenPwned using k-anonymity — your password never leaves your device |
| **Education Tab** | Plain-English explanations of entropy, dictionary attacks, why `P@ssw0rd` is weak, and more |

---

## How the breach check works (and why it's private)

When you click "Check breach", YourPassword:

1. Computes a SHA-1 hash of your password locally using the Web Crypto API
2. Sends only the **first 5 characters** of that hash to [HaveIBeenPwned](https://haveibeenpwned.com/API/v3)
3. Receives a list of all hash suffixes that match that prefix
4. Checks locally whether your full hash is in the list

Your password — and even its full hash — is **never transmitted**. This technique is called [k-anonymity](https://en.wikipedia.org/wiki/K-anonymity).

---

## Tech stack

- **Pure HTML, CSS, and JavaScript** — no frameworks, no build step, no dependencies
- **Web Crypto API** — native SHA-1 hashing in the browser
- **HaveIBeenPwned API** — breach data via k-anonymity
- **GitHub Pages** — static hosting, deploys on push to `main`

---

## Run locally

No installation needed. Just clone and open:

```bash
git clone https://github.com/Bernardo-R-O-Carvalho/YourPassword.git
cd YourPassword
open yourpassword.html   # or double-click the file
```

That's it — no `npm install`, no server, no environment variables.

---

## Project structure

```
YourPassword/
├── yourpassword.html       # The entire application (HTML + CSS + JS)
└── docs/
    ├── scope.md            # What's in and out of scope
    ├── prd.md              # User stories and acceptance criteria
    ├── technical-spec.md   # Architecture and technical decisions
    └── build-checklist.md  # Feature completion tracking
```

---

## What I learned

This project was built as part of the [Devpost Learning Hackathon](https://devpost.com) focused on **spec-driven development** — planning with AI before building, so the result is intentional rather than generic.

Key takeaways:
- Writing a scope document before coding forces you to make real decisions upfront and prevents scope creep
- Defining user stories with acceptance criteria makes it obvious when a feature is actually done
- A technical spec written before building surfaces architectural choices you'd otherwise make accidentally
- Single-file architecture is underrated for tools that don't need a backend — it simplifies everything from debugging to deployment

---

## Privacy

- No analytics, no cookies, no tracking
- No data stored anywhere (no `localStorage`, no database)
- The only network request is to `api.pwnedpasswords.com` — only when you explicitly click "Check breach", and only a 5-character hash prefix is sent

---

## License

MIT
