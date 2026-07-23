---
name: Browse AlterEstate property listings and project units
description: Search property listings with filters and currency conversion, drill into a property's detail view, and enumerate a development project's buildings and units.
api: openapi/alterestate-openapi.yml
operations: [listProperties, getProperty, getBuildings, getUnits, listCities, listSectors]
---

# Browse AlterEstate listings

Read-only discovery over an AlterEstate account's public inventory.

## Auth
Read endpoints use the public token header: `aetoken: <public_token>` (from the dashboard Settings > Public API Token). Base: `https://secure.alterestate.com/api/v1`.

## Steps
1. (Optional) Resolve geography first: **listCities** — `GET /cities/?country=<id>` and **listSectors** — `GET /sectors/?city=<id>`. Provinces are nested in the results.
2. Search listings with **listProperties** — `GET /properties/filter/`. Useful filters: `search`, `city`, `sector`, `province`, `listing_type`, `category`, `rooms`, `bathrooms`, `parking`, `area`, `agent`. Set `currency` (USD, DOP, MXN, COP, CRC, GTQ, PEN) to have the backend convert prices for you.
3. Open a listing with **getProperty** — `GET /properties/view/{property_slug}/` to get the agent, gallery and amenities.
4. For a development project, enumerate structure with **getBuildings** — `GET /projects/buildings/{project_slug}/` then **getUnits** — `GET /properties/public/units/{project_slug}/`.

## Notes
- All paths use trailing slashes. See `conventions/alterestate-conventions.yml`.
- `404` means an unknown slug/path; `500` is a server error. See `errors/alterestate-problem-types.yml`.
