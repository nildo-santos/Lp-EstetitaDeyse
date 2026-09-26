<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'
import ServiceCard from './components/ServiceCard.vue'
import QuestionForm from './components/QuestionForm.vue'
import CareGallery from './components/CareGallery.vue'
import FirstVisit from './components/FirstVisit.vue'
import gsap from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'
import { services, booking, money, whatsapp } from './data'
import heroPhoto from './assets/massagem_relaxante02.jpeg'
import detailPhoto from './assets/massagem_relaxante05.jpeg'
import profile from './assets/Perfil.png'

const menuOpen = ref(false)
const openTechnique = ref<string | null>(null)
const showTop = ref(false)
const serviceIndex = ref(0)
const durationIndex = ref(0)
function choosePlan(serviceId: string, index: number) {
  serviceIndex.value = services.findIndex(service => service.id === serviceId)
  durationIndex.value = index
}
const activeService = computed(() => services[serviceIndex.value]!)
const activeDuration = computed(() => activeService.value.durations[durationIndex.value]!)
const plans = computed(() => [
  { name: 'Uma pausa', sessions: 1, label: 'Sessão avulsa', price: activeDuration.value.single, description: 'Um momento de cuidado no seu ritmo.' },
  { name: 'Seu equilíbrio', sessions: 2, label: '2 sessões ao mês', price: activeDuration.value.twice, description: 'Duas pausas para incluir você na agenda.' },
  { name: 'Seu ritual', sessions: 4, label: '4 sessões ao mês', price: activeDuration.value.four, description: 'Faça do bem-estar uma presença na rotina.' },
])
const maps = 'https://www.google.com/maps/search/?api=1&query=' + encodeURIComponent('Avenida Dom Hélder Câmara, 5644, sala 607, Rio de Janeiro, 20771-004')
const mapEmbed = 'https://www.google.com/maps?q=-22.8872409,-43.28582&z=17&output=embed'
const businessHours = [
  { days: 'Segunda a sexta', hours: '09h às 18h' },
  { days: 'Sábado', hours: '09h às 17h' },
  { days: 'Domingo', hours: 'Fechado' },
]
gsap.registerPlugin(ScrollTrigger)
let motion: gsap.MatchMedia | undefined
let progressContext: gsap.Context | undefined
let layoutObserver: ResizeObserver | undefined
let refreshCall: gsap.core.Tween | undefined
onMounted(() => {
  progressContext = gsap.context(() => {
    ScrollTrigger.create({ start: 350, end: 'max', onUpdate: self => { showTop.value = self.scroll() > 350 } })
    gsap.fromTo('.reading-progress', { scaleX: 0 }, { scaleX: 1, ease: 'none', scrollTrigger: { start: 0, end: 'max', scrub: true } })
  })
  motion = gsap.matchMedia()
  motion.add('(prefers-reduced-motion: no-preference)', () => {
    gsap.timeline({ defaults: { ease: 'power2.out', duration: 0.7 } })
      .from('.brand-mark', { opacity: 0, scale: 0.88, duration: 0.8 })
      .from('.hero-copy > *', { opacity: 0, y: 22, stagger: 0.1, clearProps: 'all' }, 0.12)
      .from('.hero-main-photo', { opacity: 0, y: 28, duration: 0.95, clearProps: 'all' }, 0.2)
      .from('.hero-detail-photo, .photo-caption', { opacity: 0, y: 18, stagger: 0.12, clearProps: 'all' }, 0.5)
      .from('.floating-actions > *', { opacity: 0, x: 16, stagger: 0.08, clearProps: 'all' }, 0.8)
    gsap.utils.toArray<HTMLElement>('.reveal, .gallery-layout, .question-form').forEach(element => {
      gsap.from(element, { opacity: 0, y: 26, duration: 0.65, ease: 'power2.out', clearProps: 'all', scrollTrigger: { trigger: element, start: 'top 92%', once: true } })
    })
  })
  motion.add('(min-width: 801px) and (prefers-reduced-motion: no-preference)', () => {
    gsap.fromTo('.hero-main-photo img', { yPercent: -3, scale: 1.09 }, { yPercent: 4, ease: 'none', scrollTrigger: { trigger: '.hero', start: 'top top', end: 'bottom top', scrub: 0.8 } })
  })
  layoutObserver = new ResizeObserver(() => {
    refreshCall?.kill()
    refreshCall = gsap.delayedCall(0.15, () => ScrollTrigger.refresh())
  })
  const main = document.querySelector('main')
  if (main) layoutObserver.observe(main)
})
onBeforeUnmount(() => { layoutObserver?.disconnect(); refreshCall?.kill(); motion?.revert(); progressContext?.revert() })
</script>

