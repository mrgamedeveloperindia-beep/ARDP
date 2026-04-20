# 🌿 Arokiam — Wellness Marketplace Platform

A full-stack, production-ready SaaS wellness marketplace with 7-role RBAC, BMI health engine, AI-ready nutrition tracking, and a complete shopping/logistics system.

---

## 🛠 Tech Stack

| Layer      | Technology                                     |
|------------|------------------------------------------------|
| Frontend   | React 18 + Vite + TypeScript + Tailwind CSS + Framer Motion |
| State      | Zustand (cart) + TanStack React Query (server state) |
| Backend    | Node.js + Express.js (ESM)                     |
| Database   | MongoDB Atlas + Mongoose                        |
| Auth       | Firebase Auth (Google + Email)                  |
| Cache      | Upstash Redis                                   |
| Email      | Nodemailer (Gmail/SendGrid)                     |
| Charts     | Recharts                                        |
| Hosting    | Vercel (client) + Render/Railway (server)       |

---

## 🚀 Quick Start

### Prerequisites
- Node.js 18+
- MongoDB Atlas account (connection string ready)
- Firebase project (download `serviceAccountKey.json`)

### 1. Backend Setup

```bash
cd server
cp .env.example .env
# Edit .env — fill in MONGO_URI, MAIL_USER, MAIL_PASS
# Place your Firebase serviceAccountKey.json in server/src/config/
npm install
npm run dev
```

Server starts at `http://localhost:5000`

### 2. Frontend Setup

```bash
cd client
npm install
npm run dev
```

Client starts at `http://localhost:5173`

---

## 🔐 Environment Variables

See `server/.env.example` for all required variables.

**Required:**
- `MONGO_URI` — MongoDB Atlas connection string (replace `<db_password>`)
- `FIREBASE_SERVICE_ACCOUNT_KEY` — Path to Firebase Admin SDK JSON key
- `MAIL_USER` + `MAIL_PASS` — Gmail or SendGrid credentials

**Pre-filled (yours):**
- Firebase client config — already in `client/src/firebase.ts`
- Upstash Redis URL/token — already in `.env.example`

---

## 👥 The 7 Roles

| Role              | Dashboard Route | Access Level                                   |
|-------------------|-----------------|------------------------------------------------|
| `super_admin`     | `/dashboard`    | Full platform control, fees, all users         |
| `admin`           | `/dashboard`    | Vendor onboarding, delivery, regional inventory |
| `nutritionist`    | `/dashboard`    | Consultations, BMI reports, live chat           |
| `vendor`          | `/dashboard`    | Own inventory, product management, order queue  |
| `sales_person`    | `/dashboard`    | Lead CRM, offline order entry                   |
| `delivery_partner`| `/dashboard`    | Slot booking, order queue, navigation           |
| `user`            | `/dashboard`    | Health tracker, orders, subscription            |

To promote a user, use the Super Admin dashboard → Users → Edit Role,
or PATCH `/api/users/:uid/role` with `{ role: "admin" }`.

---

## 📁 Project Structure

```
arokiam-workspace/
├── client/                    # React + Vite frontend
│   ├── src/
│   │   ├── components/        # Shared UI components
│   │   │   ├── cart/          # CartDrawer
│   │   │   ├── common/        # AuthModal, ProtectedRoute
│   │   │   ├── layout/        # Header, Footer
│   │   │   └── product/       # ProductCard
│   │   ├── context/           # AuthContext, CartContext
│   │   ├── firebase.ts        # Firebase client config
│   │   ├── pages/             # All route pages
│   │   │   └── dashboards/    # 7 role dashboards
│   │   ├── router/            # DashboardRouter (RBAC)
│   │   ├── services/          # Axios API helpers
│   │   ├── styles/            # Tailwind global CSS
│   │   └── utils/             # BMI engine, helpers
│   └── package.json
│
└── server/                    # Express backend
    ├── src/
    │   ├── config/            # DB, Firebase Admin, Mailer
    │   ├── middlewares/       # Auth verify, role guards, error handler
    │   ├── models/            # Mongoose schemas
    │   │   ├── User.model.js
    │   │   ├── Product.model.js
    │   │   ├── Order.model.js
    │   │   └── Subscription.model.js (+ HealthLog)
    │   └── routes/            # All API endpoints
    │       ├── auth.routes.js
    │       ├── product.routes.js
    │       ├── order.routes.js
    │       ├── health.routes.js
    │       ├── subscription.routes.js
    │       ├── inventory.routes.js
    │       └── user.routes.js
    ├── .env.example
    └── package.json
```

---

## 🌐 API Endpoints

```
Auth
  GET  /api/auth/me
  POST /api/auth/register
  PATCH /api/auth/profile

Products
  GET  /api/products
  GET  /api/products/top-sellers
  GET  /api/products/:id
  POST /api/products             [vendor]
  PUT  /api/products/:id         [vendor]

Orders
  POST /api/orders               [user]
  GET  /api/orders/my            [user]
  GET  /api/orders               [admin]
  GET  /api/orders/:id
  PATCH /api/orders/:id/status
  PATCH /api/orders/:id/assign-delivery [admin]

Health
  POST /api/health/bmi
  GET  /api/health/log?range=30
  POST /api/health/log

Subscriptions
  POST   /api/subscriptions
  GET    /api/subscriptions/my
  DELETE /api/subscriptions/:id

Inventory
  GET /api/inventory             [vendor]
  PUT /api/inventory/:id         [vendor]
  GET /api/inventory/forecast    [vendor]

Users (Super Admin only)
  GET   /api/users
  PATCH /api/users/:uid/role
  PATCH /api/users/:uid/suspend
```

---

## 🚢 Deployment

### Frontend → Vercel
```bash
cd client
npm run build
# Push to GitHub → connect to Vercel → auto-deploys
```

### Backend → Render / Railway
```bash
# Set environment variables in dashboard
# Build command: npm install
# Start command: npm start
```

---

## 📧 Email Setup (Gmail)

1. Enable 2FA on your Gmail account
2. Go to Google Account → Security → App Passwords
3. Generate a password for "Mail"
4. Use that 16-char password as `MAIL_PASS` in `.env`

---

## 🔥 Firebase Setup

1. Firebase Console → Project Settings → Service Accounts
2. Click "Generate new private key" → download JSON
3. Place it at `server/src/config/serviceAccountKey.json`

---

## 📞 Support

Built by the Arokiam engineering team.
Email: dev@arokiam.com
