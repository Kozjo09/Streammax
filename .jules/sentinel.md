# Sentinel Security Journal

## 2026-10-14 - Single Quote Escape in URI Encoded Attributes
**Vulnerability:** Standard `encodeURIComponent()` does not encode single quotes (`'`), leaving string interpolations inside single-quoted inline JavaScript attributes (e.g. `onclick="openDetailsModal('${encodeURIComponent(item.id)}')"` ) vulnerable to DOM-based XSS attribute breakouts if an ID or string contains `'`.
**Learning:** `encodeURIComponent()` escapes most special characters but preserves `'`, `(`, `)`, `!`, `~`, `*`. When interpolated inside inline JavaScript event attributes wrapped in single quotes, a single quote breaks out of the JS string argument.
**Prevention:** Replace single quotes with `%27` (`.replace(/'/g, '%27')`) when preparing parameters for inline JavaScript event handlers in HTML templates.

## 2026-09-28 - Incomplete DOM XSS Sanitization Across Render Functions
**Vulnerability:** Incomplete HTML escaping on user-controlled or external TMDb API data (titles, genres, IDs) injected into innerHTML strings in secondary views (`loadModalRelated`, `renderHomeHistory`, `renderHistoryPage`, `openDetailsModal`).
**Learning:** While `escapeHTML` helper was defined and used in primary view render functions, several secondary or feature-specific functions relied on partial escaping (e.g. `.replace(/"/g, '&quot;')`) or raw interpolation, leaving the application vulnerable to DOM XSS.
**Prevention:** Always pass all dynamic string interpolations in `innerHTML` template literals through `escapeHTML()`, or convert template generation to DOM node creation (`textContent`).
