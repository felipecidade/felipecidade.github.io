---
title: "Como construí um PWA de camping com custo de infraestrutura próximo de zero"
date: 2026-06-03
draft: false
author: "Felipe Cidade"
tags: ["cloudflare", "edge", "pwa", "react", "arquitetura", "finops"]
description: "A stack do campd.app.br: Workers, D1, KV e VAPID na edge da Cloudflare. Decisões deliberadas que parecem erradas mas fazem sentido."
toc: true
---

Tem uma versão do campd que eu poderia ter construído em dois fins de semana: Next.js no Vercel, Supabase no free tier, autenticação via Clerk, mapas via Google Maps. Tudo gerenciado, tudo com SDK, tudo documentado. Teria funcionado.

Não foi essa a escolha. E vale explicar por quê.

## O que é o campd

[campd.app.br](https://campd.app.br) é um PWA de planejamento de expedições e camping voltado para o mercado brasileiro. A ideia central é simples: você tem um grupo de amigos que quer acampar, e o app resolve a parte chata: janelas de feriado disponíveis, logística de quem vai, lista de itens por expedição, descoberta de camping com mapa. O produto existe porque eu precisava dele. Toda expedição que organizo começa no WhatsApp e termina em confusão. O campd é a tentativa de resolver isso.

## A stack

Frontend em React 18 + Vite + Tailwind. Sem TypeScript, mas voltarei a isso. Backend é um Worker [Hono 4](https://hono.dev/) rodando na edge da Cloudflare. Dados no D1 (SQLite gerenciado), sessões no KV, push notifications via VAPID. Email transacional com Resend. Mapas com MapLibre GL sobre tiles OpenStreetMap. Auth própria com PBKDF2-SHA256. Deploy automatizado via Semantic Release com hook no Cloudflare.

Custo de infraestrutura atual: zero. A Cloudflare tem um tier gratuito generoso o suficiente para um produto early-stage não pagar nada de relevante. Isso não é acidente: foi uma restrição de projeto desde o início.

## Cloudflare como plataforma, não como CDN

A maioria das pessoas conhece a Cloudflare como CDN ou proteção DDoS. O que mudou nos últimos anos é que ela virou uma plataforma de computação de verdade. Workers roda código na edge em mais de 300 cidades, com latência sub-10ms para a maioria dos usuários brasileiros. D1 é SQLite gerenciado com replicação de leitura global. KV é um store chave-valor com consistência eventual, bom o suficiente para sessões.

A vantagem não é só custo. É que todo o backend do campd vive na mesma plataforma, com roteamento e deploy unificados. Não tem VPC para configurar, não tem grupo de segurança, não tem load balancer. O Worker recebe a requisição, faz a query no D1, escreve no KV se precisar, devolve. O modelo é radical na simplicidade.

O trade-off real: o runtime dos Workers não é Node.js. É um subconjunto do V8. Isso significa que qualquer biblioteca que depende de APIs nativas do Node (`fs`, `crypto` no modelo antigo, `net`) não roda. Você aprende isso na hora errada quando uma dependência explode no build. Com o tempo aprende a filtrar bibliotecas antes de instalar.

## Auth própria: quando faz sentido não delegar

Usar Clerk, Auth0 ou NextAuth é a decisão certa na maioria dos projetos. No campd, optei por auth própria por duas razões.

A primeira é aprendizado deliberado. Não tem como entender o que um provider de auth faz por você sem ter implementado o fluxo uma vez. PBKDF2-SHA256 para hash de senha, token de sessão no KV com TTL, refresh via cookie httpOnly. Nada disso é novo, mas você solidifica o entendimento de por que as coisas funcionam desse jeito.

A segunda é controle de dependência. Auth própria significa zero dependência de terceiro para o fluxo crítico do produto. Sem vendor lock, sem mudança de preço, sem API que deprecia. Para um projeto pessoal de longo prazo, isso pesa.

O risco é óbvio: implementação própria de segurança tem margem para erro. Mitigação: o código é simples o suficiente para auditar inteiramente, e PBKDF2 é um padrão estabelecido, não algo que inventei.

## O App.jsx de 3000 linhas

Aqui está a decisão que mais causa reação quando conto para outros devs: o campd não usa React Router. Todo o roteamento vive num `setCurrentView()` dentro de um único `App.jsx` com cerca de 3000 linhas.

Isso parece errado à primeira vista. Mas tem um raciocínio.

O campd é mantido por uma pessoa. Todo o estado global, toda a navegação e toda a lógica de orquestração de views vivem num único lugar. Isso significa que quando preciso entender o fluxo de qualquer feature, abro um arquivo. Não tem indireção de router, não tem contexto espalhado em múltiplos providers, não tem convenção de pasta pra descobrir onde uma view está registrada. O modelo mental cabe inteiro na cabeça de quem mantém.

React Router resolve problemas reais, especialmente em times onde diferentes pessoas trabalham em diferentes rotas sem se atropelar. Num projeto solo, ele introduz uma camada de abstração que não paga o que cobra. A escolha foi manter o `App.jsx` como ponto único de verdade para navegação e aceitar que ele vai crescer junto com o produto.

## Sobre o TypeScript

Não tem TypeScript no campd. Foi uma escolha deliberada de velocidade de iteração no início: queria poucos pontos de fricção para adicionar uma feature nova. Em projetos solo onde você tem o modelo mental completo do código, os benefícios do TypeScript diminuem e a cerimônia de manter tipos aumenta.

Para um time, mudaria. Para produto solo em fase de descoberta, manteria a mesma decisão.

## MapLibre sobre OSM

Google Maps API custa dinheiro a partir de certo volume e tem termos que restringem cache de tiles. MapLibre GL é open source, baseado no Mapbox GL mas sem o vendor lock, e funciona muito bem com tiles do OpenStreetMap. Para um produto de camping, onde o mapa é funcionalidade central mas não diferencial competitivo, essa combinação entrega o necessário sem custo e sem amarração.

O trabalho extra foi estilizar os tiles para uso outdoor. OSM padrão não é bonito, mas é configurável, e esse custo é previsível. Uma API proprietária cobra em dinheiro quando você menos espera.

## O que fica

A stack do campd é uma aposta no modelo de computação da Cloudflare para o próximo ciclo. Workers + D1 + KV resolve a maioria dos casos de uso de uma aplicação web com custo marginal próximo de zero até escala considerável. Isso não é só relevante para projetos pessoais: qualquer produto early-stage deveria considerar esse modelo antes de provisionar um EC2 ou contratar um RDS.
