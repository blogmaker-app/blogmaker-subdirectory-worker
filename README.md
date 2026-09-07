# Blogmaker subdirectory Worker

Serve a Blogmaker blog at a path such as `https://example.com/blog`, while leaving the rest of
the existing website with its current host.

## Setup

1. In Blogmaker, open **Settings → Domains and URLs → /subdirectory** and save your full blog URL.
   Subdirectory hosting requires Blogmaker's Expert plan. The existing domain must use
   Cloudflare DNS, with its web records proxied. Keep its existing origin and email records.
2. Select **Connect Cloudflare**. Blogmaker requests only the Worker and route access needed for
   this setup and shows every planned change before applying it.
3. Review and confirm the setup. Blogmaker uploads the Worker, configures its values, adds the
   saved routes, and revokes its temporary Cloudflare access afterward.
4. Confirm the domain's SSL/TLS mode is **Full** and visit the saved blog URL. The `workers.dev`
   preview address only shows a setup message; the blog is served on its configured hostname.

This repository remains a public mirror of the standalone Worker source used by Blogmaker's
managed setup flow. It contains no application code, customer data, credentials, or deploy link.

If the OAuth connection is unavailable, expand **Set up without connecting Cloudflare** in
Blogmaker and follow the generated Worker-code and route instructions there.

## Behavior

- Only the configured hostname and path are sent to the configured Blogmaker origin.
- Blogmaker origins are restricted to a single blog subdomain of `bmaker.app` or `bstatic.io`, or to the existing custom subdomain shown by Blogmaker.
- Other paths on the configured hostname pass through to the existing site.
- Query strings, methods, request bodies, and cookies are preserved.
- HTML links, images, and forms and same-origin redirects are rewritten to the public blog path.
- External URLs, binary assets, and JSON responses are left intact.
- The Worker does not cache personalized responses or follow upstream redirects automatically.

## Local checks

Use Node.js 22 or newer:

```sh
npm ci
npm test
npm run check
```

Tests run in a local Workers runtime with mocked outbound requests. `npm run check` bundles the
Worker with Wrangler's dry-run mode; neither command deploys or changes Cloudflare resources.
