# Nakhrali Luxury

A production e-commerce platform for [Nakhrali](https://nakhraliluxury.com), a Pune-born direct-to-consumer fashion jewellery brand serving customers across India. Designed, built, deployed, and maintained as a solo engineer.

**Live site:** [nakhraliluxury.com](https://nakhraliluxury.com)

[![Live](https://img.shields.io/badge/status-live%20in%20production-22c55e?style=flat-square&labelColor=1a1a1a)](https://nakhraliluxury.com)
[![Stack](https://img.shields.io/badge/stack-React%2019%20·%20Express%205%20·%20PostgreSQL-3b82f6?style=flat-square&labelColor=1a1a1a)]()
[![Team](https://img.shields.io/badge/team-solo%20engineer-a855f7?style=flat-square&labelColor=1a1a1a)]()
[![License](https://img.shields.io/badge/source-private%20(showcase%20only)-525252?style=flat-square&labelColor=1a1a1a)]()

---

## About this repository

This repository is a public case study, not the source code. The actual application is a paid client engagement and lives in a private repository under a confidentiality agreement.

Hiring teams and recruiters can request read access to the private repo by email — I'm happy to walk through the codebase on a call.

---

## Screenshots

> Images live in [`docs/screenshots/`](docs/screenshots/). Captured from production at nakhraliluxury.com.

| Storefront | Product detail |
| :---: | :---: |
| ![Storefront](docs/screenshots/01-storefront.png) | ![Product detail](docs/screenshots/02-product-detail.png) |
| **Hamper builder** | **Checkout** |
| ![Hamper builder](docs/screenshots/03-hamper-builder.png) | ![Checkout](docs/screenshots/04-checkout.png) |
| **Admin dashboard** | **Product scraper** |
| ![Admin dashboard](docs/screenshots/05-admin-dashboard.png) | ![Product scraper](docs/screenshots/06-product-scraper.png) |

---

## Summary

| | |
| :--- | :--- |
| **What it is** | Full-stack e-commerce platform — storefront, custom hamper builder, admin dashboard, payments, analytics |
| **Who built it** | Solo end-to-end (frontend, backend, database, infrastructure, security, ongoing maintenance) |
| **Stack** | React 19, Vite, Tailwind, Express 5, PostgreSQL, JWT, Google OAuth |
| **Hosting** | GoDaddy (frontend), Render (backend), Supabase (database), Cloudflare (CDN) |
| **Scale** | ~14,000 lines of code, 14 frontend routes, 9 backend resources, 9 database migrations |
| **Status** | Live, actively maintained, on retainer for new features |

---

## What makes this project worth a look

Most e-commerce projects on GitHub are tutorials or starter kits. This one is processing real orders, taking real payments, and is run day-to-day by a non-technical client.

The pieces I'm proudest of:

- **Custom hamper builder.** An interactive gift-box configurator with live pricing and stock validation. Not something you find in starter projects.
- **One-click product import.** The admin pastes an Amazon or Flipkart URL and the backend scrapes title, description, price, and all gallery images. Cuts SKU onboarding from roughly ten minutes per product to about ten seconds.
- **Self-serve admin dashboard.** The client manages all merchandising — products, banners, hampers, occasions, orders — without ever touching code.
- **Razorpay live payments.** UPI, cards, netbanking, wallets, and COD, with server-side HMAC-SHA256 signature verification so the frontend can never lie about payment success.
- **Production-grade security baseline.** Helmet, rate limiting, XSS sanitization, parameterized queries, bcrypt at 12 rounds, and an OWASP Top 10 hardening pass.
- **Sub-second LCP on mobile** via Cloudflare edge caching, image optimization, code splitting, and preconnect hints.

---

## Architecture

```mermaid
flowchart TB
    user([Customer / Admin])
    cf[Cloudflare<br/>DNS · CDN · WAF]
    fe[React 19 + Vite SPA<br/>GoDaddy cPanel]
    be[Express 5 API<br/>Render · autoscale]
    db[(PostgreSQL<br/>Supabase · containerized)]
    rzp[Razorpay<br/>Payments]
    gauth[Google<br/>OAuth 2.0]
    meta[Meta Pixel<br/>Analytics]

    user -->|HTTPS| cf
    cf --> fe
    fe -->|JSON / JWT| be
    be --> db
    be --> rzp
    be --> gauth
    fe -.->|client events| meta

    classDef edge fill:#f38020,stroke:#1a1a1a,color:#fff;
    classDef app fill:#3b82f6,stroke:#1a1a1a,color:#fff;
    classDef data fill:#4169e1,stroke:#1a1a1a,color:#fff;
    classDef ext fill:#525252,stroke:#1a1a1a,color:#fff;
    class cf edge;
    class fe,be app;
    class db data;
    class rzp,gauth,meta ext;
```

The frontend is a static React build served from GoDaddy and accelerated by Cloudflare. The Express API runs on Render and is the only client of the database. The database itself is never publicly addressable — it lives in a Supabase-managed container reachable only via the API's connection string. Razorpay and Google OAuth are server-to-server integrations; secrets never touch the browser.

---

## Tech decisions

The interesting part of any project is what was rejected, not just what was chosen.

**Why Express 5 over Next.js.** Next.js is the default reach for "I need a website with a backend," but the brand needed clear separation: a marketing-friendly static frontend the client could move between hosts if needed, and an API I could scale or replace independently. A monolithic SSR framework would have coupled the two and made the hosting story (GoDaddy + Render) impossible. The trade-off — losing built-in SSR — was acceptable because the storefront's SEO surface is small and Cloudflare handles the performance side.

**Why Postgres on Supabase instead of a self-hosted DB.** Supabase gives a managed Postgres in a Docker container with automated backups and no public exposure. Self-hosting a database for a small D2C brand is a liability, not a feature. The application uses raw SQL with parameterized queries through `pg` — no ORM lock-in, full control over query plans, and easy to migrate off Supabase later if needed.

**Why no ORM.** Sequelize and Prisma are excellent, but the API surface here is small (nine resources) and the queries are interesting (JSONB tag search, manual relevance ranking, hamper composition). Hand-written SQL keeps the data layer transparent and lets me hit Postgres-specific features without fighting a query builder.

**Why GoDaddy for the frontend.** It's not glamorous, but the client already had the domain there and a static React build is just files. The frontend host is interchangeable — the build artifact is portable, and Cloudflare sits in front anyway. Picking the boring option saved a migration headache for the brand.

**Why Render for the backend.** Predictable monthly cost over per-request serverless billing. For a D2C brand with bursty but bounded traffic, a small always-on instance with autoscaling is cheaper and more predictable than Lambda-style billing, and avoids cold-start latency on the checkout flow.

**Why JWTs over server sessions.** The API is stateless, which means it can scale horizontally without sticky sessions or a shared session store. Refresh tokens are intentionally not implemented — short-lived (1h) JWTs with re-login on expiry is enough for the threat model and removes a class of token-rotation bugs.

**Why Postgres `LIKE` + JSONB instead of Elasticsearch.** Elasticsearch would be expensive overkill for hundreds of SKUs. Multi-field `LIKE` matching, JSONB tag search via `jsonb_array_elements_text`, and a hand-rolled `CASE WHEN` ranking gives sub-50ms search-as-you-type with zero new infrastructure. When the catalog grows past the point where this stops working, swapping in Postgres full-text search is a one-day change.

**Why Cloudflare in front of everything.** It absorbs DDoS, image traffic, and bot scraping at the edge for free. The origin only handles dynamic API calls, which keeps Render bills small and the site fast.

---

## Tech stack

**Frontend.** React 19, Vite 7, Tailwind CSS, React Router v7, Swiper for carousels.

**Backend.** Node.js, Express 5, JWT auth, bcrypt, Helmet, `express-rate-limit`, `express-mongo-sanitize`, `xss-clean`, `hpp`.

**Data and infrastructure.** PostgreSQL on Supabase, Render for the API, GoDaddy for static frontend hosting, Cloudflare for DNS, CDN, and WAF.

**Integrations.** Google OAuth 2.0, Razorpay (UPI, cards, netbanking, wallets, COD), Meta Pixel for client-side analytics.

---

## Feature surface

### Customer-facing

Curated category storefronts (necklaces, mangalsutras, bangles, earrings, hair accessories), the custom hamper builder with drag-and-drop SKU selection, pre-curated hamper suggestions by occasion, multi-image product detail with zoom, persistent cart and wishlist, multi-address book with a default address, Razorpay checkout with COD, order history and tracking, email/password plus Google OAuth login, "remember me" with smart `localStorage` ↔ `sessionStorage` switching, mobile-first responsive UI.

### Admin

Full product CRUD with multi-image upload, the one-click Amazon/Flipkart scraper, hamper composer for bundling SKUs into curated gift sets, occasion-based collection builder with image banners, banner scheduler with redirect links, order management with status updates, a real-time metrics dashboard (revenue, users, orders, top products), email-whitelisted admin RBAC, and profile picture management.

---

## Engineering deep dives

<details>
<summary><b>The Amazon/Flipkart product scraper — turning a ten-minute task into ten seconds</b></summary>

**Problem.** The client adds five to ten SKUs per week. Each one means manually typing the title, copying the description, formatting the price, and downloading four to six product images — roughly ten minutes of data entry per SKU.

**Solution.** An admin-only scraper endpoint. The admin pastes an Amazon or Flipkart URL, the server fetches the page with a real-browser `User-Agent`, `cheerio` parses the DOM, and site-specific selectors extract:

- Title (`#productTitle` for Amazon, `_35KyD6` for Flipkart)
- Description (feature bullets joined, or the product description block)
- Price (handles fractional `.a-price-whole` plus `.a-price-fraction` reassembly)
- All images including gallery thumbnails (`#altImages img`, `.imageThumbnail img`)

The admin form pre-fills with the extracted data, the admin reviews, and clicks save. SKU onboarding dropped from about ten minutes to about ten seconds.

```js
// Simplified flow
const response = await axios.get(url, {
  headers: { 'User-Agent': '...' },
  timeout: 10000,
});
const $ = cheerio.load(response.data);

if (url.includes('amazon.')) {
  productData.name = $('#productTitle').text().trim();
  productData.description = $('#feature-bullets ul li')
    .map((_, el) => $(el).text().trim())
    .get()
    .join(' ');
  // ... price reassembly, image gallery extraction
}
```

</details>

<details>
<summary><b>Search — fast, ranked, multi-field, zero infrastructure overhead</b></summary>

The catalog is small enough (hundreds of SKUs, growing) that pulling in Elasticsearch would be expensive overkill. Search runs entirely on Postgres:

- Multi-field `LIKE` matching across `name`, `description`, and `category`
- JSONB tag-array search using `jsonb_array_elements_text`
- Manual relevance ranking via `CASE WHEN` — exact prefix matches first, then substring matches, then featured products, then newest
- Returns products plus matching categories plus tag-based suggestions in a single round-trip

```sql
ORDER BY
  CASE
    WHEN LOWER(name) LIKE $2 THEN 1   -- exact prefix
    WHEN LOWER(name) LIKE $1 THEN 2   -- substring
    ELSE 3
  END,
  featured DESC,
  created_at DESC
LIMIT 10;
```

Search-as-you-type latency stays under 50ms with no new infrastructure and no new monthly bills.

</details>

<details>
<summary><b>Database security — containerized, never publicly addressable</b></summary>

The database holds customer addresses, order history, and PII. Direct internet exposure was never on the table.

- Postgres lives in a Supabase-managed Docker container
- Only the Render-hosted Express API holds the connection string
- The connection string is injected via Render env vars — never in code, never in git
- Parameterized queries everywhere; no string concatenation in SQL
- `express-mongo-sanitize` and `xss-clean` middleware on every request
- Nine versioned SQL migrations tracked in `backend/migrations/`

</details>

<details>
<summary><b>Payment trust — Razorpay with server-side signature verification</b></summary>

E-commerce frontends can lie about payment success. Naive integrations confirm orders client-side and lose money to tampering. The Razorpay integration is structured so that's not possible:

1. The order is created server-side by hitting `api.razorpay.com/orders` with the keyed secret. The frontend never sees the secret.
2. Razorpay Checkout opens on the client; on success, the SDK returns `razorpay_order_id`, `razorpay_payment_id`, and `razorpay_signature`.
3. The backend recomputes `HMAC-SHA256(order_id + "|" + payment_id, KEY_SECRET)` and constant-time compares it to the signature.
4. Only on signature match is the order status flipped to `paid` in the database and the cart cleared.

Live payment methods: UPI, cards, netbanking, wallets, and COD (handled separately, no gateway round-trip). The client UI never has authority to confirm a payment.

</details>

<details>
<summary><b>OWASP Top 10 hardening</b></summary>

| Control | Implementation |
| :--- | :--- |
| Authentication | JWT with 1h expiry, bcrypt at 12 rounds, email-whitelist admin RBAC |
| Input validation | Server-side validation on every endpoint, HTML tag stripping, parameterized SQL |
| Network | `express-rate-limit` (100 req / 10 min general, 5 req / 15 min auth), CORS allowlist |
| Headers | Helmet with strict CSP, HSTS, `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff` |
| Injection | `xss-clean`, `express-mongo-sanitize`, `hpp` |
| Secrets | All credentials in env vars, `.env` git-ignored from day one, GitHub Secret Scanning enabled |
| Database | Containerized, no public connection, parameterized queries everywhere |

</details>

<details>
<summary><b>Performance — sub-second LCP on 4G mobile</b></summary>

- Code splitting via Vite into `react-vendor`, `ui-vendor`, and app bundles
- Cloudflare edge cache for static assets and product images
- Cloudflare auto-optimization for images
- Preconnect hints to fonts and the API origin in `<head>` — saves roughly 200ms on cold-cache loads
- Lazy-loaded routes for non-critical pages
- CSP-compliant async font loading so fonts never block first paint
- Cloudflare Insights for RUM monitoring

</details>

---

## At a glance

| Team size | Timeline | Status | Scale | Critical security issues at launch |
| :---: | :---: | :---: | :---: | :---: |
| Solo (FE + BE + DB + ops) | ~2 months to production | Live and maintained | ~14k LOC, 14 routes, 9 resources | 0 |

---

## About me

I'm Shubh Ghiya, a full-stack engineer pursuing a BS in Data Science at IIT Madras. I built Nakhrali end-to-end as a paid client engagement: requirements, architecture, frontend, backend, database design, deployment, security, monitoring, and ongoing maintenance. The brand is live, the client runs the store independently, and I'm still on retainer for new features.

I'm looking for full-stack or backend roles where I can ship production systems end-to-end. Startups especially welcome.

**Email:** [ghiyashubh23@gmail.com](mailto:ghiyashubh23@gmail.com)
**Live site:** [nakhraliluxury.com](https://nakhraliluxury.com)

Hiring teams: request read access to the private source repo by email and I'll walk you through the codebase on a call.

---

<sub>© Nakhrali. Code proprietary. This README is a public case study published with permission of the brand.</sub>
