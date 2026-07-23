---
name: Capture a real-estate lead into AlterEstate
description: Submit an inbound lead (with optional property interest and attribution) into an AlterEstate CRM account, handling the duplicate-contact case.
api: openapi/alterestate-openapi.yml
operations: [createLead]
---

# Capture a lead into AlterEstate

Use this to push an inbound contact into an AlterEstate CRM account.

## Auth
Send the account API key as a header: `Authorization: Token <api_key>`.
(This is different from the public read token, which uses the `aetoken` header.)

## Steps
1. Collect the required fields: `full_name`, `phone`, `email`. Phone is normalized to E.164; email is stored lowercased.
2. Optionally set `property_uid` to tie the lead to a specific listing, plus `notes`, `via`, `listing_type`, `currency`, `budget`, and attribution fields (`utm_source`, `utm_campaign`, `campaign_name`, `form_name`, `platform`).
3. Call **createLead** — `POST /leads/` on base `https://secure.alterestate.com/api/v1` with a JSON body.
4. Interpret the response:
   - `201` — a new lead/deal was created.
   - `200` — a duplicate contact already exists; no new deal was created, but any `custom_fields` you sent updated the existing contact. Treat as success, not error.
   - `400` — validation error; fix the failing fields and retry.

## Notes
- There is no client-supplied idempotency key; the `200` duplicate response is the dedup mechanism. See `conventions/alterestate-conventions.yml`.
- Error handling: `errors/alterestate-problem-types.yml`.
