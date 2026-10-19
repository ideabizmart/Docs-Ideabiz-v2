# IP-Blocks Resolver — IdeaBiz Channel (`ideabiz.lk`)

Lookup API for partners integrating through the **IdeaBiz** API Manager. Resolve an
IP address, a CIDR block, an AS number, or a country to its network-ownership details
(the owning Autonomous System and its announced IP prefixes), sourced from the
`ipverse/as-ip-blocks` dataset.

This is the **IdeaBiz** channel. The MIFE/Dialog channel is identical except for the
host — see `API_MIFE_IPResolve.md`.

---

## What it does

Given a public IP, return the most specific announced CIDR block that contains it and
the Autonomous System (AS) that owns it, with AS metadata (handle, description,
country, category, network role). Also supports looking up a specific block, an AS by
its number (with its prefixes), and all blocks for a country (with counts). The
dataset is refreshed from the daily `ipverse/as-ip-blocks` release.

---

## How to get access

1. **Application on the IdeaBiz portal** — create/subscribe an application. The API
   Manager issues your **application id** and an OAuth2 **access token**.
2. **Application authorization** — based on your application id, the API owner must
   register your application in the resolver so calls are accepted. Until then, calls
   are rejected with 401.
3. **Firewall / allowlist** — raise a connectivity request for your calling system if
   required by the environment.

Contact the IdeaBiz platform team to initiate these.

> **Application authorization (important).** Access is granted **per application id**.
> Share your API Manager application id with the API owner so they can register it in
> the resolver's app registry. Requests from an application that has not been
> registered (or is disabled) are rejected with 401.

---

## Endpoints

Base URL (IdeaBiz channel):

```
https://ideabiz.lk/apicall/ip-resolve/v1
```

- **Auth:** standard API Manager OAuth2 — send your access token as a bearer token:
  `Authorization: Bearer <access_token>`. The gateway validates it and injects the
  identity assertion the resolver uses; you only ever send the bearer token.
- All endpoints are **GET**. Responses are `application/json`.

> The path shape is the same across channels:
> `/apicall/ip-resolve/v1/...`. Only the host differs (`ideabiz.lk` here;
> `mife.dialog.lk` for the MIFE channel). Requests and responses are identical on both.
> The platform team confirms the exact base URL on access approval.

| Method | Path | Purpose |
|---|---|---|
| GET | `/ip/{ip}` | Resolve an IPv4/IPv6 address to its containing block + owning AS. |
| GET | `/block/{cidr}` | Details for a specific CIDR block (slash URL-encoded as `%2F`), or use `/block?prefix=&len=`. |
| GET | `/asn/{asn}` | AS metadata + its announced prefixes (paginated). |
| GET | `/country/{code}` | A country's blocks with total counts (paginated). |

### Versioning

The version is the `v1` segment in the path. The current version is **`v1`**.

- Backward-compatible changes (new **response** fields) are made in place on `v1` and
  do **not** change the URL. Always ignore response fields you don't recognise.
- A new version (`v2`) is published only for a breaking change, at a new path
  (`.../ip-resolve/v2/...`). `v1` keeps running in parallel during a deprecation
  window, so pinning to `/v1/` keeps your integration stable.

### Pagination

`/asn` and `/country` accept `page` (1-based) and `page_size`. `page_size` defaults to
**50** and is **hard-capped at 100** — larger values are clamped to 100. The response
echoes the effective `page` and `page_size`.

---

## 1. Resolve an IP — `GET /ip/{ip}`

Returns the most specific (longest-prefix) announced block that contains the IP, and
the AS that owns it. Family (v4/v6) is auto-detected.

### curl

```bash
curl -sS \
  "https://ideabiz.lk/apicall/ip-resolve/v1/ip/175.157.61.186" \
  -H "Authorization: Bearer <access_token>"
```

For an IPv6 address, URL-encode the colons as `%3A`:

```bash
curl -sS \
  "https://ideabiz.lk/apicall/ip-resolve/v1/ip/2402%3A4000%3A126d%3Ab7c3%3A1%3A0%3A273f%3A1c9a" \
  -H "Authorization: Bearer <access_token>"
```

### Response (200)

