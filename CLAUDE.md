# CLAUDE.md — Snugglez Theme

## PROJECT IDS (verify before every push)
- Store: `xsqnkn-r2.myshopify.com`
- DRAFT_ID: `192152568125` (Sense, unpublished). This is the ONLY push target.
- Live theme: Horizon `192152305981`. Never push, pull-over, or publish to it.
- GitHub: `https://github.com/noahdressler2-prog/snugglez-theme` (private)

## DECISIONS OVERRIDE
`DECISIONS.md` in this folder records choices made after this spec was written (fonts Cormorant + Jost, `--snug-rose-text: #9A4A5C` for links/small text, secondary button hover, Hole 4 stays a placeholder, shipping tiers). Read it at the start of every stage. Where it conflicts with the sections below, DECISIONS.md wins.

## WINDOWS NOTES
- Terminal is Windows PowerShell 5.1: no `&&`. Give the user commands that work in PowerShell (e.g. `git -C "<path>" ...`).
- In Claude's PowerShell calls, prepend `$env:Path += ";$env:APPDATA\npm"` so `shopify` resolves.
- Git: `core.autocrlf=false` in this repo. Keep it.
## HARD RULES (never break these)

1. **Never publish.** Never run `shopify theme publish`. Never use `--publish`, `--live`, or `--allow-live`. Only push to the draft theme ID I approve. Before every push, print the target theme ID and role, and stop if the role is `live`.
2. **Never buy anything.** Do not install a paid theme or app. If something truly needs one, stop and tell me the name and monthly cost. We have a $50 approval rule.
3. **Never enter my passwords or log in for me.** When Shopify CLI or GitHub opens a browser login, I do it myself. Never ask me to paste a password, token, or API key into this chat.
4. **Do not invent anything.** That covers products, prices, reviews, ratings, testimonials, discounts, "free shipping" claims, "best seller" badges, a logo, and customer counts. Use clearly marked placeholders.
5. **Do not write or change legal policies.** Refund and shipping policies are already saved. Do not touch privacy or terms. Link to policies only through Shopify's `shop.policies` objects.
6. **Do not change store data.** Do not create, edit, or publish products, collections, menus, shipping rates, or settings through any API. For admin tasks, give me click-by-click instructions and I do them.
7. **Only one contact detail.** The email is the only contact detail. No phone number and no street address anywhere.
8. **Pull before you edit.** Run `shopify theme pull --theme <DRAFT_ID>` at the start of every build session.
9. **Keep Sense upgradeable.** Put new work in files prefixed `snugglez-`. Only edit Sense core files where this prompt allows it, and list every core edit in your report.
10. **Stay inside the connector stack.** Use only the tools listed in the Connector Stack section below.

## CONNECTOR STACK (all free, $0 total)

| Layer | Tool | Purpose |
|---|---|---|
| Builder | Claude Code (you) | Write and edit the theme files |
| Store link | Shopify CLI | Pull, preview, check, and push the draft theme |
| Shopify knowledge | Shopify Dev MCP (official, `@shopify/dev-mcp`) | Look up current Shopify and Liquid docs before writing Liquid. It needs no store access. |
| Undo button | Git and a private GitHub repo | Keep version history so any change can be rolled back |

**Do not use:**
- Store Admin MCPs or connectors. They can edit products, which breaks Rule 6.
- Third-party Liquid MCPs. They duplicate the official one.
- Any other MCP servers that happen to be connected, such as Vercel, Supabase, Runway, or Higgsfield. Every loaded server adds tool definitions to every message and wastes credits.

The Dev MCP's Liquid validation is experimental. `shopify theme check` stays the official test.


## STORE FACTS

- Name: Snugglez. Public email: `snuggless.hello@gmail.com`. The double "s" is intentional and confirmed.
- United States only, US dollars, English.
- Theme copy says shipping is "calculated at checkout." Never print a shipping price or percentage.
- One-time purchases only. No subscription UI, tips, or gift cards.
- The storefront password stays on. Preview with `shopify theme dev`.
- No social accounts exist yet. Every social link and feed stays hidden until a URL is set.
- Planned categories (collections by product type): dog hoodies, dog accessories, beds and blankets, and dog portrait canvases. Do not create any of them. Only build tiles that can point at them later.

## BRAND SYSTEM

**Palette.** Put these tokens in `assets/snugglez.css`, scoped under `.snugglez`, and mirror them into a Sense color scheme.

| Token | Hex | Use |
|---|---|---|
| `--snug-cream` | `#FBF6EF` | Main background |
| `--snug-cream-dark` | `#F1E7DA` | Alternate section background |
| `--snug-blush` | `#F2C9D0` | Playful accent blocks, hover fills |
| `--snug-blush-deep` | `#C9798A` | Small accents and links (check contrast) |
| `--snug-champagne` | `#D4B47A` | Hairlines, dividers, icons, button borders |
| `--snug-cocoa` | `#4A3438` | Body text, headings, primary button fill |
| `--snug-white` | `#FFFFFF` | Cards, form fields |

- Primary button: cocoa fill with cream text. Secondary button: transparent with a 1px champagne border and cocoa text.
- Every text and background pair must pass WCAG AA (4.5:1 for body text). Report the ratios. If `--snug-blush-deep` fails on cream, darken it and tell me the new hex.

