# Storefront roadmap

- [x] Replace Shopify-dependent catalog, cart, and checkout assumptions with an independent store foundation.
- [x] Finish the independent product catalog, cart, WhatsApp ordering, and public product pages.
- [x] Add the owner sign-in, admin product/order screens, delivery settings, and invited-owner access.
- [ ] Online gateway (Paystack etc.) — on hold by owner; manual payments for now.
- [x] Add the ten owner-provided product photos and prices to the independent catalog.
- [ ] Set product stock and delivery charges through the admin area; stock was left unspecified.
- [x] n8n webhook section in Admin (paste address anytime, choose events, send test).
- [x] Buyer order page updates by itself when the admin changes the order.
- [ ] Owner pastes her n8n Production webhook address in Admin → Automations when ready.
## Move into this project
- [x] Recreate store pages, design, database rules, and product photos in this project.
- [ ] Import existing products, delivery charges, orders and owner invitations — waiting on exports from the original project.
- [ ] Owner re-registers with the same email on /admin/login to regain admin access.

## Manual payments
- [x] Bank transfer + OPay payment details managed from admin
- [x] Payment instructions page after checkout, "I've made payment", proof upload, WhatsApp with reference
- [x] Orders start Pending Payment; only admin confirms Paid
- [x] Live admin alerts for new orders and payment reports (owner chose dashboard alerts only)

## Delivery
- [x] Free "Pick up at our store" option (address set in Admin → Delivery).
- [x] Owner's store pickup address saved: Iju Market, Atan Road, Ogun State.
- [x] Distance-based delivery fee: ₦550/km from the store, no starting fee (editable in Admin → Delivery).

## Better address lookup (in progress)
- [ ] Address suggestions as you type (free map search; Google Places once Google Maps is connected — owner skipped connecting for now).
- [ ] "Use my current location" and a draggable map pin.
- [ ] Price from store → chosen spot using the per-km rate.
- [ ] Backup list of delivery areas with a fee each (Sango, Ota, Ijoko, Lagos areas), editable in Admin; never blocks checkout.
