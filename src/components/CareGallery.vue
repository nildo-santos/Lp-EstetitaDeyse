<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import gsap from 'gsap'
import massage from '../assets/massagem_relaxante.jpeg'
import hands from '../assets/massagem_relaxante03.jpeg'
import cups from '../assets/massagem_Ventosas.jpeg'
import feet from '../assets/massagem_relaxante04.jpeg'

const slides = [
  { src: massage, alt: 'Deyse realizando uma massagem manual na clínica', title: 'Um momento inteiramente seu.', text: 'O atendimento começa na escuta e continua em cada toque.' },
  { src: hands, alt: 'Detalhe das mãos trabalhando a musculatura das costas', title: 'Presença em cada movimento.', text: 'Um cuidado manual atento às tensões e ao seu ritmo.' },
  { src: cups, alt: 'Aplicação de ventosas na região das costas', title: 'Técnicas que se complementam.', text: 'Os recursos são escolhidos de acordo com a avaliação individual.' },
  { src: feet, alt: 'Massagem relaxante nos pés durante o atendimento', title: 'Cuidado até nos detalhes.', text: 'Uma pausa para desacelerar e reencontrar a sensação de leveza.' },
]
const track = ref<HTMLElement>()
const active = ref(0)
let resize: ResizeObserver | undefined
let tween: gsap.core.Tween | undefined
function stopMotion() {
  tween?.kill()
  tween = undefined
  if (track.value) track.value.style.scrollSnapType = ''
}
function goTo(index: number) {
  if (!track.value || index < 0 || index >= slides.length) return
  tween?.kill()
  const left = index * track.value.clientWidth
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) track.value.scrollLeft = left
  else {
    track.value.style.scrollSnapType = 'none'
    tween = gsap.to(track.value, { scrollLeft: left, duration: 0.65, ease: 'power2.inOut', onComplete: () => { if (track.value) track.value.style.scrollSnapType = ''; tween = undefined } })
  }
}
function syncSlide() {
  if (track.value?.clientWidth) active.value = Math.round(track.value.scrollLeft / track.value.clientWidth)
}
onMounted(() => {
  resize = new ResizeObserver(() => {
    stopMotion()
    if (track.value) track.value.scrollLeft = active.value * track.value.clientWidth
  })
  if (track.value) resize.observe(track.value)
})
onBeforeUnmount(() => { resize?.disconnect(); tween?.kill() })
</script>

<template>
  <section class="gallery-section shell section-space" aria-label="Conheça o cuidado da clínica">
    <div class="gallery-layout">
      <div class="gallery-frame"><div id="care-gallery" ref="track" class="gallery-track" tabindex="0" role="region" aria-roledescription="carrossel" aria-label="Fotos dos atendimentos. Use as setas para navegar." @scroll.passive="syncSlide" @keydown.left.prevent="goTo(active - 1)" @keydown.right.prevent="goTo(active + 1)" @keydown.home.prevent="goTo(0)" @keydown.end.prevent="goTo(slides.length - 1)" @pointerdown="stopMotion">
        <figure v-for="(slide, i) in slides" :key="slide.src" class="gallery-slide" role="group" aria-roledescription="slide" :aria-label="`${i + 1} de ${slides.length}: ${slide.title}`"><img :src="slide.src" :alt="slide.alt" loading="lazy" width="900" height="1000" /></figure>
      </div><span class="gallery-image-note">O cuidado acontece aqui.</span></div>
      <div class="gallery-copy"><p class="eyebrow">Um olhar mais de perto</p><h2>Tempo para sentir.<br /><em>Espaço para relaxar.</em></h2><div class="gallery-caption" aria-live="polite" aria-atomic="true"><h3>{{ slides[active]?.title }}</h3><p>{{ slides[active]?.text }}</p></div><div class="gallery-controls"><div class="gallery-arrows"><button type="button" aria-label="Foto anterior" aria-controls="care-gallery" :disabled="active === 0" @click="goTo(active - 1)">←</button><button type="button" aria-label="Próxima foto" aria-controls="care-gallery" :disabled="active === slides.length - 1" @click="goTo(active + 1)">→</button></div></div><div class="gallery-dots" aria-label="Escolher foto"><button v-for="(slide, i) in slides" :key="slide.src" :aria-label="`Ver foto ${i + 1}`" :aria-current="active === i ? 'true' : undefined" aria-controls="care-gallery" @click="goTo(i)"><span /></button></div><a class="instagram-link" href="https://www.instagram.com/deyserodriguesestetica/" target="_blank" rel="noopener noreferrer">Mais momentos no Instagram <span aria-hidden="true">↗</span></a></div>
    </div>
  </section>
</template>
