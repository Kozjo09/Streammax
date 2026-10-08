# Sentinel Security Journal

## 2026-09-28 - Incomplete DOM XSS Sanitization Across Render Functions
**Vulnerability:** Incomplete HTML escaping on user-controlled or external TMDb API data (titles, genres, IDs) injected into innerHTML strings in secondary views (`loadModalRelated`, `renderHomeHistory`, `renderHistoryPage`, `openDetailsModal`).
**Learning:** While `escapeHTML` helper was defined and used in primary view render functions, several secondary or feature-specific functions relied on partial escaping (e.g. `.replace(/"/g, '&quot;')`) or raw interpolation, leaving the application vulnerable to DOM XSS.
**Prevention:** Always pass all dynamic string interpolations in `innerHTML` template literals through `escapeHTML()`, or convert template generation to DOM node creation (`textContent`).
