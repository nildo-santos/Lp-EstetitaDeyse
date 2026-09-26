<script setup lang="ts">
import { computed, ref } from 'vue'
import { booking, money, type Service } from '../data'
const props = defineProps<{ service: Service; index: number; expanded: boolean }>()
const emit = defineEmits<{ plan: [serviceId: string, durationIndex: number]; toggle: [] }>()
const selected = ref(0)
const duration = computed(() => props.service.durations[selected.value]!)
</script>

<template>
  <article class="service-card">
    <div class="service-photo"><img :src="service.image" :alt="service.imageAlt" loading="lazy" width="720" height="800" /><span class="photo-note">{{ index === 0 ? 'Toque & calor' : 'Cuidado & tecnologia' }}</span></div>
    <div class="service-body">
      <p class="service-subtitle">{{ service.subtitle }}</p>
      <h3>{{ service.name }}</h3>
      <p class="service-description">{{ service.description }}</p>
      <ul class="benefits"><li v-for="benefit in service.benefits" :key="benefit"><span aria-hidden="true">✓</span>{{ benefit }}</li></ul>
      <div class="techniques"><button class="technique-toggle" type="button" :aria-expanded="expanded" :aria-controls="`${service.id}-techniques`" @click="emit('toggle')">Conheça as técnicas <span aria-hidden="true">{{ expanded ? '−' : '+' }}</span></button><div v-show="expanded" :id="`${service.id}-techniques`"><dl><div v-for="technique in service.techniques" :key="technique.name"><dt>{{ technique.name }}</dt><dd>{{ technique.description }}</dd></div></dl></div></div>
      <div class="service-booking">
        <div class="duration-row"><span :id="`${service.id}-duration`">Tempo para você</span><div class="segmented" role="group" :aria-labelledby="`${service.id}-duration`"><button v-for="(option, i) in service.durations" :key="option.minutes" :aria-pressed="selected === i" @click="selected = i">{{ option.minutes }} min</button></div></div>
        <div class="single-price" aria-live="polite" aria-atomic="true"><span>Sessão avulsa</span><strong>{{ money(duration.single) }}</strong></div>
        <a class="button primary service-cta" :href="booking(service, duration)" target="_blank" rel="noopener noreferrer" :aria-label="`Quero esse serviço: ${service.shortName}, ${duration.minutes} minutos`">Quero esse serviço <span aria-hidden="true">↗</span></a>
        <a class="plan-link" href="#planos" @click="emit('plan', service.id, selected)">Prefere uma rotina de cuidado? Veja os planos <span aria-hidden="true">→</span></a>
      </div>
    </div>
  </article>
</template>

