# Secretus — Share Secrets That Disappear 🔒

**Send passwords, API keys, and confidential files as end-to-end encrypted, one-time links that self-destruct after they're opened.**

[secretus.app](https://secretus.app) · [Plans & pricing](https://secretus.app/offer) · [Blog](https://secretus.app/blog) · [Security glossary](https://secretus.app/dex) · [Status](https://secretus.app/status)

Secretus is a zero-knowledge secret-sharing platform operated from the EU. Every secret is encrypted **in your browser** before anything leaves your device — our servers never receive plaintext secrets or decryption keys. Not at upload, not in transit, not ever.

Stop pasting credentials into Slack, email, and ticket threads. Send a link that works once, then vanishes.

## ✨ Why people use Secretus

- 🔥 **One-time viewing** — the link is invalidated the moment it's opened
- 🕑 **Expiry you control** — from 15 minutes to 30 days, or a specific date
- 🛡️ **Zero-knowledge by design** — encryption and decryption happen only in the browser, using the browser's native Web Crypto
- 🔬 **Post-quantum component** — ML-KEM-768 (NIST FIPS 203) hybrid key agreement in live P2P transfers, designed to reduce harvest-now-decrypt-later exposure
- 🇪🇺 **EU-operated, GDPR-first** — EU infrastructure, consent-gated analytics, transparent [provider register](https://secretus.app/subprocessors)
- 📬 **Both directions** — send secrets, or request that someone sends one securely to you

## Three ways to share, for three threat models

### ⚡ Standard Mode — send now, they open anytime
An encrypted one-time link (AES-256-GCM). The decryption key travels only inside the link itself — in the URL fragment, which browsers never send to servers — so what our servers hold is ciphertext they cannot read. Opened once, the link dies and the ciphertext is deleted shortly after.

### 🔒 Maximum Security — live browser-to-browser transfer
For your most sensitive material: a direct peer-to-peer session between sender and recipient, secured by an authenticated X3DH-style handshake — the session-setup pattern popularized by modern secure messengers — combined with ML-KEM-768 hybrid key agreement and per-message key ratcheting. A human-verifiable safety number confirms you're talking to the right person. In this mode, the secret payload is never stored on our servers.

### 🔑 Team Split — no single person can open it
Splits a secret into N shares using Shamir's Secret Sharing. Any K holders together can reconstruct it; fewer than K learn *mathematically nothing* — that's information-theoretic security, independent of computing power. Perfect for root credentials, recovery codes, and anything that should require more than one pair of hands.

## 🧰 Built for real work

- **Labels & delivery confirmation** — know the moment a secret is opened
- **Instant revocation** — kill an unopened link at any time
- **Secret request links** — ask a client or colleague to send *you* a secret; only you can decrypt it
- **Structured templates** — SSH keys, database credentials, API keys, Wi-Fi
- **Audit log** — searchable history with JSON/CSV export, plus a one-click **Compliance PDF** (evidence for SOC 2, GDPR Art. 30, DORA)
- **Teams** — share a Business plan with up to 5 members via single-use invite links
- **REST API** — automate secret delivery from your own tooling
- **2FA (TOTP), installable PWA, QR sharing, dark mode**

## 🧩 Browser extension

Share a one-time secret from any page — select text, right-click, done. Same zero-knowledge encryption as the web app; the decryption key never leaves your device. Rolling out for Firefox and Chrome.

## 🔐 What we never store

- ❌ Plaintext secrets — encryption happens before upload
- ❌ Decryption keys — they exist only in the links you share
- ❌ P2P payloads — live transfers are never written to our servers
- ✅ Standard mode stores ciphertext only — unreadable to us, deleted after first view or expiry

## 💳 Plans

Every plan starts with a **14-day full-access free trial — no credit card required**.

| Plan | For | Price |
|---|---|---|
| **Starter** | Async one-time links for individuals | €9/mo |
| **Pro** | Adds live P2P transfers with the post-quantum component | €19/mo |
| **Business** | Files & voice, Team Split, Teams, API, Compliance PDF | €33/mo |

Annual billing gets you **2 months free**. Details: [secretus.app/offer](https://secretus.app/offer)

## 🆚 How it compares

- [Secretus vs. OneTimeSecret](https://secretus.app/vs/onetimesecret)
- [Secretus vs. Yopass](https://secretus.app/vs/yopass)
- [Secretus vs. 1Password sharing](https://secretus.app/vs/1password)

## 📚 Learn more

- [Blog — breach analysis & practical secret-sharing guides](https://secretus.app/blog)
- [Security glossary (/dex)](https://secretus.app/dex)
- [Medium](https://medium.com/@secretusapp)
- [Privacy](https://secretus.app/privacy) · [Terms](https://secretus.app/terms) · [Sub-processors](https://secretus.app/subprocessors)

---

**Encrypted in your browser. Opened once. Gone forever.** 🔒
