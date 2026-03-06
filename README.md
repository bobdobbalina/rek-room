# rek-room

A zero-dependency CSS starting point for styling HTML primitives and simple components
to match a brand aesthetic. Pure CSS — no JavaScript required.

## Philosophy

- **Style everything but layout** — base styles, typography, forms, and components are handled; you bring the layout
- **Custom property driven** — override any design token by setting a CSS custom property
- **No specificity wars** — the entire system uses `@layer`, so your overrides always win
- **Fluid by default** — typography and spacing scale smoothly across viewports
- **Dark mode included** — automatic light/dark theming via `light-dark()` and `color-scheme`
- **Scalable** — change `html { font-size }` to proportionally scale everything

## Quick Start

### Use in your project

```bash
npm install rek-room
```

```css
/* Import the bundled CSS */
@import "rek-room";

/* Or import individual parts */
@import "rek-room/reset";
@import "rek-room/tokens";
```

### Develop locally

1. `npm install`
2. `npm run dev` — starts Vite dev server with CSS hot reload
3. Open `http://localhost:5173` to see the styleguide
4. `npm run build` then copy `dist/style.css` to your project

## Customization

### Override tokens

Because the system uses `@layer`, any CSS you write without `@layer`
automatically takes precedence:

```css
:root {
  --color-primary: #ff6600;
  --font-family: "My Custom Font", sans-serif;
  --border-radius: 0;
}
```

### Type scale

Change a single variable to switch the entire typographic scale:

```css
:root {
  --type-ratio: 1.333; /* Perfect Fourth */
}
```

Available ratios: 1.067 (Minor Second), 1.125 (Major Second), 1.200 (Minor Third), 1.250 (Major Third, default), 1.333 (Perfect Fourth), 1.414 (Augmented Fourth), 1.500 (Perfect Fifth), 1.618 (Golden Ratio).

### Custom fonts

Edit `app/css/tokens/fonts.css` to add `@font-face` declarations,
then update the font stack tokens in `app/css/tokens/core.css`.

### Scale everything

All tokens use `rem` units. Adjust the root font-size to scale:

```css
html { font-size: 14px; }  /* smaller */
html { font-size: 18px; }  /* larger */
```

### Dark mode

Automatic light/dark theming works via `color-scheme` and `light-dark()`. To add a manual toggle, set the `data-theme` attribute on `<html>`:

```js
// Force dark mode
document.documentElement.setAttribute('data-theme', 'dark');

// Force light mode
document.documentElement.setAttribute('data-theme', 'light');

// Follow system preference (default)
document.documentElement.removeAttribute('data-theme');
```

### Breakpoint tokens

Named breakpoints are available as `@custom-media` tokens, resolved at build time by Lightning CSS:

```css
@media (--sm)  { /* 640px+  — small tablets */ }
@media (--md)  { /* 768px+  — tablets */ }
@media (--lg)  { /* 1024px+ — laptops */ }
@media (--xl)  { /* 1280px+ — desktops */ }
@media (--2xl) { /* 1536px+ — large screens */ }
```

> **Note:** `@custom-media` is a draft spec. Lightning CSS resolves these into standard `@media (min-width: ...)` queries at build time.

## Build

- `npm run dev` — Vite dev server with CSS HMR (instant style updates, no page reload)
- `npm run build` — bundle + minify via Lightning CSS → `dist/style.css`
- `npm run lint` — run Stylelint on all source CSS
- `npm run lint:fix` — auto-fix Stylelint issues

## Architecture

```
@layer reset       → Minimal reset + OpenType normalization
@layer tokens      → Design tokens (colors, typography, spacing, motion)
@layer base        → HTML primitive styling (headings, links, lists, tables)
@layer components  → Interactive elements (buttons, forms, select, toggle, dialog)
@layer utilities   → Helpers (sr-only, print styles)
```

## File Structure

```
app/css/
├── style.css              Entry point (@layer declarations + imports)
├── reset.css              Minimal reset + inlined normalize-opentype
├── tokens/
│   ├── core.css           Design tokens (colors, type scale, spacing, motion)
│   ├── fonts.css          @font-face templates + font stack config
│   └── breakpoints.css    @custom-media breakpoint tokens
├── base/
│   ├── typography.css     Headings, links, blockquotes, code
│   ├── layout.css         html/body foundations
│   ├── media.css          Images, figures, figcaptions
│   ├── lists.css          List reset + .bullets restore class
│   └── tables.css         Table styling
├── components/
│   ├── buttons.css        Buttons with component-scoped variants
│   ├── forms.css          Inputs, textareas, validation feedback
│   ├── select.css         Two-tier select (base + appearance: base-select)
│   ├── toggle.css         Pure CSS toggle switch
│   ├── scrollbars.css     Standards + WebKit scrollbar styling
│   ├── dialog.css         Native <dialog> with animations
│   ├── details.css        <details>/<summary> accordion
│   ├── progress.css       <progress> and <meter> styling
│   └── popover.css        Popover API styling with animations
└── utilities.css          .sr-only, print styles, content-visibility
```

## Modern CSS Features Used

- `@layer` — cascade control, zero specificity wars
- `light-dark()` — automatic dark mode theming
- `pow()` — computed type scale from a single ratio
- `clamp()` — fluid typography and spacing
- CSS nesting — co-located responsive rules
- `:has()` — parent-aware selectors (toggle, validation)
- `:user-invalid` / `:user-valid` — JS-free form validation
- `appearance: base-select` — fully customizable `<select>`
- `@starting-style` — entry/exit animations for dialog, select, and popover
- `@custom-media` — named breakpoint tokens resolved at build time
- `@property` — smooth custom property transitions
- Relative color syntax — `oklch(from ...)` color manipulation
- `text-box` — heading whitespace trimming
- `lh` units — line-height-relative vertical rhythm
- Logical properties — RTL-ready throughout
- `interpolate-size: allow-keywords` — animate to/from auto

## Browser Support

Targets modern browsers (Baseline 2024+):
Chrome/Edge 123+, Firefox 128+, Safari 17.5+.
