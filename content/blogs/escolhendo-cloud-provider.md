---
title: "AWS, Azure ou GCP: como escolher o cloud provider certo"
date: 2026-05-15
draft: true
author: "Felipe Cidade"
tags: ["aws", "azure", "gcp", "arquitetura", "estratégia"]
description: "Um guia prático para ajudar CTOs e arquitetos a tomar a decisão certa — sem viés de vendor."
toc: true
---

Essa é uma das perguntas que mais recebo: qual cloud provider usar?

A resposta honesta é que depende. Mas "depende" sem contexto não ajuda ninguém, então vou mostrar como penso sobre essa decisão quando alguém me traz esse problema.

## O erro mais comum

A maioria das organizações escolhe o provider pelo motivo errado. "Meu time conhece AWS" é viés de familiaridade. "Azure integra com nosso Office 365" é lock-in por conveniência. "GCP tem os melhores preços de ML" é otimização prematura.

Nenhum desses critérios é necessariamente errado. O problema é quando eles se tornam o critério principal, porque nenhum deles olha para onde a organização vai estar em três anos.

## O que olhar de verdade

O ponto de partida é o workload. Cada provider tem uma proposta diferente e isso se reflete onde eles investem:

| Provider | Melhor para |
|----------|-------------|
| **AWS** | Variedade de serviços, ecossistema maduro, startup-friendly |
| **Azure** | Enterprise com Microsoft stack, Active Directory, compliance |
| **GCP** | Big Data, ML/AI, organizações com cultura de engenharia forte |

Depois do workload, compliance e soberania de dados. Para setores regulados como financeiro, saúde e governo, a disponibilidade de regiões locais e certificações específicas pode ser determinante. Não adianta o serviço ser tecnicamente superior se ele não consegue ficar dentro das fronteiras que a regulação exige.

O terceiro ponto, e o mais subestimado, é o custo total de propriedade. O preço por hora de compute é só a ponta do iceberg. Egress de dados, suporte empresarial, treinamento e certificações do time, e o custo de migração futura se você precisar sair: tudo isso entra na conta. Organizações que escolhem provider olhando só para o custo de compute costumam se surpreender negativamente quando a fatura chega.

Por último, estratégia de longo prazo. Você está construindo para vender a empresa nos próximos dois anos? Para escalar globalmente? Para um mercado específico com regulações próprias? A resposta muda a equação porque o custo de mudar de provider depois de três anos de investimento em managed services é considerável.

## O que eu recomendaria em cada cenário

Greenfield sem restrições de stack: AWS, pela maturidade do ecossistema e pela quantidade de engenheiros que já conhecem a plataforma. Microsoft shop com Active Directory e Azure DevOps já no lugar: Azure faz sentido natural. Workloads pesados de ML ou big data com time de engenharia forte: GCP pela BigQuery e pelos TPUs. Empresa madura com workloads críticos e tolerância a complexidade operacional: multi-cloud começa a fazer sentido, mas só se você tiver time para operar isso.

Não existe resposta universal aqui. O que existe é a decisão errada de não fazer essa pergunta cedo o suficiente, quando ainda é barato mudar de ideia.

Se quiser discutir o caso específico da sua organização, [fale comigo](mailto:felipe.cidade@outlook.com).
