# 05 · HTML, CSS, Preprocessors, Methodologies, a11y

Matrix (A2): **HTML: Advanced** · **CSS: Advanced** (both Core) · CSS Preprocessors, CSS Methodologies, CSS Frameworks, Cross-browser markup: Intermediate · Frontend Accessibility: Novice.
My stack: Tailwind + shadcn/ui (Radix primitives).

## Checklist

### HTML (A2 Advanced)
- [ ] Document structure, `<!doctype>`, `<head>`: `charset`, **meta viewport**, SEO/OG meta, `<link rel=preload|preconnect|stylesheet>`
- [ ] Including CSS (inline / `<style>` / external) and JS (inline / external, `defer`/`async`/`module`), render-blocking resources
- [ ] **Semantic HTML5**: `header/nav/main/article/section/aside/footer`, `figure`, `time`, `dialog`, `details/summary`; headings hierarchy
- [ ] Forms: `label for`, `fieldset/legend`, HTML5 input types (`email`, `number`, `date`, `range`, `color`, `search`, `tel`, `url`), attributes (`required`, `pattern`, `min/max`, `autocomplete`, `inputmode`), constraint validation
- [ ] Tables: `thead/tbody/tfoot`, `th scope`, `caption`, `colspan/rowspan`
- [ ] Rare-but-asked: `<picture>`/`srcset`/`sizes`, `loading="lazy"`, `<template>`, `<slot>`, `data-*` attributes, `contenteditable`
- [ ] "HTML in testing efforts" (matrix wording): stable selectors (`data-testid`, roles), semantic markup makes tests easier

### CSS (A2 Advanced)
- [ ] **Cascade**: origin → `!important` → `@layer` → specificity → order; **specificity** calculation (0-1-0-0), `:where()` (0) vs `:is()`
- [ ] Inheritance: inherited vs non-inherited props, `inherit`/`initial`/`unset`/`revert`
- [ ] Box model, `box-sizing`, margin collapse, BFC (what creates one)
- [ ] **Flexbox** (grow/shrink/basis, alignment) and **Grid** (`fr`, `minmax`, `auto-fit` vs `auto-fill`, areas, subgrid); when to use which
- [ ] **Positioning**: static/relative/absolute/fixed/sticky; containing block
- [ ] **Stacking context**: what creates one (`z-index`+position, `opacity<1`, `transform`, `isolation`…), why `z-index: 9999` doesn't work
- [ ] Overflow control, `text-overflow: ellipsis`, `line-clamp`, scroll snap
- [ ] Pseudo-classes (`:hover :focus-visible :has() :not() :nth-child()`), pseudo-elements (`::before ::after ::placeholder`)
- [ ] Responsive: media queries & breakpoints, mobile-first, units (`rem em % vw dvh ch`), `clamp()`, **container queries** (+)
- [ ] **Custom properties** & theming (light/dark via `[data-theme]` / `prefers-color-scheme`): this is how shadcn themes work
- [ ] Transitions vs animations, `transform`/`opacity` for GPU-friendly animations, `will-change`
- [ ] Style performance: selector cost (minor), reflow vs repaint, `content-visibility`, critical CSS, unused CSS
- [ ] Modern: nesting in native CSS, `@layer`, `@supports`, logical properties
- [ ] Debugging: DevTools computed tab, toggle states, flex/grid overlays, layers panel

### Preprocessors (A2 Intermediate)
- [ ] Sass/SCSS: variables, nesting (`&`), mixins (`@mixin/@include`), functions, `@extend` (and why to avoid), partials, **`@use`/`@forward`** (vs deprecated `@import`), maps, `@if/@else`, `@each/@for`
- [ ] Sass vars (compile-time) vs CSS custom props (runtime)
- [ ] Compile errors: undefined variable/mixin, module namespace issues; PostCSS vs preprocessor (Tailwind is a PostCSS plugin / Vite plugin)

