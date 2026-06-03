---
title: "Dois projetos que nasceram de madrugada, cada um por um motivo diferente"
date: 2026-06-03
draft: false
author: "Felipe Cidade"
tags: ["produto", "indie", "campd", "promo-agent", "construindo em público"]
description: "Campd e o promo-agent têm origens diferentes, mas a mesma condição de construção: problema real, sem orçamento, programando de madrugada."
toc: false
---

O [Campd](https://campd.app.br/) começou no WhatsApp. Não como ideia de produto, como thread de grupo que ninguém conseguia acompanhar.

Toda expedição de camping que organizo passa pelo mesmo ciclo: alguém propõe uma data, alguém pergunta onde, alguém manda um link de camping, alguém pergunta de novo porque a mensagem se perdeu, e no final ninguém tem certeza do que foi decidido. O WhatsApp não foi feito para isso. Funciona para conversa, não para coordenação com estado persistente. Quando a thread chega em 60 mensagens e a informação importante está no meio, o problema já não é de comunicação, é de ferramenta.

O Campd nasceu para ser essa ferramenta. Não houve discovery, não houve pesquisa de mercado, não houve canvas. Houve um problema que eu vivia em primeira pessoa e a pergunta prática de quanto tempo levaria para construir algo que resolvesse. Começou como site, está virando app porque o uso em campo pede isso, e a evolução está sendo guiada pela mesma lógica: o que trava uma expedição real vai antes.

O [promo-agent](https://github.com/felipecidade/promo-agent/) tem uma origem diferente. Não nasceu de frustração pessoal direta, mas de observação.

Grupos de promoção no WhatsApp são um fenômeno curioso. Existem dezenas deles, cada um com milhares de membros, e a curadoria na maioria dos casos é feita manualmente por uma ou duas pessoas. O fluxo é sempre parecido: abre o Mercado Livre, navega pelas ofertas, avalia se o desconto é real ou é preço inflado ontem, copia o link de afiliado, escreve uma mensagem, posta. Repete. Às vezes de hora em hora.

Quando percebi esse padrão, a pergunta que veio não foi sobre oportunidade de negócio. Foi mais técnica do que isso: qual parte desse fluxo é genuinamente humana e qual parte é trabalho repetitivo que uma máquina faz melhor? A curadoria de "esse desconto é bom?" tem critério objetivável: histórico de preço, reputação do vendedor, volume de vendas, frescor da oferta. Não é julgamento estético, é análise de dado. E análise de dado automatiza.

A constraint era clara desde o início: CLT, programando de madrugada, sem orçamento para infraestrutura. O projeto precisava ser barato de manter e inteligente o suficiente para não postar lixo. Essas duas restrições juntas forçaram decisões que um projeto com folga não tomaria: fila de envio em disco em vez de banco, scraping com interceptação de XHR porque a API do Mercado Livre bloqueia busca para contas brasileiras desde 2024, score objetivo com valor neutro quando faltam dados em vez de punir o item por ausência de informação.

O que os dois projetos têm em comum não é o domínio, é a condição de construção.

Quando você é o usuário do que está construindo, o critério de "pronto" muda. Não é uma métrica de produto, não é uma data de entrega, não é aprovação de stakeholder. É você abrindo o app antes de uma expedição e sentindo que falta alguma coisa, ou você vendo uma promoção ruim sair do agente e sabendo exatamente o que está errado no score. O feedback loop é imediato e honesto de um jeito que nenhum processo de produto formal consegue replicar.

Isso não significa que projetos pessoais são superiores a produtos para terceiros. Significa que eles têm uma vantagem específica: a ausência de distância entre quem decide e quem usa. Essa distância é o que gera a maioria dos erros de priorização em produto, e aqui ela simplesmente não existe.

O custo é real também. Sem time, sem processo, a dívida técnica acumula sem ninguém para cobrar. Features que não me afetam pessoalmente ficam no backlog indefinidamente, mesmo que sejam importantes para outros usuários. A documentação existe quando eu lembro de escrever. A cobertura de testes é proporcional ao quanto aquela parte específica do sistema já me deu problema. Não é um modelo que escala para um produto com equipe, e fingir que é seria desonesto.

Mas para o momento em que esses dois projetos existem, a condição funciona. O Campd resolve o problema que me trouxe até ele. O promo-agent automatiza algo que eu observei sendo feito manualmente com critério subjetivo e o faz com critério objetivo e custo marginal próximo de zero. Os dois existem porque foram construídos com pressão real, não com tempo livre.

Uma parte relevante do trabalho braçal de desenvolvimento dos dois projetos foi feita com Claude Code. Não como gerador de código automático, mas como par de trabalho de madrugada: você descreve o problema, itera no resultado, mantém o controle das decisões. Isso mudou a equação de quanto uma pessoa sozinha consegue entregar em tempo limitado, e merece uma conversa separada.

Madrugada tem uma qualidade específica para esse tipo de trabalho: sem interrupção, sem contexto externo, com o problema inteiro na cabeça. Não é romantismo, é condição. E às vezes a condição é parte do método.
