# Pahadi Traveller

Static landing page for Pahadi Traveller — treks, homestays and slow travel across
the Indian Himalayas.

## Structure

```
index.html    landing page
styles.css    all styles (light + dark)
favicon.svg   site icon
vercel.json   static hosting config (clean URLs)
```

No build step and no dependencies — it's plain HTML and CSS.

## Running locally

Open `index.html` directly in a browser, or serve the folder:

```bash
python3 -m http.server 3000
# → http://localhost:3000
```

## Deploying

The repo is connected to Vercel. Pushes to `main` deploy to production; every
other branch gets a preview deployment.

In the Vercel project settings, the framework preset should be **Other** with no
build command and the output directory set to the repository root.

### Domains

Production is served at **https://www.pahaditraveller.in**. The apex
`pahaditraveller.in` redirects to `www`, and `pahadi-traveller.vercel.app`
remains as a fallback alias.

DNS is delegated to Vercel — `pahaditraveller.in` uses `ns1.vercel-dns.com` and
`ns2.vercel-dns.com` as its nameservers. Manage all records (MX, TXT,
subdomains) under Vercel → Domains → pahaditraveller.in → DNS Records. Records
added in the GoDaddy panel have no effect, since GoDaddy is only the registrar
and no longer answers DNS for this domain.