### Methodologies (A2 Intermediate: advanced in one + compare)
- [ ] **BEM** deep: block / element / modifier, naming, no nesting by structure, mixes
- [ ] Compare: OOCSS (separate structure/skin), SMACSS (base/layout/module/state/theme), ITCSS (inverted triangle by specificity), Atomic/utility CSS (Tailwind)
- [ ] **CSS Modules** (local scope by hashing, `composes`) vs **CSS-in-JS** (styled-components/emotion: runtime cost, SSR issues; zero-runtime: vanilla-extract, Panda)
- [ ] Why Tailwind for us: design tokens via config, no naming, purge → small CSS; trade-offs (long class lists → `cva`, `cn`/`tailwind-merge`)

### CSS frameworks (A2 Intermediate)
- [ ] Bootstrap-style (components + grid) vs utility-first (Tailwind) vs headless (Radix) + copy-in (shadcn)
- [ ] Tailwind: responsive prefixes, state variants, `@apply`, theme customization (v4 CSS-first `@theme`), dark mode, arbitrary values
- [ ] Conflicts between framework and custom CSS: specificity, order, `@layer`, `tailwind-merge`

### Cross-browser (A2 Intermediate)
- [ ] Browserslist target, **Autoprefixer** (vendor prefixes automatically), `@supports` fallbacks, polyfills vs transpiling
- [ ] Can I Use, Baseline; testing in Chrome/Firefox/Safari + mobile (BrowserStack, Playwright projects, DevTools device mode)
- [ ] Safari quirks (`100vh` → `dvh`, date inputs, flex gap history); normalize/reset CSS

### Accessibility (A2 Novice, but easy wins)
- [ ] WCAG 2.2 levels A/AA/AAA, POUR principles; WAI-ARIA: first rule of ARIA ("don't use ARIA if native HTML works")
- [ ] Semantic tags & implicit roles, landmarks, accessible name (`label`, `aria-label`, `aria-labelledby`), `alt` text
- [ ] Keyboard: focus order, `tabindex` 0 / -1, focus trap in modals, `:focus-visible`, skip links
- [ ] Color contrast (4.5:1), `prefers-reduced-motion`
- [ ] Screen readers: VoiceOver (Mac: `Cmd+F5`), NVDA; tools: axe DevTools, Lighthouse, WAVE, eslint-plugin-jsx-a11y; RTL role queries test a11y for free
- [ ] Why Radix/shadcn help: ARIA patterns already implemented

## Key questions

1. How is specificity calculated? What wins: `#id` or `.a.b.c.d`? Where does `@layer` fit?
2. What is a stacking context and how do you fix a modal hidden under a header?
3. Flex vs Grid: give a case for each.
4. Centering an element: 3 ways.
5. BEM vs CSS Modules vs Tailwind vs CSS-in-JS: pros/cons.
6. Sass variables vs CSS custom properties.
7. How do you implement theming / dark mode?
8. Reflow vs repaint: what triggers each?
9. How would you make a custom dropdown accessible?
10. How do you ensure cross-browser support?

## Hands-on

- [ ] Build a responsive card grid with `grid-template-columns: repeat(auto-fit, minmax(…))` + a sticky header, no framework
- [ ] Reproduce a stacking-context bug (transform on parent) and fix it
- [ ] Small SCSS file with `@use`, a map of breakpoints and a `respond-to` mixin with `@each`
- [ ] Navigate one page of my project with only the keyboard + VoiceOver; run axe

## Links

- [web.dev Learn CSS](https://web.dev/learn/css) · [web.dev Learn Accessibility](https://web.dev/learn/accessibility) · [web.dev Learn HTML](https://web.dev/learn/html)
- [MDN: Stacking context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context)
- [Sass basics](https://sass-lang.com/guide/) · [getbem.com](https://getbem.com/)
- [WAI-ARIA Authoring Practices (patterns)](https://www.w3.org/WAI/ARIA/apg/patterns/)
