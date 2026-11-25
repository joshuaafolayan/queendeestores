<!-- LOVABLE:BEGIN -->
> [!IMPORTANT]
> This project is connected to [Lovable](https://lovable.dev). Avoid rewriting
> published git history — force pushing, or rebasing/amending/squashing commits
> that are already pushed — as it rewrites history on Lovable's side and the
> user will likely lose their project history.
>
> Commits you push to the connected branch sync back to Lovable and show up in
> the editor, so keep the branch in a working state.

## What Lovable manages

Rules outside this block are the project's own. Change the pieces below in
Lovable, not by hand. With the [Lovable MCP](https://mcp.lovable.dev)
connected, `send_message` asks Lovable to do it; otherwise ask the user to
use the editor.

- **Database.** Do not add or edit `supabase/migrations/` or `drizzle/`.
  Ask Lovable to change the schema.
- **Generated files.** Do not edit a file whose header says it is automatically
  generated.
- **AI gateway.** Keep model calls on Lovable's gateway, and keep
  `LOVABLE_API_KEY` out of browser code.
- **Connectors.** Lovable provides native connectors. `list_connectors` shows
  which are enabled; `add_connector` only returns the dashboard link. When a
  connector is enabled, calls go through the connector gateway and need
  `LOVABLE_API_KEY`. When Lovable does not provide one, users can bring their
  own key and store it in Secrets.
- **Secrets.** Lovable manages them. Do not put secret values in the repo.
<!-- LOVABLE:END -->

## Storefront integration
- QueenDee's catalog, cart, orders, and delivery settings live in Lovable Cloud; keep product and payment data authoritative there so the storefront operates independently of Shopify, and keep owner access in protected role records.
- Owner accounts are claimed, not imported: an owner signs up with their email, confirms it, and `claim_admin_access` grants the role server-side; keeps identity tied to verified email ownership.
- Payments go through `store_payment_methods` (one row per method `kind`, `provider_type` manual|gateway) and `src/lib/payments.ts`; orders record `payment_method`, `payment_provider` and `payment_reference`, so an online gateway is added as a new method plus a server-side webhook without new order tables.
- Customers reach their order only through `get_store_order_for_customer` / `notify_store_order_payment` with order id + reference, and upload proofs into `queendee-payment-proofs/<order id>/`; only admins can update orders or read proofs, so marking Paid stays an admin-only action.
- Automations (n8n etc.) are configured in admin-only `store_webhooks` (one row per `provider`) and delivered by the `send_store_order_webhooks` trigger via pg_net; order triggers must always `RETURN` and swallow their own errors so automations can never block checkout or order updates.
- Buyer order pages refresh via a data-free realtime "doorbell" on topic `store-order:<id>:<reference>` and re-read through `get_store_order_for_customer`, so order rows stay admin-only.
- Store pickup is the `delivery_area` value `Store pickup`; `create_store_order` charges it ₦0 (also enforced by a CHECK) and offers it only when `store_settings.pickup_enabled` and `pickup_address` are set, with `deliveryFeeFor` in `src/lib/independent-store.ts` mirroring that rule in the UI.
- Distance delivery: the browser measures road distance (OpenStreetMap + OSRM, free) and `create_store_order` recomputes the fee from `store_settings` base + per-km rates; flat Lagos/Ogun fees apply only when distance pricing is off. Why: no paid maps API, and the price itself stays server-side.
- Shoppers read store settings only through `get_store_checkout_settings` (no anon SELECT on `store_settings`), and can open `queendee-products` files only when an active product points to them. Why: keeps settings and unlisted uploads admin-only.
