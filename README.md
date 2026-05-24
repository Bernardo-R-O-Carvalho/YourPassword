# YourPassword

A browser-based password security toolkit built during the **Learning Hackathon: Spec Driven Development**.

[**Try it live →**](https://bernardo-r-o-carvalho.github.io/YourPassword/yourpassword.html)

---

## Inspiration

Most password tools give you a colored bar and nothing else. They say "medium strength" without explaining why — or what to do about it.

I wanted to build something that actually teaches. A tool where a non-technical person could type their password and walk away understanding entropy, dictionary attacks, and why `P@ssw0rd` fools nobody. Security education shouldn't require a computer science degree.

---

## What it does

Seven features, one HTML file, zero dependencies:

| Feature | Description |
|---|---|
| **Strength Checker** | Real-time analysis with per-criterion feedback, crack time estimate, and entropy in bits |
| **Inline Annotations** | Pattern detection while you type — leet-speak substitutions, keyboard walks, date patterns, repeated characters |
| **Entropy Chart** | Animated bar chart showing how each extra character exponentially multiplies the search space |
| **Live Attack Simulation** | Terminal-style counter simulating a GPU cluster trying to crack your password at 10B guesses/sec |
| **Password Breakdown** | Dissects your password into color-coded chunks explaining each segment — dictionary words, years, symbols, strong random parts |
| **Password Generator** | Configurable length (8–64) and character sets, powered by `crypto.getRandomValues()` |
| **Passphrase Generator** | Random words from the EFF Large Wordlist (7,776 words, ~12.9 bits/word), cryptographically selected |
| **Breach Check** | Checks against HaveIBeenPwned's database of billions of leaked credentials using k-anonymity — your password never leaves your device |
| **Comparator** | Side-by-side comparison of two passwords with entropy delta and relative crack time |
| **History** | Tracks your last 30 analyses locally — score, entropy, and length only. Password text is never stored |
| **Education Tab** | Plain-English explanations of entropy, dictionary attacks, passphrases, k-anonymity, and why `Math.random()` is dangerous |

---

## How I built it

This hackathon taught spec-driven development — and I took it seriously. Before writing a single line of code, I produced:

- A **scope document** defining what was in and out of v1
- A **PRD** with user stories and acceptance criteria for each feature
- A **technical specification** with architecture decisions made upfront

That process changed how I built. Instead of figuring things out mid-implementation, I had a clear picture of the finished product before I started. Every feature had a definition of done.

The app itself is a single HTML file — no frameworks, no build step, no dependencies. Just HTML, CSS, and vanilla JavaScript.

### Security decisions worth explaining

**`crypto.getRandomValues()` with rejection sampling**

`Math.random()` is not cryptographically secure — its output can be predicted by observing enough values. Every random selection in this tool uses `crypto.getRandomValues()`. Rejection sampling eliminates modulo bias, where naive `rand % max` makes some values slightly more likely than others.

```javascript
function cryptoRandInt(max) {
  const arr = new Uint32Array(1);
  const limit = Math.floor(0xFFFFFFFF / max) * max;
  do { crypto.getRandomValues(arr); } while (arr[0] >= limit);
  return arr[0] % max;
}
```

**k-anonymity on the breach check**

1. The password is hashed with SHA-1 client-side via the Web Crypto API
2. Only the **first 5 hex characters** of the hash are sent to HaveIBeenPwned
3. The server returns all hash suffixes matching those 5 characters
4. The full match is checked locally

The password itself — and even its complete hash — is never transmitted.

**EFF Large Wordlist**

The passphrase generator uses the [EFF Large Wordlist](https://www.eff.org/files/2016/07/18/eff_large_wordlist.txt) (7,776 words chosen by security researchers for memorability and diceware compatibility). Each word contributes `log₂(7776) ≈ 12.9 bits` of entropy.

**History privacy**

The history tab stores only entropy score, strength label, length, and timestamp — never the password text. This is a deliberate architectural decision: a tool about not trusting things with your passwords shouldn't ask you to trust it with your passwords.

---

## Challenges

**Pattern detection without a library**

Rather than importing zxcvbn, I implemented pattern detection from scratch: leet-speak normalization, keyboard walk detection, date patterns, repetition checks, and comparison against a curated list of the most common leaked passwords. This kept the tool dependency-free and forced me to understand what actually makes a password weak.

**The attack simulation**

The hardest part of the simulation wasn't the counter — it was making it honest. Simulating 2^80 guesses visually is impossible, so the counter scales to a representable portion of the search space while displaying the real total. The goal is intuition, not theater.

**Scope discipline**

The hardest challenge was deciding what not to build. The spec document saved me from myself multiple times.

---

## What I learned

**Planning before building is a skill, not a formality.** Writing a PRD forced me to make real decisions upfront instead of discovering them mid-build and backtracking.

**Single-file architecture is underrated.** No build pipeline means no build failures. No dependencies means no dependency hell. For a security tool, it also means the entire codebase is auditable in one scroll.

**Privacy can be a feature.** The k-anonymity implementation and the history tab's deliberate omission of password text are both worth explaining to users. People respond well to transparency about how their data is handled.

**The spec-driven process produces artifacts you actually reuse.** The scope doc, PRD, and technical spec aren't just hackathon deliverables — they're templates I'll carry into future projects.

---

## Built with

- Vanilla HTML, CSS, JavaScript — no frameworks, no dependencies
- [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) — `crypto.getRandomValues()` and `crypto.subtle.digest()`
- [HaveIBeenPwned API v3](https://haveibeenpwned.com/API/v3) — breach check with k-anonymity
- [EFF Large Wordlist](https://www.eff.org/files/2016/07/18/eff_large_wordlist.txt) — passphrase generation

---

## Running locally

No build step needed:

```bash
open yourpassword.html
```

Or serve locally to avoid CORS issues with the breach check:

```bash
npx serve .
```
