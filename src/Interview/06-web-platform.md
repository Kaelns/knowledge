# 06 · Web platform: protocols, rendering, performance, security, PWA, CMS

Matrix (A2): Web Communication Protocols **Intermediate** · Rendering Strategies Novice · Common Security Novice · Web Performance Novice *(optional)* · PWA & AMP Intermediate *(optional)* · CMS Novice *(optional)*.
Old notes: [Core js 2 → Web protocols / Security](../../Evernote/Core%20js%202/Core%20js%202.md#web-communication-protocols).

**Pass 1 priority.** P1: URL → page flow, methods/status codes, caching headers, cookies vs JWT, CORS, XSS/CSRF. P2: TLS, WebSocket/SSE, CSR/SSR/SSG/ISR + hydration, Nginx SPA fallback, CSP. P3 (Pass 2): Web Performance deep dive, PWA & AMP, CMS, HTTP/2–3.

## Checklist

### Protocols (A2 Intermediate), Pass 1
📚 Old notes: [[Core js 2#HTTP vs HTTPS|HTTP vs HTTPS + TLS]] · [[Core js 2#HTTP methods, headers and status codes|Methods, headers, status codes]] · [[Core js 2#REST|REST]] · [[Core js 2#CORS|CORS]] · [[Core js 2#Authentication JWT and OAuth|JWT & OAuth]] · [[Core js 1#Web Storage API and Cookies|Cookies vs storage]] · [[Core js 2#Fetch|Fetch]] · [[Core js 2#XMLHttpRequest|XMLHttpRequest]]
- [ ] 🟡 SHALLOW URL anatomy: scheme, host, port, path, query, fragment; encoding (`encodeURIComponent`)
- [ ] 🔴 CORE "What happens when you type a URL": DNS → TCP → TLS handshake → HTTP request → response → parsing → render (critical rendering path)
- [ ] 🔴 CORE HTTP methods, **idempotency** & safety, status codes by family (200/201/204, 301/302/304, 400/401/403/404/409/422/429, 500/502/503)
- [ ] 🟡 SHALLOW Headers: `Content-Type`, `Accept`, `Authorization`, `Cache-Control`, `ETag`/`If-None-Match`, `Set-Cookie`, `Origin`, CORS headers
- [ ] 🔴 CORE **HTTP caching**: `max-age`, `no-cache` vs `no-store`, `immutable`, `stale-while-revalidate`, ETag revalidation (304), hashed filenames strategy
- [ ] 🔴 CORE **Stateful web**: cookies (attributes), sessions (server-side) vs tokens (JWT: header.payload.signature, not encrypted!), access + refresh token flow, where to store them
- [ ] 🟡 SHALLOW OAuth 2.0 / OIDC at a high level: authorization code + PKCE flow
- [ ] 🔴 CORE **CORS**: same-origin policy, simple vs preflight (`OPTIONS`) requests, `Access-Control-Allow-*`, credentials
- [ ] 🟡 SHALLOW Sync request-response vs async browser communication: XHR/fetch, polling, long polling, **SSE**, **WebSocket**
- [ ] ⚪ SKIP HTTP/1.1 vs HTTP/2 (multiplexing, header compression) vs HTTP/3 (QUIC/UDP) (+)
- [ ] 🟡 SHALLOW TLS: certificates, CA chain, validation, asymmetric for key exchange → symmetric session key, HSTS
- [ ] 🟡 SHALLOW REST principles vs GraphQL vs RPC (+); OpenAPI (→ Orval)
- [ ] ⚪ SKIP Troubleshooting: DevTools Network (status, timing, headers, preflight), `curl -v`, Postman/Bruno

### Rendering strategies (A2 Novice), Pass 1
📚 Old notes: [[Core js 2#Critical rendering path|Critical rendering path]] · [[Core js 2#History API|History API (how CSR routing works)]]
- [ ] 🔴 CORE **CSR** (SPA: fast navigation, poor first load/SEO), **SSR** (per-request HTML, hydration), **SSG** (build-time), **ISR** (revalidate static pages), hybrid
- [ ] ⚪ SKIP Streaming SSR + Suspense, React Server Components, **PPR** (partial prerendering: static shell + dynamic holes), islands architecture (Astro) (+)
- [ ] 🟡 SHALLOW Edge rendering / CDN; hydration cost and mismatches
- [ ] ⚪ SKIP Pros/cons table: TTFB, FCP, LCP, TTI, SEO, server cost, complexity
- [ ] 🟡 SHALLOW At least one web server: **Nginx** (static hosting, SPA fallback `try_files $uri /index.html`, gzip, cache headers, reverse proxy) or Express
- [ ] ⚪ SKIP Client-server architecture; caching layers: browser → service worker → CDN → reverse proxy → app → DB

### Security (A2 Novice, but always asked), Pass 1
📚 Old notes: [[Core js 2#CORS|CORS]] · [[Core js 1#Web Storage API and Cookies|Token storage: cookies vs localStorage]] · [[Core js 2#Authentication JWT and OAuth|JWT & OAuth]]
- [ ] 🔴 CORE **XSS**: stored / reflected / DOM-based; React escapes by default, danger of `dangerouslySetInnerHTML`, `href="javascript:"`; sanitize (DOMPurify); **CSP**
- [ ] 🔴 CORE **CSRF**: why cookies make it possible, `SameSite`, CSRF tokens, checking `Origin`
- [ ] ⚪ SKIP **SSRF**, SQL/command injection, path traversal (server-side awareness)
- [ ] ⚪ SKIP Clickjacking (`X-Frame-Options` / `frame-ancestors`), open redirects
- [ ] 🔴 CORE Token storage trade-offs (`HttpOnly` cookie vs memory vs localStorage)
- [ ] ⚪ SKIP Supply chain: `npm audit`, lockfiles, Dependabot/Renovate (+)
- [ ] 🟡 SHALLOW OWASP Top 10 headlines; security headers (CSP, HSTS, X-Content-Type-Options, Referrer-Policy)

### Web performance (optional), Pass 2 deep dive
📚 Old notes: [[Core js 2#Critical rendering path|Critical rendering path]] · [[Core js 2#Layout, Paint and Composite|Reflow / repaint / composite]] · [[Core js 2#Dynamic imports|Code splitting]] · [[Core js 2#Timers and requestAnimationFrame|requestAnimationFrame]]
- [ ] 🟡 SHALLOW **Core Web Vitals**: LCP, **INP**, CLS: what they measure, thresholds, how to improve each (+)
- [ ] 🟡 SHALLOW Critical rendering path, render-blocking CSS/JS, **reflow/repaint** and layout thrashing
- [ ] 🟡 SHALLOW Page load: minification, compression (gzip/brotli), code splitting, tree shaking, lazy loading, preload/prefetch/preconnect, image formats (WebP/AVIF), responsive images, font loading (`font-display`)
- [ ] ⚪ SKIP Network: latency vs bandwidth, throttling in DevTools, waterfall, CDN
- [ ] ⚪ SKIP Framework-level: memoization, virtualization, React Compiler, avoiding waterfalls (React Query prefetch, route loaders)
- [ ] ⚪ SKIP Tools: Lighthouse, PageSpeed Insights, DevTools Performance panel, `rollup-plugin-visualizer` / bundle analyzer, `web-vitals` lib

### PWA & AMP (optional, A2 Intermediate), Pass 2
- [ ] 🟡 SHALLOW PWA: HTTPS + **web app manifest** (name, icons, `display`, `start_url`, theme) + **service worker** (lifecycle: install/activate/fetch; cache strategies: cache-first, network-first, stale-while-revalidate), offline, installability, push notifications
- [ ] ⚪ SKIP Workbox / `vite-plugin-pwa`
- [ ] ⚪ SKIP AMP: restricted HTML/JS for fast mobile pages, AMP cache; mostly legacy now (Google no longer requires it for Top Stories)

### CMS (optional, Novice), Pass 2
- [ ] ⚪ SKIP Traditional (WordPress) vs **headless** (Contentful, Strapi, Sanity) vs hybrid; frontend consumes content via REST/GraphQL
- [ ] ⚪ SKIP Content modeling, categories/tags, localization (locales, fallback), SEO-friendly URLs (slugs) & metadata, roles & permissions, preview/draft, webhooks → rebuild (SSG/ISR)

## Key questions

1. 🔴 CORE What happens when you type a URL and press Enter?
2. 🔴 CORE `Cache-Control: no-cache` vs `no-store`. How do you cache a hashed JS bundle vs `index.html`?
3. 🔴 CORE Cookies vs JWT in localStorage: which is safer and why?
4. 🔴 CORE What is CORS, who enforces it, when is a preflight sent?
5. 🔴 CORE XSS vs CSRF: how does each work and how do you defend?
6. 🔴 CORE CSR vs SSR vs SSG vs ISR: pick one for a blog, a dashboard, an e-shop.
7. 🟡 SHALLOW What is hydration and what causes hydration mismatches?
8. 🟡 SHALLOW WebSocket vs SSE vs polling.
9. 🟡 SHALLOW What are Core Web Vitals and how would you improve LCP / INP / CLS?
10. 🟡 SHALLOW How does a service worker enable offline mode?

## Hands-on

- [ ] ⚪ SKIP DevTools Network on my project: find a preflight request, a 304, the cache headers of the JS bundle
- [ ] ⚪ SKIP `curl -v https://example.com`: read the TLS + headers
- [ ] ⚪ SKIP Lighthouse + Performance panel on my project; write 3 concrete improvements
- [ ] ⚪ SKIP Pass 2: add `vite-plugin-pwa` to the lab repo, make it installable + offline

## Links

- [MDN: HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) · [HTTP caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching) · [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [patterns.dev: Rendering patterns](https://www.patterns.dev/react/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) · [OWASP Cheat Sheets (XSS, CSRF)](https://cheatsheetseries.owasp.org/)
- [web.dev: Core Web Vitals](https://web.dev/articles/vitals) · [web.dev Learn PWA](https://web.dev/learn/pwa)
