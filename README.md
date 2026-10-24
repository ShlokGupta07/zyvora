# ZYVORA — Everything. Elevated.

An Indian e-commerce store with 16 categories and 36 demo products, built with Node.js and Express. Prices and payments are checked on the server, and payments go through Razorpay.

> **Status:** early version. Payments have **not** yet been tested against a real Razorpay account. Use test keys first. See [Not included yet](#not-included-yet).

## Features
- Product browsing, search, category filters, sorting, saved items, cart (max 10 per item, stock limits)
- Checkout form with validation for name, mobile, email, address, city, state and PIN code
- Card, UPI app and netbanking payments through Razorpay Checkout
- **Pay via UPI QR:** a fresh single-use QR for each order, created by Razorpay
- Server-side pricing in paise, stock reservation, duplicate-checkout protection
- Orders marked paid only after a verified Razorpay signature, webhook or payment lookup
- Security headers, rate limiting, request size limits

## Quick start
You need Node.js 18 or newer.

```bash
npm install
cp .env.example .env      # then fill in your Razorpay TEST keys
set -a; . ./.env; set +a
npm start
```
Open http://localhost:3000 (do not open `index.html` as a file).

Run the tests with `npm test`.

## Edit products and prices
Open `products.json`:
- `p` = price in whole rupees
- `s` = stock (the 50s are placeholders, set real numbers)
- `f` = 1 to show the item under "Featured"

Product photos live in `public/img/` and are named by SKU, for example `ZV-0013.jpg`.

## Settings (`.env`)
| Name | What it is |
|---|---|
| `RAZORPAY_KEY_ID` | Razorpay key ID (`rzp_test_...` while testing) |
| `RAZORPAY_KEY_SECRET` | Razorpay secret. Never share or commit it |
| `RAZORPAY_WEBHOOK_SECRET` | Secret you set when creating the webhook |
| `APP_BASE_URL` | Your site address |
| `MERCHANT_NAME`, `MERCHANT_UPI_ID` | Optional. Shown on the QR panel only if you fill them in |

The real `.env` file is ignored by Git. Never upload it.

## Razorpay setup
1. Create a Razorpay account and use **Test Mode**.
2. Copy the test Key ID and Secret into `.env`.
3. Add a webhook: `https://YOUR-DOMAIN/api/webhook`. Events: `payment.captured`, `payment.failed`, `order.paid`, `qr_code.credited`. Use an HTTPS tunnel such as ngrok when testing locally. Check these event names against Razorpay's current docs.
4. UPI QR needs **QR Codes** enabled on your Razorpay account. If it isn't, the site shows a clear "not available" message.

## Before going live
- Razorpay account activated and test payments passed (success, failure, cancel, QR)
- Webhook confirmed working end to end
- Privacy, returns, shipping and terms pages and a support contact added and legally reviewed
- Orders moved from `data/orders.json` to a real database with backups
- Tax (GST) treatment confirmed with your accountant
- Host on HTTPS with `NODE_ENV=production` and secrets in the host's settings

## Not included yet
Customer accounts and order history, admin dashboard (products, stock, orders, refunds), coupons, seller portal, a database. Orders are stored in a local file for now.

## Project layout
```
server.js        API, webhook, payments
lib.js           pricing, stock and signature logic
products.json    catalogue (edit prices here)
public/          storefront (index.html, app.js, style.css, img/)
test/            unit tests
```
