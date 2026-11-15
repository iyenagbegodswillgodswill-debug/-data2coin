# Data2Coin Ghana — Project Handover & Continuity Record

**Last updated:** 2026-11-14  
**Purpose:** Permanent working record for continuing Data2Coin when a ChatGPT conversation reaches its length limit. Update this file after each meaningful design, code, database, payment, or deployment change.

> **Security:** This file must never contain passwords, Supabase service-role/secret keys, Paystack secret keys, payment credentials, private tracking codes, or personal identity/payment documents. Record secret *variable names* only. The frontend currently contains a Supabase publishable key; do not copy keys into this handover.

---

## 1. Project identity

- **Project name:** Data2Coin Ghana
- **Tagline:** “Buy and sell MTN data bundles in Ghana.”
- **Purpose:** A Ghana-based marketplace connecting MTN data sellers with customers who want to buy data bundles.
- **Core network at launch:** MTN Ghana.
- **Currency:** Ghana cedis (GH₵).
- **Payment direction:** Paystack Mobile Money checkout.
- **Platform fee:** 2% of successful orders.
- **Website (GitHub Pages URL previously used):** https://iyenagbegodswillgodswill-debug.github.io/-data2coin/
- **GitHub repository:** https://github.com/iyenagbegodswillgodswill-debug/-data2coin
- **Default branch:** `main`.
- **Supabase project reference:** `lulbwtfxavlddcemyfyx`.
- **Audience:** Ghanaian buyers and approved sellers; administrator manages seller approvals and operations.

## 2. Owner's working preferences

- Give simple, short instructions, one small step at a time.
- Assume the owner may be working from an iPhone; give exact tap/paste directions.
- Avoid long code hunts, many separate snippets, and unnecessary screenshots.
- When code changes are needed, inspect the current version first and prefer one complete, ready-to-paste file where practical.
- Preserve working features. Do not redesign or remove parts without clear agreement.
- Do not make the owner repeat project history already recorded here.
- Explain what changed and how to verify it.
- Avoid unnecessary upfront costs and paid services where a free option can meet the requirement.
- Use Ghana-specific examples, GH₵ pricing, MTN Ghana, and Mobile Money.

## 3. Product rules and intended customer experience

### Core marketplace
1. Sellers register and submit their MTN contact number, bundle size, and selling price.
2. Seller registrations are reviewed by an administrator.
3. Only approved sellers' genuine available bundles should appear in the public marketplace.
4. Buyers browse approved listings, select a bundle, enter their MTN phone number and email, and pay through Paystack Mobile Money.
5. An order must be marked **Paid only after the payment has been verified server-side**.
6. Buyers receive/retain an order number and private tracking code to check order progress.
7. Sellers fulfil the order only after verified payment and according to the delivery workflow.
8. The platform fee is 2%; the seller amount should be calculated transparently.

### Pricing
- The owner chose **Option B**: price bundles only slightly below comparable official MTN bundle offers, and only when the seller can genuinely supply the bundle.
- Use current, verified Ghana MTN bundle references before making price recommendations.
- Do not invent official MTN prices or assume old listing prices are market prices.
- Do not promise a bundle is available unless the seller/listing is actually approved and active.

### Payment and delivery safety
- Paystack Mobile Money is the intended checkout.
- A browser redirect or a test checkout screen is not proof of payment.
- Do not mark an order Paid based only on a frontend message, callback query parameter, or unverified test response.
- Verify payment on a trusted server/Edge Function using Paystack's current verification mechanism and confirm amount, currency, reference, and order association.
- Prevent duplicate payment processing and duplicate fulfilment.
- Keep pending, paid, awaiting delivery, and completed states distinct.
- Never deliver data for an unverified payment.
- Clearly label test mode and test transactions.

## 4. Current front-end repository snapshot

The current `main` branch `index.html` was inspected on 2026-11-14.

