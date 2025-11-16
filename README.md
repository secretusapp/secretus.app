# secretus.app
# Secretus - Signal Protocol Encrypted Secret Sharing

Secretus is a peer-to-peer secret sharing platform that uses **Signal Protocol encryption**—the same encryption technology used by Signal. All encryption happens entirely in your browser using the Web Crypto API, ensuring zero-knowledge architecture and maximum privacy.

## 🔒 Security Features

- **Signal Protocol Encryption**: Industry-proven end-to-end encryption with X3DH key agreement and Double Ratchet algorithm
- **Zero-Knowledge Architecture**: We never have access to your encryption keys
- **One-Time Viewing**: Secrets self-destruct after being viewed once
- **No Server Storage**: Secrets are never stored on our servers
- **Browser-Only Encryption**: All cryptographic operations happen in your browser
- **Forward Secrecy**: Compromising one message doesn't compromise others
- **Post-Compromise Security**: The protocol automatically recovers from key compromises

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        Secretus Architecture                     │
└─────────────────────────────────────────────────────────────────┘

┌──────────────┐                    ┌──────────────┐
│   Sender     │                    │   Receiver   │
│  (Browser)   │                    │  (Browser)   │
└──────┬───────┘                    └──────┬───────┘
       │                                    │
       │  1. Generate Signal Protocol Keys  │
       │     - Identity Key (ECDH + ECDSA) │
       │     - Signed Prekey               │
       │     - One-Time Prekey             │
       │                                    │
       │  2. Exchange Prekey Bundles       │
       │     via Signaling Server          │
       │                                    │
       ├────────────────────────────────────┤
       │                                    │
       │  3. X3DH Key Agreement            │
       │     - Derive Shared Secret        │
       │                                    │
       │  4. Initialize Signal Session     │
       │     - Double Ratchet Setup        │
       │     - Chain Keys Derivation       │
       │                                    │
       │  5. WebRTC Connection             │
       │     - Direct P2P Channel          │
       │                                    │
       │  6. Encrypt & Send Secret         │
       │     - AES-256-GCM Encryption      │
       │     - Message Key Derivation      │
       │                                    │
       │  7. Receive & Decrypt Secret      │
       │     - Decrypt with Message Key    │
       │                                    │
       │  8. Mark Session as Complete      │
       │     - Invalidate Session          │
       │                                    │
       └────────────────────────────────────┘
                    │
                    ▼
         ┌──────────────────────┐
         │  Signaling Server   │
         │  (api.secretus.app) │
         │                      │
         │  - Relay Prekey     │
         │    Bundles           │
         │  - WebRTC Signaling  │
         │  - Session Tracking  │
         └──────────────────────┘
```

## 🔐 Signal Protocol Implementation

### X3DH Key Agreement Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    X3DH Key Agreement Process                   │
└─────────────────────────────────────────────────────────────────┘

Sender (Initiator)                    Receiver (Responder)
──────────────────                    ──────────────────

1. Generate Keys:                     1. Generate Keys:
   - Identity Key (IK_A)                  - Identity Key (IK_B)
   - Signed Prekey (SPK_B)                - Signed Prekey (SPK_B)
   - One-Time Prekey (OPK_B)              - One-Time Prekey (OPK_B)

2. Send Prekey Bundle                 2. Send Prekey Bundle
   to Signaling Server                    to Signaling Server

3. Receive Receiver's Bundle           3. Receive Sender's Bundle

4. Perform X3DH:                      4. Perform X3DH:
   DH1 = ECDH(IK_A, SPK_B)              DH1 = ECDH(SPK_B, IK_A)
   DH2 = ECDH(SPK_A, IK_B)              DH2 = ECDH(IK_B, SPK_A)
   DH3 = ECDH(SPK_A, SPK_B)             DH3 = ECDH(SPK_B, SPK_A)
   DH4 = ECDH(SPK_A, OPK_B)             DH4 = ECDH(OPK_B, SPK_A)

5. Combine: DH1 || DH2 || DH3 || DH4   5. Combine: DH1 || DH2 || DH3 || DH4

6. Derive Shared Secret:              6. Derive Shared Secret:
   HKDF(DH_combined, salt, info)         HKDF(DH_combined, salt, info)
   → Shared Secret (32 bytes)             → Shared Secret (32 bytes)
```

