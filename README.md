# Secretus — Share Secrets That Disappear 🔒

**Share passwords, API keys, and confidential files with browser-side encryption, controlled access, and clear storage boundaries.**

[secretus.app](https://secretus.app) · [Plans & pricing](https://secretus.app/offer) · [Blog](https://secretus.app/blog) · [Security glossary](https://secretus.app/dex) · [Status](https://secretus.app/status)

Secretus is a secret-sharing platform operated from Romania, with core infrastructure in Frankfurt, Germany. Encryption happens in your browser. During normal operation, Secretus secret endpoints do not receive plaintext secret content or decryption keys.

Stop leaving credentials in Slack, email, and ticket threads. Choose an encrypted one-time link, a live browser-to-browser transfer, or threshold sharing that requires multiple holders.

## ✨ Why people use Secretus

- 🔥 **One-time Standard links** — the link is invalidated before the first successful payload response; ciphertext cleanup follows
- 🕑 **Expiry you control** — Standard links expire after 15 minutes to 30 days, or at a selected date within that window
- 🛡️ **Browser-side cryptography** — encryption and decryption run locally, using Web Crypto and a dedicated ML-KEM library for the hybrid P2P component
- 🔬 **Hybrid post-quantum key agreement** — Maximum Security combines classical key agreement with ML-KEM-768, standardized in NIST FIPS 203
- 🇪🇺 **EU operation** — Romanian operator, Frankfurt core infrastructure, consent-gated analytics, and a transparent [provider register](https://secretus.app/subprocessors)
- 📬 **Both directions** — send secrets or request that someone sends one securely to you
- 👤 **No recipient account required** — senders need an account and an active trial or plan

## Three ways to share

### ⚡ Standard Mode — send now, they open later

Create an AES-256-GCM encrypted one-time link. The decryption key is carried in the URL fragment, which browsers omit from HTTP requests. The recipient’s browser uses it to decrypt the ciphertext locally.

The link is invalidated before the first successful payload response. Ciphertext deletion follows, with storage lifecycle cleanup as a backstop. Unread links can be revoked or left to expire.

Available on all plans. Files and audio require Business.

### 🔒 Maximum Security — live browser-to-browser transfer

Transfer secrets between online sender and recipient browsers using authenticated X3DH-style session setup, ML-KEM-768 hybrid key agreement, and a per-message symmetric ratchet.

Compare the safety number over an independent channel to verify the peer and detect identity-bundle substitution.

The secret payload is not stored on Secretus servers. Encrypted traffic may pass through a TURN relay when a direct connection is unavailable; signaling and connection metadata are still processed.

Available on Pro and Business. Files and audio require Business.

### 🔑 Team Split — require multiple holders

Split a text secret into N shares using Shamir’s Secret Sharing. Any K holders can reconstruct it locally; fewer than K shares reveal no information about the secret under the scheme’s assumptions.

Shares are created and reconstructed in participating browsers and carried in URL fragments. Secretus does not upload or store the share material through this workflow.

Team Split supports **text only**. It does not support files or voice, and its share links do not inherit Standard mode’s server-enforced one-time access, expiry, or revocation.

Available on Business.

## 🧰 Built for real work

- **Labels & delivery confirmation** — organize shares and see when access occurs
- **Revocation** — invalidate an unread Standard link
- **Secret request links** — ask a client or colleague to send you a secret securely
- **Structured templates** — SSH keys, database credentials, API keys, and Wi-Fi details
- **Audit log** — searchable history with JSON/CSV export
- **Compliance PDF** — export evidence to support SOC 2, GDPR Art. 30, and DORA documentation; this is not a certification or guarantee of compliance
- **Teams** — share a Business plan with up to 5 members through single-use invite links
- **REST API** — automate secret delivery from your own tooling
- **2FA (TOTP), installable PWA, QR sharing, and dark mode**

Avoid putting sensitive content in optional labels: labels are not encrypted secret payloads.

## 🧩 Browser extension

Create an encrypted one-time secret from selected text using the browser’s context menu.

**Available for Firefox:** [Install Secretus — Share a Secret](https://addons.mozilla.org/firefox/addon/secretus-share-a-secret/)

Encryption happens locally. The decryption key is included in the link you share with the recipient and is omitted from normal secret endpoint requests.

## 🔐 Storage and security boundaries

| Mode | Secret material stored by Secretus |
|---|---|
| **Standard** | Encrypted ciphertext until access, revocation, or expiry cleanup |
| **Maximum Security** | No server-side secret-payload storage |
| **Team Split** | No server-side share-material storage through the sharing workflow |

Account, audit, security, and operational metadata are processed as described in the [Privacy Policy](https://secretus.app/privacy).

Browser-side encryption does not protect a compromised device or malicious application code delivered before encryption. Anyone who obtains a complete Standard link can use its decryption key, subject to the link’s access state.

One-time access cannot prevent a recipient from copying, saving, or photographing a secret. Deletion applies to Secretus-controlled data, not copies held by recipients or external messaging services.

## 💳 Plans

Start with a **14-day full-access free trial — no credit card required**.

| Plan | Includes | Monthly price |
|---|---|---|
| **Starter** | Standard text links, requests, templates, MFA, and 90-day audit history | €9 |
| **Pro** | Everything in Starter, plus Maximum Security live P2P transfers | €19 |
| **Business** | Everything in Pro, files and audio up to 5 MB in Standard/P2P, text-only Team Split, Teams, up to 5 API keys, Compliance PDF, and 1-year audit history | €33 |

Prices exclude applicable VAT, sales tax, or GST. The final total is shown before checkout confirmation.

Annual billing gives you **2 months free**. No plan-based monthly secret quota; rate and abuse limits apply.

See [plans & pricing](https://secretus.app/offer) for full details. For larger teams or custom requirements, [contact us](https://secretus.app/contact).

## 🆚 How it compares

- [Secretus vs. OneTimeSecret](https://secretus.app/vs/onetimesecret)
- [Secretus vs. Yopass](https://secretus.app/vs/yopass)
- [Secretus vs. 1Password sharing](https://secretus.app/vs/1password)
- [Secretus vs. Bitwarden Send](https://secretus.app/vs/bitwarden-send)

## 📚 Learn more

- [Blog — breach analysis & practical secret-sharing guides](https://secretus.app/blog)
- [Security glossary](https://secretus.app/dex)
- [Privacy](https://secretus.app/privacy) · [Terms](https://secretus.app/terms) · [Providers & sub-processors](https://secretus.app/subprocessors) · [Data Processing Agreement](https://secretus.app/dpa)

## 🌐 Follow Secretus

- [X — @secretusapp](https://x.com/secretusapp)
- [Reddit — u/secretusapp](https://www.reddit.com/user/secretusapp/)
- [Medium — @secretusapp](https://medium.com/@secretusapp)

---

**Encrypted in your browser. Shared with clear limits.** 🔒
