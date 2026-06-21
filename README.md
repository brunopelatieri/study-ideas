# 📚 Study Ideas — Base de Conhecimento Técnico

> **Repositório pessoal de estudos, guias de referência e documentação técnica** — produzido e mantido como base de conhecimento para consulta rápida em projetos de IA, automação, DevOps e desenvolvimento Full Stack.

[![Markdown](https://img.shields.io/badge/Formato-Markdown-blue?style=flat-square&logo=markdown)](.)
[![Idioma](https://img.shields.io/badge/Idioma-PT--BR-green?style=flat-square)](.)
[![Tópicos](https://img.shields.io/badge/Tópicos-9%20categorias-orange?style=flat-square)](.)
[![Autor](https://img.shields.io/badge/Autor-Bruno%20Pelatieri-purple?style=flat-square&logo=github)](https://github.com/brunopelatieri)

---

## 🎯 Sobre este repositório

Este repositório reúne **anotações de estudo, guias de referência rápida e documentação técnica aprofundada** acumulados ao longo do trabalho com IA, automação, DevOps e desenvolvimento web. Não é um projeto de software — é uma **base de conhecimento pessoal**, organizada por tema, pensada para consulta futura própria e como material de apoio reutilizável.

O conteúdo varia entre dois formatos:

- **Guias estruturados** — escritos com padrão Enterprise (índice, tabelas comparativas, exemplos práticos, troubleshooting)
- **Respostas de pesquisa salvas** — capturas de consultas a ferramentas de IA (Perplexity, LLMs) sobre problemas técnicos específicos, preservadas como referência futura

---

## 🗂️ Estrutura do Repositório

```
study-ideas/
├── AI-Agents/                       # Frameworks e arquitetura de agentes de IA
├── AI-LLMs/                         # Comparativos e guias de modelos de linguagem
├── Chatwoot/                        # Integrações e automações via webhook
├── DevOps/                          # Deploy, infraestrutura e VPS
├── Error-Guide/                     # Soluções de erros reais enfrentados
├── Quick-Expression-Command-Guide/  # Cheatsheets de comandos e expressões
├── Self-Hosting/                    # Stacks de self-hosting (Docker Compose/Swarm)
├── json-n8n/                        # Workflows e integrações n8n
└── zz_Miscellaneous/                # Conteúdo diverso sem categoria fixa
```

---

## 📂 Conteúdo por Categoria

### 🤖 AI-Agents
Arquitetura e comparação de frameworks de agentes de IA.

| Arquivo | Descrição |
|---|---|
| `AI_Agent_Frameworks_Guide.md` | Guia comparativo de frameworks open source para AI Agents (LangGraph, CrewAI, AutoGen, Mastra, OpenClaw), incluindo os conceitos fundamentais de **Skill**, **Agent** e **Harness**, arquiteturas de referência e critérios de escolha Enterprise |

---

### 🧠 AI-LLMs
Comparativos técnicos entre modelos de linguagem para diferentes casos de uso.

| Arquivo | Descrição |
|---|---|
| `guia-llm-automacao-agentes-ia-2026.md` | Guia definitivo de LLMs para automação, agentes de IA, LangChain, Supabase e engenharia de prompts — comparando Claude, GPT, Gemini e DeepSeek |
| `llm-comparison-matrix-IDE-cursor-evaluation-guide.md` | Matriz de avaliação de LLMs aplicada ao ciclo de desenvolvimento de software (SDLC) dentro do Cursor IDE |
| `gpt-4o_vs_analise_automation_n8n_chat.md` | Análise de qual LLM performa melhor em automações de chat via n8n (WhatsApp/Instagram) |
| `GPT-5_1-chat-latest_vs_gpt-5-mini_melhor_custo_beneficio.md` | Comparativo de custo-benefício entre GPT-5.1-chat-latest e GPT-5-mini para automação de alto volume |

---

### 💬 Chatwoot
Automações e integrações com a plataforma de atendimento Chatwoot.

| Arquivo | Descrição |
|---|---|
| `Instruction_webhook_tag_chatwoot.md` | Como aplicar etiquetas (labels) em contatos via webhook do Chatwoot através do n8n |

---

### 🐳 DevOps
Estratégias de deploy e arquitetura de infraestrutura.

| Arquivo | Descrição |
|---|---|
| `VPS_deploy_guide.md` | Guia de referência para deploy de stacks (React + Node.js) em VPS Ubuntu com Docker e Portainer CE — comparando Repositório Git direto, Docker Hub, GitLab Registry e GitHub Container Registry (GHCR) |

---

### 🐛 Error-Guide
Soluções documentadas de erros reais enfrentados em projetos.

| Arquivo | Descrição |
|---|---|
| `resolver_erro_ERESOLVE_npm_problema_Vite_NPM_LEGACY_PEER_DEPS.md` | Solução para o erro `ERESOLVE` do npm em projetos Vite, relacionado a conflitos de dependências (`--legacy-peer-deps`) |

---

### ⚡ Quick-Expression-Command-Guide
Cheatsheets e referências rápidas de comandos — a categoria mais extensa do repositório.

| Arquivo | Descrição |
|---|---|
| `git_quick_guide.md` | Guia de referência com os comandos Git mais usados, ordenados por frequência de uso |
| `npm_react_quick_guide.md` | Referência completa de comandos NPM, com foco em scripts (`npm run`), hooks `pre`/`post` e automação de projetos React |
| `docker_complete_quick_guide.md` | Guia de operações e troubleshooting Docker, com foco em Python, LangChain, LangGraph, n8n, Evolution API e Traefik |
| `docker_dockerhub_npm_quick_guide.md` | Documentação de build, push e deploy de imagens Docker para o Docker Hub (projeto MCP BRU IA) |
| `n8n_expressions_quick_guide.md` | Referência avançada de expressions JavaScript do n8n, com padrão de governança Enterprise |
| `supabase_SELECT_requisition_quick_guide.md` | Operadores de comparação e filtros para requisições GET (PostgREST) no Supabase |
| `supabase_UPSERT_requisition_quick_guide.md` | Guia de upsert no Supabase via nó HTTP Request do n8n, com tratamento de resiliência |
| `Google_Cloud_Plataforma_GCP_quick_guide.md` | Referência de comandos `gcloud` CLI — autenticação, Cloud Run, Cloud Functions, Cloud Storage, Cloud SQL e Secrets Manager |
| `WSL_windows_quick_guide.md` | Comandos essenciais do WSL (Windows Subsystem for Linux) |
| `README_XML_Workflow_Enterprise_Unified_quick_guide.md` | Arquitetura unificada de workflow Enterprise combinando XML Prompting, FSM (Finite State Machine) determinístico e n8n |
| `build_AI_studio_google_app_quick_guide.md` | Guia de build e deploy de projetos exportados do Google AI Studio |

---

### 🏠 Self-Hosting
Stacks de infraestrutura própria via Docker Compose e Docker Swarm.

| Arquivo | Descrição |
|---|---|
| `Evolution_API_Redis_docker_compose_yaml.md` | Stack de instalação da Evolution API (v2.3.8) com Redis via Docker Compose |
| `Evolution_GO_portainer_stack_docker_swarm_compose_yaml.md` | Deploy da Evolution API (Engine Go, versão Premium) em Docker Swarm com Traefik e PostgreSQL |

---

### 🔗 json-n8n
Workflows e integrações específicas do n8n.

| Arquivo | Descrição |
|---|---|
| `Extraction_OCR_PDF_With_Gemini_n8n.md` | Extração estruturada de dados via OCR de imagem/PDF integrando Evolution API, n8n e Google Gemini |

---

### 📦 zz_Miscellaneous
Conteúdo diverso, sem categoria fixa definida.

| Arquivo | Descrição |
|---|---|
| `ghl-crm-comunica-soul.md` | Material de apoio sobre GoHighLevel (GHL) para centralização e automação de marketing |
| `provider_send_emails_dev.md` | Comparativo de provedores gratuitos de envio de e-mail transacional (Brevo, Resend, etc.) para uso com n8n/Chatwoot |

---

## 🧩 Stack e Temas Recorrentes

```
🤖 IA & LLMs        Claude, GPT, Gemini, DeepSeek · LangChain · LangGraph
🔧 Automação        n8n · Evolution API · Chatwoot · GoHighLevel
🐳 DevOps           Docker · Docker Swarm · Portainer · Traefik · GitHub/GitLab Registry
☁️ Cloud            Google Cloud Platform (gcloud CLI)
🗄️ Dados            Supabase (PostgREST), Redis, PostgreSQL
⚡ Frontend/Tooling  React, Vite, NPM, WSL
📐 Arquitetura      XML Prompting, FSM determinístico, padrões Enterprise
```

---

## 📌 Natureza do Conteúdo

> ⚠️ Este repositório é uma **base de estudo e referência pessoal**, não um produto ou biblioteca de software. Os arquivos variam em formalidade: alguns são guias polidos e estruturados; outros são capturas diretas de pesquisas e respostas técnicas, mantidos como estavam no momento da consulta.

- Sem licença de software aplicável — conteúdo é majoritariamente documentação e notas
- Sem dependências para instalar — é uma coleção de arquivos `.md`
- Atualizado conforme novos aprendizados, problemas resolvidos e pesquisas são realizados

---

<div align="center">

**Mantido por** [Bruno Pelatieri](https://github.com/brunopelatieri) · [brunogoulart.com.br](https://brunogoulart.com.br/)

*Conhecimento documentado é conhecimento que não se perde.*

</div>
