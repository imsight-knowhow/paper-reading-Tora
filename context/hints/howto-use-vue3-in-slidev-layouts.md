# How to use Vue 3 to build Slidev layouts

This guide explains how Slidev relates to Vue 3 and shows practical Vue 3 patterns to create custom layouts and interactions in Slidev.

## Slidev ↔ Vue 3: what’s the relationship

- Slidev is a Vue 3 app rendered by Vite. You can extend the Vue app and use the full Composition API, SFCs, and ecosystem.
  - Docs: Configure Vue App – https://sli.dev/custom/config-vue
- Layouts are Vue components placed in `layouts/`; components go in `components/` and can be used directly in Markdown.
  - Docs: Writing Layouts – https://sli.dev/guide/write-layout
  - Docs: Layouts – https://sli.dev/guide/layout
  - Docs: Components in Slides – https://sli.dev/guide/component
- Slidev exposes global context (navigation, configs, current slide/frontmatter) accessible in Vue templates or via composables.
  - Docs: Global Context – https://sli.dev/guide/global-context

## Quick start: create a custom Vue layout

1) Create `layouts/SplitHero.vue`:

```vue
<template>
  <section class="slidev-layout grid grid-cols-[1.2fr_1fr] gap-6 items-start">
    <header class="col-span-2">
      <!-- From current slide frontmatter or slot -->
      <h1 class="text-3xl font-bold">{{ $frontmatter.title ?? 'Untitled' }}</h1>
      <p v-if="$frontmatter.subtitle" class="opacity-70">{{ $frontmatter.subtitle }}</p>
    </header>

    <div>
      <!-- default slot = main content written in slides.md -->
      <slot />
    </div>
    <aside>
      <!-- named slot for sidebar visuals -->
      <slot name="aside" />
    </aside>
  </section>
</template>

<script setup lang="ts">
import { computed } from 'vue'
// Example: react to click steps using global context
// Prefer composables for type-safety
import { useSlideContext, useNav } from '@slidev/client'

const { $slidev } = useSlideContext()
const { clicks } = useNav()

const showHint = computed(() => clicks.value >= 2)
// showHint can drive conditional UI in the template via v-if
</script>
```

2) Use it in a slide:

```md
---
layout: SplitHero
title: Method Overview
subtitle: One slide summary
---

Key points on the left with Markdown.

::aside::
<img class="rounded shadow" src="/figure1.png" alt="diagram" />
```

Sources: Writing Layouts – https://sli.dev/guide/write-layout, Global Context – https://sli.dev/guide/global-context

## Vue 3 techniques that work great in layouts

1) Composition API inside layouts

```vue
<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

const count = ref(0)
const doubled = computed(() => count.value * 2)
onMounted(() => {/* DOM work / analytics */})
</script>

<template>
  <div>
    <button @click="count++">{{ doubled }}</button>
  </div>
</template>
```

2) Named slots + “slot sugar” in Markdown

```vue
<!-- layouts/TwoCols.vue -->
<template>
  <div class="slidev-layout grid grid-cols-2 gap-6">
    <div><slot /></div>
    <div><slot name="right" /></div>
  </div>
  <!-- default + right slots -->
  <!-- https://sli.dev/features/slot-sugar -->
</template>
```

```md
---
layout: TwoCols
---
Left content

::right::
Right content
```

3) Read frontmatter in layouts (per-slide config)

```vue
<template>
  <div class="slidev-layout" :class="$frontmatter.class">
    <h2 v-if="$frontmatter.title">{{ $frontmatter.title }}</h2>
    <slot />
  </div>
  <!-- $frontmatter is reactive for the current slide -->
</template>
```

4) Use Slidev composables for navigation/state

```vue
<script setup>
import { useNav, useDarkMode } from '@slidev/client'
const { currentPage, next, nextSlide } = useNav()
const { isDark } = useDarkMode()
</script>

<template>
  <footer class="absolute bottom-4 right-4 opacity-70 text-sm">
    Page {{ currentPage }} <span v-if="isDark">🌙</span>
    <button class="ml-3" @click="next()">Next ▶</button>
  </footer>
</template>
```

Docs: Global Context – https://sli.dev/guide/global-context

## Extending the Vue app (plugins, stores, directives)

Register plugins once for the whole deck in `./setup/main.ts`:

```ts
// setup/main.ts
import { defineAppSetup } from '@slidev/types'
import { createPinia } from 'pinia'

export default defineAppSetup(({ app }) => {
  app.use(createPinia())
  // app.use(YourPlugin)
})
```

Use Pinia anywhere (layouts, components, slides):

```ts
// stores/useDeck.ts
import { defineStore } from 'pinia'
export const useDeck = defineStore('deck', {
  state: () => ({ liked: 0 })
})
```

```vue
<script setup>
import { useDeck } from '@/stores/useDeck'
const deck = useDeck()
</script>

<template>
  <button @click="deck.liked++">Likes: {{ deck.liked }}</button>
  <slot />
  <!-- Works in layouts or components -->
</template>
```

Docs: Configure Vue App – https://sli.dev/custom/config-vue

## Reusable Vue components in slides

Place SFCs under `components/` and use them directly in Markdown (auto-registered):

```bash
your-slidev/
  components/
    Counter.vue
  slides.md
```

```vue
<!-- components/Counter.vue -->
<script setup>
import { ref } from 'vue'
const n = ref(0)
</script>
<template>
  <button class="border p-2" @click="n++">{{ n }}</button>
  <slot />
  <!-- slot lets you wrap content in slides -->
  <!-- e.g. <Counter>Label</Counter> -->
</template>
```

```md
# Demo

<Counter class="mt-4" />
```

Docs: Components – https://sli.dev/guide/component

## Dynamic behavior powered by Vue + Slidev

- Step reveals: branch on `$clicks` or `useNav()` clicks.

```html
<div v-if="$clicks >= 2" class="text-green-600">Now reveal details</div>
```

- Use utility CSS (UnoCSS) in SFC/Markdown for quick layouting (grid/flex).
  - Docs: UnoCSS – https://sli.dev/custom/config-unocss

## Tips

- Prefer composables from `@slidev/client` over importing internal paths.
- Keep layouts dumb and content-driven; push app-level concerns to `setup/main.ts`.
- Name your slots explicitly and document them in the layout’s header comment.
- Use absolute asset paths from `public/` (e.g., `/figure1.png`) to be build-safe.

## References

- Slidev – https://sli.dev/
- Writing Layouts – https://sli.dev/guide/write-layout
- Layouts – https://sli.dev/guide/layout
- Components – https://sli.dev/guide/component
- Global Context – https://sli.dev/guide/global-context
- Configure Vue App – https://sli.dev/custom/config-vue
