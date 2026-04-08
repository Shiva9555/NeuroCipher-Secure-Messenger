# 🔐 NeuroCipher Secure Messenger

> **A Hybrid ML-Powered Encryption & Authentication System**  
> Internship Project — Exposys Data Labs, Bengaluru  
> **Kuruva Shiva** | Roll No: 22G31A3221 | St. Johns College of Engineering & Technology, Yemmiganur

---

## 📌 Overview

NeuroCipher is a novel hybrid encryption system that enhances AES-256-GCM by incorporating a Machine Learning–generated dynamic S-Box, making every message's substitution layer unique and unpredictable. Combined with a 3-factor authentication pipeline (bcrypt + TOTP + Keystroke Dynamics ML), it addresses the key weaknesses of standard cryptographic implementations.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                   NEUROCPHER SYSTEM ARCHITECTURE                │
├─────────────────────────────────────────────────────────────────┤
│  LAYER 1 — CLIENT LAYER                                         │
│  ┌──────────────────────┐  ┌──────────────────────────────┐    │
│  │   Flask Web UI        │  │  Keystroke Capture (JS)      │    │
│  │   (HTML/CSS/JS)       │  │  14 timing features captured │    │
│  └──────────┬────────────┘  └──────────────┬───────────────┘   │
├─────────────┼──────────────────────────────┼────────────────────┤
│  LAYER 2 — APPLICATION LAYER (Flask app.py)                     │
│  ┌──────────▼──────────────────────────────▼──────────────┐    │
│  │   Flask Routes: /login /register /send /inbox /analytics│    │
│  └──────────┬────────────────────────────────┬─────────────┘   │
├─────────────┼────────────────────────────────┼────────────────  │
│  LAYER 3A — AUTH           │   LAYER 3B — ENCRYPTION           │
│  authenticator.py          │   neuro_cipher.py                  │
│  bcrypt (40%)              │   ML S-Box (MLP Net)               │
│  pyotp  (30%)              │   AES-256-GCM                      │
│  ML KS  (30%)              │   RSA-2048 OAEP                    │
├─────────────────────────────────────────────────────────────────┤
│  LAYER 4 — DATA LAYER                                           │
│  SQLite DB (Users/Messages/SecurityLog) | ML Models (.pkl)      │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✨ Key Features

| Feature | Detail |
|---|---|
| 🔒 ML Dynamic S-Box | MLP (64→128→64) generates a unique 256-byte permutation per message |
| 🔑 AES-256-GCM | Authenticated symmetric encryption with 128-bit auth tag |
| 🗝️ RSA-2048 OAEP | Secure key exchange — AES key + neural seed encrypted per recipient |
| 🧬 Keystroke Dynamics | 14 biometric features, Random Forest + Isolation Forest (100% CV acc.) |
| 🕐 TOTP 2FA | RFC 6238 time-based OTP via Google Authenticator |
| 🔐 bcrypt | Password hashing with gensalt(rounds=12) |
| 📊 Security Analytics | Trust score history, risk engine, audit log |
| 📁 File Encryption | Text and file encryption up to 16 MB |

---

## ⚙️ NeuroCipher Algorithm — 10 Steps

```
Step 01 → User Authentication:    bcrypt + TOTP + Keystroke Dynamics ML
Step 02 → Input:                  Plaintext message or file
Step 03 → Generate Keys:          256-bit AES key + 32-byte neural seed (os.urandom)
Step 04 → ML S-Box Generation:    MLP (64→128→64) produces unique 256-byte permutation
Step 05 → Neural Transform:       XOR diffusion (SHA-512 of seed) + S-Box substitution
Step 06 → AES-256-GCM Encrypt:   Authenticated encryption → ciphertext + 128-bit auth tag
Step 07 → RSA-2048 OAEP:         Encrypts AES key + neural seed with recipient public key
Step 08 → Package:                Ciphertext + Encrypted Key Bundle + Nonce + Auth Tag
Step 09 → Transmit:               Send over cloud/network
Step 10 → Decrypt:                RSA → AES-GCM verify tag → Inverse S-Box → Inverse XOR → Plaintext
```

---

## 📁 Project Structure

```
secure-messenger/
├── app.py                  # Main Flask application (580 lines, 12 routes)
├── config.py               # Configuration settings
├── demo.py                 # CLI demo & benchmark script
├── create_test_user.py     # Test user creation utility
├── requirements.txt        # Pinned dependencies
│
├── encryption/
│   ├── neuro_cipher.py     # NeuroCipher-v1 hybrid algorithm (402 lines)
│   └── benchmark.py        # AES vs NeuroCipher benchmarking (184 lines)
│
├── auth/
│   ├── authenticator.py    # Multi-factor authentication (281 lines)
│   └── ml_verifier.py      # Keystroke dynamics ML (448 lines)
│
├── models/
│   └── database.py         # SQLAlchemy ORM: User/Message/SecurityLog (83 lines)
│
├── static/
│   ├── css/style.css       # Premium dark-theme UI (1754 lines)
│   └── js/app.js           # Keystroke capture + API calls (103 lines)
│
├── templates/              # 8 Jinja2 HTML templates
│   ├── base.html
│   ├── login.html
│   ├── register.html
│   ├── dashboard.html
│   ├── chat.html
│   ├── inbox.html
│   ├── analytics.html
│   └── setup_totp.html
│
├── uploads/                # Temporary encrypted file storage
└── ml_models/              # Saved ML models: classifier.pkl, scaler.pkl, anomaly.pkl
```

