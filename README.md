<div align="center">

# 💎 Nakhrali

### Full-stack e-commerce platform for a Pune-born D2C fashion jewellery brand

#### *Designed, built, deployed, and maintained — solo. Live in production.*

[![Live Site](https://img.shields.io/badge/🌐_Live_Site-nakhraliluxury.com-c9a961?style=for-the-badge&labelColor=1a1a1a)](https://nakhraliluxury.com)
[![Status](https://img.shields.io/badge/Status-In_Production-22c55e?style=for-the-badge&labelColor=1a1a1a)](https://nakhraliluxury.com)
[![Stack](https://img.shields.io/badge/Stack-React_19_·_Express_5_·_PostgreSQL-3b82f6?style=for-the-badge&labelColor=1a1a1a)]()
[![Solo](https://img.shields.io/badge/Team-Solo_Engineer-a855f7?style=for-the-badge&labelColor=1a1a1a)]()

### **[👉 Visit the live site](https://nakhraliluxury.com)** &nbsp;·&nbsp;
</div>

---

## 📌 What is this repository?

This is a **public showcase** of a real, paid, production e-commerce build for **Nakhrali** — a Pune-born D2C fashion jewellery brand currently serving customers across India.

> 🔒 The actual source code lives in a **private repository** under client confidentiality.
> Recruiters and hiring managers can request read access via email below — happy to walk through the codebase on a call.

> *"Nakhrali is a brand devoted to the Indian woman — designed for confidence, expression, and everyday elegance. Born in Pune, we are a D2C fashion jewellery brand that transforms everyday adornment into timeless stories."*

---

## ⚡ TL;DR

| | |
|:---|:---|
| **What** | Full-stack e-commerce platform — storefront, custom hamper builder, admin dashboard, payments, analytics |
| **Who** | Built end-to-end as the **solo engineer** for Nakhrali (Pune, India) |
| **Stack** | React 19 · Vite · Tailwind · Express 5 · PostgreSQL · JWT · Google OAuth |
| **Hosting** | GoDaddy (frontend) · Render (backend) · Supabase (database) · Cloudflare (CDN) |
| **Scale** | ~14,000 lines of code · 14 frontend routes · 9 backend resources · 9 DB migrations |
| **Status** | Live, actively maintained, on retainer for new features |

---

## ✨ Why this build is different

Most "e-commerce projects" you see on GitHub are tutorials or starter kits. **This one is shipping orders.** Real customers. Real money. Real client running merchandising daily without engineering involvement.

Highlights that make it stand out:

- 🎁 **Custom hamper builder** — interactive gift-box configurator with live pricing and stock validation. *Not a feature you see in starter projects.*
- 🤖 **One-click product import scraper** — admin pastes an Amazon or Flipkart URL → backend uses `axios` + `cheerio` to extract title, description, price, and all images automatically. **Cuts product onboarding from 10 minutes per SKU to 10 seconds.**
- 🛠️ **Self-serve admin dashboard** — non-technical client manages 100% of merchandising (products, banners, hampers, occasions, orders) without ever touching code.
- 💳 **Razorpay live payments** — UPI, cards, netbanking, and wallets, with **server-side HMAC-SHA256 signature verification** so the frontend can never lie about payment success.
- 🔐 **Production-grade security** — Helmet, rate limiting, XSS sanitization, parameterized queries, bcrypt 12-rounds, OWASP Top 10 hardened.
- 🚀 **Sub-second LCP** on mobile via Cloudflare edge cache, image polish, code-splitting, and preconnect hints.

---

## 🧱 Tech Stack

### Frontend
![React](https://img.shields.io/badge/React_19-61dafb?style=flat-square&logo=react&logoColor=000)
![Vite](https://img.shields.io/badge/Vite_7-646cff?style=flat-square&logo=vite&logoColor=fff)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-06b6d4?style=flat-square&logo=tailwindcss&logoColor=fff)
![Router](https://img.shields.io/badge/React_Router_v7-ca4245?style=flat-square&logo=reactrouter&logoColor=fff)
![Swiper](https://img.shields.io/badge/Swiper-6332f6?style=flat-square&logo=swiper&logoColor=fff)

### Backend
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=fff)
![Express](https://img.shields.io/badge/Express_5-000?style=flat-square&logo=express&logoColor=fff)
![JWT](https://img.shields.io/badge/JWT-000?style=flat-square&logo=jsonwebtokens&logoColor=fff)
![bcrypt](https://img.shields.io/badge/bcrypt-525252?style=flat-square)
![Helmet](https://img.shields.io/badge/Helmet-CC3534?style=flat-square)

### Data & Infrastructure
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169e1?style=flat-square&logo=postgresql&logoColor=fff)
![Supabase](https://img.shields.io/badge/Supabase-3ecf8e?style=flat-square&logo=supabase&logoColor=fff)
![Render](https://img.shields.io/badge/Render-46e3b7?style=flat-square&logo=render&logoColor=000)
![GoDaddy](https://img.shields.io/badge/GoDaddy-1bdbdb?style=flat-square&logo=godaddy&logoColor=000)
![Cloudflare](https://img.shields.io/badge/Cloudflare-f38020?style=flat-square&logo=cloudflare&logoColor=fff)

### Integrations
![Google OAuth](https://img.shields.io/badge/Google_OAuth-4285f4?style=flat-square&logo=google&logoColor=fff)
![Razorpay](https://img.shields.io/badge/Razorpay-02042b?style=flat-square&logo=razorpay&logoColor=fff)
![Meta Pixel](https://img.shields.io/badge/Meta_Pixel-1877f2?style=flat-square&logo=meta&logoColor=fff)

---

## 🏗️ Architecture

```
       ┌──────────────────────────────────────────────────────────────┐
       │                        nakhraliluxury.com                    │
       │                            (Cloudflare DNS + CDN)            │
       └──────────────────────────────────────────────────────────────┘
                                     │
                ┌────────────────────┴────────────────────┐
                ▼                                         ▼
     ┌──────────────────────┐                ┌──────────────────────┐
     │   React + Vite SPA   │  ──── HTTPS ──►│   Express 5 API      │
     │   (GoDaddy cPanel)   │   JSON / JWT   │   (Render, autoscale)│
     │                      │◄──── 200 ──────│                      │
     │   • Tailwind UI      │                │   • REST endpoints   │
     │   • Code-split       │                │   • Auth middleware  │
     │   • Lazy routes      │                │   • Rate limiting    │
     │   • Cloudflare cache │                │   • Helmet + CSP     │
     └──────────────────────┘                └──────────┬───────────┘
                                                        │
                              ┌─────────────────────────┼─────────────────────────┐
                              ▼                         ▼                         ▼
                    ┌──────────────────┐      ┌──────────────────┐      ┌──────────────────┐
                    │   PostgreSQL     │      │  Razorpay API    │      │  Google OAuth    │
                    │   (Supabase,     │      │  (Payments)      │      │  (Auth)          │
                    │   containerized) │      │                  │      │                  │
                    └──────────────────┘      └──────────────────┘      └──────────────────┘
```

**Why this shape:**
- **GoDaddy + Render** keeps hosting cost predictable for an early-stage D2C brand (no surprise serverless bills)
- **Supabase** gives a managed Postgres in a Docker container — no public DB exposure, only the API speaks to it
- **Cloudflare** eats the brunt of media delivery and DDoS at the edge; origin only handles dynamic API calls
- **JWT + bcrypt** keeps the API stateless and horizontally scalable when the brand outgrows a single Render instance

---

## 🎯 Key Features

<table>
<tr>
<td width="50%" valign="top">

### 🛍️ Customer-facing
- Curated category storefronts: necklaces, mangalsutras, bangles, earrings, hair accessories
- **Custom hamper builder** with drag-and-drop SKU selection
- Pre-curated hamper suggestions by occasion
- Multi-image product detail with zoom
- Persistent cart and wishlist
- **Multi-address book** with default address
- **Razorpay checkout** — UPI, cards, netbanking, wallets, plus COD
- Order history and order tracking
- Email/password + Google OAuth login
- "Remember me" with smart `localStorage` ↔ `sessionStorage` switching
- Mobile-first responsive UI (Tailwind)

</td>
<td width="50%" valign="top">

### 🛠️ Admin
- Full **product CRUD** with multi-image upload
- **One-click product scraper** — paste an Amazon/Flipkart URL, get title/description/price/images auto-filled
- **Hamper composer** — bundle SKUs into curated gift sets
- **Occasion-based collection builder** with image banners
- **Banner scheduler** with redirect links
- **Order management** with status updates
- **Real-time metrics dashboard** — revenue, users, orders, top products
- Email-whitelisted admin RBAC
- Profile picture management

</td>
</tr>
</table>

---

## 🔍 Engineering Deep-Dives

<details>
<summary><b>🤖 The Amazon/Flipkart product scraper — turning a 10-minute task into 10 seconds</b></summary>
<br>

**Problem:** The client adds 5–10 SKUs per week. Each one requires manually typing the title, copying the description, formatting the price, and downloading 4–6 product images. That's ~10 minutes of tedious data entry per SKU.

**Solution:** Built an admin-only scraper endpoint. Admin pastes any Amazon or Flipkart URL → server fetches the page with a real-browser `User-Agent` → `cheerio` parses the DOM → site-specific selectors extract:

- Title (`#productTitle` for Amazon, `_35KyD6` for Flipkart)
- Description (feature bullets joined / product description block)
- Price (handles fractional `.a-price-whole` + `.a-price-fraction` reassembly)
- All images including thumbnails from the gallery (`#altImages img`, `.imageThumbnail img`)

The admin form pre-fills with extracted data, admin reviews and clicks save. **Onboarding time dropped from 10 minutes to 10 seconds per SKU.**

```js
// Simplified flow
const response = await axios.get(url, { headers: { 'User-Agent': '...' }, timeout: 10000 });
const $ = cheerio.load(response.data);

if (url.includes('amazon.')) {
  productData.name = $('#productTitle').text().trim();
  productData.description = $('#feature-bullets ul li').map((i, el) => $(el).text().trim()).get().join(' ');
  // ... price reassembly, image gallery extraction
}
```

</details>

<details>
<summary><b>🔎 Search — fast, ranked, multi-field, zero infrastructure overhead</b></summary>
<br>

The catalog is small enough (hundreds of SKUs, growing) that pulling in Elasticsearch would be **expensive overkill**. Instead, search runs entirely on Postgres:

- Multi-field `LIKE` matching across `name`, `description`, `category`
- **JSONB tag-array search** using `jsonb_array_elements_text` for tag matching
- **Manual relevance ranking** via `CASE WHEN` — exact prefix matches rank first, then substring matches, then featured products, then newest
- Returns products + matching categories + tag-based suggestions in a single round-trip

```sql
ORDER BY 
  CASE 
    WHEN LOWER(name) LIKE $2 THEN 1   -- exact prefix
    WHEN LOWER(name) LIKE $1 THEN 2   -- substring
    ELSE 3
  END,
  featured DESC,
  created_at DESC
LIMIT 10
```

Result: search-as-you-type latency under 50ms, zero new infrastructure, zero new monthly bills.

</details>

<details>
<summary><b>🔐 Database security — containerized, never publicly addressable</b></summary>
<br>

The database stores customer addresses, order history, and PII. Direct internet exposure was never an option.

- Postgres lives in a **Supabase-managed Docker container**
- Only the Render-hosted Express API holds the connection string
- Connection string injected via Render env vars — never in code, never in git
- **Parameterized queries everywhere** — no string concatenation in SQL
- `express-mongo-sanitize` and `xss-clean` middleware on every request
- **9 versioned SQL migrations** tracked in `backend/migrations/`

</details>

<details>
<summary><b>💳 Payment trust — Razorpay with server-side signature verification</b></summary>
<br>

E-commerce frontends can lie about payment success. Naive integrations confirm orders client-side and lose money to tampering. The Razorpay integration here is structured to make that impossible:

1. Order is created **server-side** by hitting `api.razorpay.com/orders` with the keyed secret — frontend never sees the secret
2. Razorpay Checkout opens on the client; on success, the SDK returns `razorpay_order_id`, `razorpay_payment_id`, `razorpay_signature`
3. Backend recomputes `HMAC-SHA256(order_id + "|" + payment_id, KEY_SECRET)` and **constant-time compares** it to the signature
4. **Only on signature match** is the order status flipped to `paid` in the DB and the cart cleared

Supported methods on live: **UPI · Cards · Netbanking · Wallets · COD** (COD is handled separately, no payment gateway round-trip).

The client UI never has authority to confirm a payment.

</details>

<details>
<summary><b>🛡️ OWASP Top 10 hardening</b></summary>
<br>

| Control | Implementation |
|---|---|
| **Auth** | JWT 1h expiry · bcrypt 12 rounds · email-whitelist admin RBAC |
| **Input** | Server-side validation on every endpoint · HTML tag stripping · parameterized SQL |
| **Network** | `express-rate-limit` (100 req/10min general, 5 req/15min auth) · CORS allowlist |
| **Headers** | Helmet (strict CSP, HSTS, X-Frame-Options DENY, X-Content-Type-Options) |
| **Injection** | `xss-clean` · `express-mongo-sanitize` · `hpp` |
| **Secrets** | All credentials in env vars · `.env` git-ignored from day one · GitHub Secret Scanning enabled |
| **Database** | Containerized · no public connection · parameterized queries everywhere |

</details>

<details>
<summary><b>⚡ Performance — sub-second LCP on 4G mobile</b></summary>
<br>

- **Code splitting** via Vite into `react-vendor`, `ui-vendor`, and app bundles
- **Cloudflare edge cache** for static assets and product images
- **Image polish** via Cloudflare auto-optimization
- **Preconnect hints** to fonts and API origin in `<head>` — shaves ~200ms off cold-cache loads
- **Lazy-loaded routes** for non-critical pages
- **CSP-compliant async font loading** — fonts never block first paint
- Cloudflare Insights for RUM monitoring

</details>

---

## 📊 At a Glance

<div align="center">

| 🧑‍💻 **Team size** | 📅 **Timeline** | 🚦 **Status** | 📄 **Scale** | 🔒 **Security issues at launch** |
|:---:|:---:|:---:|:---:|:---:|
| Solo (FE + BE + DB + ops) | ~2 months to prod | Live & maintained | ~14k LOC · 14 routes · 9 resources | 0 critical |

</div>

---

## 🧑‍💻 About me

Hi, I'm **Shubh Ghiya** — a full-stack engineer pursuing my **BS in Data Science at IIT Madras**.

I built Nakhrali end-to-end as a paid client engagement: requirements, architecture, frontend, backend, database design, deployment, security, monitoring, and ongoing maintenance. The brand is live, the client runs the store independently, and I'm still on retainer for new features.

**What I'm looking for:** Full-stack / backend roles where I can ship production systems end-to-end. SDE / Software Engineer roles. Startups especially welcome.

### 📬 Get in touch

[![Email](https://img.shields.io/badge/Email-ghiyashubh23@gmail.com-d44638?style=for-the-badge&logo=gmail&logoColor=fff)](mailto:ghiyashubh23@gmail.com)
[![Live Site](https://img.shields.io/badge/See_it_live-nakhraliluxury.com-c9a961?style=for-the-badge)](https://nakhraliluxury.com)


> 🔑 **Hiring teams:** request **read access to the private source repo** by email — happy to do a code walkthrough on a call.

---

<div align="center">
<sub>© Nakhrali. Code proprietary. This README is a public case study published with permission of the brand.</sub>
</div>

</div>