### Double Ratchet Algorithm

```
┌─────────────────────────────────────────────────────────────────┐
│                    Double Ratchet Algorithm                      │
└─────────────────────────────────────────────────────────────────┘

Initial Setup:
─────────────
Shared Secret (from X3DH)
    │
    ├─→ Derive Root Key (HKDF)
    │
    ├─→ Derive Sending Chain Key (HKDF with "SendingChainKey" info)
    │   └─→ Sender uses this to encrypt
    │
    └─→ Derive Receiving Chain Key (HKDF with "ReceivingChainKey" info)
        └─→ Receiver uses this to decrypt

Message Encryption (Sender):
────────────────────────────
Sending Chain Key
    │
    ├─→ Derive Message Key (HKDF with "MessageKey-{number}")
    │   └─→ AES-256-GCM Encryption Key
    │
    ├─→ Encrypt Plaintext
    │   └─→ Ciphertext + IV + Auth Tag
    │
    └─→ Update Chain Key (Ratchet Forward)
        └─→ Increment Message Number

Message Decryption (Receiver):
──────────────────────────────
Receiving Chain Key
    │
    ├─→ Derive Message Key (HKDF with "MessageKey-{number}")
    │   └─→ AES-256-GCM Decryption Key
    │
    ├─→ Decrypt Ciphertext
    │   └─→ Plaintext
    │
    └─→ Update Chain Key (Ratchet Forward)
        └─→ Increment Message Number

Security Properties:
────────────────────
✓ Forward Secrecy: Each message uses a unique key
✓ Post-Compromise Security: Chain keys update automatically
✓ Perfect Forward Secrecy: Compromising one key doesn't affect others
```

## 📡 Complete Secret Sharing Flow

```
┌─────────────────────────────────────────────────────────────────┐
│              End-to-End Secret Sharing Flow                     │
└─────────────────────────────────────────────────────────────────┘

┌─────────────┐         ┌──────────────┐         ┌─────────────┐
│   Sender    │         │   Signaling  │         │  Receiver    │
│             │         │    Server    │         │             │
└──────┬──────┘         └──────┬───────┘         └──────┬──────┘
       │                       │                         │
       │ 1. Generate Keys      │                         │
       │    (Signal Protocol)  │                         │
       │                       │                         │
       │ 2. Join Session        │                         │
       ├───────────────────────>│                         │
       │                       │                         │
       │ 3. Send Prekey Bundle │                         │
       ├───────────────────────>│                         │
       │                       │                         │
       │                       │ 4. Join Session         │
       │                       │<───────────────────────┤
       │                       │                         │
       │                       │ 5. Send Prekey Bundle   │
       │                       │<───────────────────────┤
       │                       │                         │
       │ 6. Receive Bundle     │                         │
       │<───────────────────────┤                         │
       │                       │ 7. Receive Bundle       │
       │                       ├─────────────────────────>│
       │                       │                         │
       │ 8. X3DH Key Agreement │                         │
       │    (Local)            │                         │
       │                       │ 9. X3DH Key Agreement  │
       │                       │    (Local)             │
       │                       │                         │
       │ 10. Initialize Session│                         │
       │     (Double Ratchet)  │                         │
       │                       │ 11. Initialize Session │
       │                       │     (Double Ratchet)   │
       │                       │                         │
       │ 12. WebRTC Offer      │                         │
       ├───────────────────────>│                         │
       │                       │ 13. Relay Offer         │
       │                       ├─────────────────────────>│
       │                       │                         │
       │                       │ 14. WebRTC Answer       │
       │                       │<────────────────────────┤
       │ 15. Relay Answer      │                         │
       │<───────────────────────┤                         │
       │                       │                         │
       │ 16. ICE Candidates    │                         │
       │<═══════════════════════╪═════════════════════════>│
       │                       │                         │
       │ 17. P2P Connection    │                         │
       │<═══════════════════════╪═════════════════════════>│
       │    Established        │                         │
       │                       │                         │
       │ 18. Encrypt Secret    │                         │
       │     (Signal Protocol) │                         │
       │                       │                         │
       │ 19. Send Encrypted    │                         │
       │     Secret (P2P)      │                         │
       │═══════════════════════>│                         │
       │                       │                         │
       │                       │ 20. Receive Encrypted   │
       │                       │     Secret              │
       │                       │                         │
       │                       │ 21. Decrypt Secret     │
       │                       │     (Signal Protocol)   │
       │                       │                         │
       │                       │ 22. Mark Complete       │
       │                       │<────────────────────────┤
       │                       │                         │
       │                       │ 23. Invalidate Session  │
       │                       │     (Server-side)       │
       │                       │                         │
       └───────────────────────┴─────────────────────────┘
```

