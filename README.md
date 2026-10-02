# Henna By Ayushi — site

`index.html` — landing page (UGC-portfolio reference layout, brand sage palette).
`cones.html` — shop page: organic cone and bulk paste pricing.
`index-v1-sage.html` — an earlier, different layout built from the guidelines.

Serve locally: `node "C:/Claude Code/serve.mjs" ./henna-by-ayushi`

## Palette (from the brand guidelines)
| Role | Hex |
|---|---|
| Page ground — the 60 | Raw Paper `#F6F7F2` |
| Lighter band / cards | `#FFFFFF` |
| Fills: cards, badges, pill — the 30 | Tint 30 `#D2D9CB` |
| Accent: script words, pill hover — the 10 | Ayushi Sage `#9CA88C` |
| Text | Henna Ink `#1F2421` |
| Muted text | `#6D7762` |
| Hairlines | `#E1E6DC` |
| CTA band | Deep Leaf `#2E3B30` |

Type: Playfair Display (headlines), Parisienne (script accents), Caveat
(handwritten bio), Jost (labels/UI). Faint blueprint grid over the ground.

## Process section
Three cards under "From leaf to cone" sit as flat brand-colour blocks and reveal
their photo on hover (tap on touch, focus for keyboard). Copy is a draft —
adjust it to match the real process.

## cones.html — shop
Every item has a quantity stepper and an Add button; a floating Cart button
opens a drawer with line items, editable quantities, a running total, and
"Send this order" (a pre-filled mailto). The cart persists in `localStorage`.

Taking card payments needs a payment provider — see "Payments" below.

## cones.html — links
Reached from three places on the landing page: the "Shop cones" nav link, the
button beside the Organic Cones heading, and the "Shop cones" pill in the
closing CTA band. Carries the full price list from the guidelines — single
cones, 20 g and 25 g bundles, bulk paste — and an order CTA (mailto with a
pre-filled subject, plus Instagram DM).

## Logo
`assets/logo.png` is the full mark, extracted from the brand-guidelines PDF.
`assets/logo-watermark.png` is the same mark as white linework on transparency,
used at 20% behind the closing CTA band.

## Images
All photos live in `assets/`, resized to 1100–1600px and re-encoded at 82–84%
with EXIF rotation baked in. Gallery and below-fold images are lazy-loaded.

## Before launch
1. All image slots now use real photography — no placeholders left.

2. Replace the three placeholder testimonials with real quotes and names.
3. The five reel cards play real Instagram videos from `assets/reel-1..5.mp4`
   (compressed to 540px wide; posters are `reel-N.jpg`).

## Payments
The cart is a complete order flow, but it does not charge cards — that needs a
payment provider and an account, which the site does not have. The checkout
sends an itemised order by email so payment can be arranged directly.

To take payment on the page, the usual options are:
- **Stripe Payment Links** — one link per product, no backend. Simplest.
- **Stripe Checkout** — needs a small server endpoint to create sessions.
- **Shopify Buy Button** or **Square Online** — hosted cart and checkout.

With an account set up, the Add buttons can hand off to any of these.

## Build config (for rebuilding assets/tailwind.css)
`tailwind.config.js`
```js
module.exports = {
  content: ["index.html", "cones.html"],
  theme: { extend: {
    colors: { cream:'#F6F7F2', shell:'#FFFFFF', blush:'#D2D9CB', rose:'#9CA88C',
              ink:'#1F2421', mute:'#6D7762', line:'#E1E6DC' },
    fontFamily: { serif:['"Playfair Display"','Georgia','serif'],
                  hand:['Caveat','cursive'], script:['Parisienne','cursive'],
                  ui:['Jost','Avenir','sans-serif'] }
  }}
}
```
`input.css` is just the three `@tailwind base; components; utilities;` lines.

## Deploying
See `DEPLOY.md`.
