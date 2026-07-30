# Custom domain: www.theroccoapp.com

GitHub Pages repo: `SebSte85/rocco-website`  
Canonical host: **www.theroccoapp.com** (Apex `theroccoapp.com` should redirect to www)

Privacy URL for App Store Connect:
`https://www.theroccoapp.com/privacy/`  
(trailing slash optional; both resolve once Pages is live)

## 1. DNS at your registrar (required)

Until these exist, GitHub cannot finish HTTPS for the custom domain.

### A) `www` → GitHub Pages

| Type | Name / Host | Value / Target |
|------|-------------|----------------|
| **CNAME** | `www` | `sebste85.github.io` |

### B) Apex `theroccoapp.com` → GitHub (so bare domain works / redirects)

| Type | Name / Host | Value |
|------|-------------|-------|
| **A** | `@` | `185.199.108.153` |
| **A** | `@` | `185.199.109.153` |
| **A** | `@` | `185.199.110.153` |
| **A** | `@` | `185.199.111.153` |

Optional IPv6 (AAAA), same hosts:

- `2606:50c0:8000::153`
- `2606:50c0:8001::153`
- `2606:50c0:8002::153`
- `2606:50c0:8003::153`

TTL: 300–3600 is fine. Propagation: minutes to a few hours.

## 2. GitHub Pages UI (after DNS is set)

Repo **rocco-website** → **Settings → Pages**:

1. Custom domain: `www.theroccoapp.com`
2. Wait until DNS check is green
3. Enable **Enforce HTTPS**
4. Enable **Redirect apex to www** (if shown)

The repo already contains a `CNAME` file with `www.theroccoapp.com` — push `main` so Pages picks it up.

## 3. Verify

```bash
dig +short www.theroccoapp.com CNAME
# → sebste85.github.io.

curl -sI https://www.theroccoapp.com/privacy/ | head -5
curl -sI https://www.theroccoapp.com/.well-known/apple-app-site-association | head -10
```

## 4. App side (already prepared in repo)

- Associated Domains: `applinks:www.theroccoapp.com` + `applinks:theroccoapp.com`
- Buddy invite/key base URLs point at `https://www.theroccoapp.com/…`
- Ship a new TestFlight build after DNS + HTTPS are live so Universal Links work on device
