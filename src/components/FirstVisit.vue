<script setup lang="ts">
import { whatsapp } from '../data'

const steps = [
  { title: 'Um momento de escuta', text: 'Conte como você está se sentindo, sua rotina e o que busca com a massagem. O primeiro encontro começa por entender você.' },
  { title: 'Um cuidado escolhido com você', text: 'Deyse avalia suas necessidades, explica as técnicas indicadas e esclarece suas dúvidas antes do atendimento.' },
  { title: 'Atenção em cada etapa', text: 'Ao longo da sessão, o cuidado respeita seu momento, com acompanhamento e orientações da profissional.' },
]
</script>

<template>
  <section id="primeira-sessao" class="first-visit section-space" aria-labelledby="first-visit-title">
    <div class="shell">
      <div class="visit-heading reveal">
        <div><p class="eyebrow">Sua primeira sessão</p><h2 id="first-visit-title">Você chega.<br /><em>O cuidado começa.</em></h2></div>
        <p>Não precisa chegar com todas as respostas. Deyse ajuda você a entender qual cuidado faz sentido para o seu momento.</p>
      </div>
      <ol class="visit-steps">
        <li v-for="(step, index) in steps" :key="step.title" class="reveal">
          <span class="visit-number" aria-hidden="true">0{{ index + 1 }}</span>
          <div><h3>{{ step.title }}</h3><p>{{ step.text }}</p></div>
        </li>
      </ol>
      <div class="visit-footer reveal">
        <p>Seu atendimento é individual e com hora marcada.</p>
        <a class="text-link" :href="whatsapp('Olá, Deyse! Será minha primeira sessão. Gostaria de entender como funciona o atendimento e consultar os horários disponíveis.')" target="_blank" rel="noopener noreferrer">Conversar sobre minha primeira sessão <span aria-hidden="true">↗</span></a>
      </div>
    </div>
  </section>
</template>
