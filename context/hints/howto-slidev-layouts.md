# How to build effective layouts in Slidev

This guide distills practical ways to compose clean, responsive Slidev slides using built‑in layouts, slot sugar, and utility‑first CSS (UnoCSS/Tailwind‑like classes). Includes patterns, tips, and concise examples you can copy‑paste.

## Quick start: use built‑in layouts via frontmatter

Add a layout in the slide’s frontmatter. Common options from the built‑ins:

```md
---
layout: cover        # full-bleed title slide
---

# My Talk Title
Subtitle / Name
```

```md
---
layout: image-left   # image on left, content on right
---

![w:600](./public/img/diagram.png)

## Key points
- Concise bullets
- Contrast text vs background
```

```md
---
layout: image-right  # image on right, content on left
---

![w:600](./public/img/example.png)

Some content on the left.
```

```md
---
layout: image        # image as the main content
---

![bg contain](./public/img/hero.png)
```

```md
---
layout: iframe-left  # webpage on left, text on right
---

<iframe src="https://example.com" />

### Notes
Explain what the audience should look at.
```

```md
---
layout: iframe-right # webpage on right, text on left
---

<iframe src="https://example.com" />

Key commentary on the left side.
```

```md
---
layout: two-cols     # split into two columns
---

::left::
- Left column content
- Bullets / images / code

::right::
1. Right column content
2. Complementary visuals
```

```md
---
layout: none         # fully custom – you control everything
---

<div class="grid grid-cols-2 gap-6">
  <div>Left</div>
  <div>Right</div>
</div>
```

Sources: Built‑in Layouts, see links below.

## Two‑column slides: slot sugar patterns

The `two-cols` layout exposes named slots using “slot sugar,” which keeps your Markdown tidy.

```md
---
layout: two-cols
---

## Title above the columns

::left::
### Key ideas
- Point A
- Point B

::right::
![w:500](./public/img/chart.png)
```

Tips
- Put a brief “thesis” or mini‑heading above the columns to orient the audience.
- Keep textual density low—use the right column for visual support.

Advanced: nest structure inside each slot

```md
---
layout: two-cols
---

::left::
<div class="flex flex-col gap-3">
  <div class="font-bold">Concept</div>
  <ul class="list-disc pl-6 opacity-90">
    <li>Definition</li>
    <li>Contrast</li>
  </ul>
</div>

::right::
<div class="grid grid-cols-2 gap-4 items-center">
  <img class="rounded shadow" src="./public/img/a.png" />
  <img class="rounded shadow" src="./public/img/b.png" />
</div>
```

## Utility‑CSS layouts: grid and flex (UnoCSS/Tailwind‑like)

Slidev ships with UnoCSS by default. You can use utility classes directly in HTML blocks inside your Markdown to build layouts fast.

Two columns with equal gap

```md
<div class="grid grid-cols-2 gap-6">
  <div>
    <h3>Left</h3>
    <p>Short, scannable content.</p>
  </div>
  <div>
    <h3>Right</h3>
    <p>Complementary visuals or code.</p>
  </div>
  
</div>
```

Responsive column switch (1 col on small, 2 cols on medium+)

```md
<div class="grid grid-cols-1 md:grid-cols-2 gap-6">
  <div>Item 1</div>
  <div>Item 2</div>
</div>
```

Auto‑sizing columns with `grid-cols-[...]`

```md
<div class="grid grid-cols-[1.2fr_1fr] gap-6 items-start">
  <div>Wider text column</div>
  <div>Narrow visual/support</div>
</div>
```

Flexbox for vertical stacking with responsive row switch

```md
<div class="flex flex-col md:flex-row gap-6 items-start">
  <div class="flex-1">Block A</div>
  <div class="flex-1">Block B</div>
</div>
```

Centering content