- Repository: `iyenagbegodswillgodswill-debug/-data2coin`.
- `index.html` is the main single-page app and contains the current Data2Coin Ghana UI.
- `README.md` currently contains only the repository title `# -data2coin`.
- The front-end loads Supabase JavaScript v2 from jsDelivr.
- The source uses Supabase project URL `https://lulbwtfxavlddcemyfyx.supabase.co` and a publishable client key. Do not copy the key into this handover or expose any secret/service-role key.
- The current source reads public marketplace listings from `public_data_listings` selecting `id,bundle_size,price_ghs,network,data_size_gb`, ordered newest first.
- Buyer orders are also cached locally in browser `localStorage` under `data2coin_orders`. This is a convenience for that device, not the authoritative order database.

### Existing pages and navigation in current source
Bottom navigation currently contains:
1. Home
2. Buy Data
3. Sell
4. Payments
5. Airtime
6. Account

Current visible UI details:
- Dark black/green background with gold/yellow highlights and Ghana flag identity.
- Premium layered/glow visual treatment.
- Home hero: “Your Data. Your Money.”
- Marketplace list with bundle size, network, price, availability badge, and Order Data action.
- Seller registration form: name, MTN phone number, bundle size, and selling price.
- Payments information describing Paystack Mobile Money, GH₵, and 2% fee; test-mode warning.
- Airtime page is marked as a later feature.
- Account page includes saved orders, order tracking using order number and private tracking code, and a customer support placeholder.
- The UI is mobile responsive and designed for phone-sized use.

### Current source warning
The source snapshot is a record of what was present when inspected. It is not proof every feature is fully working in production. Always inspect the latest repository code and test the live site before assuming functionality.

## 5. Database and Supabase — known historical details

Supabase project reference: `lulbwtfxavlddcemyfyx`.

Tables historically observed:
- `admin_users`
- `data_listings`
- `data_orders`
- `seller_registrations`

RLS was enabled on the tables in the prior inspection.

Previously described policies included:
- Public seller-registration insert.
- Buyer order insert for anon/authenticated.
- Sellers can view their own listings/orders.
- Admins can view/update records.

Previously observed triggers:
- `publish_approved_seller_trigger` → `publish_approved_seller`
- `prepare_data_order_trigger` → `prepare_data_order`

Previously observed business logic:
- `prepare_data_order` sets the 2% platform fee and seller amount.
- Seller approval publishes an approved seller's listing.
- Administrator account intended to retain Administrator role: `allenjamesl1014@gmail.com`.
- The owner previously chose to keep the Allen administrator account.
- A seller account referred to as Much was approved and linked in prior testing.

### Important current/desired account experience
- The owner wants buyer and seller experiences brought together into a **single combined account/dashboard**, rather than forcing users into separate buyer and seller dashboards.
- Keep the existing Allen administrator account and admin role; do not accidentally replace or demote it.
- A seller should be able to use buyer features and seller tools from one account, subject to seller approval and permissions.
- Admin controls must remain separate and protected from ordinary buyer/seller permissions.
- Confirm current auth identities and ownership links before merging or changing account relationships. Do not merge accounts by guessing email/user IDs.

### Known historical data examples — not current price recommendations
Earlier test listings included:
- 2 GB at GH₵1 (seller name recorded as Mmm)
- 1 GB at GH₵10 (Mk)
- 5 GB at GH₵10 (older listing)
- 2 GB at GH₵15 (Much)

Earlier order examples included GH₵1 and GH₵10 paid/pending records. Historical reporting showed fee examples GH₵0.02 on GH₵1 and GH₵0.20 on GH₵10, with an admin dashboard snapshot of 4 orders, 2 awaiting payment, 2 paid, 0 completed, and GH₵0.22 recorded earnings. These are historical test observations only; query the live database for current counts and values.

### Supabase details still to inspect
Before changing schema or policies, inspect and record:
1. Exact current table columns and constraints.
2. RLS policies and grants for each table.
3. Auth users and relationships between buyer/seller/admin accounts (do not expose sensitive personal data in this file).
4. Edge Functions, deployed versions, and source.
5. Paystack initialization and verification function names.
6. Order status transitions and who is allowed to update each status.
7. Triggers/functions and their exact logic.
8. Storage buckets/policies if used.
9. Current data state and any duplicate/test records.
10. Supabase logs and recent payment verification events.

Do not make database changes simply to fill this document. Inspect first. Never expose a service-role key in frontend code.

## 6. Payment integration and current status