**Type.** Use Shopify font library fonts only, set through theme settings.
- Headings: an elegant serif. Check which of Playfair Display, Cormorant, or Bodoni Moda is available, using the Dev MCP or the theme's font list.
- Body: a clean, quiet sans.
- Signature: a headline can end with an *italic serif accent line*.

**Feel.** Use generous white space, 12–16px card corners, and pill-shaped buttons. Keep shadows barely visible. Motion is limited to a subtle fade or lift that respects `prefers-reduced-motion`. Build mobile-first.

**Voice ("elegant with a wink").** All default copy lives in schema settings, and every label is tagged `[EDIT]`. Sample tone:
- Hero: "Spoiled, *beautifully.*" / "Luxe comforts for the dog who owns the good side of the bed."
- Newsletter: "Join the inner circle. *Tails will wag.*"

## HOLES (clearly marked placeholders)

In the theme editor, each hole shows a dashed champagne box labeled `HOLE — <name>`. On the storefront, a hole renders nothing when it's unset. Use `request.design_mode` to tell the two apart.

1. **Logo:** use Sense's logo setting. If it's empty, show the shop name in the heading serif. Do not design a logo.
2. **Photos:** all images use `image_picker`. When empty, show a blush and cream placeholder labeled "Photo coming."
3. **Product names and prices:** these come only from Shopify objects. No fake product cards.
4. **Product grid, variant picker, product page, and collection layout:** not decided yet, and Noah decides this week. Do not restyle `main-product`, `main-collection-product-grid`, or `card-product`. Build only `snugglez-product-grid-hole.liquid`.
5. **Instagram and UGC feed:** build the shell (heading, CTA url setting, up to 6 image_picker blocks). It stays hidden unless a URL or image is set.
6. **Portrait canvases:** tile slot only. There's no upload UI, because customer uploads would need a paid personalizer app.

## FILES

**New files:**
- `assets/snugglez.css`: tokens and all styles, scoped. No `!important` unless you explain why.
- `sections/snugglez-hero.liquid`: desktop and mobile image_pickers, a heading plus italic accent, a subtext, and one primary CTA using a url setting. Height options: small, medium, full.
- `sections/snugglez-category-tiles.liquid`: `tile` blocks, each with a collection picker, an optional image, and a label. Default is 4 empty tiles: Hoodies, Accessories, Beds and blankets, Portrait canvases. Use 2 columns on mobile and 4 on desktop.
- `sections/snugglez-social-feed.liquid`: see Hole 5.
- `sections/snugglez-newsletter.liquid`: native `{% form 'customer' %}` with `contact[tags]` set to `newsletter`. Include success and error states and an accessible label. No discount offer.
- `sections/snugglez-product-grid-hole.liquid`: see Hole 4.
- `sections/snugglez-footer.liquid`: the shop name or logo, an editable brand line, a "Shop by category" link_list, a mailto link for the email only, `shop.policies` links, hidden-until-set social icons, `shop.enabled_payment_types` icons, and © current year Snugglez.

**Allowed core edits (list each one in your report):**
- `layout/theme.liquid`: add `{{ 'snugglez.css' | asset_url | stylesheet_tag }}` after the base CSS, and add the class `snugglez` to `<body>`.
- `templates/index.json`: hero, tiles, grid hole, social feed, newsletter.
- `sections/footer-group.json`: use `snugglez-footer`. Leave the original Sense footer untouched.
- `config/settings_data.json`: colors and fonts only, and only after pulling.

**Header:** keep Sense's header, because it runs the cart drawer, search, and mobile menu. Restyle it with settings and CSS only. Center the logo, point the menu at "Shop by category", and turn the announcement bar off.


## BUILD STAGES AND STOP GATES

- **Stage 0: Safety check.** Run `shopify theme list` and confirm `DRAFT_ID` and its role. **STOP.**
- **Stage 1: Plan only.** Write the section plan, file list, contrast table, and chosen font. No code yet. **STOP.**
- **Stage 2: Build.** Look up anything uncertain with the Dev MCP before writing Liquid. Create the files. Run `shopify theme check` and fix every error. Commit the work with the message "Stage 2 build."
- **Stage 3: Preview.** Run `shopify theme dev`, and I'll enter the storefront password. Check 375px and 1440px widths, with holes both empty and filled with test images. **STOP** for my batched review.
- **Stage 4: Push to draft.** Print the ID and role again, then run `shopify theme push --theme <DRAFT_ID>`. Commit and push to GitHub with the message "Stage 4 pushed to draft."

## CREDIT RULES FOR YOU

- Keep your explanations short, and show commands instead of long paragraphs.
- Don't reread the whole theme. Open only the files the current stage needs.
- When I report a bug, ask for the error message and fix only the affected file.
- End every session by telling me the exact short message to paste next time, such as "Run Stage 2."

## FINAL REPORT (end every build stage with this)

1. Files created and core files edited.
2. Every hole and where it lives.
3. Contrast ratios checked.
4. `theme check` results.
5. Anything that needs an app, money, or Noah's decision.
6. A confirmation that nothing was published and no product, price, policy, menu, or setting was changed.
