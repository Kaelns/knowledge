# 05 · HTML, CSS, Preprocessors, Methodologies, a11y

Matrix (A2): **HTML: Advanced** · **CSS: Advanced** (both Core) · CSS Preprocessors, CSS Methodologies, CSS Frameworks, Cross-browser markup: Intermediate · Frontend Accessibility: Novice.
My stack: Tailwind + shadcn/ui (Radix primitives).

**Pass 1 priority.** P1: specificity/cascade, flex vs grid, positioning + stacking context, semantic HTML, forms, responsive/media queries, BEM vs CSS Modules vs Tailwind. P2: custom properties/theming, Sass `@use`/mixins, a11y basics, cross-browser (Autoprefixer, `@supports`). P3: rare tags, container queries, CSS-in-JS details.

## Checklist

### HTML (A2 Advanced)
📚 Old notes: [[Core js 1#Types of DOM events|Form / document events]] · [[Core js 2#Critical rendering path|Parsing & render-blocking]]
- [ ] 🟡 SHALLOW (rusty) Document structure, `<!doctype>`, `<head>`: `charset`, **meta viewport**, SEO/OG meta, `<link rel=preload|preconnect|stylesheet>`
- <!DOCTYPE html> - is needed to say for browser to work in standard not legacy way
- [x] 🟡 SHALLOW Including CSS (inline / `<style>` / external) and JS (inline / external, `defer`/`async`/`module`), render-blocking resources
- [ ] 🔴 CORE **Semantic HTML5**: `header/nav/main/article/section/aside/footer`, `figure`, `time`, `dialog`, `details/summary`; headings hierarchy
- **figure** + **figcaption** - container for **autonomous content** images or other (code, diagrams) with comment (optional, 1st or last)
- [ ] 🟡 SHALLOW Forms: `label for`, `fieldset/legend`, HTML5 input types (`email`, `number`, `date`, `range`, `color`, `search`, `tel`, `url`), attributes (`required`, `pattern`, `min/max`, `autocomplete`, `inputmode`), constraint validation
- [ ] ⚪ SKIP Tables: `thead/tbody/tfoot`, `th scope`, `caption`, `colspan/rowspan`
- [ ] ⚪ SKIP Rare-but-asked: `<picture>`/`srcset`/`sizes`, `loading="lazy"`, `<template>`, `<slot>`, `data-*` attributes, `contenteditable`
- [ ] 🟡 SHALLOW "HTML in testing efforts" (matrix wording): stable selectors (`data-testid`, roles), semantic markup makes tests easier

### CSS (A2 Advanced)
📚 Old notes: [[Core js 2#Critical rendering path|Critical rendering path]] · [[Core js 2#Layout, Paint and Composite|Reflow / repaint / composite, will-change]]
- [ ] 🔴 CORE **Cascade**: origin → `!important` → `@layer` → specificity → order; **specificity** calculation (0-1-0-0), `:where()` (0) vs `:is()`
- is and where
	- Zero-specificity reset (`:where`) (or use @layout)
	- great combinators
	- forgive missing comma
- [ ] 🟡 SHALLOW Inheritance: inherited vs non-inherited props, `inherit`/`initial`/`unset`/`revert`
- [ ] 🔴 CORE Box model, `box-sizing`, margin collapse, BFC (what creates one)
- [ ] 🔴 CORE **Flexbox** (grow/shrink/basis, alignment) and **Grid** (`fr`, `minmax`, `auto-fit` vs `auto-fill`, areas, subgrid); when to use which
- [ ] 🔴 CORE **Positioning**: static/relative/absolute/fixed/sticky; containing block
- [ ] 🔴 CORE **Stacking context**: what creates one (`z-index`+position, `opacity<1`, `transform`, `isolation`…), why `z-index: 9999` doesn't work
- [ ] ⚪ SKIP Overflow control, `text-overflow: ellipsis`, `line-clamp`, scroll snap
- [ ] 🟡 SHALLOW Pseudo-classes (`:hover :focus-visible :has() :not() :nth-child()`), pseudo-elements (`::before ::after ::placeholder`)
- ```css
.tree-root:has(.blue:hover) .purple {}
```
- [ ] 🔴 CORE Responsive: media queries & breakpoints, mobile-first, units (`rem em % vw dvh ch`), `clamp()`, **container queries** (+)
- [ ] 🟡 SHALLOW **Custom properties** & theming (light/dark via `[data-theme]` / `prefers-color-scheme`): this is how shadcn themes work
- [ ] 🟡 SHALLOW Transitions vs animations, `transform`/`opacity` for GPU-friendly animations, `will-change`
- [ ] 🟡 SHALLOW Style performance: selector cost (minor), reflow vs repaint, `content-visibility`, critical CSS, unused CSS
- [ ] 🟡 SHALLOW Modern: nesting in native CSS, `@layer`, `@supports`, logical properties
- [ ] ⚪ SKIP Debugging: DevTools computed tab, toggle states, flex/grid overlays, layers panel

### Preprocessors (A2 Intermediate)
- [ ] 🟡 SHALLOW Sass/SCSS: variables, nesting (`&`), mixins (`@mixin/@include`), functions, `@extend` (and why to avoid), partials, **`@use`/`@forward`** (vs deprecated `@import`), maps, `@if/@else`, `@each/@for`
- [ ] 🟡 SHALLOW Sass vars (compile-time) vs CSS custom props (runtime)
- [ ] ⚪ SKIP Compile errors: undefined variable/mixin, module namespace issues; PostCSS vs preprocessor (Tailwind is a PostCSS plugin / Vite plugin)

### Methodologies (A2 Intermediate: advanced in one + compare)
- [ ] 🔴 CORE **BEM** deep: block / element / modifier, naming, no nesting by structure, mixes
- [ ] 🟡 SHALLOW Compare: OOCSS (separate structure/skin), SMACSS (base/layout/module/state/theme), ITCSS (inverted triangle by specificity), Atomic/utility CSS (Tailwind)
- [ ] 🔴 CORE **CSS Modules** (local scope by hashing, `composes`) vs **CSS-in-JS** (styled-components/emotion: runtime cost, SSR issues; zero-runtime: vanilla-extract, Panda)
- [ ] 🔴 CORE Why Tailwind for us: design tokens via config, no naming, purge → small CSS; trade-offs (long class lists → `cva`, `cn`/`tailwind-merge`)

### CSS frameworks (A2 Intermediate)
- [ ] 🟡 SHALLOW Bootstrap-style (components + grid) vs utility-first (Tailwind) vs headless (Radix) + copy-in (shadcn)
- [ ] 🟡 SHALLOW Tailwind: responsive prefixes, state variants, `@apply`, theme customization (v4 CSS-first `@theme`), dark mode, arbitrary values
- [ ] 🟡 SHALLOW Conflicts between framework and custom CSS: specificity, order, `@layer`, `tailwind-merge`

### Cross-browser (A2 Intermediate)
📚 Old notes: [[Core js 1#Polyfills|Polyfills]] · [[Core js 2#Navigator|User agent detection]]
- [ ] 🟡 SHALLOW Browserslist target, **Autoprefixer** (vendor prefixes automatically), `@supports` fallbacks, polyfills vs transpiling
- [ ] 🟡 SHALLOW Can I Use, Baseline; testing in Chrome/Firefox/Safari + mobile (BrowserStack, Playwright projects, DevTools device mode)
- [ ] ⚪ SKIP Safari quirks (`100vh` → `dvh`, date inputs, flex gap history); normalize/reset CSS

### Accessibility (A2 Novice, but easy wins)
- [ ] 🟡 SHALLOW WCAG 2.2 levels A/AA/AAA, POUR principles; WAI-ARIA: first rule of ARIA ("don't use ARIA if native HTML works")
- [ ] 🟡 SHALLOW Semantic tags & implicit roles, landmarks, accessible name (`label`, `aria-label`, `aria-labelledby`), `alt` text
- [ ] 🟡 SHALLOW Keyboard: focus order, `tabindex` 0 / -1, focus trap in modals, `:focus-visible`, skip links
- [ ] ⚪ SKIP Color contrast (4.5:1), `prefers-reduced-motion`
- [ ] ⚪ SKIP Screen readers: VoiceOver (Mac: `Cmd+F5`), NVDA; tools: axe DevTools, Lighthouse, WAVE, eslint-plugin-jsx-a11y; RTL role queries test a11y for free
- [ ] 🟡 SHALLOW Why Radix/shadcn help: ARIA patterns already implemented

## Key questions

1. 🔴 CORE How is specificity calculated? What wins: `#id` or `.a.b.c.d`? Where does `@layer` fit?
2. 🔴 CORE What is a stacking context and how do you fix a modal hidden under a header?
3. 🔴 CORE Flex vs Grid: give a case for each.
4. 🔴 CORE Centering an element: 3 ways.
5. 🔴 CORE BEM vs CSS Modules vs Tailwind vs CSS-in-JS: pros/cons.
6. 🟡 SHALLOW Sass variables vs CSS custom properties.
7. 🟡 SHALLOW How do you implement theming / dark mode?
8. 🟡 SHALLOW Reflow vs repaint: what triggers each?
9. 🟡 SHALLOW How would you make a custom dropdown accessible?
10. 🟡 SHALLOW How do you ensure cross-browser support?

## Hands-on

- [ ] ⚪ SKIP Build a responsive card grid with `grid-template-columns: repeat(auto-fit, minmax(…))` + a sticky header, no framework
- [ ] ⚪ SKIP Reproduce a stacking-context bug (transform on parent) and fix it
- [ ] ⚪ SKIP Small SCSS file with `@use`, a map of breakpoints and a `respond-to` mixin with `@each`
- [ ] ⚪ SKIP Navigate one page of my project with only the keyboard + VoiceOver; run axe

## Links

- [web.dev Learn CSS](https://web.dev/learn/css) · [web.dev Learn Accessibility](https://web.dev/learn/accessibility) · [web.dev Learn HTML](https://web.dev/learn/html)
- [MDN: Stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context)
- [Sass basics](https://sass-lang.com/guide/) · [getbem.com](https://getbem.com/)
- [WAI-ARIA Authoring Practices (patterns)](https://www.w3.org/WAI/ARIA/apg/patterns/)
