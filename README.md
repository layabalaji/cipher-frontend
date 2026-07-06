# AI Cipher Decoder

An AI-powered tool for encrypting, decrypting, and auto-cracking classical ciphers — Caesar, Atbash, Affine, Vigenère, Hill, Nihilist, Baconian, Porta, Pollux, Morbit, Fractionated Morse, Aristocrats, Patristocrats, Xenocrypts, and more — with a chatbot that explains each step so users learn the underlying technique, not just get an answer.

## How it works

The project has two parts working together:

1. **A deterministic cipher engine** — pure math/code that encrypts and decrypts any cipher when the key is known. No AI involved, just correct implementations of each algorithm.
2. **A cryptanalysis solver** — for ciphers meant to be "cracked" without a key (mainly Aristocrats/Patristocrats and Vigenère), using frequency analysis, quadgram scoring, and hill-climbing/simulated annealing search. This is the actual AI/ML part of the project.

## Why this project

Classical ciphers are a great sandbox for combining algorithmic thinking (implementing exact cipher math) with applied AI/ML techniques (frequency analysis, language modeling, and search-based optimization for code-breaking). This project was built to explore both sides — clean deterministic implementations plus a from-scratch cryptanalysis engine — wrapped in a conversational interface that explains its reasoning.

A chatbot layer (powered by the Anthropic API) sits on top of both: it parses what the user is asking for in plain English, calls the right cipher or solver function, and explains the result step by step — like a tutor, not just a calculator.

## Repo structure

This project is split across two repositories:

- **`ai-cipher-decoder-frontend`** — plain HTML/CSS/JS. No framework, no build step.
- **`ai-cipher-decoder-backend`** — Python + FastAPI, hosts the cipher engine, solvers, and chatbot orchestration.

### Frontend (`ai-cipher-decoder-frontend`)

```
ai-cipher-decoder-frontend/
├── index.html          # landing page
├── practice.html        # practice mode (timer, random ciphertext, reveal steps)
├── solve.html            # manual encrypt/decrypt tool
├── chat.html             # chatbot interface
├── css/                  # per-page + shared styles
├── js/
│   ├── api.js             # fetch wrappers calling the backend
│   ├── ciphers-ui.js       # shared cipher-selector logic
│   ├── solve.js
│   ├── practice.js
│   ├── chat.js
│   └── utils.js
└── assets/
```

### Backend (`ai-cipher-decoder-backend`)

```
ai-cipher-decoder-backend/
├── app/
│   ├── main.py            # FastAPI entrypoint
│   ├── ciphers/            # keyed encrypt/decrypt for every supported cipher
│   ├── solvers/             # quadgram scoring, hill-climbing, Aristocrat/Vigenère crackers
│   ├── data/                 # English + Spanish quadgram frequency tables
│   ├── chatbot/               # Anthropic API client, tool definitions, orchestrator
│   ├── routes/                 # /encrypt, /decrypt, /solve, /chat endpoints
│   └── tests/
├── requirements.txt
└── .env.example
```

## Getting started

### Backend

```bash
cd ai-cipher-decoder-backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env             # add your ANTHROPIC_API_KEY
uvicorn app.main:app --reload
```

Backend runs at `http://localhost:8000`.

### Frontend

No build step required — just open the HTML files directly in a browser, or serve the folder locally:

```bash
cd ai-cipher-decoder-frontend
python -m http.server 5500
```

Then visit `http://localhost:5500`. Make sure `js/api.js` points at your backend URL (`http://localhost:8000` while developing).

## Roadmap

- [ ] Phase 1 — Cipher engine: Caesar, Atbash, Affine, Vigenère
- [ ] Phase 1 — Cipher engine: Hill, Nihilist, Baconian, Porta, Pollux, Morbit, Fractionated Morse
- [ ] Phase 2 — Aristocrat/Patristocrat auto-solver (quadgram + hill-climbing)
- [ ] Phase 2 — Vigenère auto-solver (Kasiski + Index of Coincidence)
- [ ] Phase 2 — Xenocrypt (Spanish) solver
- [ ] Phase 3 — Chatbot orchestration via Anthropic API tool use
- [ ] Phase 4 — Practice mode with timer and step-by-step reveal
- [ ] Deploy frontend (Netlify/Vercel/GitHub Pages) + backend (Render/Railway)

## Resources used

- [practicalcryptography.com](http://practicalcryptography.com) — quadgram frequency data and cryptanalysis background
- [dCode.fr](https://www.dcode.fr) — reference implementations for edge-case checking
- [Anthropic API docs](https://docs.claude.com) — chatbot tool-use integration

## License

TBD