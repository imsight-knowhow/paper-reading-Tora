# How to Apply Frontend Techniques in Slidev

This guide explains Slidev's architecture and demonstrates advanced frontend techniques for creating professional slides based on analysis of the Slidev source code.

## Core Architecture

Slidev is built on a powerful Vue 3 + Vite stack with these key components:

- **Vue 3 App**: Full Vue 3 application with Composition API, reactive state, and component system
- **Vite Build**: Fast HMR, optimized builds, and plugin ecosystem
- **UnoCSS**: Atomic CSS with full Tailwind-compatible utilities
- **TypeScript**: Full TypeScript support in layouts, components, and setup files

## Layout System Architecture

Layouts in Slidev are Vue SFC components located in `layouts/` directory:

```vue
<!-- layouts/CustomLayout.vue -->
<template>
  <div class="slidev-layout custom-layout">
    <!-- Access frontmatter properties -->
    <h1 v-if="$frontmatter.title">{{ $frontmatter.title }}</h1>
    
    <!-- Default slot for main content -->
    <slot />
    
    <!-- Named slots for structured content -->
    <slot name="aside" />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $frontmatter } = useSlideContext()
// Full Vue 3 Composition API available
</script>
```

Source: packages/client/layouts/

## Built-in Components Pattern

Slidev provides powerful built-in components following these patterns:

### Transform Component for Scaling
```vue
<!-- Usage in slides -->
<Transform :scale="0.8" origin="center">
  <YourContent />
</Transform>
```
Source: packages/client/builtin/Transform.vue

### AutoFitText for Dynamic Sizing
```vue
<AutoFitText :max="200" :min="50">
  Dynamic text that scales to fit
</AutoFitText>
```
Source: packages/client/builtin/AutoFitText.vue

### Conditional Rendering with Clicks
```vue
<div v-if="$clicks >= 2">
  Appears after second click
</div>
```

## Advanced Frontend Techniques

### 1. Responsive Scaling with Vue Reactivity

Create layouts that automatically scale based on viewport:

```vue
<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const viewportWidth = ref(window.innerWidth)
const viewportHeight = ref(window.innerHeight)

const scaleFactor = computed(() => {
  const scaleX = viewportWidth.value / 1280
  const scaleY = viewportHeight.value / 720
  return Math.min(scaleX, scaleY, 1)
})

const updateDimensions = () => {
  viewportWidth.value = window.innerWidth
  viewportHeight.value = window.innerHeight
}

onMounted(() => window.addEventListener('resize', updateDimensions))
onUnmounted(() => window.removeEventListener('resize', updateDimensions))
</script>

<template>
  <div :style="{ transform: `scale(${scaleFactor})` }">
    <slot />
  </div>
</template>
```

### 2. Grid-Based Layouts with UnoCSS

Leverage UnoCSS grid utilities for complex layouts:

```vue
<template>
  <div class="slidev-layout grid grid-cols-[1fr_2fr] gap-6">
    <aside class="flex flex-col justify-center">
      <slot name="sidebar" />
    </aside>
    <main class="flex items-center">
      <slot />
    </main>
  </div>
</template>
```

Source pattern: packages/client/layouts/two-cols.vue

### 3. State Management with Composables

Use Slidev's composables for navigation and state:

```ts
import { useNav, useDarkMode, useSlideContext } from '@slidev/client'

const { currentPage, clicks, next, prev } = useNav()
const { isDark } = useDarkMode()
const { $slidev, $frontmatter } = useSlideContext()
```

Source: packages/client/composables/

### 4. Dynamic Component Registration

Components in `components/` are auto-registered globally:

```vue
<!-- components/FigureWithCaption.vue -->
<template>
  <figure class="flex flex-col gap-2">
    <img :src="src" :alt="alt" class="rounded shadow-lg" />
    <figcaption class="text-sm text-gray-600 italic text-center">
      {{ caption }}
    </figcaption>
  </figure>
</template>

<script setup lang="ts">
defineProps<{
  src: string
  alt?: string
  caption?: string
}>()
</script>
```

Usage in slides:
```md
<FigureWithCaption 
  src="/image.png" 
  alt="Description"
  caption="Figure 1: Example"
/>
```

### 5. Slot Sugar for Clean Markdown

Use Slidev's slot sugar syntax (`::slotname::`) for cleaner markdown:

```md
---
layout: TwoColumns
---

Main content on the left

::right::

Content for the right column
```

Source: packages/slidev/node/syntax/transform/slot-sugar.ts

### 6. CSS-in-JS with Scoped Styles

Apply slide-specific styles with scoped CSS:

```vue
<template>
  <div class="special-slide">
    <slot />
  </div>
</template>

<style scoped>
.special-slide {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 3rem;
}

/* UnoCSS utilities also work */
.special-slide :deep(h1) {
  @apply text-white text-6xl font-bold;
}
</style>
```

### 7. Animation with Vue Transitions

Implement smooth transitions between states:

```vue
<template>
  <TransitionGroup name="fade">
    <div v-for="(item, i) in items" 
         v-show="$clicks >= i" 
         :key="item">
      {{ item }}
    </div>
  </TransitionGroup>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
```

## Best Practices from Source Code

1. **Use `slidev-layout` class**: All layouts should have this base class
2. **Props over hardcoding**: Accept `class` and other props for flexibility
3. **Leverage handleBackground helper**: For image/video backgrounds
4. **Keep layouts composable**: Use slots extensively
5. **TypeScript for props**: Define props with TypeScript for better DX

## File Organization Pattern

```
your-slidev-project/
├── layouts/          # Custom layouts (Vue SFCs)
├── components/       # Reusable components (auto-registered)
├── setup/           # App setup (main.ts, shiki.ts, etc.)
├── styles/          # Global styles and CSS
├── public/          # Static assets
└── slides.md        # Main slide content
```

## Advanced Configuration

### Custom App Setup
```ts
// setup/main.ts
import { defineAppSetup } from '@slidev/types'

export default defineAppSetup(({ app, router }) => {
  // Install Vue plugins
  // Setup global properties
  // Register directives
})
```

### UnoCSS Configuration
```ts
// uno.config.ts
import { defineConfig } from 'unocss'

export default defineConfig({
  shortcuts: {
    'btn': 'px-4 py-2 rounded bg-blue-500 text-white hover:bg-blue-600',
  },
})
```

## References

- Slidev Client Source: packages/client/
- Layout Examples: packages/client/layouts/
- Built-in Components: packages/client/builtin/
- Composables: packages/client/composables/
- Demo Projects: demo/