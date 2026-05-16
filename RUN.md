# Running SquadScan Locally

> Step-by-step guide to get the backend and mobile app running on your machine.

---

## Prerequisites

- **Node.js** v18+ (`node -v`)
- **PostgreSQL** installed and running
- **npm** or **yarn**
- **Expo CLI** (`npm install -g expo-cli`)
- **ngrok** account (free tier works) — [sign up here](https://ngrok.com/)

---

## 1. Start PostgreSQL

Make sure your PostgreSQL server is running locally.

```bash
# macOS (Homebrew)
brew services start postgresql

# Linux (systemd)
sudo systemctl start postgresql

# Create the database
psql -U postgres -c "CREATE DATABASE squadscan_db;"
```

---

## 2. Run the Backend

```bash
cd backend

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
```

Edit `.env` with your values:

```env
PORT=3000
NODE_ENV=development
DATABASE_URL="postgresql://postgres:YOUR_PASSWORD@localhost:5432/squadscan_db?schema=public"
JWT_SECRET="any-long-random-string-here"
GEMINI_API_KEY="your-google-gemini-api-key"
SQUAD_SECRET_KEY="your-squadco-secret-key"
SQUAD_PUBLIC_KEY="your-squadco-public-key"
```

Run migrations and start:

```bash
# Generate Prisma client
npx prisma generate

# Run database migrations
npm run prisma:migrate

# (Optional) Seed with sample data
npm run seed

# Start development server
npm run dev
```

The backend will be available at `http://localhost:3000`.

---

## 3. Expose the Backend with ngrok (for Webhooks)

SquadCo sends payment webhooks to a public URL. ngrok tunnels your local backend so it can receive them.

### Install ngrok

```bash
# macOS
brew install ngrok

# Linux
snap install ngrok

# Or download from https://ngrok.com/download
```

### Authenticate

```bash
ngrok config add-authtoken YOUR_NGROK_AUTH_TOKEN
```

Find your auth token at: https://dashboard.ngrok.com/get-started/your-authtoken

### Start the Tunnel

```bash
ngrok http 3000
```

You'll see output like:

```
Forwarding  https://a1b2c3d4.ngrok-free.app -> http://localhost:3000
```

Copy the **Forwarding URL** (e.g. `https://a1b2c3d4.ngrok-free.app`) — this is your public backend URL.

### Configure SquadCo Webhook URL

In your SquadCo dashboard, set the webhook URL to:

```
https://YOUR_NGROK_URL/api/v1/payments/webhook
```

Example:
```
https://a1b2c3d4.ngrok-free.app/api/v1/payments/webhook
```

> **Note:** Every time you restart ngrok, the URL changes. You'll need to update the webhook URL in SquadCo or use a paid ngrok plan for a fixed domain.

---

## 4. Run the Mobile App

```bash
cd mobile

# Install dependencies
npm install

# Start Expo development server
npx expo start
```

A QR code will appear in your terminal. Scan it with the **Expo Go** app on your phone.

Or press:
- `i` → open in iOS Simulator
- `a` → open in Android Emulator

### Point the App to Your Backend

The app needs to know where your backend is. Update the base URL in `mobile/services/api.ts` (or wherever `BASE_URL` is defined):

```typescript
const BASE_URL = "http://YOUR_COMPUTER_IP:3000";
// Example: "http://192.168.1.45:3000"
```

> **Don't use `localhost`** — that refers to the phone itself, not your computer. Use your computer's actual LAN IP address.

To find your IP:
- **macOS/Linux**: `ifconfig` or `ip addr`
- **Windows**: `ipconfig`

---

## 5. Full Development Flow

With everything running, your setup looks like this:

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│   Mobile    │────→│   Backend    │←────│   SquadCo   │
│  (Expo Go)  │     │ (localhost)  │     │  (webhooks) │
└─────────────┘     └──────┬───────┘     └─────────────┘
                           │
                    ┌──────┴───────┐
                    │     ngrok    │
                    │ (public URL) │
                    └──────────────┘
```

### Typical Workflow

1. **Start PostgreSQL**
2. **Start backend** (`npm run dev` in `backend/`)
3. **Start ngrok** (`ngrok http 3000`) and copy the URL
4. **Update SquadCo webhook** with your ngrok URL
5. **Start mobile** (`npx expo start` in `mobile/`)
6. **Register/login** on the app with a phone number
7. **OTP appears in backend console** (mock SMS)
8. **Start scanning/selling!**

---

## 6. Environment Variables Reference

### Backend (`backend/.env`)

| Variable | Required | Description |
|----------|----------|-------------|
| `PORT` | Yes | Server port (default: 3000) |
| `NODE_ENV` | Yes | `development` or `production` |
| `DATABASE_URL` | Yes | PostgreSQL connection string |
| `JWT_SECRET` | Yes | Secret for signing JWT tokens |
| `GEMINI_API_KEY` | No | Google Gemini API key (for AI insights) |
| `SQUAD_SECRET_KEY` | No | SquadCo secret key (for payments) |
| `SQUAD_PUBLIC_KEY` | No | SquadCo public key (for payments) |

### Mobile

No `.env` file needed. Update `BASE_URL` directly in the API service file.

---

## 7. Common Issues

### "Cannot connect to backend"
- Make sure you're using your computer's LAN IP, not `localhost`
- Check firewall settings aren't blocking port 3000
- Ensure both phone and computer are on the same WiFi network

### "ngrok URL changes every time"
- Free ngrok assigns a new random URL on each start
- Either update the SquadCo webhook URL each time, or upgrade to ngrok Pro for a fixed subdomain

### "Prisma Client not found"
```bash
cd backend
npx prisma generate
```

### "Database does not exist"
```bash
psql -U postgres -c "CREATE DATABASE squadscan_db;"
```

### "SquadCo webhooks not arriving"
- Verify the webhook URL in SquadCo dashboard matches your current ngrok URL
- Check backend logs for incoming requests
- Ensure `NODE_ENV` is NOT set to `test` (webhooks are skipped in test mode)

---

## 8. Production Deployment Notes

For production, replace ngrok with a proper domain:

1. Deploy backend to a cloud provider (Render, Railway, AWS, etc.)
2. Set a fixed domain (e.g. `https://api.squadscan.app`)
3. Configure SquadCo webhook to use that fixed domain
4. Update mobile app `BASE_URL` to the production domain
5. Set `NODE_ENV=production`

No code changes needed — just environment configuration.
