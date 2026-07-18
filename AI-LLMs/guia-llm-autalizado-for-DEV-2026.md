# 🧠 Qual LLM Usar Para Programar — Guia de Decisão Rápida (Regra 80/20)

> **Comparativo prático de modelos de IA para desenvolvimento — React, Next.js, Node.js, Drizzle, Supabase, LangChain/LangGraph, n8n, PHP, Solidity e DevOps — pensado para decisão em segundos, não para leitura de 20 minutos.**

[![Atualizado](https://img.shields.io/badge/Atualizado-Julho%202026-blue?style=flat-square)](.)
[![Modelos](https://img.shields.io/badge/Modelos-7%20Analisados-green?style=flat-square)](.)
[![Regra](https://img.shields.io/badge/Método-Pareto%2080%2F20-orange?style=flat-square)](.)
[![Idioma](https://img.shields.io/badge/Idioma-PT--BR-informational?style=flat-square)](.)

---

## 📋 Índice

- [🧠 Qual LLM Usar Para Programar — Guia de Decisão Rápida (Regra 80/20)](#-qual-llm-usar-para-programar--guia-de-decisão-rápida-regra-8020)
  - [📋 Índice](#-índice)
  - [⚡ TL;DR — Decisão em 10 segundos](#-tldr--decisão-em-10-segundos)
  - [📊 Tabela Mestra: Preço, Contexto e Perfil](#-tabela-mestra-preço-contexto-e-perfil)
  - [🎯 Os 20% que Resolvem 80% dos Casos](#-os-20-que-resolvem-80-dos-casos)
  - [🔍 Perfil Detalhado de Cada Modelo](#-perfil-detalhado-de-cada-modelo)
    - [🥇 Claude Fable 5 — O topo absoluto](#-claude-fable-5--o-topo-absoluto)
    - [🥈 Claude Opus 4.8 — Orquestrador premium](#-claude-opus-48--orquestrador-premium)
    - [🥉 Claude Sonnet 5 — O novo default (equilíbrio)](#-claude-sonnet-5--o-novo-default-equilíbrio)
    - [⚡ Claude Haiku 4.5 — Fast worker / subagente](#-claude-haiku-45--fast-worker--subagente)
    - [🚀 Grok 4.5 — Código + agentes + custo agressivo](#-grok-45--código--agentes--custo-agressivo)
    - [🎨 GPT-5.6 (família Sol / Terra / Luna) — Produtividade e frontend](#-gpt-56-família-sol--terra--luna--produtividade-e-frontend)
    - [🖥️ GPT-5.3-Codex — Especialista em terminal/agentic coding](#️-gpt-53-codex--especialista-em-terminalagentic-coding)
  - [🛠️ Por Área Técnica da Stack](#️-por-área-técnica-da-stack)
    - [React, Next.js e Vite](#react-nextjs-e-vite)
    - [Node.js, Hono SSR e APIs](#nodejs-hono-ssr-e-apis)
    - [Drizzle, Neon e SQL](#drizzle-neon-e-sql)
    - [Supabase OAuth](#supabase-oauth)
    - [SDD (Specification-Driven Development) e agentes](#sdd-specification-driven-development-e-agentes)
    - [LangChain, LangGraph e n8n](#langchain-langgraph-e-n8n)
    - [PHP e Solidity](#php-e-solidity)
    - [DevOps e deploy](#devops-e-deploy)
  - [🧩 Estratégia de Roteamento (Orquestrador + Executor + Worker)](#-estratégia-de-roteamento-orquestrador--executor--worker)
  - [💡 Exemplo Prático — Montando um SaaS](#-exemplo-prático--montando-um-saas)
  - [⚠️ Correções Importantes Sobre Nomenclatura](#️-correções-importantes-sobre-nomenclatura)
  - [🧭 Árvore de Decisão](#-árvore-de-decisão)
  - [📚 Referências](#-referências)

---

## ⚡ TL;DR — Decisão em 10 segundos

```
Não sabe qual usar agora? → Sonnet 5. É o novo default e resolve ~80% do trabalho real.

Problema difícil, refatoração grande, sessão agêntica longa? → Opus 4.8

Precisa do máximo de inteligência e o custo não é o fator decisivo? → Fable 5

Subagente, lote, teste, seed, classificação, alto volume? → Haiku 4.5

Quer o melhor custo/token em código com boa velocidade? → Grok 4.5

Quer o melhor julgamento visual/estético para UI? → GPT-5.6 Sol (ou Terra p/ custo)

Terminal-first, agente que só refatora/testa repositório? → GPT-5.3-Codex
```

---

## 📊 Tabela Mestra: Preço, Contexto e Perfil

> Preços em USD por milhão de tokens (input / output). Dados verificados em julho de 2026.

| Modelo | Empresa | Input / Output | Contexto | Perfil Principal |
|---|---|---|---|---|
| **Claude Haiku 4.5** | Anthropic | $1 / $5 | 200K | Fast worker — volume, custo baixo, subagentes |
| **Grok 4.5** | xAI (SpaceXAI) | $2 / $6 | 500K | Código + agentes, melhor custo/desempenho |
| **GPT-5.6 Terra** | OpenAI | $2,50 / $15 | 1,05M | Equilíbrio — substitui o antigo "default" |
| **Claude Sonnet 5** | Anthropic | $2–3 / $10–15* | 1M | **Novo default** — equilíbrio qualidade/custo |
| **GPT-5.6 Luna** | OpenAI | $1 / $6 | 1,05M | Volume, baixo custo, tarefas simples |
| **Claude Opus 4.8** | Anthropic | $5 / $25 | 1M | Raciocínio profundo, tarefas longas e críticas |
| **GPT-5.6 Sol** | OpenAI | $5 / $30 | 1,05M | Flagship — coding difícil, UI/UX, pesquisa |
| **GPT-5.3-Codex** | OpenAI | — (via Codex) | 400K | Especialista em terminal/agentic coding |
| **Claude Fable 5** | Anthropic | $10 / $50 | 1M | Topo absoluto — Mythos-class, máxima capacidade |

<sup>*Sonnet 5: preço promocional $2/$10 até 31/ago/2026; depois $3/$15 padrão.</sup>

---

## 🎯 Os 20% que Resolvem 80% dos Casos

Se você só vai lembrar de **3 modelos**, lembre destes:

| # | Modelo | Por quê é o 20% que resolve 80% |
|---|---|---|
| 1️⃣ | **Sonnet 5** | Default atual da Anthropic para Free/Pro desde 01/jul/2026. Mesmo sistema de "esforço adaptativo" do Opus (low/medium/high/xhigh/max), mas mais barato. Resolve a esmagadora maioria de tarefas de código do dia a dia. |
| 2️⃣ | **Opus 4.8** | Quando Sonnet 5 "não é suficiente" — coding genuinamente difícil, sessões agênticas longas, múltiplos arquivos com dependências complexas. |
| 3️⃣ | **Haiku 4.5** | Motor de qualquer pipeline com volume: subagentes, testes, geração em lote, classificação, n8n. Mais barato output de todo o mercado analisado ($5/M). |

> 💡 **Regra prática:** comece tudo em Sonnet 5. Suba para Opus 4.8 só quando perceber retrabalho (o modelo errou, você teve que corrigir 2-3 vezes). Desça para Haiku 4.5 assim que a tarefa for repetitiva ou paralelizável.

---

## 🔍 Perfil Detalhado de Cada Modelo

### 🥇 Claude Fable 5 — O topo absoluto

Modelo **Mythos-class** da Anthropic, acima do tier Opus. Segundo o Artificial Analysis Intelligence Index de julho/2026, lidera o ranking geral com 60 pontos (à frente de Opus 4.8 com 56 e GPT-5.5 com 55).

| | |
|---|---|
| **Preço** | $10 / $50 por MTok |
| **Contexto** | 1M tokens |
| **Pontos fortes** | Liderança em benchmarks de código difícil; melhor escolha quando você já mediu que até o Opus fica aquém |
| **Cautela** | Pode ser "overpowered" (e caro) para tarefas simples do dia a dia |
| **Melhor uso na stack** | Refatorações grandes, migrações críticas (Drizzle/Neon), auditoria de segurança em Solidity, SDD pesado como orquestrador máximo |

> ⚠️ Fable 5 é Mythos-tier — teve uma suspensão temporária por controles de exportação dos EUA entre 12 e 30 de junho de 2026, restaurado em 1º de julho. Se seu workflow depende dele em produção crítica, vale ter fallback configurado para Opus 4.8.

---

### 🥈 Claude Opus 4.8 — Orquestrador premium

| | |
|---|---|
| **Preço** | $5 / $25 por MTok |
| **Contexto** | 1M tokens |
| **Pontos fortes** | Forte em coding e tarefas agênticas longas; ótimo para coordenar subagentes e manter consistência em execuções extensas |
| **Cautela** | Mais caro que os modelos médios; nem sempre a melhor escolha para tarefas simples |
| **Melhor uso na stack** | Arquitetura, revisões profundas, migração de monorepo, auditoria de segurança, contratos Solidity |

---

### 🥉 Claude Sonnet 5 — O novo default (equilíbrio)

| | |
|---|---|
| **Preço** | $2/$10 (intro até 31/ago/2026) → $3/$15 padrão |
| **Contexto** | 1M tokens |
| **Pontos fortes** | Adotou o mesmo sistema de esforço adaptativo do Opus 4.8; muito bom em tool use, planejamento e loops agênticos; é o default para Free e Pro desde julho/2026 |
| **Cautela** | Ainda fica abaixo do Opus 4.8 nas tarefas de coding mais duras |
| **Melhor uso na stack** | Desenvolvimento diário em React/Next/Vite, APIs Node/Hono, geração de componentes, a maior parte do trabalho profissional |

---

### ⚡ Claude Haiku 4.5 — Fast worker / subagente

| | |
|---|---|
| **Preço** | $1 / $5 por MTok — menor preço de output de toda a comparação |
| **Contexto** | 200K tokens |
| **Pontos fortes** | Muito rápido e barato; forte em coding, computer use e tarefas de agente em escala |
| **Cautela** | Menos indicado como "cérebro" principal para problemas profundos |
| **Melhor uso na stack** | Subagentes, testes, lint fixes, geração em lote, automações n8n, tarefas repetitivas |

---

### 🚀 Grok 4.5 — Código + agentes + custo agressivo

xAI foi adquirida pela SpaceX (a marca aparece como "SpaceXAI" em anúncios recentes). Grok 4.5 foi treinado em parceria com a Cursor, usando dados reais de sessões de desenvolvedores.

| | |
|---|---|
| **Preço** | $2 / $6 por MTok (cache input a $0,50) |
| **Contexto** | 500K tokens |
| **Pontos fortes** | #4 geral no Artificial Analysis Intelligence Index (54 pontos), mas **#1 em uso agêntico de ferramentas**; ~80 tokens/s; usa cerca de metade dos tokens de modelos comparáveis para a mesma tarefa |
| **Cautela** | Contexto menor que a geração anterior (Grok 4.3 tinha 1M-2M); taxa de alucinação confiante mais alta em benchmark independente; ainda sem disponibilidade na UE |
| **Melhor uso na stack** | Code review, agentes, ferramentas, automações e experimentação rápida — ótimo custo-benefício quando "bom o suficiente e barato" vence "o melhor absoluto" |

---

### 🎨 GPT-5.6 (família Sol / Terra / Luna) — Produtividade e frontend

> ⚠️ **Correção importante:** "GPT 5.6" não é um único modelo — é uma família de três modelos lançada em 9 de julho de 2026, com contexto de 1,05M tokens e 128K de output máximo em todos os tiers.

| Tier | Preço | Perfil |
|---|---|---|
| **Sol** | $5 / $30 | Flagship — coding difícil, trabalho de longo horizonte, pesquisa em segurança |
| **Terra** | $2,50 / $15 | Balanceado — combina qualidade do antigo GPT-5.5 por ~metade do preço |
| **Luna** | $1 / $6 | Volume, custo-sensível |

**Pontos fortes:** a OpenAI destaca ganhos em estética de frontend, hierarquia visual e julgamento de design — relevante direto para React + shadcn/ui + Tailwind. **Cautela:** para código muito profundo ou agentes de sessão muito longa, Sol não é necessariamente o topo absoluto (Fable 5 e Opus 4.8 lideram benchmarks de coding puro).

**Melhor uso na stack:** UI com shadcn/ui + Tailwind, apps Next.js, produtos com boa UX — Sol para o componente crítico, Terra para o resto do app.

---

### 🖥️ GPT-5.3-Codex — Especialista em terminal/agentic coding

Diferente da família GPT-5.6 (chat/reasoning geral), o GPT-5.3-Codex é o modelo especializado que roda dentro do produto **Codex** da OpenAI — foco em navegar repositórios, rodar comandos de terminal e depurar código de forma autônoma.

| | |
|---|---|
| **Contexto** | 400K tokens, 128K de output máximo |
| **Lançamento** | 5 de fevereiro de 2026 |
| **Pontos fortes** | ~25% mais rápido que a geração anterior (5.2-Codex); estado da arte em SWE-Bench Pro e Terminal-Bench 2.0 na época do lançamento |
| **Cautela** | Já foi parcialmente sucedido pelo GPT-5.4 como modelo principal dentro do produto Codex (março/2026) — vale checar qual versão está ativa no seu plano antes de assumir que é a mais recente |
| **Melhor uso na stack** | Fluxos terminal-first: navegação de repositório, execução de comandos, depuração autônoma dentro do Codex CLI/app |

---

## 🛠️ Por Área Técnica da Stack

### React, Next.js e Vite

**GPT-5.6 Sol** se destaca pela ênfase da OpenAI em estética de frontend e julgamento de design — direto no alvo para shadcn/ui e Tailwind. **Sonnet 5** é o "workhorse" para gerar componentes e iterar com ferramentas no dia a dia. **Haiku 4.5** funciona muito bem como trabalhador rápido para variações e ajustes finos. **Opus 4.8** vira a melhor escolha quando o app está grande, com muitos estados, múltiplas integrações e decisões arquiteturais consistentes.

### Node.js, Hono SSR e APIs

**Sonnet 5** como padrão de produtividade; **Opus 4.8** quando a API exige raciocínio mais longo e mudanças em múltiplos arquivos. **Grok 4.5** entra bem por seu forte tool calling e baixa alucinação em handlers, rotas e middlewares. **Haiku 4.5** é ótimo para endpoints simples, testes e boilerplate.

### Drizzle, Neon e SQL

O que mais pesa aqui é consistência entre schema, queries, migrations e tipagem. **Opus 4.8** e **Sonnet 5** são os mais seguros como "cérebro" nesse cenário. **Haiku 4.5** fica excelente para queries auxiliares, seeds e pequenas migrações. **Grok 4.5** é boa alternativa quando você quer velocidade e custo agressivo.

### Supabase OAuth

Para OAuth, o que mais importa é evitar bugs de fluxo, callback, sessão e edge cases de segurança. **Opus 4.8** tende a ser o mais confiável para desenhar o fluxo completo; **Sonnet 5** implementa bem o caminho principal e os testes. **Haiku 4.5** ajuda em automações e documentação do fluxo.

### SDD (Specification-Driven Development) e agentes

O modelo precisa transformar especificação em plano, subtarefas, critérios de aceite e validação. Combinação recomendada: **Opus 4.8 ou Fable 5** como orquestrador, **Sonnet 5** como executor principal, **Haiku 4.5** como worker de alta escala. Ver [`specifyx-guide`](../specifyx-guide/) para o ciclo completo de SDD aplicado.

### LangChain, LangGraph e n8n

**Opus 4.8** e **Grok 4.5** se encaixam naturalmente como modelos de coordenação — ambos com forte suporte a tarefas agênticas e tool calling. **Haiku 4.5** é a peça mais barata para nós de alto volume, execução paralela e respostas curtas. **Sonnet 5** é o melhor "meio-termo" para equilíbrio entre inteligência e custo.

### PHP e Solidity

Em **PHP**, qualquer modelo forte resolve bem, mas **Sonnet 5** e **Opus 4.8** são os mais confiáveis para refatoração legada, testes e organização de camadas. Em **Solidity/Ethereum DApps**, prefira **Opus 4.8 ou Fable 5** para análise de segurança, invariantes e arquitetura de contratos — tarefas críticas e longas são o ponto forte desses tiers. **GPT-5.6 Sol** e **Grok 4.5** entram bem como copilotos para contratos, testes e scripts auxiliares — revise tudo com atenção redobrada antes de deploy.

### DevOps e deploy

Para Docker, VPS, CI/CD, Traefik, env vars e pipeline, **Opus 4.8** é o mais indicado quando há muitas dependências e risco operacional. **Sonnet 5** funciona muito bem no dia a dia para compose files, scripts de deploy e documentação operacional. **Haiku 4.5** assume tarefas menores e repetitivas — checks, templates, automações rápidas.

---

## 🧩 Estratégia de Roteamento (Orquestrador + Executor + Worker)

O padrão mais eficiente em custo para times que usam múltiplos modelos:

```
┌─────────────────────────────────────────────────────────┐
│  ORQUESTRADOR (5-10% do volume)                         │
│  Fable 5 ou Opus 4.8                                     │
│  → Arquitetura, decisões críticas, revisão de segurança  │
└───────────────────────┬───────────────────────────────────┘
                         │
┌────────────────────────▼───────────────────────────────────┐
│  EXECUTOR (30-35% do volume)                              │
│  Sonnet 5                                                  │
│  → Implementação do dia a dia, componentes, rotas          │
└────────────────────────┬───────────────────────────────────┘
                         │
┌────────────────────────▼───────────────────────────────────┐
│  WORKER (55-60% do volume)                                 │
│  Haiku 4.5 (ou Grok 4.5 para código com custo agressivo)  │
│  → Testes, seeds, lote, classificação, subagentes n8n      │
└─────────────────────────────────────────────────────────────┘
```

> 💡 Uma estratégia de roteamento bem calibrada (60% Haiku / 35% Sonnet / 5% Opus-Fable) tende a reduzir o custo médio em 30-40% comparado a rodar tudo em Sonnet 5, sem perda perceptível de qualidade nas tarefas certas.

---

## 💡 Exemplo Prático — Montando um SaaS

Cenário: Next.js + Supabase OAuth + Drizzle/Neon + shadcn/ui + Tailwind + automações n8n.

```
1. Arquitetura e revisão de segurança      → Opus 4.8 (ou Fable 5 se crítico)
2. Implementação de páginas, rotas, APIs   → Sonnet 5
3. Capricho visual da UI (shadcn + Tailwind) → GPT-5.6 Sol
4. Testes, seeds, componentes repetitivos  → Haiku 4.5
5. Experimentos rápidos com agentes/n8n    → Grok 4.5
```

---

## ⚠️ Correções Importantes Sobre Nomenclatura

O texto original desta pesquisa tratava alguns nomes de forma imprecisa. Correções para julho/2026:

| Termo usado originalmente | Correção |
|---|---|
| "GPT 5.6" como modelo único | É uma **família de 3 modelos**: Sol (flagship), Terra (balanceado), Luna (volume) — lançados juntos em 9/jul/2026 |
| "Codex 5.3" sem fonte confirmada | Confirmado como **GPT-5.3-Codex**, lançado 5/fev/2026, especializado em terminal/agentic coding dentro do produto Codex — já parcialmente sucedido pelo GPT-5.4 como modelo principal do Codex desde março/2026 |
| Grok 4.5 como "xAI" | A empresa opera hoje sob a marca **SpaceXAI** após aquisição pela SpaceX |
| Ausência de menção ao Fable 5 como tier próprio | Fable 5 é **Mythos-class**, um tier acima de Opus — não é "só mais um modelo Claude", é a categoria mais alta da Anthropic |

---

## 🧭 Árvore de Decisão

```
A tarefa é crítica, cara de errar, ou muito longa/complexa?
   SIM → Fable 5 (se disponível no plano) ou Opus 4.8
   NÃO ↓

É trabalho de desenvolvimento do dia a dia?
   SIM → Sonnet 5 (comece aqui sempre)
   NÃO ↓

É volume alto, repetitivo, ou paralelo (testes, seeds, lote)?
   SIM → Haiku 4.5
   NÃO ↓

É UI/UX visual crítico (design, estética, hierarquia)?
   SIM → GPT-5.6 Sol (ou Terra para economizar)
   NÃO ↓

Quer o melhor custo por token em código com boa velocidade?
   SIM → Grok 4.5
   NÃO ↓

É fluxo terminal-first dentro do produto Codex?
   SIM → GPT-5.3-Codex (ou GPT-5.4, verifique a versão ativa)
```

---

## 📚 Referências

- [Anthropic — Claude Opus 4.8](https://www.anthropic.com/news/claude-opus-4-8)
- [Anthropic — Claude Haiku 4.5](https://www.anthropic.com/news/claude-haiku-4-5)
- [Anthropic — Claude Fable 5 / Mythos access](https://www.anthropic.com/news/fable-mythos-access)
- [xAI (SpaceXAI) — Grok 4.5 Announcement](https://x.ai/news/grok-4-5)
- [OpenAI — API Pricing](https://openai.com/api/pricing/)
- [Artificial Analysis — Intelligence Index](https://artificialanalysis.ai/)
- [BenchLM — Claude API Pricing (Julho 2026)](https://benchlm.ai/blog/posts/claude-api-pricing)
- [BenchLM — OpenAI API Pricing (Julho 2026)](https://benchlm.ai/openai/api-pricing)

---

<div align="center">

*Preços e specs mudam rápido neste mercado — revise antes de decisões de orçamento em produção.*

**Última verificação:** Julho 2026

</div>