```md
<div class="grid place-items-center h-[70vh]">
  <div class="text-center">
    <h2 class="text-3xl font-bold">Centerpiece</h2>
    <p class="opacity-80">One key message.</p>
  </div>
</div>
```

Side‑by‑side image + caption

```md
<div class="grid grid-cols-2 gap-4 items-center">
  <img class="w-full rounded" src="./public/img/result.png" />
  <div class="text-sm opacity-85">
    <strong>Figure:</strong> Brief caption that explains the takeaway.
  </div>
</div>
```

Using UnoCSS attributify (optional, if enabled)

```md
<div grid="~ cols-2 gap-6" items="center">
  <div>Left</div>
  <div>Right</div>
</div>
```

## Practical layout tips and tricks

- Reduce text per slide; prefer diagrams and simple sequences.
- Use `gap-*` generously for breathing room; 16–24px gaps read well at distance.
- Balance columns: widen text column via `grid-cols-[1.2fr_1fr]` or `flex-[1.2]`.
- Constrain content width for readability: wrap text in `max-w-[60ch]`.
- For dense visuals, group with `flex flex-wrap gap-4` to avoid edge‑to‑edge clutter.
- Ensure contrast: darken images (e.g., `opacity-80`) or use a subtle overlay when placing text over visuals.
- Hide non‑essential detail on small screens: `hidden md:block` for large figures.
- If a layout is “just different enough,” prefer `layout: none` and compose with grid/flex.

## Patterns you’ll reuse

Header + two columns

```md
---
layout: two-cols
---

### Why this matters

::left::
<div class="max-w-[60ch] space-y-3">
  <p>Short, persuasive paragraph.</p>
  <ul class="list-disc pl-6">
    <li>Benefit A</li>
    <li>Benefit B</li>
  </ul>
</div>

::right::
<div class="grid grid-cols-2 gap-3">
  <img class="rounded shadow" src="./public/img/1.png" />
  <img class="rounded shadow" src="./public/img/2.png" />
</div>
```

Three cards in a row (responsive)

```md
<div class="grid grid-cols-1 md:grid-cols-3 gap-6">
  <div class="p-4 rounded border">
    <h4 class="font-semibold">Card 1</h4>
    <p class="opacity-80">Summary</p>
  </div>
  <div class="p-4 rounded border">
    <h4 class="font-semibold">Card 2</h4>
    <p class="opacity-80">Summary</p>
  </div>
  <div class="p-4 rounded border">
    <h4 class="font-semibold">Card 3</h4>
    <p class="opacity-80">Summary</p>
  </div>
</div>
```

Image with callouts

```md
<div class="relative">
  <img class="w-full" src="./public/img/system.png" />
  <div class="absolute top-6 left-6 bg-black/60 text-white px-3 py-2 rounded">
    1. Input stage
  </div>
  <div class="absolute bottom-6 right-6 bg-black/60 text-white px-3 py-2 rounded">
    2. Output stage
  </div>
  
</div>
```

## References and further reading

- Slidev Built‑in Layouts: https://sli.dev/builtin/layouts
- Slot Sugar for Layouts (e.g., `two-cols`): https://sli.dev/features/slot-sugar
- FAQ: positioning, CSS grids & flexboxes in Slidev: https://sli.dev/guide/faq
- Configure UnoCSS in Slidev (defaults & options): https://sli.dev/custom/config-unocss
- Tailwind docs for grid and flex utilities (syntax reference that maps well to UnoCSS)
  - Grid template columns: https://tailwindcss.com/docs/grid-template-columns
  - Grid column spanning: https://tailwindcss.com/docs/grid-column
  - Flex direction: https://tailwindcss.com/docs/flex-direction

Notes
- Slidev uses UnoCSS by default (v0.42+). Utility class names shown above follow Tailwind‑like conventions supported by UnoCSS. If you prefer attributify syntax, enable it in your UnoCSS config.
- Built‑in layouts may vary by theme; check your theme’s docs for any extras.
