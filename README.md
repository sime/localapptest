# localapptest

A static page at https://sime.github.io/localapptest that fetches
`http://sime.dev.ii.zone:8000/data.json`, a private-network (10.x) HTTP host,
from a public HTTPS origin.

## Client-side tricks

- `fetch(url, { targetAddressSpace: 'local' })`: Chrome's Local Network Access (142+).
  This skips the HTTPS→HTTP mixed-content block and shows the local-network permission prompt.
- Falls back to `targetAddressSpace: 'private'` (older Private Network Access builds), then to a plain fetch.
- `mode: 'cors'`, `credentials: 'omit'` and no custom headers, so the request stays a "simple" CORS request.
- Reports the permission state (`local-network-access` / `local-network`) and explains failures.

## What the server must send

- `Access-Control-Allow-Origin: https://sime.github.io` (or `*`)
- For older Chrome PNA preflights: answer `OPTIONS` with `Access-Control-Allow-Private-Network: true`

Override the endpoint with `?url=http://host:port/path`.

## Deploy

GitHub Actions (`.github/workflows/pages.yml`) publishes `site/` on every push to `main`.
