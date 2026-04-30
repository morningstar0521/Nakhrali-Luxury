<div align="center">

# 💎 Nakhrali Fashion

### Full-stack e-commerce platform for a Pune-based D2C fashion jewellery brand

[![Live](https://img.shields.io/badge/Live%20Site-nakhraliluxury.com-c9a961?style=for-the-badge)](https://nakhraliluxury.com)
[![Status](https://img.shields.io/badge/Status-Production-success?style=for-the-badge)](https://nakhraliluxury.com)
[![Stack](https://img.shields.io/badge/Stack-React%20%7C%20Express%20%7C%20Postgres-blue?style=for-the-badge)]()

**[🌐 Visit Live Site →](https://nakhraliluxury.com)**

</div>

---

## 📌 About This Repository

This is a **public showcase** of my work on **Nakhrali** — a Pune-born D2C fashion jewellery brand designed for the modern Indian woman. I built the entire e-commerce platform solo, end-to-end, and it's live in production.

> *"Nakhrali is a brand devoted to the Indian woman—designed for confidence, expression, and everyday elegance. Born in Pune, we are a D2C fashion jewellery brand that transforms everyday adornment into timeless stories."*

> 🔒 The actual source code is in a **private repository** under client confidentiality.
> Hiring teams and recruiters can request read access via email below.

---

## ✨ What I Built

A complete D2C e-commerce stack — frontend, backend, database, deployment, security, and ongoing maintenance — as the **sole engineer**.

- 🛍️ Storefront with curated category pages (necklaces, mangalsutras, bangles, earrings, hair accessories) plus search, filters, wishlist, and cart
- 🎁 **Custom hamper builder** — interactive gift box configurator with live pricing
- 💳 **Razorpay** payment integration with server-verified order signatures (UPI, cards, netbanking, wallets)
- 🔐 **Dual auth** — email/password (bcrypt) + Google OAuth
- 🛠️ **Admin dashboard** — non-technical client manages 100% of merchandising
- 📊 **Meta Pixel** for ad attribution and Cloudflare for CDN/caching
- 🛡️ **OWASP Top 10 hardened** with rate limiting, CSP, input sanitization

---

## 🧱 Tech Stack

| Layer | Stack |
|---|---|
| **Frontend** | React 19 · Vite 7 · Tailwind CSS · React Router v7 · Swiper |
| **Backend** | Node.js · Express 5 · JWT · bcrypt |
| **Database** | PostgreSQL (Supabase, containerized) |
| **Auth** | JWT + Google OAuth (`google-auth-library`) |
| **Payments** | Razorpay Checkout |
| **Hosting** | Vercel (frontend) · Render (backend) |
| **CDN / Images** | Cloudflare |
| **Analytics** | Meta Pixel · Cloudflare Insights |
| **Security** | Helmet · `xss-clean` · `express-rate-limit` · `hpp` · `express-mongo-sanitize` |

---

## 🏗️ Architecture

```
┌─────────────────┐      HTTPS       ┌──────────────────┐
│  React + Vite   │  ───────────►    │  Express API     │
│  (Vercel)       │                  │  (Render)        │
└─────────────────┘                  └────────┬─────────┘
        │                                      │
        ▼                                      ▼
┌─────────────────┐                  ┌──────────────────┐
│  Cloudflare CDN │                  │  PostgreSQL      │
│  (Images/SSL)   │                  │  (Supabase)      │
└─────────────────┘                  └──────────────────┘
                                               │
                                               ▼
                                     ┌──────────────────┐
                                     │  Razorpay API    │
                                     └──────────────────┘
```

- Static frontend on Vercel's edge network
- Express API on Render with autoscaling
- Postgres in a Supabase-managed Docker container — never internet-exposed; only the API speaks to it
- Cloudflare in front of media for cache, image polish, and DDoS protection
- Razorpay payments verified server-side via HMAC-SHA256 to prevent client-side tampering

---

## 🎯 Key Engineering Decisions

<details>
<summary><b>🔐 Database security — containerized, never publicly addressable</b></summary>

The brand stores customer addresses, orders, and PII for shoppers across India. Putting Postgres directly on the public internet was a non-starter. The DB lives in a Supabase-managed Docker container reachable only by the Express API. Connection string is injected via Render env vars; parameterized queries everywhere — no string concatenation in SQL.
</details>

<details>
<summary><b>💳 Payment trust — server-side signature verification</b></summary>

Naive integrations confirm orders client-side and lose money to tampering. Here, Razorpay orders are created server-side, payment signatures verified using HMAC-SHA256 against the keyed secret, and only then is order status flipped to `paid` in the database.
</details>

<details>
<summary><b>⚡ Image delivery — Cloudflare polish + edge cache</b></summary>

Fashion jewellery needs hi-res photography, but the brand's audience shops on mobile across patchy 4G. Cloudflare sits in front of all media with auto-optimization, polish, and edge caching. Preconnect hints to font and API origins shave ~200ms off cold-cache loads.
</details>

<details>
<summary><b>🛠️ Admin workflow — zero engineering for merchandising</b></summary>

The client adds new SKUs, banners, and gift hampers weekly. Pushing code per change is unsustainable. Built a full admin dashboard — product CRUD with multi-image upload, hamper composer, occasion tags, banner scheduler. Client manages 100% of the storefront without touching code.
</details>

<details>
<summary><b>🛡️ OWASP Top 10 hardening</b></summary>

Helmet with strict CSP · `express-rate-limit` (100 req/10min general, 5 req/15min auth) · `xss-clean` and `hpp` middleware · server-side validation on every endpoint · bcrypt at 12 rounds · JWT 1h expiry · email-whitelisted admin RBAC.
</details>

---

## 📊 At a Glance

<div align="center">

| | |
|:---:|:---:|
| **🧑‍💻 Team size** | Solo (frontend + backend + DB + ops) |
| **📅 Timeline** | ~2 months to production |
| **🚦 Status** | Live, actively maintained |
| **📄 Routes** | 14 frontend pages · 9 backend resources |
| **🔒 Critical security issues at launch** | 0 |

</div>

---

## 🤔 What I'd Do Differently

- **TypeScript from day one** — refactor cost grows with surface area
- **httpOnly cookies for JWT** instead of localStorage — better default for production
- **Webhook-driven order finalization** — more resilient to client disconnects than synchronous verify
- **PgBouncer** for Postgres connection pooling before the next traffic spike

---

## 📬 Contact

Built and maintained by **Shubh Ghiya** · Full-stack engineer

📧 **Email:** [ghiyashubh23@gmail.com](mailto:ghiyashubh23@gmail.com)
🌐 **Live site:** [nakhraliluxury.com](https://nakhraliluxury.com)

> 🔑 **For source code access** (recruiters, hiring teams, collaborators) — please reach out via email. Happy to walk through the codebase on a call.

---

<div align="center">
<sub>© Nakhrali Fashion. Code proprietary. Case study published with permission.</sub>
</div>