- Paystack Mobile Money is the intended Ghana payment method.
- The owner previously reported a test issue where the checkout looked successful before the Mobile Money approval/code step had been completed.
- The frontend has used Edge Function names:
  - `initialize-payment`
  - `lookup-order`
- Confirm the current deployed function names and source before changing them.
- The order should remain pending/awaiting payment until server-side verification confirms successful payment.
- Verify the exact Paystack reference, expected amount, GH₵ currency, order ID, and final status on the server.
- The buyer-facing page must not be able to mark its own order Paid.
- Confirm how Paystack webhooks are authenticated and whether webhook retries are idempotent.
- Do not store Paystack secret keys in GitHub or browser code.

### Payment approval / API access
The owner recently said they had **not yet received API approval** and wanted to build the app and be able to log in before the approval arrives.
- Treat the relevant API approval as pending until the owner confirms it has been granted.
- Build and test the app shell, authentication, account navigation, and non-live workflows without pretending live payments are enabled.
- Clearly distinguish test/demo mode from live payment mode.
- Do not block basic app login on an API approval if login can be built safely without it.
- Do not claim live payment readiness until credentials/approval, server verification, and a successful end-to-end Mobile Money test are confirmed.

## 7. Seller and buyer account workflow

Intended seller flow:
1. Seller signs up/signs in.
2. Seller completes seller registration.
3. Admin reviews registration.
4. Admin approves or rejects.
5. Only approved seller's valid bundles are published.
6. Seller can view and manage their own listings/orders according to authorization rules.

Intended buyer flow:
1. Buyer signs up/signs in or uses the agreed guest checkout flow.
2. Buyer browses approved listings.
3. Buyer places an order.
4. Buyer pays by Mobile Money.
5. Payment is verified server-side.
6. Buyer sees order status and can track using protected tracking credentials.

Desired merged dashboard:
- One account area with buyer and seller sections/roles as applicable.
- Buyer order history and tracking.
- Seller registration/approval status.
- Seller listings and seller orders after approval.
- Admin-only controls accessible only to the admin role.
- No leaking another user's orders, phone numbers, tracking tokens, or seller records.
- Preserve least-privilege RLS and server-side authorization.

## 8. Latest design / rebuild direction (2026-11-14)

The owner recently asked to return to development and prepare the app, saying:
- Remove unnecessary/previously added things they no longer want.
- They have not yet received the required API approval.
- Build the app first so they can log in before API approval arrives.
- Include the previously discussed “Mkfuniture” design element/style. The exact reference and meaning should be confirmed from the original design file/conversation before implementing; do not guess what the requested design looks like.
- The owner said they had already sent a reference file and wants the design followed, not reinvented.

**Important:** Before altering the current design:
1. Locate and inspect the original design/reference file the owner supplied.
2. Confirm which existing features they want removed; do not delete core payment/security/account logic by assumption.
3. Build the login-ready app shell first if that is the current priority.
4. Keep the current Data2Coin marketplace purpose and brand unless the owner explicitly changes it.
5. Preserve backend data and working payment safeguards while updating the UI.
6. Record precisely which features were removed, retained, or deferred.

## 9. Deployment and project access

- Earlier site URL: https://iyenagbegodswillgodswill-debug.github.io/-data2coin/
- GitHub repository: https://github.com/iyenagbegodswillgodswill-debug/-data2coin
- Branch: `main`.
- Verify the current Pages settings and deployment status before assuming GitHub Pages remains the active deployment.
- The owner generally works from iPhone and sometimes laptop using GitHub/GitHub.dev and Supabase web interfaces.
- After each code commit, verify the deployed site loads and the expected version is live.
- Do not change hosting providers or deployment configuration without explaining why and confirming it is needed.

## 10. Security and do-not-break rules

- Never mark payment Paid without trusted server-side verification.
- Never fulfil an order before verified payment.
- Never put Paystack secret keys or Supabase service-role keys in frontend code or public GitHub.
- Keep RLS enabled on exposed tables and validate policies with actual user roles.
- Buyer must only see their own private order data.
- Seller must only manage their own listings/orders.
- Admin functions must require server-verified admin authorization; never trust a client-supplied role or editable user metadata.
- Tracking codes must be treated as private credentials; do not expose them in public listing queries or logs.
- Prevent duplicate order creation, duplicate payment confirmation, and duplicate delivery.
- Keep seller approval required before publishing a seller listing.
- Keep the 2% fee calculation consistent and auditable.
- Preserve the Allen administrator account and verify admin access after any auth changes.
- Do not merge buyer/seller dashboards by weakening RLS or granting broad public access.
- Do not remove the main buyer/seller/order tracking/payment functionality without explicit instruction.
- Airtime is secondary and can remain deferred until the core data marketplace is stable.
- Do not claim the app is live-payment-ready while API approval or verification is pending.

