<template>
  <div class="slidev-layout figure-slide">
    <div class="slide-header">
      <h2 v-if="$frontmatter.title" class="slide-title">
        {{ $frontmatter.title }}
      </h2>
    </div>
    
    <div class="figure-wrapper">
      <img 
        v-if="$frontmatter.figure" 
        :src="$frontmatter.figure"
        :alt="$frontmatter.alt || 'Figure'"
        class="figure-image"
      />
      <slot name="figure" />
    </div>
    
    <div v-if="$frontmatter.caption" class="figure-caption">
      <strong v-if="$frontmatter.figureNumber">
        Figure {{ $frontmatter.figureNumber }}:
      </strong>
      {{ $frontmatter.caption }}
    </div>
    
    <slot />
  </div>
</template>

<script setup lang="ts">
import { useSlideContext } from '@slidev/client'

const { $frontmatter } = useSlideContext()
</script>

<style scoped>
.figure-slide {
  width: 100%;
  /* Use container height to avoid double-scaling inside Slidev */
  height: 100%;
  display: grid;
  grid-template-rows: auto 1fr auto;
  padding: 1.5rem;
  box-sizing: border-box;
  overflow: hidden;
  gap: 1rem;
}

.slide-header {
  display: flex;
  align-items: flex-start;
}

.slide-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin: 0;
  /* Inherit theme color so it works in dark/light modes */
  color: inherit;
}

.figure-wrapper {
  display: flex;
  align-items: flex-start; /* anchor image near the title, not vertical center */
  justify-content: center;
  min-height: 0; /* allow image to shrink within grid */
  overflow: hidden;
}

.figure-image {
  max-width: 100%;
  max-height: 100%;
  width: auto;
  height: auto;
  object-fit: contain;
  display: block; /* remove extra bottom whitespace */
}

.figure-caption {
  font-size: 0.8rem;
  line-height: 1.3;
  /* Softer default; theme can override */
  color: #6b7280;
  font-style: italic;
  text-align: center;
  padding: 0;
  max-height: 20vh;
  overflow-y: auto;
}

/* Ensure caption text doesn't get too cramped */
@media (max-height: 600px) {
  .figure-caption {
    font-size: 0.7rem;
    line-height: 1.2;
  }
}
</style>