```json
{
  "ip": "175.157.61.186",
  "matched_cidr": "175.157.0.0/16",
  "family": 4,
  "asn": 18001,
  "source": "ipverse",
  "country_code": "LK",
  "country": "Sri Lanka",
  "as_metadata": {
    "asn": 18001,
    "handle": "DIALOG-AS",
    "description": "Dialog Axiata Plc",
    "country_code": "LK",
    "country": "Sri Lanka",
    "origin": "authoritative",
    "category": "isp",
    "network_role": "midsize_transit",
    "operator": { "code": "DIALOG", "name": "Dialog Axiata" }
  },
  "tags": ["FIXED_ISP"]
}
```

- `matched_cidr` — the specific block that contains the IP.
- `family` — `4` or `6`.
- `source` — which dataset the winning block came from (`ipverse` or `apnic`).
- `as_metadata.operator` — a nested `{code, name}` object: the resolved canonical
  operator when the AS is grouped under one, or `null`. Use it for a clean operator
  identity instead of the raw `handle`.
- `tags` — network-type tags for the matched **block** (`CLOUD`, `CDN`, `FIXED_ISP`,
  `VPN`, …); a list, possibly empty. Tags are per-block, not per-operator: the same
  company can have differently-typed ranges (Google Fiber = `FIXED_ISP`, Google Cloud
  = `CLOUD`).
- `as_metadata` is `null` for a block from a source without ASN data (APNIC); `asn`
  is `null` too, but `country_code`/`country`, `source`, and `tags` are still returned.
- `400` on an invalid IP; `404` if no announced block contains it.

---

## 2. Block details — `GET /block/{cidr}`

Details for one CIDR block. Supply the CIDR with its slash URL-encoded (`%2F`), or as
two query params.

### curl

```bash
# path form (slash encoded as %2F)
curl -sS \
  "https://ideabiz.lk/apicall/ip-resolve/v1/block/175.157.0.0%2F16" \
  -H "Authorization: Bearer <access_token>"

# query form
curl -sS \
  "https://ideabiz.lk/apicall/ip-resolve/v1/block?prefix=175.157.0.0&len=16" \
  -H "Authorization: Bearer <access_token>"
```

### Response (200)

```json
{
  "cidr": "175.157.0.0/16",
  "family": 4,
  "prefix_len": 16,
  "asn": 18001,
  "source": "ipverse",
  "country_code": "LK",
  "country": "Sri Lanka",
  "as_metadata": {
    "asn": 18001,
    "handle": "DIALOG-AS",
    "description": "Dialog Axiata Plc",
    "country_code": "LK",
    "country": "Sri Lanka",
    "origin": "authoritative",
    "category": "isp",
    "network_role": "midsize_transit",
    "operator": { "code": "DIALOG", "name": "Dialog Axiata" }
  },
  "tags": ["FIXED_ISP"]
}
```

A block from a source without ASN data (APNIC) has `asn` and `as_metadata` set to
`null`, but still carries `source`, `country_code`, `country`, and its block `tags`:

```json
{
  "cidr": "1.0.4.0/22",
  "family": 4,
  "prefix_len": 22,
  "asn": null,
  "source": "apnic",
  "country_code": "AU",
  "country": null,
  "tags": [],
  "as_metadata": null
}
```

- `source` — the dataset the returned block came from. When the same CIDR exists in
  more than one source, the highest-priority source is returned.
- `as_metadata.operator` — nested `{code, name}` of the resolved operator grouping, or
  `null`; `tags` (block-level) — the network-type tag list for this block.
- `400` if neither `/block/{cidr}` nor both `prefix` and `len` are supplied, or the
  CIDR/length is malformed; `404` if the block is not in the dataset. The CIDR is
  matched in canonical form, so host-bit / compression / case differences still match.

---

## 3. AS lookup / search by AS code — `GET /asn/{asn}`

AS metadata plus its announced prefixes, paginated (max 100 per page).

### curl

```bash
curl -sS \
  "https://ideabiz.lk/apicall/ip-resolve/v1/asn/9329?page=1&page_size=50" \
  -H "Authorization: Bearer <access_token>"
```

### Response (200)

Example: AS9329 is one of several Sri Lanka Telecom ASNs; they are all grouped under
the single `SLT` operator (see Operator grouping), so the resolved
`as_metadata.operator` is consistent across them even though their raw `handle`s
differ.

