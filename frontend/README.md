# VoicePay AI — Frontend

> AI-powered voice-guided payment application using PayPal Sandbox.

## Quick Start

### 1. Open the frontend

Simply open `frontend/index.html` in a modern browser (Chrome, Edge, Firefox):

```bash
# On Windows — double-click the file, or:
start frontend/index.html

# Or use any static file server (e.g. Python):
cd frontend
python -m http.server 5500
# Then open http://localhost:5500
```

> **Note:** Speech recognition works best in **Google Chrome**. A microphone permission prompt will appear on first use.

### 2. Configure the backend URL

Edit the top of `frontend/app.js`:

```js
const API_BASE = 'http://localhost:8000';
```

Change this to your FastAPI backend's URL in production.

### 3. Demo Mode vs Live Mode

| Mode | When | Behavior |
|------|------|----------|
| **Demo** | Backend unreachable | Local mock parsing, simulated checkout, no real payments |
| **Live** | Backend responds at `API_BASE` | Real AI parsing, PayPal Sandbox orders, backend-verified capture |

The frontend automatically detects backend availability on load.

---

## Backend API Contract

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/parse-payment` | POST | Send `{ "message": "..." }` → receive `{ recipient, amount, currency, purpose }` |
| `/create-order` | POST | Send validated payment details → receive `{ order_id }` |
| `/capture-order` | POST | Send `{ "order_id": "..." }` → receive capture confirmation |
| `/transactions` | GET | Fetch transaction history |

---

## PayPal SDK Integration

To enable real PayPal Sandbox buttons:

1. Add the PayPal JS SDK to `index.html` before `app.js`:
   ```html
   <script src="https://www.paypal.com/sdk/js?client-id=YOUR_SANDBOX_CLIENT_ID"></script>
   ```

2. The `renderPayPalButtons()` function in `app.js` will automatically detect the SDK and render buttons.

3. **Never put** PayPal secrets or API keys in frontend code.

---

## Tech Stack

- HTML5 / CSS3 / Vanilla JavaScript
- Web Speech API (recognition + synthesis)
- Lucide Icons (CDN)
- Inter font (Google Fonts)
- No build tools, no frameworks

---

## File Structure

```
frontend/
├── index.html   — Semantic HTML structure
├── style.css    — Design tokens, responsive layout, animations
├── app.js       — Application logic, speech, API, payment flow
└── README.md    — This file
```
