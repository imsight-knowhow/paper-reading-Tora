# How to organize a Slidev project (split slides, structure, and best practices)

Purpose: Practical patterns to keep Slidev decks maintainable as they grow. Includes splitting a deck across files, reusing slides, and a clean project layout.

## TL;DR
- Split content into multiple Markdown files via frontmatter `src:` for readability and reuse.
- Keep global config in the first slide’s headmatter; keep per-slide frontmatter minimal.
- Use conventional folders: `components/`, `layouts/`, `public/`, `styles/`, `snippets/` to avoid ad‑hoc clutter.
- Extract repeated UI/content into Vue components and/or custom layouts.
- Store assets in `public/` and reference them with absolute paths like `/img.png`.
- Use the Slidev VS Code extension to manage multiple decks and slide reordering.

## Split one deck into multiple files
Use the `src` frontmatter to import slides from other Markdown files. Everything below the `---` block is ignored for that slide when `src` is present.

Example: `slides.md`

```md
---
title: My Deck
---

# Intro
Some opening content.

---
src: ./pages/agenda.md
---

---
# Closing
Thanks!
```

Example: `pages/agenda.md`

```md
# Agenda
- Part 1
- Part 2
```

Reuse specific slides from another deck or file using a hash selector:

```md
---
# Import slides 2 and 5–7 from another deck
src: ./other-deck.md#2,5-7
---
```

References:
- Importing Slides: https://sli.dev/features/importing-slides
- Syntax Guide (Importing Slides section): https://sli.dev/guide/syntax#importing-slides

## Recommended project structure
Follow Slidev’s conventional layout to reduce config and improve discoverability:

```text
your-slidev/
├─ components/   # Reusable Vue components usable in slides
├─ layouts/      # Custom slide layouts (Vue components wrapping content)
├─ public/       # Static assets served at /
├─ setup/        # Optional hooks/custom setup files
├─ snippets/     # Reusable code snippets for import
├─ styles/       # Global styles; manage order via styles/index.ts
├─ index.html    # Optional head/body injections
├─ slides.md     # Main entry (or multiple *.md entries)
└─ vite.config.ts
```

Reference: Directory Structure https://sli.dev/custom/directory-structure

## Reusable components and layouts
- Components: Put shared content blocks into `components/` and use them directly in Markdown via MDC.
- Layouts: Create consistent slide skeletons (title, columns, footers) in `layouts/`. Apply with `layout:` in frontmatter.

Examples:

```md
---
layout: quote
---
A quote with a dedicated layout
```

```vue
<!-- layouts/twocol.vue -->
<script setup lang="ts">
// optional props
</script>
<template>
  <div class="grid grid-cols-[2fr,1fr] gap-6 items-start">
    <slot />
  </div>
</template>
```

```md
---
layout: twocol
---
# Title

Left column content

::right::
Right column content
```

Layout basics: https://sli.dev/guide/layout

## Assets and paths
- Put images and downloadable files in `public/` (e.g., `public/figs/chart.png`).
- Reference with `/figs/chart.png` so paths work in dev, build, and export.
- Avoid embedding large base64 images in Markdown; keep repo light and diffs clean.

FAQ on assets: see Directory Structure for `public/` behavior https://sli.dev/custom/directory-structure#public

## Styles hygiene
Prefer a single entry that forwards multiple CSS files to control ordering:

```text
styles/
├─ index.ts
├─ base.css
├─ code.css
└─ layouts.css
```

```ts
// styles/index.ts
import './base.css'
import './code.css'
import './layouts.css'
```

Then Slidev injects styles automatically per the convention. More: https://sli.dev/custom/directory-structure#style

## Code reuse with snippets
Place shareable code in `snippets/` and import into slides (pairs well with code groups, magic move, etc.).
- Code Groups: https://sli.dev/features/code-groups
- Import code snippets: https://sli.dev/features/import-snippet

## Multiple decks in one repo
You can keep several entry files (e.g., `intro.md`, `workshop.md`, `deep-dive.md`) at the project root or within subfolders. The Slidev VS Code extension can discover them and provides a “projects” view.

VS Code extension:
- Feature page: https://sli.dev/features/vscode-extension
- Example setting to limit discovery to a specific pattern:

```jsonc
{
  // In .vscode/settings.json
  "slidev.include": ["**/presentation.md", "**/slides.md"]
}
```

## Headmatter vs per-slide frontmatter
- Headmatter (the very first `---` block) is global: theme, title, fonts, addons.
- Per-slide frontmatter only for slide-specific needs (layout, class, background, `src`, etc.).
- Keep globals in one place to avoid duplication; rely on per-slide only when needed.

Syntax and configurations:
- Syntax Guide: https://sli.dev/guide/syntax
- Configurations: https://sli.dev/custom/

## Practical naming conventions
- pages/*.md: Use prefixes to preserve reading order, e.g., `01_intro.md`, `02_method.md`, `99_appendix.md`.
- components: `XyzBlock.vue` for repeated content (e.g., `Callout.vue`, `Kpi.vue`).
- assets: group by type under `public/figs`, `public/logos`, `public/downloads`.

## Common pitfalls and tips
- Don’t mix heavy global CSS in slide Markdown; put it in `styles/` instead.
- Prefer `src:` to reuse content across decks rather than duplicating Markdown.
- For consistent headers/footers, consider global layers (`global-top.vue`, `slide-bottom.vue`). See: https://sli.dev/custom/directory-structure#global-layers
- When exporting, verify image paths render identically in dev and build.
- Keep slide files short; one logical section per file improves maintainability and review.

## Minimal working example (putting it together)

```md
# slides.md
---
title: Demo Deck
theme: seriph
---

# Cover
Welcome!

---
src: ./pages/01_intro.md
---

---
src: ./pages/02_topic.md
---

# Thanks
```

```md
# pages/01_intro.md
---
layout: default
---
# Introduction
<Callout>Key message</Callout>
```

```md
# pages/02_topic.md
---
layout: twocol
---
## Left
Some text

::right::
## Right
![Chart](/figs/chart.png)
```

```vue
<!-- components/Callout.vue -->
<template>
  <div class="border-l-4 border-primary pl-3 py-2 my-3 text-lg">
    <slot />
  </div>
</template>
```

```vue
<!-- layouts/twocol.vue -->
<template>
  <div class="grid grid-cols-[2fr,1fr] gap-6">
    <slot />
  </div>
</template>
```

## Further reading
- Getting Started: https://sli.dev/guide/
- Syntax Guide: https://sli.dev/guide/syntax
- Importing Slides: https://sli.dev/features/importing-slides
- Directory Structure: https://sli.dev/custom/directory-structure
- Layouts: https://sli.dev/guide/layout
- VS Code Extension: https://sli.dev/features/vscode-extension