## 11. Priorities and recommended work order

### Priority 1 — Confirm the requested design and scope
- Find the original reference/design file from the earlier conversation or Library.
- Confirm the exact “Mkfuniture” design reference.
- Clarify which previously added UI/features the owner wants removed.
- Create the app shell and login experience first, independent of live API approval where possible.

### Priority 2 — Inspect current implementation before editing
- Read the latest `index.html` and all related files.
- Inspect Supabase schema, policies, functions, triggers, and auth setup.
- Identify what is working, what is test-only, and what is unfinished.
- Back up current code before major redesign.

### Priority 3 — Complete core account experience
- Login, signup, password reset, session persistence, and sign-out.
- One combined buyer/seller account dashboard with role-appropriate sections.
- Preserve admin-only access for the Allen administrator account.
- Test buyer, seller-pending, seller-approved, and admin views separately.

### Priority 4 — Complete safe marketplace flow
- Approved listing publication.
- Buyer order creation.
- Paystack initialization.
- Server-side payment verification/webhook.
- Correct order status updates.
- Private order tracking.
- Seller fulfilment and completion tracking.

### Priority 5 — Validate and launch
- Test with sandbox/test credentials first.
- Confirm failed, cancelled, pending, duplicate, and successful payments.
- Confirm no delivery occurs for unverified payments.
- Confirm seller approval and buyer/seller ownership rules.
- Confirm mobile layout and production deployment.
- Enable live payments only after API approval and successful end-to-end verification.

## 12. Change discipline

For every meaningful change:
1. Inspect the current GitHub file/function first.
2. Inspect related Supabase functions/policies before touching payment/account logic.
3. Make the smallest safe change.
4. Commit with a descriptive message.
5. Verify the commit and deployment.
6. Test the actual buyer/seller/admin flow affected.
7. Update this handover with date, files/functions changed, commit, test evidence, and remaining issues.

Do not rely on old screenshots or old code snippets if the live repository has changed.

## 13. Details still missing — do not guess

- Original design/reference file and exact requested “Mkfuniture” design details.
- Exact list of UI/features the owner wants removed.
- Whether the app is being moved from the old GitHub Pages URL to a different hosting/deployment setup.
- Current deployed frontend commit and URL status.
- Current Supabase schema and exact RLS policy definitions.
- Current Edge Function source and deployed versions.
- Current Paystack API approval status and whether live credentials are available.
- Current webhook configuration and verification logs.
- Current auth user IDs and role relationships (do not put sensitive identifiers in this public handover).
- Current status of the Much seller account and Allen admin access.
- Current live listings/orders and test data.
- Current seller fulfilment/delivery mechanism and proof-of-delivery workflow.
- Whether Airtime is to be removed from the immediate UI or simply deferred.

## 14. New-chat continuation prompt

When opening a new ChatGPT conversation, share this file/link and say:

> Continue Data2Coin Ghana from this handover. First inspect the latest GitHub code and Supabase setup; do not assume old snippets are current. Give me one small step at a time and preserve working features and payment safeguards. My latest priority is to build the login-ready app before API approval, use the original design reference I supplied (including the requested Mkfuniture design direction), and combine buyer and seller dashboards while keeping the Allen administrator account. Do not guess missing design details or mark payments Paid without server-side verification. Update this handover after meaningful progress.

---

## 15. Change log

| Date | Update |
|---|---|
| 2026-11-14 | Created initial continuity record from the current GitHub frontend inspection and available project history. Recorded marketplace rules, Supabase project reference, known tables/triggers, Paystack verification safeguards, account/dashboard requirements, deployment link, and latest requested login-first/design direction. Exact current backend state and original design reference remain to be inspected. |