---

## 🚀 Quick Start

```bash
# Step 1: Clone the repository
git clone https://github.com/YOUR_USERNAME/NeuroCipher.git
cd NeuroCipher

# Step 2: Install dependencies
pip install -r requirements.txt

# Step 3: Run the CLI demo
python demo.py
# → Demonstrates encryption, ML training, authentication, benchmark

# Step 4: Create a test user
python create_test_user.py
# → Creates user 'alice_demo' with password DemoPass@2026

# Step 5: Start the web app
python app.py
# → Server starts at http://127.0.0.1:5000
```

Then open your browser:

| URL | Purpose |
|---|---|
| `/register` | Create your account |
| `/login` | Login with MFA (password + TOTP + keystroke) |
| `/send` | Send encrypted message or file |
| `/inbox` | Read decrypted messages |
| `/analytics` | Security dashboard & trust score |

---

## 📦 Technology Stack

| Category | Library | Version | Purpose |
|---|---|---|---|
| Language | Python | 3.13 | Runtime |
| Web Framework | Flask | 3.0.0 | HTTP server, Jinja2 |
| Encryption | PyCryptodome | 3.19.0 | AES-256-GCM, RSA-2048, PKCS1_OAEP |
| Passwords | bcrypt | 4.1.2 | hashpw/checkpw, gensalt(rounds=12) |
| ML | scikit-learn | 1.3.2 | MLP, RandomForest, IsolationForest |
| Numerics | NumPy | 1.26.2 | Array ops, argsort, SHA-based XOR |
| 2FA | pyotp | 2.9.0 | TOTP, RFC 6238 |
| ORM | Flask-SQLAlchemy | 3.1.1 | SQLite models |
| ML I/O | joblib | 1.3.2 | Model persistence (.pkl) |
| QR Code | qrcode | 7.4.2 | Google Authenticator setup |

---

## 📊 Results & Benchmarks

### Encryption Integrity — All 5 Tests PASSED ✅

| Test | Size | Enc. Time | Dec. Time | Result |
|---|---|---|---|---|
| Standard 63-char message | 63 B | 23.87 ms | ~12 ms | ✅ PASSED |
| Short: "Short msg" | 9 B | 5.90 ms | ~3 ms | ✅ PASSED |
| Large: 1000×"A" | 1000 B | 7.69 ms | ~4 ms | ✅ PASSED |
| Unicode text | 38 B | 5.20 ms | ~3 ms | ✅ PASSED |
| Special chars !@#$%^&*() | 39 B | 5.11 ms | ~3 ms | ✅ PASSED |

### Performance: NeuroCipher vs AES-256-GCM

| Data Size | AES-GCM | NeuroCipher | Security Layers |
|---|---|---|---|
| 100 B | 0.43 ms | 71.88 ms | 5 vs 1 |
| 1 KB | 0.34 ms | 74.17 ms | 5 vs 1 |
| 10 KB | 0.37 ms | 82.33 ms | 5 vs 1 |
| 50 KB | 0.51 ms | 109.96 ms | 5 vs 1 |

> Overhead is from RSA-2048 key exchange (~60ms) + MLP forward pass (~10ms). For messaging, 71–110ms is imperceptible.

### System Testing — 29/29 Tests Passed ✅

| Test Type | Ran | Passed | Pass Rate |
|---|---|---|---|
| Encryption Unit | 5 | 5 | 100% |
| Authentication Unit | 10 | 10 | 100% |
| Keystroke ML Unit | 4 | 4 | 100% |
| Integration | 6 | 6 | 100% |
| Benchmark | 4 | 4 | 100% |
| **TOTAL** | **29** | **29** | **100%** |

---

## 🔮 Future Enhancements

- **Post-Quantum Cryptography** — Replace RSA-2048 with CRYSTALS-Kyber (NIST PQC standard)
- **Real-time Chat** — Add WebSocket (Flask-SocketIO) for instant encrypted messaging
- **Transformer S-Box** — Replace MLP with a Transformer-based neural network
- **Continuous Authentication** — Keystroke monitoring throughout the session
- **Mobile App** — Flutter + fingerprint/face ID integration
- **Federated ML** — Train keystroke models across devices without centralising biometric data

---

## 📚 References

1. NIST FIPS PUB 197 — Advanced Encryption Standard (AES), 2001
2. Rivest, Shamir, Adleman — RSA Public-Key Cryptosystem, Comm. ACM, 1978
3. Dworkin — NIST SP 800-38D: GCM Block Cipher Mode, 2007
4. M'Raihi et al. — TOTP: RFC 6238, IETF, 2011
5. Provos & Mazières — bcrypt (A Future-Adaptable Password Scheme), USENIX, 1999
6. Killourhy & Maxion — Keystroke Dynamics Anomaly Detection, IEEE DSN, 2009
7. Pedregosa et al. — Scikit-learn: ML in Python, JMLR vol.12, 2011
8. Breiman — Random Forests, Machine Learning 45(1), 2001
9. Liu, Ting, Zhou — Isolation Forest, IEEE ICDM, 2008

---

## 👤 Author

**Kuruva Shiva**  
Roll No: 22G31A3221  
St. Johns College of Engineering & Technology, Yemmiganur, Kurnool, Andhra Pradesh  
Internship: **Exposys Data Labs**, Bengaluru, Karnataka

---

*Built with ❤️ during Exposys Data Labs Internship — Software Development using Python & ML*
