# Snugglez — Decisions Log

Started 2026-09-30. Every choice made before and during the build is recorded here.

## Setup
| # | Decision | Choice | Notes |
|---|---|---|---|
| 1 | Theme folder | `C:\Shopify\snugglez-theme` | Outside OneDrive to avoid sync conflicts with Git and `.shopify/`. |
| 2 | Unused connectors | Leave all on | Claude will not use any tool outside the Connector Stack (Rule 10). |
| 3 | GitHub | Account `noahdressler2-prog` | Repo: private `github.com/noahdressler2-prog/snugglez-theme`. Noah handles all logins. |
| 3b | Brand name | **Snugglez** (was Snuggles) | Changed 2026-09-30. File prefix `snugglez-`, CSS scope `.snugglez`; color tokens keep `--snug-`. Email unchanged: `snuggless.hello@gmail.com`. |
| 4 | Dev MCP install method | Project `.mcp.json` | The `claude` CLI isn't installed (desktop app), so the S7 command is replaced by this file. |

## Brand
| # | Decision | Choice | Notes |
|---|---|---|---|
| 5 | Heading serif | Cormorant | Verify in Shopify font library. Mobile mitigation: headings only, weight 500+, min ~28px on mobile; body stays sans. |
| 6 | Blush-deep contrast | New token `--snug-rose-text: #9A4A5C` for links/small text; `#C9798A` decorative only | #9A4A5C: 5.57 on cream, 4.90 on cream-dark, 5.99 on white — all pass AA. Fails on blush (4.00), so text on blush is always cocoa (7.63). |
| 5b | Body sans | Jost | Verify in Shopify font library; fallback Assistant (Sense default). |
| 7 | Secondary button | Champagne 1px border + blush fill on hover/focus + visible focus ring | Champagne border alone is 1.84:1 on cream; the cocoa text (10.62:1) identifies the button. |

## Scope
| # | Decision | Choice | Notes |
|---|---|---|---|
| 8 | Hole 4 (grid / PDP / variant picker) | Placeholder this build | Build only `snugglez-product-grid-hole.liquid`. Sense product and collection files untouched. |
| 8b | Portrait canvases | Tile only, no upload UI | Custom orders by email until a personalizer app is approved ($50 rule). |
| 9 | Store address | `xsqnkn-r2.myshopify.com` | Confirmed at S4 (2026-09-30). |
| 9b | DRAFT_ID | `192152568125` (Sense, unpublished) | Live theme is Horizon `192152305981` — NEVER push to it. |

## Business (admin checklist — Noah does these in Shopify admin)
| # | Decision | Choice | Notes |
|---|---|---|---|
| 10 | Small-order shipping | Flat $5.00 for $0.00–$24.99 | Replaces $1.88 (12.50 × 0.15). |
| 11 | Large-order shipping | Tiers extended to $400 | See final table below. |
| 12 | Public email | Keep `snuggless.hello@gmail.com` for launch | Revisit a domain email once a domain is bought ($50 rule). |
| 13 | Grok bot API access | Noah, after theme setup (checklist A4) | Product scopes only, no theme access; products created as Draft. |

### Final shipping tiers (checklist A1)
| Order subtotal | Math | Charge |
|---|---|---|
| $0.00–$24.99 | Flat minimum | $5.00 |
| $25.00–$49.99 | $37.50 × 0.15 | $5.63 |
| $50.00–$74.99 | $62.50 × 0.15 | $9.38 |
| $75.00–$99.99 | $87.50 × 0.15 | $13.13 |
| $100.00–$149.99 | $125.00 × 0.15 | $18.75 |
| $150.00–$199.99 | $175.00 × 0.15 | $26.25 |
| $200.00–$299.99 | $250.00 × 0.15 | $37.50 |
| $300.00–$399.99 | $350.00 × 0.15 | $52.50 |
| $400.00 and up | $400.00 × 0.15 (floor) | $60.00 |

Remaining gap: orders above $400 still pay $60 (e.g. $600 → $60, not $90). Charges still rise with each tier ($5.00 < $5.63 < …).
