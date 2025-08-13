---
theme: default
highlighter: shiki
drawings:
  persist: false
transition: slide-left
title: 'Tora: Trajectory-oriented Diffusion Transformer for Video Generation'
mdc: true
---

<h1 class="text-center">Tora: Trajectory-oriented Diffusion Transformer for Video Generation</h1>

<div class="text-center">
  <p class="text-2xl font-semibold text-blue-600 mt-4">CVPR 2025</p>
  
  <div class="mt-12 px-12">
    <p class="text-xl leading-relaxed text-gray-700">
      The first trajectory-oriented Diffusion Transformer framework that enables scalable video generation with effective motion guidance through integrated textual, visual, and trajectory conditions.
    </p>
  </div>
</div>

<style>
.text-center {
  text-align: center;
}
.text-2xl {
  font-size: 1.5rem;
}
.text-xl {
  font-size: 1.25rem;
}
.font-semibold {
  font-weight: 600;
}
.text-blue-600 {
  color: #2563eb;
}
.text-gray-700 {
  color: #374151;
}
.mt-4 {
  margin-top: 1rem;
}
.mt-8 {
  margin-top: 2rem;
}
.mt-12 {
  margin-top: 3rem;
}
.px-12 {
  padding-left: 3rem;
  padding-right: 3rem;
}
.leading-relaxed {
  line-height: 1.625;
}
</style>

---
layout: FigureSlide
title: Introduction
figure: /figure1.png
figureNumber: 1
caption: Tora is capable of generating videos guided by arbitrary trajectories, images, texts, or combinations thereof. Our proposed motion modules integrate seamlessly with the scalability of DiT, ensuring that the generated movements not only adhere precisely to the specified trajectory but also effectively emulate physical world dynamics.
---

---
title: Agenda
---

- Motivation and Problem Setting
- Method: Trajectory-oriented Diffusion Transformer (Tora)
- Experiments and Results
- Ablations and Analysis
- Conclusion and Outlook
- References

---
title: Video Demo
---

<h2>Trajectory-guided generation</h2>

<video src="/example.mp4" controls loop muted style="width: 100%; border-radius: 12px; outline: none;"></video>

<small>Demo video illustrating trajectory-conditioned motion generation quality and adherence.</small>

---
title: References
---

1. Tora: Trajectory-oriented Diffusion Transformer for Video Generation. CVPR 2025.
  - Authors: [Add author list]
  - Project assets: figures in <code>/slides/public</code>, paper in <code>/papers/tora</code>

<small>If you need a BibTeX block on a slide, place it here or link to <code>/papers/tora/main.bib</code>.</small>