<template>
  <a class="skip-link" href="#conteudo">Pular para o conteúdo</a>
  <header class="site-header">
    <div class="reading-progress" aria-hidden="true"></div>
    <a class="brand" href="#inicio" aria-label="Deyse Rodrigues, início"><img class="brand-mark" src="/dr-monogram.svg" alt="" width="64" height="64" aria-hidden="true" /><span class="brand-text">Deyse Rodrigues<small>Estética integrativa</small></span></a>
    <nav class="desktop-nav" aria-label="Navegação principal"><a href="#massagens">Massagens</a><a href="#planos">Planos mensais</a><a href="#sobre">Sobre Deyse</a><a href="#contato">Contato</a><a href="https://www.instagram.com/deyserodriguesestetica/" target="_blank" rel="noopener noreferrer" class="nav-instagram">Instagram ↗</a></nav>
    <a class="header-book" :href="whatsapp()" target="_blank" rel="noopener noreferrer">Agendar meu momento <span aria-hidden="true">↗</span></a>
    <button class="menu-toggle" :aria-expanded="menuOpen" aria-controls="mobile-nav" @click="menuOpen = !menuOpen">{{ menuOpen ? 'Fechar' : 'Menu' }} <span aria-hidden="true">{{ menuOpen ? '×' : '☰' }}</span></button>
    <nav v-if="menuOpen" id="mobile-nav" class="mobile-nav" aria-label="Navegação móvel" @keydown.esc="menuOpen = false"><a v-for="item in [['Massagens', '#massagens'], ['Planos mensais', '#planos'], ['Sobre Deyse', '#sobre'], ['Contato', '#contato']]" :key="item[1]" :href="item[1]" @click="menuOpen = false">{{ item[0] }} <span aria-hidden="true">↗</span></a></nav>
  </header>

  <main id="conteudo">
    <section id="inicio" class="hero shell">
      <div class="hero-copy"><p class="eyebrow">Massagens & bem-estar · Rio de Janeiro</p><h1>Solte as tensões.<br /><em>Volte para você.</em></h1><p class="hero-description">Uma pausa no ritmo do dia. Um cuidado que acolhe seu corpo e respeita o seu momento.</p><div class="hero-actions"><a class="button primary" href="#massagens">Encontre sua massagem <span aria-hidden="true">↗</span></a><a class="text-link" href="#planos">Conheça os planos</a></div><p class="hero-note"><span aria-hidden="true">◌</span> Atendimento individual, com hora marcada.</p></div>
      <div class="hero-visual"><div class="hero-main-photo"><img :src="heroPhoto" alt="Mãos da profissional realizando uma massagem relaxante nas costas" width="1122" height="1280" fetchpriority="high" /></div><div class="hero-detail-photo"><img :src="detailPhoto" alt="Detalhe de uma massagem manual nos pés" width="720" height="1280" /></div><div class="photo-caption"><span>Presença em cada toque.</span><small>Cuidado em cada detalhe.</small></div></div>
    </section>

    <div class="care-strip"><div class="shell"><span>Um cuidado pensado para você</span><span>Massagens para homens e mulheres</span><span>Sessões de 30, 40 ou 60 minutos</span></div></div>

    <section id="massagens" class="services-section shell section-space">
      <div class="section-heading reveal"><div><p class="eyebrow">Seu momento de pausa</p><h2>Como seu corpo<br />quer se sentir <em>hoje?</em></h2></div><p>Do relaxamento às tensões mais profundas, encontre um cuidado que faça sentido para você.</p></div>
      <div class="services-grid"><ServiceCard v-for="(service, i) in services" :key="service.id" :service="service" :index="i" :expanded="openTechnique === service.id" class="reveal" @plan="choosePlan" @toggle="openTechnique = openTechnique === service.id ? null : service.id" /></div>
      <p class="service-footnote">As técnicas são definidas a partir da avaliação individual. Converse com a especialista para saber qual atendimento é indicado para você.</p>
    </section>

    <section id="planos" class="plans-section section-space">
      <div class="shell"><div class="plans-heading reveal"><div><h2>O bem-estar merece<br />espaço na sua <em>rotina.</em></h2><p>Escolha seu cuidado e descubra os valores dos planos mensais.</p></div><span class="plans-side-note">Mais constância.<br />Mais tempo para você.</span></div>
      <div class="plan-controls"><div class="service-picker"><span id="service-picker-label" class="control-label">Escolha sua massagem</span><div class="segmented service-options" role="group" aria-labelledby="service-picker-label"><button v-for="(service, i) in services" :key="service.id" type="button" :aria-pressed="serviceIndex === i" @click="serviceIndex = i; durationIndex = 0"><span class="service-option-title">{{ service.name }}<span v-if="serviceIndex === i" class="selection-check" aria-hidden="true">✓</span></span><span class="service-option-detail">{{ i === 0 ? 'Pedras quentes e ventosas' : 'Relaxante, miofascial e terapêutica' }}</span></button></div></div><div class="plan-duration"><span id="plan-duration-label">Duração de cada sessão</span><div class="segmented" role="group" aria-labelledby="plan-duration-label"><button v-for="(duration, i) in activeService.durations" :key="duration.minutes" :aria-pressed="durationIndex === i" @click="durationIndex = i">{{ duration.minutes }} min</button></div></div></div>
      <p class="selection-summary" aria-live="polite">{{ activeService.shortName }} · {{ activeDuration.minutes }} minutos por sessão</p>
      <div class="plans-grid"><article v-for="plan in plans" :key="plan.sessions" class="plan-card" :class="{ featured: plan.sessions === 4 }"><span v-if="plan.sessions === 4" class="plan-highlight">Maior economia por sessão</span><p class="plan-frequency"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" aria-hidden="true"><rect x="4" y="5" width="16" height="16" rx="3"/><path d="M8 3v4m8-4v4M4 11h16m-11 5h3m3 0h2"/></svg>{{ plan.label }}</p><h3>{{ plan.name }}</h3><p class="plan-description">{{ plan.description }}</p><p class="plan-price-caption">{{ plan.sessions === 1 ? 'Seu momento de cuidado' : 'Investimento mensal' }}</p><div class="plan-price"><strong>{{ money(plan.price) }}</strong><span>{{ plan.sessions === 1 ? '/ sessão' : '/ mês' }}</span></div><p class="per-session">{{ plan.sessions === 1 ? 'Uma sessão para cuidar de você' : `${money(plan.price / plan.sessions)} por sessão` }}</p><div class="plan-saving"><span v-if="plan.sessions > 1" class="saving-symbol" aria-hidden="true">↘</span>{{ plan.sessions === 1 ? 'Sem plano mensal' : `Economize ${money(activeDuration.single * plan.sessions - plan.price)} ao mês*` }}</div><ul class="plan-includes"><li><span aria-hidden="true">✓</span>{{ activeDuration.minutes }} minutos por sessão</li><li><span aria-hidden="true">✓</span>Atendimento individual</li><li><span aria-hidden="true">✓</span>Horário combinado com você</li></ul><a class="button" :class="plan.sessions === 4 ? 'light-button' : 'outline-button'" :href="booking(activeService, activeDuration, plan.sessions)" target="_blank" rel="noopener noreferrer" :aria-label="`${plan.sessions === 1 ? 'Quero a sessão avulsa' : `Quero o plano de ${plan.sessions} sessões`}, ${activeService.shortName}, ${activeDuration.minutes} minutos`">{{ plan.sessions === 1 ? 'Quero essa sessão' : 'Quero esse plano' }} <span aria-hidden="true">↗</span></a></article></div>
      <p class="plans-note">*Economia em relação ao mesmo número de sessões avulsas. Os valores dos planos correspondem ao total mensal. Combine os horários e as condições do plano pelo WhatsApp.</p></div>
    </section>

    <CareGallery />

    <section id="sobre" class="about-section shell section-space reveal"><div class="portrait-wrap"><img :src="profile" alt="Deyse Rodrigues, profissional de estética integrativa" width="510" height="516" loading="lazy" /><span class="portrait-signature">Deyse Rodrigues</span></div><div class="about-copy"><p class="eyebrow">Quem cuida de você</p><h2>Antes de qualquer técnica,<br /><em>escuta e cuidado.</em></h2><p>Sou Deyse Rodrigues. Há mais de 8 anos, trabalho com estética integrativa e com um olhar individual para cada pessoa que chega até mim.</p><p>Acredito em um atendimento que começa por entender você: sua rotina, suas necessidades e como seu corpo está se sentindo. Aqui, o seu momento é respeitado.</p><a class="text-link" :href="whatsapp('Olá, Deyse! Gostaria de conversar para entender qual massagem é mais indicada para mim.')" target="_blank" rel="noopener noreferrer">Converse comigo <span aria-hidden="true">↗</span></a></div></section>

    <FirstVisit />

    <section id="duvidas" class="faq-section shell section-space reveal" aria-labelledby="faq-title"><div><h2 id="faq-title">Para você chegar<br /><em>com tranquilidade.</em></h2><p>Algumas respostas antes do seu momento de cuidado.</p></div><div class="faq-list"><details><summary>Qual massagem escolher? <span aria-hidden="true">+</span></summary><p>A massagem manual combina toque, pedras quentes e ventosas. A integrada reúne a abordagem relaxante, miofascial ou terapêutica com recursos complementares. Conte à Deyse como você está se sentindo para avaliar a opção mais adequada.</p></details><details><summary>Existem contraindicações? <span aria-hidden="true">+</span></summary><div class="faq-answer"><p>Informe suas condições de saúde antes de agendar. Entre as contraindicações informadas pela clínica estão:</p><ul><li>Febre, infecções agudas ou processos inflamatórios.</li><li>Feridas abertas, lesões de pele ou áreas irritadas.</li><li>Trombose, problemas circulatórios graves ou hipertensão descontrolada.</li><li>Fraturas recentes, luxações ou lesões agudas.</li><li>Primeiro trimestre de gravidez.</li><li>Pele sensível, queimaduras ou manchas escuras na região de aplicação das ventosas ou pedras.</li></ul><p>Em caso de dúvida ou condição de saúde, consulte seu médico antes do atendimento. Os recursos complementares também dependem de avaliação individual.</p></div></details><details><summary>Como funcionam os planos mensais? <span aria-hidden="true">+</span></summary><p>Você escolhe a massagem, a duração e a frequência de 2 ou 4 sessões ao mês. O valor exibido é o total mensal. Agendamento, validade, remarcações e demais condições devem ser combinados diretamente com a clínica.</p></details><details><summary>Quais são as formas de pagamento? <span aria-hidden="true">+</span></summary><p>Aceitamos cartão de débito, cartão de crédito, Pix e dinheiro. Consulte diretamente a clínica sobre as condições de pagamento.</p></details><details><summary>Como agendar meu atendimento? <span aria-hidden="true">+</span></summary><p>Clique no botão do serviço ou plano escolhido. O WhatsApp abrirá com sua escolha preenchida; basta enviar a mensagem e combinar o melhor horário com a especialista. Atendimento somente com hora marcada.</p></details></div><QuestionForm /></section>

    <section id="contato" class="contact-section" aria-label="Contato, horários e localização">
      <div class="shell contact-grid">
        <div class="contact-invite">
          <p class="eyebrow">Seu próximo momento de cuidado</p>
          <h2>Você cuida de tanta coisa.<br /><em>Deixe a gente cuidar de você.</em></h2>
          <section class="visit-hours" aria-labelledby="hours-title">
            <h3 id="hours-title"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" aria-hidden="true"><circle cx="12" cy="12" r="9"/><path d="M12 7v5l3 2"/></svg>Planeje sua visita</h3>
            <dl><div v-for="schedule in businessHours" :key="schedule.days" :class="{ 'day-closed': schedule.hours === 'Fechado' }"><dt>{{ schedule.days }}</dt><dd>{{ schedule.hours }}</dd></div></dl>
            <p>Atendimento com hora marcada. Consulte a disponibilidade antes de vir.</p>
          </section>
          <a class="button light-button" :href="whatsapp('Olá, Deyse! Gostaria de agendar uma massagem. Quais horários estão disponíveis?')" target="_blank" rel="noopener noreferrer">Consultar horários e agendar <span aria-hidden="true">↗</span></a>
          <a class="phone-link" href="tel:+5521977353902">(21) 97735-3902</a>
        </div>
        <div class="contact-info">
          <p class="eyebrow">Como chegar</p>
          <h3>Seu momento tem endereço.</h3>
          <address>Avenida Dom Hélder Câmara, 5644<br /><strong>Sala 607</strong> · Engenho de Dentro<br />Rio de Janeiro, RJ · CEP 20771-004</address>
          <iframe class="location-map" :src="mapEmbed" title="Localização da clínica na Avenida Dom Hélder Câmara, 5644" loading="lazy" referrerpolicy="no-referrer-when-downgrade" allowfullscreen></iframe>
          <a :href="maps" class="map-link" target="_blank" rel="noopener noreferrer">Abrir no Google Maps <span aria-hidden="true">↗</span></a>
          <p class="arrival-note">Precisa de ajuda para chegar? <a :href="whatsapp('Olá, Deyse! Gostaria de orientações para chegar à clínica na Avenida Dom Hélder Câmara, 5644, sala 607.')" target="_blank" rel="noopener noreferrer">Peça orientações à Deyse.</a></p>
          <p class="payment-note">Pix · Crédito · Débito · Dinheiro</p>
          <a class="contact-instagram" href="https://www.instagram.com/deyserodriguesestetica/" target="_blank" rel="noopener noreferrer">Instagram <span>@deyserodriguesestetica ↗</span></a>
        </div>
      </div>
    </section>
  </main>
  <footer class="site-footer shell"><a class="footer-brand" href="#inicio">Deyse Rodrigues<span>Estética integrativa</span></a><p>Cuidado que respeita quem você é.</p><a href="https://www.instagram.com/deyserodriguesestetica/" target="_blank" rel="noopener noreferrer">Instagram <span aria-hidden="true">↗</span></a></footer>
  <nav class="floating-actions" aria-label="Acesso rápido">
    <a v-show="showTop" href="#inicio" class="float-button float-top" aria-label="Voltar ao início" title="Voltar ao início"><span aria-hidden="true">↑</span><span class="float-label">Voltar ao início</span></a>
    <a href="#duvidas" class="float-button float-question" aria-label="Escrever uma dúvida" title="Escrever uma dúvida"><span aria-hidden="true">?</span><span class="float-label">Tire sua dúvida</span></a>
    <a href="https://www.instagram.com/deyserodriguesestetica/" class="float-button float-instagram" target="_blank" rel="noopener noreferrer" aria-label="Instagram da Deyse" title="Instagram da Deyse"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" aria-hidden="true"><rect x="3" y="3" width="18" height="18" rx="5"/><circle cx="12" cy="12" r="4"/><circle cx="17.5" cy="6.5" r=".8" fill="currentColor" stroke="none"/></svg><span class="float-label">Nosso Instagram</span></a>
    <a class="float-button float-whatsapp" :href="whatsapp()" target="_blank" rel="noopener noreferrer" aria-label="Fale com Deyse pelo WhatsApp" title="WhatsApp da Deyse"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" aria-hidden="true"><path d="M21 11.5a8.5 8.5 0 0 1-8.5 8.5 9 9 0 0 1-4-.9L3 21l1.9-5.5a9 9 0 0 1-.9-4A8.5 8.5 0 0 1 12.5 3h.5a8.5 8.5 0 0 1 8 8v.5Z"/><path d="M8.5 8.5c.5 3.5 3 6 6.5 6.5l1-2-2-1-1 1-2-2 1-1-1-2-2 .5Z"/></svg><span class="float-label">Vamos conversar?</span></a>
  </nav>
</template>

