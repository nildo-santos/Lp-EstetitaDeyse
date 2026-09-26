# Deyse Rodrigues — Massagens & bem-estar

Landing page em Vue 3 + TypeScript, construída no projeto Vite existente.

## Executar

```sh
npm install
npm run dev
```

## Gerar versão para hospedagem

```sh
npm run build
npm run preview
```

O comando build verifica os tipos e gera o site estático em `dist/`. Publique o conteúdo dessa pasta em uma hospedagem estática. A prévia local não é um link público para clientes.

## Manutenção

- `src/data.ts`: serviços, técnicas, durações, preços e mensagens do WhatsApp.
- `src/components/ServiceCard.vue`: apresentação de cada serviço e seleção de duração.
- `src/App.vue`: apresentação, comparação de planos, sobre, dúvidas e contato.
- `src/style.css`: identidade visual, responsividade e movimento reduzido.
- `src/assets/`: imagens fornecidas pelo cliente.
- `.ui-craft/`: decisões de identidade e tokens.

Os planos são calculados a partir dos preços cadastrados, com total mensal, valor por sessão e economia contra sessões avulsas. Os links de WhatsApp preenchem a mensagem, sem enviá-la automaticamente. A seleção do serviço é preservada ao seguir seu link para os planos.

Animações de entrada, deslocamento da foto principal e progresso de rolagem usam GSAP e ScrollTrigger. O carrossel utiliza GSAP para navegar entre fotos, com suporte a toque, setas e teclado. A preferência por movimento reduzido desativa as animações de entrada e de deslocamento. Teclado, foco visível, menu móvel, textos alternativos e preferência por movimento reduzido estão contemplados. A fonte DM Sans usa Google Fonts e possui fallback local.

## Conteúdo a confirmar antes da publicação

- Foi usado o bairro **Engenho de Dentro**, conforme o briefing mais recente. O link antigo de Maps do projeto de referência cita **Cachambi**; confirme com a clínica.
- Não foram presumidos parcelamento, validade dos planos, cancelamento, horários de funcionamento ou regras de remarcação. A página orienta combinar esses pontos pelo WhatsApp.
- As descrições não prometem cura ou resultados garantidos. As técnicas complementares dependem de avaliação individual.

## Referências de design consultadas

- https://github.com/educlopez/ui-craft
- https://github.com/dickwu/apple-design-skill
- https://github.com/uxuiprinciples/agent-skills
- https://github.com/dembrandt/dembrandt-skills

Aplicadas à hierarquia, consistência visual, informações progressivas, controles acessíveis e adaptação de layout. Não foi necessária a instalação dos pacotes nem o uso de APIs pagas.

## Atualizações de interação

- Formulário de dúvidas exige nome e mensagem, rejeita campos vazios ou só com espaços e abre o WhatsApp com ambos preenchidos. O envio final é realizado pela pessoa no WhatsApp; nenhum dado é salvo no site.
- Técnicas: apenas um painel pode ficar aberto, e a altura do outro card é preservada.
- Instagram: navegação, galeria, contato, rodapé e atalho flutuante.
- Logo vetorial dourada em public/dr-monogram.svg.
- GSAP: https://gsap.com/docs/v3/GSAP/gsap.matchMedia()/ e https://gsap.com/docs/v3/Plugins/ScrollTrigger/

