# Data2Coin Ghana — Project Handoff (15 November 2026)

## Current customer flow
- The live MTN catalogue is loaded from the KiNG FLEXY packages Edge Function.
- Customer selects a bundle and enters two separate numbers:
  1. Recipient phone: receives the data bundle.
  2. Payment phone: receives the MTN MoMo authorization request.
- Customer also enters an email required by Paystack.
- The customer must approve the MoMo request on the payer's phone. Data2Coin must never ask for or store a MoMo PIN.
- Customer-facing prices are calculated server-side from the current KiNG FLEXY supplier price. The customer never sees wholesale cost or internal margin.

## Backend deployed
Supabase project: lulbwtfxavlddcemyfyx

1. `live_data_orders` table
   - Stores recipient phone, payer phone, email, package size, supplier cost, retail price, Paystack reference, supplier reference, payment state, fulfillment state, and private tracking token.
   - RLS is enabled. The Edge Functions use server-side credentials; no public table policy was added.

2. `start-live-data-payment` Edge Function (v1)
   - Rechecks the live KiNG FLEXY MTN catalogue and supplier price.
   - Creates a live order.
   - Uses Paystack's Ghana Mobile Money Charge API with `mobile_money.phone` set to the payer phone and provider `mtn`.
   - Returns the MoMo display text and a private tracking token.

3. `paystack-webhook` Edge Function (v6)
   - Verifies Paystack HMAC signature and verifies the transaction directly with Paystack.
   - Checks exact GHS amount and reference.
   - Preserves the existing seller-listing order payment path.
   - For live orders, it claims fulfillment once and submits a KiNG FLEXY purchase using the recipient phone and stable supplier reference.
   - Repeated webhook deliveries cannot trigger a second purchase.
   - Ambiguous supplier responses/timeouts are moved to manual review; the function does not automatically retry an uncertain supplier purchase.

4. `get-live-data-order` Edge Function (v1)
   - Returns limited order status only when both order ID and private tracking token match.

## Frontend
- Updated `index.html` in GitHub repository `iyenagbegodswillgodswill-debug/-data2coin`.
- Live package checkout now asks separately for recipient phone, payer MoMo phone, and email.
- Added a Check Order Status action.
- Existing seller marketplace checkout remains separate and unchanged.

## Important pricing note
Retail price calculation currently uses:
`ceil((supplier_cost * 1.10 / 0.98) * 100) / 100`
This is the current intended markup calculation. Actual net profit still depends on all Paystack fees and operating costs.

## Not yet proven / production blockers
- No real Paystack MoMo charge has been initiated from this new flow. Do not test with a real customer; use an authorized small-value test after confirming the Paystack account is live and the payer phone is yours.
- The KiNG FLEXY purchase endpoint response shape and fulfillment states have not yet been validated with a real supplier order. The webhook is deliberately conservative: only explicit recognized success states are marked delivered; uncertain responses go to manual review.
- Verify the KiNG FLEXY wallet has enough balance before any real order.
- Confirm the Paystack webhook dashboard points to the deployed `paystack-webhook` URL and that the Paystack secret is configured.
- A real end-to-end test must confirm: payer receives prompt, payer approves, webhook verifies, supplier deducts wallet, recipient receives data, and order shows delivered.
- Never mark a bundle delivered based only on Paystack payment success.

## Latest code commit
- Frontend checkout connection: `eb6def16ebb6aee0d2bc2021418b506ec98264d7`
- Follow-up catalogue status copy: recorded in latest GitHub commit after that.
