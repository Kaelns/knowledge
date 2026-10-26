# 06 · Web platform: protocols, rendering, performance, security, PWA, CMS

Matrix (A2): Web Communication Protocols **Intermediate** · Rendering Strategies Novice · Common Security Novice · Web Performance Novice *(optional)* · PWA & AMP Intermediate *(optional)* · CMS Novice *(optional)*.
Old notes: [Core js 2 → Web protocols / Security](../../Evernote/Core%20js%202/Core%20js%202.md#web-communication-protocols).

## Checklist

### Protocols (A2 Intermediate), Phase 1
- [ ] URL anatomy: scheme, host, port, path, query, fragment; encoding (`encodeURIComponent`)
- [ ] "What happens when you type a URL": DNS → TCP → TLS handshake → HTTP request → response → parsing → render (critical rendering path)
- [ ] HTTP methods, **idempotency** & safety, status codes by family (200/201/204, 301/302/304, 400/401/403/404/409/422/429, 500/502/503)
- [ ] Headers: `Content-Type`, `Accept`, `Authorization`, `Cache-Control`, `ETag`/`If-None-Match`, `Set-Cookie`, `Origin`, CORS headers
- [ ] **HTTP caching**: `max-age`, `no-cache` vs `no-store`, `immutable`, `stale-while-revalidate`, ETag revalidation (304), hashed filenames strategy
- [ ] **Stateful web**: cookies (attributes), sessions (server-side) vs tokens (JWT: header.payload.signature, not encrypted!), access + refresh token flow, where to store them
- [ ] OAuth 2.0 / OIDC at a high level: authorization code + PKCE flow
- [ ] **CORS**: same-origin policy, simple vs preflight (`OPTIONS`) requests, `Access-Control-Allow-*`, credentials
- [ ] Sync request-response vs async browser communication: XHR/fetch, polling, long polling, **SSE**, **WebSocket**
- [ ] HTTP/1.1 vs HTTP/2 (multiplexing, header compression) vs HTTP/3 (QUIC/UDP) (+)
- [ ] TLS: certificates, CA chain, validation, asymmetric for key exchange → symmetric session key, HSTS
- [ ] REST principles vs GraphQL vs RPC (+); OpenAPI (→ Orval)
- [ ] Troubleshooting: DevTools Network (status, timing, headers, preflight), `curl -v`, Postman/Bruno

### Rendering strategies (A2 Novice), Phase 1
- [ ] **CSR** (SPA: fast navigation, poor first load/SEO), **SSR** (per-request HTML, hydration), **SSG** (build-time), **ISR** (revalidate static pages), hybrid
- [ ] Streaming SSR + Suspense, React Server Components, **PPR** (partial prerendering: static shell + dynamic holes), islands architecture (Astro) (+)
- [ ] Edge rendering / CDN; hydration cost and mismatches
- [ ] Pros/cons table: TTFB, FCP, LCP, TTI, SEO, server cost, complexity
- [ ] At least one web server: **Nginx** (static hosting, SPA fallback `try_files $uri /index.html`, gzip, cache headers, reverse proxy) or Express
- [ ] Client-server architecture; caching layers: browser → service worker → CDN → reverse proxy → app → DB

### Security (A2 Novice, but always asked), Phase 1
- [ ] **XSS**: stored / reflected / DOM-based; React escapes by default, danger of `dangerouslySetInnerHTML`, `href="javascript:"`; sanitize (DOMPurify); **CSP**
- [ ] **CSRF**: why cookies make it possible, `SameSite`, CSRF tokens, checking `Origin`
- [ ] **SSRF**, SQL/command injection, path traversal (server-side awareness)
- [ ] Clickjacking (`X-Frame-Options` / `frame-ancestors`), open redirects
- [ ] Token storage trade-offs (`HttpOnly` cookie vs memory vs localStorage)
- [ ] Supply chain: `npm audit`, lockfiles, Dependabot/Renovate (+)
- [ ] OWASP Top 10 headlines; security headers (CSP, HSTS, X-Content-Type-Options, Referrer-Policy)

### Web performance (optional), Phase 2 deep dive
- [ ] **Core Web Vitals**: LCP, **INP**, CLS: what they measure, thresholds, how to improve each (+)
- [ ] Critical rendering path, render-blocking CSS/JS, **reflow/repaint** and layout thrashing
- [ ] Page load: minification, compression (gzip/brotli), code splitting, tree shaking, lazy loading, preload/prefetch/preconnect, image formats (WebP/AVIF), responsive images, font loading (`font-display`)
- [ ] Network: latency vs bandwidth, throttling in DevTools, waterfall, CDN
- [ ] Framework-level: memoization, virtualization, React Compiler, avoiding waterfalls (React Query prefetch, route loaders)
- [ ] Tools: Lighthouse, PageSpeed Insights, DevTools Performance panel, `rollup-plugin-visualizer` / bundle analyzer, `web-vitals` lib

### PWA & AMP (optional, A2 Intermediate), Phase 2
- [ ] PWA: HTTPS + **web app manifest** (name, icons, `display`, `start_url`, theme) + **service worker** (lifecycle: install/activate/fetch; cache strategies: cache-first, network-first, stale-while-revalidate), offline, installability, push notifications
- [ ] Workbox / `vite-plugin-pwa`
- [ ] AMP: restricted HTML/JS for fast mobile pages, AMP cache; mostly legacy now (Google no longer requires it for Top Stories)

### CMS (optional, Novice), Phase 2
- [ ] Traditional (WordPress) vs **headless** (Contentful, Strapi, Sanity) vs hybrid; frontend consumes content via REST/GraphQL
- [ ] Content modeling, categories/tags, localization (locales, fallback), SEO-friendly URLs (slugs) & metadata, roles & permissions, preview/draft, webhooks → rebuild (SSG/ISR)

## Key questions

1. What happens when you type a URL and press Enter?
2. `Cache-Control: no-cache` vs `no-store`. How do you cache a hashed JS bundle vs `index.html`?
3. Cookies vs JWT in localStorage: which is safer and why?
4. What is CORS, who enforces it, when is a preflight sent?
5. XSS vs CSRF: how does each work and how do you defend?
6. CSR vs SSR vs SSG vs ISR: pick one for a blog, a dashboard, an e-shop.
7. What is hydration and what causes hydration mismatches?
8. WebSocket vs SSE vs polling.
9. What are Core Web Vitals and how would you improve LCP / INP / CLS?
10. How does a service worker enable offline mode?

## Hands-on

- [ ] DevTools Network on my project: find a preflight request, a 304, the cache headers of the JS bundle
- [ ] `curl -v https://example.com`: read the TLS + headers
- [ ] Lighthouse + Performance panel on my project; write 3 concrete improvements
- [ ] Phase 2: add `vite-plugin-pwa` to the lab repo, make it installable + offline

## Links

- [MDN: HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) · [HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) · [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [patterns.dev: Rendering patterns](https://www.patterns.dev/react/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) · [OWASP Cheat Sheets (XSS, CSRF)](https://cheatsheetseries.owasp.org/)
- [web.dev: Core Web Vitals](https://web.dev/articles/vitals) · [web.dev Learn PWA](https://web.dev/learn/pwa)
