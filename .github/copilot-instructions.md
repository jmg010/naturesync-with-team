## Repo snapshot — quick orientation

- This repository is a Shopify theme (Liquid templates + assets). Key folders: `sections/`, `snippets/`, `templates/`, `assets/`, `config/`.
- Theme UI and behavior are implemented with SVG icon snippets (in `snippets/`) and JS under `assets/` (e.g. `cart-drawer.js`, `cart-notification.js`).

## What an AI agent should know to be productive

1. Icon & UI pattern: icons are SVG snippets under `snippets/` and are included using `render 'icon-name'` from Liquid.

   - Example: the cart icon is rendered from `snippets/icon-cart.liquid` and `snippets/icon-cart-empty.liquid` (see `sections/header.liquid` and `sections/cart-icon-bubble.liquid`).
   - To change the cart icon, edit those snippets or replace `render 'icon-cart'` with `render 'icon-yourcart'` and add a new snippet.

2. Cart behavior and variants:

   - The header chooses which icon to render with
     ```liquid
     {%- if cart == empty -%}
       render 'icon-cart-empty'
     {%- else -%}
       render 'icon-cart'
     {%- endif -%}
     ```
     This logic appears in `sections/header.liquid` and `sections/cart-icon-bubble.liquid`.
   - The theme exposes a `settings.cart_type` switch (e.g., `drawer` vs `notification`) — when `drawer` is used, the assets `component-cart-drawer.css` and `cart-drawer.js` control drawer behavior.

3. CSS & sizing conventions:

   - Icons use classes like `.icon`, `.icon-cart`, `.icon-cart-empty`. Size/color is controlled by CSS in `assets/*.css` (search for `.icon` or `component-*.css`).
   - Keep SVGs' `fill="currentColor"` to respect theme color rules unless intentionally overriding color.

4. Accessibility & text:

   - Keep the visually-hidden label near the icon (e.g. `<span class="visually-hidden">{{ 'templates.cart.cart' | t }}</span>`) to preserve screen-reader behavior when editing icon snippets or replacing render calls.

5. Search & cross-references (useful search terms):
   - "render 'icon-cart'", "icon-cart-empty", `cart-count-bubble`, `cart-drawer.js`, `component-cart-drawer.css`, `sections/header.liquid`.

## Practical edits: example recipes

- Replace the cart SVG while preserving behavior (recommended):

  1. Create `snippets/icon-custom-cart.liquid` with your SVG; ensure `class="icon icon-cart"` (or a new class) and `aria-hidden="true"`.
  2. Update `sections/header.liquid` (or `sections/cart-icon-bubble.liquid`) to render the new snippet:
     ```liquid
     render 'icon-custom-cart'
     ```

- Inline replacement (quick test): replace the `render 'icon-cart'` block in `sections/header.liquid` with the SVG directly, but keep the same classes and the visually-hidden label.

## Important conventions / gotchas

- Icons are SVG snippets by design — avoid replacing them with external icon fonts unless you also add the font and update CSS.
- JS that controls cart UI lives in `assets/` and is toggled by theme settings (`settings.cart_type`). If you change markup (IDs/classes) used by `cart-drawer.js` or `cart-notification.js`, update those files accordingly.
- Keep translations and accessibility spans intact (`'templates.cart.cart' | t`, `sections.header.cart_count`) when editing header/cart snippets.

## Files to inspect for larger changes

- `sections/header.liquid` — main header, renders cart icon and count bubble.
- `sections/cart-icon-bubble.liquid` — small reusable snippet used in header.
- `snippets/icon-cart.liquid` and `snippets/icon-cart-empty.liquid` — the actual SVG sources you can edit.
- `assets/cart-drawer.js`, `assets/cart-notification.js` — JS controlling cart interactions.
- `assets/component-cart.css` and `assets/component-cart-drawer.css` — styles for cart UI.

## If you need to run or preview the theme

- This repo doesn't include an explicit build script in the repo root. It's a Shopify theme — typical local preview/edit workflows use the Shopify CLI (`shopify theme serve`) or Theme Kit. Ask the repo owner if there is a preferred CLI or CI pipeline.

---

If you'd like, I can now create or update `.github/copilot-instructions.md` in the repo with this content (I will preserve any existing content if found). Tell me to proceed or give edits you want included.
