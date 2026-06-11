[Uploading READM[README.md](https://github.com/user-attachments/files/28854326/README.md)
E.md…]()
# tzivos
# Tzivos Hashem AI Phone Bot

Automated enrollment phone bot powered by **Twilio Voice**, **Claude AI**, **Google Maps**, and **Hebrew calendar support**.

---

## What it does

- Greets callers in English or Yiddish (auto-detects language)
- Collects full name in **English and Yiddish spelling**
- Validates home address via **Google Maps** and checks for multi-unit buildings
- Accepts date of birth in **English or Hebrew calendar**
- Collects school name, language preference, and referral source
- Reads all details back for confirmation
- Sends enrollment summary to staff via **SMS + email**

---

## Setup (5 steps)

### 1. Install dependencies
```bash
npm install
```

### 2. Configure environment variables
```bash
cp .env.example .env
# Then open .env and fill in all values
```

You need accounts for:
- **Twilio** — https://twilio.com (free trial, ~$1/mo for a number)
- **Anthropic** — https://console.anthropic.com
- **Google Maps** — https://console.cloud.google.com (enable Geocoding API)

### 3. Start the server
```bash
npm start
# or for development with auto-reload:
npm run dev
```

### 4. Expose your server to the internet (for local testing)
```bash
npm run tunnel
# Copy the https://xxxx.ngrok.io URL shown
```

Or deploy to a cloud host (see Deployment section below).

### 5. Configure Twilio webhook
1. Go to https://console.twilio.com → Phone Numbers → Manage → Your number
2. Under **Voice & Fax → A Call Comes In**, set:
   - **Webhook URL**: `https://your-domain.com/voice`
   - **HTTP Method**: POST
3. Save

**That's it — call your Twilio number and the bot answers!**

---

## Project structure

```
tzivos-phone-bot/
├── server.js          # Main bot server
├── package.json
├── .env.example       # Copy to .env and fill in
└── README.md
```

---

## Enrollment steps (call flow)

| Step | What the bot collects |
|------|-----------------------|
| 1 | Full name in English |
| 2 | Yiddish spelling of name |
| 3 | Home address (Google Maps validated) |
| 4 | Apartment / unit (if multi-unit building) |
| 5 | Date of birth (English or Hebrew calendar) |
| 6 | School name |
| 7 | Language preference (English / Yiddish) |
| 8 | Referral source |
| 9 | Read-back + confirmation |
| 10 | Submit → notify staff |

---

## Hebrew date support

The bot accepts Hebrew dates spoken naturally, for example:
- *"Chof Adar 5784"*
- *"The twentieth of Shvat 5782"*

It converts to Gregorian, calculates age, and stores both calendar dates.

---

## Yiddish mode

If the caller speaks Yiddish, the bot automatically switches:
- Responses delivered in Yiddish
- Speech recognition switches to `he-IL` (closest Twilio option for Yiddish)
- Voice switches to Amazon Polly `Lea`

---

## Deployment (recommended: Railway or Render)

### Railway (easiest)
```bash
npm install -g @railway/cli
railway login
railway init
railway up
```
Then set environment variables in the Railway dashboard and point your Twilio webhook to the Railway URL.

### Render
1. Push this folder to a GitHub repo
2. Create a new **Web Service** at https://render.com
3. Set build command: `npm install`
4. Set start command: `npm start`
5. Add all environment variables from `.env.example`

---

## Staff notifications

On each completed enrollment, the bot sends:
- **SMS** to `STAFF_PHONE` with full summary
- **Email** to `STAFF_EMAIL` with all collected fields, address verification status, both DOB formats, and timestamp

---

## Health check

```
GET /health
→ { "status": "ok", "sessions": 2 }
```

---

## Support

For customizations — adding new enrollment questions, changing the voice, integrating with a CRM, or adding a Yiddish TTS voice — ask Claude for help!
