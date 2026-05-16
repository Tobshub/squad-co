# SquadScan

> **Bridging the gap in the informal retail sector by bringing large retail infrastructure to SMEs.**

SquadScan is an all-in-one smart retail assistant built specifically for small and medium-sized retailers in Nigeria and other emerging markets. It combines a powerful mobile point-of-sale (POS) system with AI-powered business intelligence, offline-first architecture, and integrated payment infrastructure — giving informal retailers access to tools previously reserved for large supermarket chains.

---

## 📖 Table of Contents

- [The Problem](#the-problem)
- [The Solution](#the-solution)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Architecture](#architecture)
- [Database Schema](#database-schema)
- [Getting Started](#getting-started)
- [API Overview](#api-overview)
- [Mobile App](#mobile-app)
- [Key Innovations](#key-innovations)
- [License](#license)

---

## The Problem

Informal retailers — the mom-and-pop shops, kiosks, and small provision stores that form the backbone of retail in Nigeria and much of Africa — operate with almost no digital infrastructure:

- **No inventory tracking**: Stockouts and overstocking are common. Shopkeepers rely on memory.
- **No sales records**: No visibility into what's selling, when, or how much profit is being made.
- **No digital payments**: Cash-only operations limit customer convenience and financial inclusion.
- **No business insights**: No data-driven decisions — everything is intuition-based.
- **No access to credit**: Without transaction history, banks won't lend to them.
- **Unreliable internet**: Most retail tools assume constant connectivity, which doesn't exist.

---

## The Solution

SquadScan brings enterprise-grade retail infrastructure to every small shopkeeper's pocket:

| Enterprise Feature | SquadScan Equivalent |
|--------------------|----------------------|
| Barcode scanner | Phone camera + barcode scanning |
| Inventory management system | SQLite-powered local inventory with sync |
| POS terminal | Mobile checkout with cart & receipts |
| Business intelligence dashboard | AI-generated insights in 5 languages |
| Payment terminal | Virtual account + SquadCo integration |
| Tax reporting | Auto-generated VAT calculations & export |
| Credit scoring | AI-powered financial health profile |

---

## Features

### 🛒 Point of Sale (POS)
- **Barcode scanning** — Scan product barcodes using the phone camera (QR, EAN-13, EAN-8, UPC-A, UPC-E)
- **AI product identification** — Take a photo of any product and Claude AI identifies the name, category, and estimated price
- **Cart & checkout** — Add items to cart, adjust quantities, and complete sales with automatic inventory deduction
- **Sales history** — View daily, weekly, and monthly sales with revenue tracking

### 📦 Inventory Management
- **Real-time stock tracking** — Every sale automatically updates inventory counts
- **Low stock alerts** — Visual warnings when items fall below configurable thresholds
- **Restock management** — Add stock by scanning or manual entry
- **AI category recommendations** — Gemini AI suggests the best category for new products
- **Soft deletes** — Products are flagged, not lost, preserving historical sales data

### 🤖 AI Business Intelligence
- **Business direction analysis** — AI evaluates your shop's performance and recommends strategic moves
- **Revenue predictions** — Forecast next month's sales based on historical trends
- **Risk alerts** — Warns about tax liabilities, stockouts, and slow-moving inventory
- **Top movers tracking** — Visual bar charts of best-selling products
- **Restock recommendations** — AI-identified items that need replenishment
- **Multilingual** — Insights delivered in English, Nigerian Pidgin, Hausa, Yoruba, or Igbo

### 💳 Payments & Virtual Accounts
- **Virtual account numbers** — Every shop gets a unique bank account for receiving transfers
- **Payment tracking** — All incoming payments are logged and matched to sales
- **Auto-linking** — Payments are automatically linked to matching sales by amount
- **Payment simulation** — Test the payment flow in sandbox mode
- **Webhook processing** — Real-time payment status updates from SquadCo

### 📊 Tax & Credit
- **Tax export** — Auto-calculated VAT (7.5%), turnover, and tax liability with PDF/CSV export
- **Credit profile** — AI-generated credit score and financial health dashboard
- **Income trend analysis** — Visual charts of revenue over time

### 🌍 Offline-First & Sync
- **Works without internet** — Full POS, inventory, and sales functionality offline
- **Automatic sync** — Operations queue syncs with the backend every 60 seconds when online
- **Conflict resolution** — Operation-based sync with idempotency and version tracking
- **Multi-device support** — Same shop account works across multiple devices

### 🌐 Localization
- **5 languages**: English, Nigerian Pidgin (`pcm`), Hausa, Yoruba, Igbo
- **Language switching** — Available during onboarding and in settings at any time

---

## Tech Stack

### Backend
| Layer | Technology |
|-------|------------|
| Runtime | Node.js (ES Modules) |
| Framework | Express.js |
| Language | TypeScript |
| Database | PostgreSQL |
| ORM | Prisma |
| Auth | JWT + OTP (mock SMS) |
| Validation | Zod |
| AI | Google Gemini API (`gemini-2.0-flash`) |
| Payments | SquadCo (`uni-sdk` + REST API) |
| Docs | Swagger / OpenAPI 3.0 |

### Mobile
| Layer | Technology |
|-------|------------|
| Framework | React Native + Expo SDK |
| Language | TypeScript |
| Navigation | React Navigation v7 |
| Local DB | `expo-sqlite` |
| Camera | `expo-camera` |
| AI Vision | Anthropic Claude API |
| Image Processing | `expo-image-manipulator` |
| Audio | `expo-av` |
| Export | `expo-print` + `expo-sharing` |

---

## Project Structure

```
squad-co/
├── backend/                    # Express.js API server
│   ├── prisma/
│   │   └── schema.prisma       # Database schema definition
│   ├── src/
│   │   ├── modules/            # Feature-based modules
│   │   │   ├── auth/           # OTP & JWT authentication
│   │   │   ├── products/       # Product CRUD + AI categories
│   │   │   ├── inventory/      # Stock tracking & restocking
│   │   │   ├── sales/          # Checkout + AI insights
│   │   │   ├── insights/       # Business intelligence
│   │   │   ├── payments/       # SquadCo integration
│   │   │   └── sync/           # Offline sync engine
│   │   ├── middlewares/        # Auth, error handling, logging
│   │   ├── config/             # Database & environment
│   │   ├── utils/              # Response helpers & validators
│   │   ├── app.ts              # Express app setup
│   │   └── server.ts           # Entry point
│   ├── .env                    # Environment variables
│   └── README.md               # Backend-specific docs
│
├── mobile/                     # React Native Expo app
│   ├── App.tsx                 # Root navigator
│   ├── assets/                 # Images, sounds, icons
│   ├── screens/
│   │   ├── Pre-Auth/
│   │   │   └── WelcomeScreen.tsx      # Onboarding carousel
│   │   ├── Auth/
│   │   │   ├── LoginScreen.tsx        # Phone + shop registration
│   │   │   └── OTPVerification.tsx    # OTP input
│   │   └── Post-Auth/
│   │       ├── HomeScreen.tsx         # Dashboard
│   │       ├── SalesPage.tsx          # Barcode scanner + AI scan + POS
│   │       ├── InventoryPage.tsx      # Product list & stock
│   │       ├── AiInsightScreen.tsx    # AI business insights
│   │       ├── TopMovers.tsx          # Best sellers chart
│   │       ├── RestockAlert.tsx       # Low stock alerts
│   │       ├── AllSalesScreen.tsx     # Sales history
│   │       ├── PaymentsScreen.tsx     # Payment history
│   │       ├── LinkPaymentScreen.tsx  # Match payments to sales
│   │       ├── ReceiptScreen.tsx      # Sale receipts
│   │       ├── TaxExportScreen.tsx    # Tax calculations
│   │       ├── CreditProfileScreen.tsx # Credit scoring
│   │       └── Settings.tsx           # App settings
│   ├── services/
│   │   ├── productService.tsx         # Product API client
│   │   ├── salesService.tsx           # Sales API client
│   │   ├── api.ts                     # Axios instance
│   │   └── notifications.tsx          # Payment notification toast
│   └── utils/
│       └── db.ts                      # SQLite database manager
│
└── squadco/                    # Docusaurus documentation site
    ├── docs/                   # API documentation
    ├── blog/                   # Blog posts
    └── src/                    # Custom components
```

---

## Architecture

### Backend Architecture

The backend follows a **modular monolith** pattern with clean separation of concerns:

```
HTTP Request → Routes → Controller → Service → Prisma → PostgreSQL
                  ↑
            Middleware (Auth, Validation, Error Handling)
```

**Key Patterns:**
- **Operation-Based Sync (Event Sourcing Lite)**: Clients send operations ("Sold 5 items", "Restocked +10") rather than final state. The server replays operations idempotently using `clientOpId`.
- **Optimistic Concurrency**: `version` fields on Product and Inventory prevent race conditions across offline devices.
- **Soft Deletes**: Products are flagged `deleted = true` rather than removed, preserving historical sales data.
- **Webhook-Driven Updates**: Payment status updates are pushed in real-time via SquadCo webhooks.

### Mobile Architecture

The mobile app uses a **local-first** architecture:

```
Camera/Screen → SQLite (local) → Sync Engine → Backend API
                    ↑
            Operations Queue (offline buffer)
```

**Sync Flow:**
1. Every local mutation (create product, sell item, restock) is recorded as an operation in `operations_queue`
2. The Sync Engine pushes queued operations to `/api/v1/sync/push` every 60 seconds
3. The engine pulls remote operations from `/api/v1/sync/pull` and applies them locally
4. Conflicts are resolved via server-side idempotency (duplicate `clientOpId` operations are skipped)

---

## Database Schema

### PostgreSQL (Backend)

| Model | Purpose |
|-------|---------|
| `User` | Shop owner profile with virtual account details |
| `Otp` | One-time password codes for login |
| `Product` | Product catalog with barcode, pricing, category |
| `Inventory` | Real-time stock levels linked to products |
| `Sale` | Sales transactions with total amount |
| `SaleItem` | Line items within each sale |
| `Payment` | Incoming payments via SquadCo |
| `Operation` | Sync operations for offline-first support |

### SQLite (Mobile)

| Table | Purpose |
|-------|---------|
| `products` | Local product cache |
| `inventory` | Local stock levels |
| `sales` | Local sales history |
| `sale_items` | Local sale line items |
| `payments` | Local payment records |
| `operations_queue` | Pending sync operations |
| `settings` | App preferences (token, language, deviceId) |

---

## Getting Started

### Prerequisites
- Node.js v18+
- PostgreSQL
- Expo CLI (`npm install -g expo-cli`)
- iOS Simulator or Android Emulator (or physical device with Expo Go)

### Backend Setup

```bash
cd backend
npm install

# Create .env file
cp .env.example .env
# Edit .env with your DATABASE_URL and JWT_SECRET

# Database
npx prisma generate
npm run prisma:migrate

# Run
npm run dev
```

### Mobile Setup

```bash
cd mobile
npm install

# Start Expo development server
npx expo start

# Scan QR code with Expo Go app, or press 'i' for iOS / 'a' for Android
```

---

## API Overview

All endpoints are prefixed with `/api/v1/`.

### Authentication
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/request-otp` | Request login OTP |
| POST | `/auth/verify-otp` | Verify OTP & get JWT |
| GET | `/auth/profile` | Get user profile |

### Products
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/products` | Create product |
| GET | `/products` | List products |
| GET | `/products/recommend-category` | AI category suggestion |
| PATCH | `/products/:id` | Update product |
| DELETE | `/products/:id` | Delete product |

### Inventory
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory` | List inventory |
| GET | `/inventory/low-stock` | Low stock items |
| POST | `/inventory/restock` | Add stock |
| POST | `/inventory/set-quantity` | Set stock level |

### Sales
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/sales/checkout` | Process sale |
| POST | `/sales/sync` | Sync offline sales |
| GET | `/sales` | List sales |
| GET | `/sales/insights` | AI business insights |

### Payments
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/payments/webhook` | SquadCo webhook |
| GET | `/payments` | List payments |
| POST | `/payments/:id/link` | Link payment to sale |
| POST | `/payments/simulate` | Simulate transfer |

### Sync
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/sync/push` | Push operations |
| GET | `/sync/pull` | Pull operations |

### Insights
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/insights/main` | Full AI business report |

---

## Mobile App

### Screens

| Screen | Description |
|--------|-------------|
| **WelcomeScreen** | 3-slide onboarding with language selection |
| **LoginScreen** | Phone number + shop name registration |
| **OTPVerification** | 6-digit code input with resend timer |
| **HomeScreen** | Dashboard with stats, virtual account, quick actions |
| **SalesPage** | Barcode scanner, AI photo scanner, cart, checkout |
| **InventoryPage** | Product list with search, filters, low-stock alerts |
| **AiInsightScreen** | AI business intelligence panel |
| **AllSalesScreen** | Daily/weekly sales with revenue charts |
| **PaymentsScreen** | Payment history with status badges |
| **LinkPaymentScreen** | Match unlinked payments to sales |
| **ReceiptScreen** | Sale receipt with AI analysis |
| **TaxExportScreen** | VAT calculations with PDF/CSV export |
| **CreditProfileScreen** | AI credit score & financial health |
| **Settings** | Profile, language, theme, data export, logout |

### Navigation

```
WelcomeScreen → LoginScreen → OTPVerification
                              ↓
                        HomeScreen (Tab Navigator)
                        ├── Home Tab (Stack)
                        │   ├── RetailHome
                        │   ├── SalesScreen (Scanner)
                        │   ├── InventoryScreen
                        │   ├── AIInsightsScreen
                        │   ├── AllSalesScreen
                        │   ├── PaymentsScreen
                        │   └── SettingsScreen
                        ├── Report Tab → AIInsightsScreen
                        └── Profile Tab → SettingsScreen
```

---

## Key Innovations

### 🧠 AI-Powered Retail
- **Claude Vision** identifies products from photos — no barcode needed
- **Gemini** generates business insights in local languages
- **AI credit scoring** gives shopkeepers access to financial metrics previously unavailable

### 📡 Offline-First by Design
- Full functionality without internet
- Automatic background sync when connectivity returns
- Operation-based sync prevents data loss and conflicts

### 💰 Integrated Payments
- Virtual bank accounts for every shop
- Auto-linking payments to sales
- Zero hardware — works with any phone

### 🌍 Built for Emerging Markets
- 5 local languages
- Works on low-end Android devices
- Minimal data usage with efficient sync
- Designed for unreliable connectivity

---

## License

ISC
