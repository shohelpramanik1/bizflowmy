<div align="center">

# 💼 BizFlow MY

### Quotations · Invoices · Tasks · Bookings · Teams — All in one place

**A modern, multi-tenant SaaS for Malaysian freelancers, service providers and SMEs.**

[![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-blue?logo=typescript)](https://www.typescriptlang.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?logo=postgresql)](https://www.postgresql.org/)
[![Drizzle ORM](https://img.shields.io/badge/Drizzle-ORM-E4C200?logo=drizzle)](https://orm.drizzle.team/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-4.1-38BDF8?logo=tailwindcss)](https://tailwindcss.com/)
[![License](https://img.shields.io/badge/License-MIT-green)](./LICENSE)

[Live demo](#demo-credentials) · [Features](#features) · [Architecture](#architecture) · [Getting started](#getting-started) · [API](#api)

</div>

---

## ✨ Why BizFlow MY?

Running a small business in Malaysia shouldn't mean juggling WhatsApp, Excel and Word. BizFlow MY replaces all of it with one clean workspace built for the way freelancers, contractors, agencies and SMEs actually work — in **Malaysian Ringgit (MYR)**, with Malaysian addresses, states, SSM registration numbers, and configurable SST fields.

> 🎯 *The most important goal:* a business owner should register, create a quotation, convert it to an invoice, record a payment, track expenses, assign tasks, manage bookings and understand their finances — **without accounting or technical knowledge**.

---

## 🚀 Features

### 💰 Finance
- **Quotations** — auto-numbered (`QT-2026-0001`), server-calculated totals, one-click conversion to invoice
- **Invoices** — locked once payments exist, overdue detection, 3 professional PDF templates
- **Payments** — full & partial, auto status updates (paid / partially paid / overdue / cancelled)
- **Expenses** — category breakdown, receipts, feeds the Profit & Loss chart
- **Reports** — sales, invoice, expense, P&L, top customers & top products — all with CSV export

### 👥 CRM & Team
- **Customers** — Malaysian address fields, SSM / tax no, total billed / paid / outstanding / overdue per customer
- **Team** — 6 roles (owner, admin, manager, staff, accountant, sales), role-based access control
- **Team dashboard** — assigned tasks, completed, pending, upcoming bookings, sales generated per member
- **Activity log** — every financial action is audited (user · action · timestamp)

### ✅ Productivity
- **Tasks** — list view + drag-and-drop Kanban (To Do → In Progress → Review → Completed)
- **Calendar** — day / week / month views for every booking
- **Bookings** — internal bookings + a public booking page (`/book/{slug}`) customers can use
- **Notifications** — in-app feed with email-style grouping and mark-all-read

### 🔗 Sharing & Automation
- **Email outbox** — 12 transactional templates (welcome, invoice sent, overdue, booking, team invite…)
- **WhatsApp deep links** with a secure tokenised document link instead of raw sensitive data
- **Customer portal** — clients can accept / decline quotations via `/d/q/:token`
- **Automation** — accepted quote → convert prompt → invoice → follow-up task; overdue → chase task + reminder

### 🛡️ SaaS platform
- **Subscription plans** — Free (RM0), Pro (RM29), Business (RM59) — data-driven, add new plans without redeploying
- **Usage meters** — server-side enforcement of limits (invoices, quotations, customers, team members)
- **Platform admin** — dark-themed dashboard with MRR, cancellations, popular plan, user/business management, support tickets, broadcast announcements
- **Multi-tenant isolation** — every query is scoped by `businessId`; a member of Business A cannot read Business B's data

---

## 🧪 Demo credentials

Sign in at `/login`:

| Role | Email | Password |
| --- | --- | --- |
| Business owner (full data) | `demo@bizflowmy.com` | `demo12345` |
| Platform admin | `admin@bizflowmy.com` | `admin12345` |

Try it out:
- Dashboard with 5 KPI cards + 4 interactive charts: [`/app`](/app)
- Public booking page for the seeded "Kreatif Studio Enterprise": [`/book/kreatif-studio`](/book/kreatif-studio)

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────┐
│  Next.js 16 (App Router) — SSR + server actions        │
│                                                          │
│  /app/*     — authenticated multi-tenant workspace       │
│  /admin     — SaaS admin dashboard                       │
│  /book/[slug]  — public booking page                     │
│  /d/[type]/[token]  — customer portal                    │
│  /api/*     — REST endpoints (auth, resources, reports)  │
│  /          — marketing landing page                     │
│                                                          │
├──────────────────────────────────────────────────────────┤
│  Security: scrypt passwords · server-side sessions       │
│  RBAC matrix · Zod validation · rate limiting            │
├──────────────────────────────────────────────────────────┤
│  PostgreSQL 16 + Drizzle ORM (26 tables, multi-tenant)   │
│  Money = integer cents · Percentages = basis points      │
└──────────────────────────────────────────────────────────┘
```

### Stack

| Layer | Choice |
| --- | --- |
| Frontend | Next.js 16 (App Router), React 19, Tailwind CSS 4 |
| Backend | Next.js Route Handlers + Server Components |
| Database | PostgreSQL 16 with Drizzle ORM |
| Auth | Scrypt password hashing, server-side sessions, HTTP-only cookies |
| Validation | Zod schemas on every API route |
| Charts | Custom inline SVG (zero dependencies) |
| PDF | Browser print CSS on server-rendered A4 document templates |

---

## 📦 Project structure

```
src/
├── app/
│   ├── page.tsx                    # Marketing landing page
│   ├── login, register, onboarding # Auth flow
│   ├── app/                        # Multi-tenant workspace
│   │   ├── page.tsx                # Dashboard
│   │   ├── customers, products, quotations, invoices
│   │   ├── payments, expenses, tasks, bookings, calendar
│   │   ├── team, reports, documents, notifications
│   │   ├── settings, subscription
│   │   └── layout.tsx              # App shell (sidebar, search, user menu)
│   ├── admin/                      # SaaS admin dashboard
│   ├── book/[slug]/                # Public booking page
│   ├── d/[type]/[token]/           # Customer portal
│   ├── help, privacy, terms        # Support + legal
│   └── api/                        # REST API routes
│       ├── auth/                   # register, login, logout
│       ├── businesses/             # workspace CRUD + switch
│       ├── customers, products, quotations, invoices
│       ├── payments, expenses, tasks, bookings
│       ├── team, documents, notifications
│       ├── reports/, share/, reminders/
│       ├── subscription/           # plan management
│       ├── admin/                  # platform admin actions
│       └── public/[slug]/          # public booking endpoint
├── components/
│   ├── app-shell.tsx               # Collapsible sidebar, global search, user menu
│   ├── ui.tsx                      # Buttons, inputs, modals, toasts
│   ├── charts.tsx                  # Bar, Line, GroupedBar, Donut (pure SVG)
│   ├── document-form.tsx           # Quotation / invoice creator
│   ├── document-preview.tsx        # A4 PDF template
│   ├── document-actions.tsx        # Share, WhatsApp, convert, record payment
│   ├── document-list.tsx           # Filterable paginated table
│   └── resource-manager.tsx        # Generic CRUD grid for customers/products/…
├── db/
│   ├── schema.ts                   # 26 Drizzle tables
│   └── index.ts                    # Singleton pool + drizzle client
└── lib/
    ├── auth.ts                     # Password hashing, session helpers, RBAC context
    ├── rbac.ts                     # Role → capability matrix
    ├── api.ts                      # Error handling, pagination, rate limit, parseBody
    ├── documents.ts                # Totals engine, document loading, number generation
    ├── status.ts                   # Status labels/tones (client-safe)
    ├── reports.ts                  # Financial summary, monthly series, CSV export
    ├── plans.ts                    # Plans, limits, usage, server-side enforcement
    ├── messaging.ts                # Email templates + WhatsApp URL builder
    ├── activity.ts                 # Activity log + notification helpers
    ├── schemas.ts                  # Shared Zod schemas
    └── format.ts                   # Money, dates, MYR formatting, MY states
```

---

## 🛠️ Getting started

### 1. Clone and install

```bash
git clone https://github.com/your-org/bizflow-my.git
cd bizflow-my
npm install
```

### 2. Configure the database

Copy the example env and edit:

```bash
cp .env.example .env
```

```bash
# .env
DATABASE_URL=postgresql://postgres:postgres@127.0.0.1:5432/bizflow_my
```

### 3. Push the schema and seed demo data

```bash
npx drizzle-kit push
npx tsx scripts/seed.ts
```

The seed creates:
- Two users (`demo@bizflowmy.com` / `demo12345` and the admin)
- A business called **Kreatif Studio Enterprise** with 3 team members
- 5 customers, 6 products, 5 quotations, 9 invoices with mixed payment statuses
- 11 expenses, 8 tasks, 4 bookings, notifications and activity logs

### 4. Run the app

```bash
npm run dev
```

Open [`http://localhost:3000`](http://localhost:3000) and sign in with the demo credentials.

### 5. Typecheck and build

```bash
npm run typecheck
npm run build
npm start
```

---

## 🔐 Environment variables

| Variable | Required | Default | Description |
| --- | --- | --- | --- |
| `DATABASE_URL` | ✅ | — | PostgreSQL connection string |
| `NEXT_PUBLIC_SITE_URL` | — | `https://bizflowmy.com` | Used for sitemap, OG tags, public links |

> 🔒 All API keys for transactional email, storage and payment gateways are read from `process.env` inside server routes and never bundled to the client.

---

## 🗄️ Database highlights

**Multi-tenant by design.** Every business-facing table has a `business_id` column; every query in the API is filtered by the signed-in user's `businessId`. A member of Business A can never touch Business B's data.

**Money safety.** All monetary columns are `integer` (cents/sen). Percentages use basis points (`600 = 6.00%`). The totals engine in [`src/lib/documents.ts`](src/lib/documents.ts) runs **exclusively server-side** — the client preview is a visual hint, the invoice is saved with the server-computed numbers.

**Audit trail.** The [`activity_logs`](src/db/schema.ts) table records every financial action (user · action · entity · timestamp · JSON metadata). Financial documents are soft-archived, not hard-deleted.

**Plan enforcement.** Every `POST` that creates a resource runs `assertWithinLimit(businessId, resource)` and rejects with HTTP `402` when the plan limit is hit.

### Core tables (26)

`users` · `businesses` · `business_settings` · `subscriptions` · `subscription_plans` · `memberships` · `sessions` · `customers` · `products` · `quotations` · `quotation_items` · `invoices` · `invoice_items` · `payments` · `expenses` · `projects` · `tasks` · `bookings` · `documents` · `notifications` · `activity_logs` · `email_logs` · `support_tickets` · `announcements` · `analytics_events`.

Full schema: [`src/db/schema.ts`](src/db/schema.ts).

---

## 📡 API

The API is a RESTful surface under `/api/*`. Every route:

1. Parses the body with a Zod schema.
2. Calls `requireBusiness("quotations.write")` — which checks the session, the tenant, and the role capability.
3. Runs the business logic.
4. Writes an activity log, notifies affected users, and tracks the analytics event.
5. Returns `{ ok: true, data }` or a `{ ok: false, error: { message, code } }` envelope — never a stack trace.

### Key endpoints

| Endpoint | Methods | Auth | Description |
| --- | --- | --- | --- |
| `/api/auth/register` | POST | public | Create user + first business |
| `/api/auth/login` / `/logout` | POST | public / session | Session management |
| `/api/businesses` | GET · POST · PATCH | session | Current workspace CRUD |
| `/api/businesses/switch` | POST | session | Switch active workspace |
| `/api/customers` | GET · POST | `customers.*` | Paginated list + create |
| `/api/customers/[id]` | GET · PATCH · DELETE | `customers.read/write` | Detail + archive |
| `/api/products` | GET · POST | `products.*` | Items catalog |
| `/api/quotations` | GET · POST | `quotations.*` | List + create (enforces plan limit) |
| `/api/quotations/[id]` | GET · PATCH · DELETE | `quotations.*` | Detail + status update |
| `/api/quotations/[id]/convert` | POST | `invoices.write` | **Convert to invoice** (preserves the original) |
| `/api/invoices/[id]` | GET · PATCH · DELETE | `invoices.*` | Locks line items if payments exist |
| `/api/payments` | GET · POST | `payments.*` | Rejects overpayments by default |
| `/api/expenses` | GET · POST | `expenses.*` | Category filter, date range |
| `/api/tasks` | GET · POST | `tasks.*` | Staff see only their own |
| `/api/bookings` | GET · POST | `bookings.*` | Internal + public bookings |
| `/api/team` | GET · POST | `team.*` | Enforces `team` plan limit |
| `/api/reports` | GET | `reports.read` | Financial summary + monthly series |
| `/api/reports/export` | GET | `reports.read` | CSV download |
| `/api/share` | POST | session | Queue email / WhatsApp share |
| `/api/reminders` | GET · POST | `invoices.write` | Sweep overdue invoices |
| `/api/subscription` | GET · POST | `billing.write` | Plan change / cancel |
| `/api/search` | GET | session | Global search (customers, invoices, quotes, tasks, bookings) |
| `/api/notifications` | GET · PATCH | session | List + mark-all-read |
| `/api/admin` | GET · POST | **platform admin** | SaaS operations |
| `/api/public/[slug]` | GET · POST | public | Public booking page (rate-limited) |
| `/api/d/[token]` | POST | public | Customer accepts / declines a quotation |
| `/api/health` | GET | public | Liveness check |

---

## ⚖️ Business rules enforced server-side

These are not configurable by the client and are baked into the API:

1. A quotation / invoice / customer always belongs to a business.
2. Users can only access records belonging to their own business.
3. Invoice totals are computed server-side (no client rounding drift).
4. Invoices with payments cannot silently change line items.
5. Payments cannot exceed the invoice balance unless explicitly allowed.
6. Overdue status is derived automatically from `dueDate < today`.
7. Quotation → Invoice conversion preserves the original quotation.
8. Financial documents are archived, not hard-deleted.
9. Every important financial action writes to `activity_logs`.
10. Subscription limits are enforced in the API, not the UI.

---

## 🇲🇾 Tax & Malaysian compliance

Tax is **fully configurable** — nothing is hard-coded.

- Toggle tax on/off per business.
- Change the label (SST / Service Tax / GST / none).
- Change the default rate (basis points).
- Overwrite per product and per line item.

> ⚠️ **Disclaimer:** BizFlow MY does not provide tax or legal advice. Tax fields are administrator-configurable and must be verified against current Malaysian requirements (LHDN, RMCD / SST regulations, upcoming e-invoicing mandate) before issuing any document.

---

## 🧩 Extending

### Add a new subscription plan

Plans are data rows in [`subscription_plans`](src/db/schema.ts). Insert a new row and it immediately becomes available in the Settings → Subscription UI — no code change required.

### Add a new payment gateway

The `paymentGateway` field is an enum string with a `gatewayConfig` JSON blob. Add a new value (e.g. `molpay`, `stripe`) and implement a handler under `src/app/api/webhooks/` — the document rendering layer doesn't change.

### Add a new email template

Templates live in [`src/lib/messaging.ts`](src/lib/messaging.ts). Return `{ subject, body }` and queue through the `email_logs` outbox. In production, swap the outbox worker for a Resend / Postmark / SES driver.

---

## 📄 License

MIT © 2026 BizFlow MY contributors. See [`LICENSE`](./LICENSE).

---

<div align="center">

Built with ❤️ in Malaysia · [Start for free](#demo-credentials)

</div>