```json
{
  "asn": 9329,
  "as_metadata": {
    "asn": 9329,
    "handle": "SLTINT-AS-AP",
    "description": "Sri Lanka Telecom Ltd",
    "country_code": "LK",
    "country": "Sri Lanka",
    "origin": "authoritative",
    "category": "isp",
    "network_role": "midsize_transit",
    "operator": { "code": "SLT", "name": "Sri Lanka Telecom" }
  },
  "page": 1,
  "page_size": 50,
  "total_prefixes": 20,
  "prefixes": [
    { "cidr": "112.135.0.0/16", "family": 4, "prefix_len": 16, "source": "ipverse", "tags": ["FIXED_ISP"] },
    { "cidr": "124.43.0.0/16", "family": 4, "prefix_len": 16, "source": "ipverse", "tags": ["FIXED_ISP"] },
    { "cidr": "203.115.0.0/18", "family": 4, "prefix_len": 18, "source": "ipverse", "tags": [] },
    { "cidr": "2402:d000::/32", "family": 6, "prefix_len": 32, "source": "ipverse", "tags": [] }
  ]
}
```

- `as_metadata.operator` — nested `{code, name}` of the resolved operator grouping (e.g.
  several raw handles grouped under one operator), or `null`. The raw `handle` is always
  preserved alongside.
- Each prefix row carries its `source` and its own block-level `tags` — different ranges
  of the same AS can have different tags.
- `total_prefixes` is the full count across all pages; `prefixes` is the current page.
- An AS with no announced prefixes returns `200` with an empty `prefixes` list.
- `404` if the ASN is unknown; `400` if `asn` is negative.

---

## 4. Country blocks + counts — `GET /country/{code}`

All announced blocks owned by ASes registered to a country, with totals, paginated
(max 100 per page). `code` is an ISO-3166 alpha-2 country code (case-insensitive).

### curl

```bash
curl -sS \
  "https://ideabiz.lk/apicall/ip-resolve/v1/country/LK?page=1&page_size=50" \
  -H "Authorization: Bearer <access_token>"
```

### Response (200)

```json
{
  "country_code": "LK",
  "total_blocks": 4821,
  "total_as": 93,
  "page": 1,
  "page_size": 50,
  "blocks": [
    {
      "cidr": "103.21.244.0/24",
      "family": 4,
      "prefix_len": 24,
      "asn": 18001,
      "source": "ipverse",
      "tags": ["FIXED_ISP"],
      "as_metadata": {
        "asn": 18001, "handle": "DIALOG-AS", "description": "Dialog Axiata Plc",
        "country_code": "LK", "country": "Sri Lanka",
        "origin": "authoritative", "category": "isp", "network_role": "midsize_transit",
        "operator": { "code": "DIALOG", "name": "Dialog Axiata" }
      }
    },
    {
      "cidr": "1.0.4.0/22",
      "family": 4,
      "prefix_len": 22,
      "asn": null,
      "source": "apnic",
      "tags": [],
      "as_metadata": null
    }
  ]
}
```

- `total_blocks` — all CIDR blocks for the country (across all sources); `total_as` —
  all ASes registered to the country (counted from the AS table, so it includes ASes
  with zero prefixes).
- Each block carries its `source`, its block-level `tags`, and the nested `as_metadata`
  object (with `operator`). Blocks from a source that carries country but no ASN (APNIC)
  appear with `asn: null` and `as_metadata: null` (but may still carry `tags`).
- Individual IP addresses are **never** enumerated — only CIDR blocks are returned.
- `400` if `code` is not two letters.

---

## Errors

Error responses carry a JSON body `{"detail": "..."}`.

| HTTP | Meaning |
|---|---|
| `400` | Invalid IP, malformed CIDR/length, bad country code, or negative ASN. |
| `401` | Missing / invalid bearer token at the gateway, or your application id is not registered / is disabled in the resolver. |
| `404` | No matching block / block / ASN found for the lookup. |
| `500` | Internal lookup or data-integrity error. |

```json
{ "detail": "no matching block" }
```

---

## Notes

- All data is derived from [`ipverse/as-ip-blocks`](https://github.com/ipverse/as-ip-blocks)
  (CC0 1.0), refreshed daily. Results reflect announced BGP prefixes, not a live
  geolocation service.
- The service meters per-application, per-hour, per-status-code request counts for
  operational reporting.
- `GET /health` exists for infrastructure liveness checks and is unauthenticated; it is
  not part of the partner API surface.