## 🔑 Key Components

### 1. Signal Protocol

Implements the core Signal Protocol functionality:

- **Key Generation**:
  -  Creates ECDH and ECDSA key pairs for identity
  -  Creates signed prekey with ECDSA signature
  -  Creates one-time prekey for X3DH

- **X3DH Key Agreement**:
  - Performs Extended Triple Diffie-Hellman key agreement
  - Derives shared secret from 4 DH operations
  - Handles both sender (initiator) and receiver (responder) roles

- **Double Ratchet**:
  -  Sets up Signal Protocol session with chain keys
  -  Encrypts messages using message keys derived from chain keys
  -  Decrypts messages using the same key derivation

### 2. WebRTC Connection

Handles peer-to-peer data channel:

- Creates RTCPeerConnection with STUN/TURN servers
- Manages data channel for encrypted secret transmission
- Handles connection state and retry logic

### 3. Signaling Server

WebSocket server for session coordination:

- Relays prekey bundles between sender and receiver
- Facilitates WebRTC signaling (offers, answers, ICE candidates)
- Tracks session state and invalidates completed sessions
- Prevents reuse of already-viewed secrets

### 4. Frontend Components

- **SecretShareForm** :
  - Sender interface for creating and sending secrets
  - Handles file and voice message encryption
  - Manages Signal Protocol session as sender

- **SecretReceiveForm** :
  - Receiver interface for viewing secrets
  - Handles decryption and file downloads
  - Manages Signal Protocol session as receiver
  - Sends completion status to invalidate session

## 🛡️ Security Guarantees

### What We Protect Against

1. **Man-in-the-Middle Attacks**: X3DH ensures authenticated key exchange
2. **Replay Attacks**: Message numbers and unique IVs prevent replay
3. **Forward Secrecy**: Each message uses a unique key derived from chain keys
4. **Post-Compromise Security**: Chain keys ratchet forward automatically
5. **Server Compromise**: Server never sees encryption keys or plaintext
6. **Session Reuse**: Completed sessions are invalidated server-side

### What We Don't Store

- ❌ Encryption keys (Identity, Prekeys, Chain Keys)
- ❌ Shared secrets
- ❌ Plaintext secrets
- ❌ Message keys
- ✅ Only session metadata logs (sessionId, timestamps, completion status)

## 🚀 Getting Started

- Modern browser with Web Crypto API support

## 📊 Technical Details

### Cryptographic Algorithms

- **Key Exchange**: X3DH (Extended Triple Diffie-Hellman)
- **Key Derivation**: HKDF-SHA-256
- **Symmetric Encryption**: AES-256-GCM
- **Digital Signatures**: ECDSA P-256
- **Elliptic Curve**: P-256 (secp256r1)

### Key Sizes

- Identity Keys: 65 bytes (uncompressed EC public key)
- Shared Secret: 32 bytes (256 bits)
- Chain Keys: 32 bytes (256 bits)
- Message Keys: 32 bytes (256 bits)
- AES-GCM IV: 12 bytes (96 bits)
- AES-GCM Auth Tag: 16 bytes (128 bits)

### Session Lifecycle

1. **Creation**: Session created when sender generates sessionId
2. **Key Exchange**: Prekey bundles exchanged via signaling server
3. **Establishment**: X3DH and Double Ratchet setup
4. **Active**: WebRTC connection established, secret transmitted
5. **Completion**: Secret decrypted, session marked complete
6. **Invalidation**: Session added to blacklist, cannot be reused


## 🙏 Acknowledgments

- Signal Protocol specification
- Web Crypto API for browser-native cryptography
- WebRTC for peer-to-peer communication

---

**Built with Signal Protocol encryption** 🔒
