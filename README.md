# Landing Page — Deyse Rodrigues Estética Integrativa

Landing page comercial desenvolvida para apresentar os serviços de massagem da clínica Deyse Rodrigues Estética Integrativa, facilitar a comparação de preços e planos mensais e direcionar novos clientes para o atendimento pelo WhatsApp.

> Projeto realizado como serviço freelancer. Esta é a primeira de duas landing pages contratadas pela cliente.

## Contexto do projeto

A clínica precisava de uma página objetiva, elegante e fácil de compartilhar com clientes. A solução reúne os principais serviços, técnicas, durações, preços e informações de atendimento em uma experiência responsiva, com foco em clareza, confiança e conversão.

Além de atender a uma necessidade real da cliente, este projeto faz parte da minha experiência profissional como **Web Designer e Desenvolvedor Front-end**, envolvendo decisões de identidade visual, experiência do usuário, responsividade e implementação.

## Minha atuação

- Planejamento da estrutura e da jornada de navegação.
- Criação da identidade visual da landing page e do monograma `DR`.
- Design responsivo para computadores, tablets e celulares.
- Desenvolvimento da interface com Vue 3 e TypeScript.
- Organização dos serviços, planos e preços para facilitar a comparação.
- Criação das interações e animações com GSAP.
- Integração com WhatsApp, Instagram e Google Maps.
- Cuidados de acessibilidade, legibilidade e redução de movimento.

## Principais recursos

- Apresentação dos serviços com benefícios, técnicas e contraindicações.
- Seleção dinâmica de serviço e duração.
- Comparação entre sessão avulsa e planos de 2 ou 4 sessões mensais.
- Cálculo e exibição da economia de cada plano.
- Botões que abrem o WhatsApp com a escolha do cliente preenchida.
- Carrossel com imagens reais dos atendimentos.
- Seção explicando como funciona a primeira sessão.
- Perguntas frequentes e formulário de dúvidas direcionado ao WhatsApp.
- Horários de atendimento, endereço e mapa integrado.
- Links para Instagram e atalhos flutuantes de contato.
- Animações de entrada e progresso de rolagem com respeito à preferência por movimento reduzido.

## Tecnologias

- [Vue 3](https://vuejs.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [Vite](https://vite.dev/)
- [GSAP](https://gsap.com/)
- CSS responsivo desenvolvido para o projeto

## Executando localmente

### Pré-requisitos

- Node.js 20 ou superior
- npm

### Instalação

```bash
git clone https://github.com/nildo-santos/Lp-EstetitaDeyse.git
cd Lp-EstetitaDeyse
npm install
npm run dev
```

O Vite exibirá no terminal o endereço da página para visualização local.

### Versão de produção

```bash
npm run build
npm run preview
```

O comando `npm run build` verifica os tipos e gera os arquivos otimizados na pasta `dist`.

## Estrutura principal

```text
src/
├── assets/                  # Fotografias e imagens do projeto
├── components/
│   ├── CareGallery.vue      # Galeria dos atendimentos
│   ├── FirstVisit.vue       # Jornada da primeira sessão
│   ├── QuestionForm.vue     # Formulário de dúvidas
│   └── ServiceCard.vue      # Serviço, duração e preço
├── App.vue                  # Estrutura principal da landing page
├── data.ts                  # Serviços, preços e mensagens do WhatsApp
├── main.ts                  # Inicialização da aplicação
└── style.css                # Identidade visual e responsividade
```

## Decisões de experiência

Os preços e planos são atualizados conforme o serviço e a duração escolhidos. As mensagens do WhatsApp são preenchidas automaticamente com a opção selecionada, mas o envio final permanece sob controle do cliente.

A interface inclui navegação por teclado, foco visível, textos alternativos, controles com identificação acessível e adaptação para pessoas que preferem menos movimento.

## Status

**Landing page 1 de 2 — desenvolvimento concluído.**

O projeto poderá continuar recebendo ajustes de conteúdo e preparação para hospedagem conforme a necessidade da cliente.

## Desenvolvedor

Desenvolvido por [nildo-santos](https://github.com/nildo-santos) como projeto freelancer de Web Design e Desenvolvimento Front-end.

