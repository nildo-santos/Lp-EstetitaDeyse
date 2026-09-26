<script setup lang="ts">
import { computed, ref } from 'vue'
import { whatsapp } from '../data'

const name = ref('')
const question = ref('')
const attempted = ref(false)
const opened = ref(false)
const nameError = computed(() => attempted.value && !name.value.trim())
const questionError = computed(() => attempted.value && !question.value.trim())
const messageLink = computed(() => whatsapp(`Olá, Deyse! Meu nome é ${name.value.trim()}. Vim pela sua página e gostaria de tirar uma dúvida:\n\n${question.value.trim()}`))
function sendQuestion() {
  attempted.value = true
  if (!name.value.trim() || !question.value.trim()) {
    document.getElementById(!name.value.trim() ? 'visitor-name' : 'visitor-question')?.focus()
    return
  }
  window.open(messageLink.value, '_blank', 'noopener,noreferrer')
  opened.value = true
}
</script>

<template>
  <form class="question-form" novalidate @submit.prevent="sendQuestion">
    <div class="question-form-heading"><span aria-hidden="true">↗</span><div><h3>Sua dúvida merece atenção.</h3><p>Escreva para a Deyse. Vamos encontrar o cuidado certo para você.</p></div></div>
    <div class="question-fields">
      <div><label for="visitor-name">Seu nome</label><input id="visitor-name" v-model="name" name="name" autocomplete="given-name" required maxlength="80" placeholder="Como podemos chamar você?" :aria-invalid="nameError || undefined" :aria-describedby="nameError ? 'name-error' : undefined" @input="opened = false" /><p v-if="nameError" id="name-error" class="field-error" role="alert">Campo obrigatório. Preencha seu nome.</p></div>
      <div><label for="visitor-question">Qual é a sua dúvida?</label><textarea id="visitor-question" v-model="question" name="question" required maxlength="1200" rows="4" placeholder="Conte o que você gostaria de saber sobre os serviços ou planos…" :aria-invalid="questionError || undefined" :aria-describedby="questionError ? 'question-error question-help' : 'question-help'" @input="opened = false"></textarea><p v-if="questionError" id="question-error" class="field-error" role="alert">Campo obrigatório. Escreva sua dúvida.</p></div>
    </div>
    <div class="question-submit"><button type="submit" class="button primary">Levar minha dúvida ao WhatsApp <span aria-hidden="true">↗</span></button><p id="question-help">Seu nome e sua dúvida vão preenchidos.<br />No WhatsApp, basta tocar em enviar.</p></div>
    <p v-if="opened" class="question-feedback" role="status">Sua mensagem está pronta. Se o WhatsApp não abriu, <a :href="messageLink" target="_blank" rel="noopener noreferrer">abra sua conversa com Deyse</a>.</p>
  </form>
</template>
