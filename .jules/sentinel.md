# Sentinel Security Journal

## 2026-10-13 - Unauthenticated Session Impersonation via Startup Query Parameter Bypass
**Vulnerability:** Hardcoded `test=true` URL query parameter check in `Animemax.html` startup sequence initialized a fake authenticated user session (`TestUser`), bypassing Supabase authentication entirely.
**Learning:** Development and debug parameter shortcuts left in production frontend initialization code bypass security controls and authentication mechanisms.
**Prevention:** Remove test/debug parameter bypass code before deploying to production, or ensure test modes are strictly gated by server-side authentication flags and non-production environment checks.

## 2026-09-28 - Incomplete DOM XSS Sanitization Across Render Functions
**Vulnerability:** Incomplete HTML escaping on user-controlled or external TMDb API data (titles, genres, IDs) injected into innerHTML strings in secondary views (`loadModalRelated`, `renderHomeHistory`, `renderHistoryPage`, `openDetailsModal`).
**Learning:** While `escapeHTML` helper was defined and used in primary view render functions, several secondary or feature-specific functions relied on partial escaping (e.g. `.replace(/"/g, '&quot;')`) or raw interpolation, leaving the application vulnerable to DOM XSS.
**Prevention:** Always pass all dynamic string interpolations in `innerHTML` template literals through `escapeHTML()`, or convert template generation to DOM node creation (`textContent`).

## 2026-03-30 - Single-Quote Escaping Gap in Inline Handler Interpolation
**Vulnerability:** Relying solely on `encodeURIComponent` for strings interpolated into single-quoted inline JavaScript attribute handlers (`onclick="foo('${val}')"`) allowed DOM XSS because `encodeURIComponent` does not encode single quotes (`'`).
**Learning:** Standard `encodeURIComponent` leaves `'` unescaped, allowing single quotes in input data to terminate single-quoted JS string literals within HTML event attributes and execute injected JavaScript.
**Prevention:** Use an explicit helper like `escapeAttrJS` (`encodeURIComponent(String(str)).replace(/'/g, '%27')`) whenever interpolating dynamic strings into single-quoted inline event attributes.
