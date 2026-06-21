# 🐳 Deploy de Stacks em VPS — Docker, Portainer, GitLab e GitHub

> **Guia de Referência DevOps: arquitetando deploys de aplicações (React + Node.js) em VPS Linux Ubuntu com Docker e Portainer CE, comparando Repositório Git direto, Docker Hub, GitLab Registry e GitHub Container Registry (GHCR).**

[![Docker](https://img.shields.io/badge/-Docker-2496ED?style=flat-square&logo=docker&logoColor=white)](https://www.docker.com/)
[![Portainer](https://img.shields.io/badge/-Portainer%20CE-13BEF9?style=flat-square&logo=portainer&logoColor=white)](https://www.portainer.io/)
[![GitHub](https://img.shields.io/badge/-GitHub%20%2F%20GHCR-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/)
[![GitLab](https://img.shields.io/badge/-GitLab%20Registry-FC6D26?style=flat-square&logo=gitlab&logoColor=white)](https://gitlab.com/)
[![Ubuntu](https://img.shields.io/badge/-Ubuntu%20Linux-E95420?style=flat-square&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Nível](https://img.shields.io/badge/Nível-Intermediário%20a%20Avançado-blue?style=flat-square)](.)

---

## 📋 Índice

- [🐳 Deploy de Stacks em VPS — Docker, Portainer, GitLab e GitHub](#-deploy-de-stacks-em-vps--docker-portainer-gitlab-e-github)
  - [📋 Índice](#-índice)
  - [🎯 Objetivo](#-objetivo)
  - [🧠 Conceito Central: Onde o Build Acontece](#-conceito-central-onde-o-build-acontece)
  - [📊 Comparativo Direto: Git vs Docker Hub](#-comparativo-direto-git-vs-docker-hub)
  - [🏗️ Os Quatro Cenários de Deploy](#️-os-quatro-cenários-de-deploy)
    - [Cenário 1 — Repositório Git Direto no Portainer](#cenário-1--repositório-git-direto-no-portainer)
    - [Cenário 2 — Docker Hub (Registry Puro)](#cenário-2--docker-hub-registry-puro)
    - [Cenário 3 — GitLab Container Registry (All-in-One)](#cenário-3--gitlab-container-registry-all-in-one)
    - [Cenário 4 — GitHub + GHCR (Ecossistema Actions)](#cenário-4--github--ghcr-ecossistema-actions)
  - [📐 Tabela Comparativa Completa dos 4 Cenários](#-tabela-comparativa-completa-dos-4-cenários)
  - [⚙️ Exemplos de Stack no Portainer CE](#️-exemplos-de-stack-no-portainer-ce)
    - [Puxando do Docker Hub](#puxando-do-docker-hub)
    - [Puxando do GitLab Container Registry](#puxando-do-gitlab-container-registry)
    - [Puxando do GitHub Container Registry (GHCR)](#puxando-do-github-container-registry-ghcr)
  - [🔐 Autenticação em Registries Privados](#-autenticação-em-registries-privados)
    - [Passo 1 — Cadastrar o Registry antes do deploy](#passo-1--cadastrar-o-registry-antes-do-deploy)
    - [Passo 2 — Usar Token, nunca senha](#passo-2--usar-token-nunca-senha)
  - [⚖️ Portainer CE vs Portainer Business (EE)](#️-portainer-ce-vs-portainer-business-ee)
    - [Comportamento por Provedor no Portainer CE](#comportamento-por-provedor-no-portainer-ce)
  - [🧭 Guia de Decisão](#-guia-de-decisão)
    - [Resumo por Cenário de Negócio](#resumo-por-cenário-de-negócio)
  - [✅ Boas Práticas](#-boas-práticas)
  - [🐛 Troubleshooting](#-troubleshooting)
  - [📚 Referências](#-referências)

---

## 🎯 Objetivo

Este documento serve como **base de pesquisa e conhecimento** para decisões de arquitetura de deploy em infraestrutura própria (VPS), cobrindo:

- Onde o processo de build deve ocorrer (local, CI externo, ou na própria VPS)
- Como cada Registry (Docker Hub, GitLab, GHCR) se integra ao Portainer CE
- Trade-offs reais de hardware, segurança, privacidade e velocidade
- Limitações conhecidas do Portainer Community Edition

> 💡 Use este guia como referência ao decidir a esteira de CI/CD para uma stack **React (Vite) + Node.js**, mas os princípios se aplicam a qualquer stack containerizada.

---

## 🧠 Conceito Central: Onde o Build Acontece

A decisão mais importante de toda essa arquitetura não é "qual Registry usar" — é **onde a compilação do código acontece**. Toda a tabela comparativa abaixo deriva dessa única pergunta.

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   BUILD LOCAL / CI EXTERNO          BUILD NA PRÓPRIA VPS   │
│   ──────────────────────            ───────────────────    │
│                                                             │
│   • Docker Hub                      • Git Repository       │
│   • GitLab CI Runners                 (direto no           │
│   • GitHub Actions                    Portainer)           │
│                                                             │
│   VPS apenas faz PULL               VPS faz BUILD + RUN     │
│   da imagem pronta                  (consome CPU/RAM)      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

| Onde builda | Quem consome recurso de build | Resultado na VPS |
|---|---|---|
| Sua máquina local | Seu computador | VPS só baixa e roda |
| Runner externo (GitHub Actions / GitLab CI) | Servidores do GitHub/GitLab | VPS só baixa e roda |
| **Dentro da própria VPS via Portainer** | **A própria VPS de produção** | VPS builda **e** roda — risco de travamento |

---

## 📊 Comparativo Direto: Git vs Docker Hub

| Característica | Imagem do Docker Hub | Repositório Git (Direto no Portainer) |
|---|---|---|
| **Onde o Build ocorre?** | Na máquina local ou em CI/CD externo (GitHub Actions, GitLab CI) | **Dentro da própria VPS**, controlada pelo Portainer |
| **Consumo de Hardware da VPS** | **Mínimo.** A VPS apenas baixa a imagem pronta e executa | **Alto durante o deploy.** A VPS instala Node, roda `npm run build` do Vite e compila os containers |
| **Velocidade do Deploy** | Muito rápido (apenas tempo de download da imagem) | Mais lento (depende do poder de processamento da VPS) |
| **Privacidade do Código** | Código-fonte protegido dentro da imagem | Portainer precisa de acesso direto ao código-fonte para buildar |
| **Praticidade de Configuração** | Exige configurar ferramenta externa para gerar e enviar imagens | **Extremamente simples.** Cola a URL do Git e o Portainer resolve o resto |

---

## 🏗️ Os Quatro Cenários de Deploy

### Cenário 1 — Repositório Git Direto no Portainer

Ideal para projetos menores, VPS com boa folga de hardware, ou quando se quer configuração rápida sem ferramentas externas.

```
[ Código no VS Code ] ──( git push )──> [ GitHub / GitLab ]
                                                │
                                       (Portainer monitora)
                                                ▼
                                 [ VPS baixa o código-fonte ]
                                                │
                                    (Roda o Docker Build)
                                                ▼
                                [ App Online em Containers ]
```

| Vantagens | Desvantagens |
|---|---|
| Setup em menos de 5 minutos | Build consome CPU/RAM da própria VPS de produção |
| "Deu push, atualizou" — fluxo GitOps simples | Pode travar o servidor durante builds pesados (Vite/React) |
| Sem ferramentas externas necessárias | Código-fonte completo fica exposto na VPS |

---

### Cenário 2 — Docker Hub (Registry Puro)

O padrão histórico da indústria. Build feito fora da VPS; apenas a imagem final é distribuída.

```
[ Código no VS Code ] ──( git push )──> [ GitHub ] ──> [ GitHub Actions builda a imagem ]
                                                                    │
                                                            (Envia a imagem)
                                                                    ▼
[ App Online na VPS ] <──( Baixa a imagem ) <── [ Portainer ] <── [ Docker Hub ]
```

| Vantagens | Desvantagens |
|---|---|
| Consumo de CPU/RAM na VPS praticamente zero | Exige build manual ou esteira CI/CD externa configurada |
| Deploy leva segundos | Sem automação real, quebra o conceito de GitOps direto |
| Funciona com qualquer ferramenta de CI/CD (Jenkins, etc.) | Requer `docker login` + `docker push` manual se não automatizado |

**Caso de uso ideal:** projetos open-source com imagem pública, ou quando já existe uma esteira de CI/CD de terceiros que só precisa de um destino final para a imagem.

---

### Cenário 3 — GitLab Container Registry (All-in-One)

O GitLab funciona como plataforma unificada: repositório de código, CI/CD nativo e Container Registry no mesmo lugar — **sem depender do Docker Hub**.

```
[ git push ] ──> [ GitLab detecta o commit ]
                          │
                 ( Ativa GitLab Runner )
                          ▼
          [ Builda frontend Vite + backend Node ]
                          │
              ( Salva no GitLab Container Registry )
                          ▼
        [ Portainer puxa via credenciais de leitura ]
                          ▼
              [ App Online em Containers ]
```

| Vantagens | Desvantagens |
|---|---|
| Centralização total: código + CI/CD + Registry no mesmo lugar | Configurar autenticação no Portainer exige atenção a detalhes |
| Governança e segurança robustas com acessos unificados | URL de imagem mais longa e com formato estrito |
| Sem custo adicional de Registry externo | Pipeline um pouco mais complexa de configurar inicialmente |

**Caso de uso ideal:** empresas e equipes que priorizam privacidade total (código e imagens fechados) e preferem manter toda a esteira de CI/CD dentro da própria infraestrutura corporativa.

---

### Cenário 4 — GitHub + GHCR (Ecossistema Actions)

O padrão mais moderno para quem já trabalha no ecossistema GitHub, especialmente em projetos JavaScript/Node.

```
[ git push ] ──> [ GitHub Actions intercepta ]
                          │
              ( Inicia Runner na nuvem do GitHub )
                          ▼
        [ Compila build estático do Vite + Nginx ]
                          │
                 ( Push para o GHCR )
                          ▼
       [ Portainer recebe Webhook, autentica via PAT ]
                          ▼
              [ App Online em Containers ]
```

| Vantagens | Desvantagens |
|---|---|
| Ecossistema extremamente rápido e maduro | Formato de autenticação do GHCR exige escopos específicos de Token |
| Excelente integração de Webhooks com Portainer | PAT (Personal Access Token) pode confundir na primeira configuração |
| Gigantesco mercado de *Marketplace Actions* prontas | — |

**Caso de uso ideal:** desenvolvedores independentes, startups e projetos já hospedados nativamente no GitHub que querem se beneficiar do ecossistema de automações do mercado.

---

## 📐 Tabela Comparativa Completa dos 4 Cenários

| Característica / Critério | Git Direto | Docker Hub | GitLab Registry | GitHub + GHCR |
|---|---|---|---|---|
| **Onde ocorre o Build?** | Na própria VPS | Local ou CI externo | GitLab Runners | GitHub Actions Runners |
| **Carga de Hardware na VPS** | 🔴 Alta | 🟢 Zero | 🟢 Zero | 🟢 Zero |
| **Velocidade de Deploy** | 🟡 Lenta (depende do hardware) | 🟢 Rápida | 🟢 Rápida | 🟢 Rápida |
| **Privacidade do Código-fonte** | 🔴 Exposto na VPS | 🟢 Protegido na imagem | 🟢 Protegido na imagem | 🟢 Protegido na imagem |
| **Facilidade de Setup Inicial** | 🟢 Extremamente simples | 🟡 Exige ferramenta externa | 🟡 Exige configuração de pipeline | 🟢 Simples via Custom Registry |
| **Tipo de Credencial** | Token Git (deploy key) | Usuário/senha Docker Hub | Personal Access Token / Deploy Token | Personal Access Token (PAT) |
| **Automação CI/CD Nativa** | ❌ Não possui | ❌ Depende de terceiros | ✅ `.gitlab-ci.yml` | ✅ `.github/workflows/*.yml` |
| **Ideal para Webhooks no Portainer** | 🟡 Médio | 🟢 Bom | 🟡 Médio (config. mais complexa) | 🟢 Excelente |
| **Centralização (código+CI+registry)** | Parcial | ❌ Não | ✅ Total | 🟡 Parcial (Registry separado) |

---

## ⚙️ Exemplos de Stack no Portainer CE

Modelos de `docker-compose.yml` para colar diretamente no **Web Editor** do Portainer CE.

### Puxando do Docker Hub

```yaml
version: '3.8'
services:
  backend:
    image: seu_usuario_docker/meu-node-app:latest
    restart: always
    environment:
      - PORT=3000

  frontend:
    image: seu_usuario_docker/meu-react-app:latest
    ports:
      - "80:80"
    restart: always
```

### Puxando do GitLab Container Registry

```yaml
version: '3.8'
services:
  backend:
    image: registry.gitlab.com/seu-grupo/seu-projeto/backend:latest
    restart: always
    environment:
      - PORT=3000

  frontend:
    image: registry.gitlab.com/seu-grupo/seu-projeto/frontend:latest
    ports:
      - "80:80"
    restart: always
```

### Puxando do GitHub Container Registry (GHCR)

```yaml
version: '3.8'
services:
  backend:
    image: ghcr.io/seu-usuario/backend:latest
    restart: always
    environment:
      - PORT=3000

  frontend:
    image: ghcr.io/seu-usuario/frontend:latest
    ports:
      - "80:80"
    restart: always
```

---

## 🔐 Autenticação em Registries Privados

Trabalhar com Registries **privados** no Portainer CE tem uma curva de aprendizado real devido a barreiras de credenciais.

### Passo 1 — Cadastrar o Registry antes do deploy

> ⚠️ **Erro comum:** colocar a imagem privada direto no compose e clicar em deploy. Isso falha com `Image Not Found` se o Registry não foi cadastrado antes.

```
Portainer → Settings → Registries → Add Registry
  → Escolher "Custom Registry"
  → URL: ghcr.io  OU  registry.gitlab.com
  → Inserir usuário + Token
```

### Passo 2 — Usar Token, nunca senha

Nem GitHub nem GitLab aceitam senha tradicional de login para autenticação Docker.

| Plataforma | Tipo de Token | Escopo necessário |
|---|---|---|
| **GitHub (GHCR)** | Personal Access Token (PAT) | `read:packages` |
| **GitLab Registry** | Personal Access Token / Deploy Token | `read_registry` |
| **Docker Hub** | Usuário + senha (ou Access Token) | — |

---

## ⚖️ Portainer CE vs Portainer Business (EE)

O Portainer CE possui limitações deliberadas que empurram o usuário para a versão Business, especialmente em automação GitOps com plataformas privadas.

| Funcionalidade | Portainer CE (Gratuito) | Portainer Business (Pago / 3 Nós Grátis) |
|---|---|---|
| **Múltiplos Registries Privados** | Manual (por nó/ambiente) | Centralizado e herdado por equipes |
| **Compose fora da raiz do Git** | ❌ Não suportado | ✅ Suportado (qualquer subpasta) |
| **Automação GitOps (Webhook/Polling)** | Básica, sujeita a falhas em repos privados | Avançada, com validação de TLS |
| **Variáveis de Ambiente** | Texto puro na interface | Mascaramento de segurança + Vaults |
| **RBAC (Controle de Acesso)** | ❌ Não possui | ✅ Permissões finas por container/stack/volume |

> 🔴 **Bloqueio crítico do CE:** se o `docker-compose.yml` não estiver na **raiz exata** do repositório, o Portainer CE simplesmente não consegue ler o arquivo. Planeje a estrutura do repositório considerando essa limitação.

### Comportamento por Provedor no Portainer CE

| Critério DevOps | Docker Hub | GitLab Registry | GitHub (GHCR) |
|---|---|---|---|
| **Facilidade de Integração no CE** | ⭐⭐⭐⭐⭐ Padrão nativo | ⭐⭐⭐ Formato de URL estrito | ⭐⭐⭐⭐ Simples via Custom Registry |
| **Credencial Requerida** | Usuário/senha Docker Hub | PAT ou Deploy Token | PAT clássico |
| **Webhooks do Portainer** | Bom (gatilhos simples) | Médio (pipeline mais complexa) | Excelente (integração nativa com Actions) |
| **Gargalo de Build na VPS** | Nenhum | Nenhum | Nenhum |

---

## 🧭 Guia de Decisão

```
Sua VPS tem menos de 2GB de RAM?
   SIM → Docker Hub ou GHCR (nunca builde Vite/React na própria VPS)
   NÃO ↓

Você já está 100% no ecossistema GitHub?
   SIM → GitHub + GHCR
   NÃO ↓

Sua empresa exige privacidade total e centralização?
   SIM → GitLab (All-in-One)
   NÃO ↓

Você quer a configuração mais rápida possível, sem CI/CD externo?
   SIM → Repositório Git direto no Portainer (aceitando o custo de hardware)
   NÃO → Reavalie os critérios acima
```

### Resumo por Cenário de Negócio

| Cenário | Recomendação |
|---|---|
| Projeto open-source, imagem pública | **Docker Hub** |
| Startup/dev independente no ecossistema JS | **GitHub + GHCR** |
| Empresa com exigência de governança e privacidade | **GitLab All-in-One** |
| Protótipo rápido, VPS robusta, sem CI/CD | **Git direto no Portainer** |
| VPS pequena (1–2GB RAM) | **Evitar build na VPS — usar qualquer Registry externo** |

---

## ✅ Boas Práticas

- **Nunca builde Vite/React diretamente em VPS com pouca RAM** — o processo de build consome memória de forma intensa e pode travar o backend de produção
- **Sempre cadastre o Registry no Portainer antes de criar a Stack** — evita o erro `Image Not Found`
- **Use Tokens com escopo mínimo necessário** — `read:packages` ou `read_registry`, nunca tokens com permissão total
- **Mantenha o `docker-compose.yml` na raiz do repositório** se for usar Portainer CE — é uma limitação não documentada claramente
- **Versione as imagens com tags semânticas** (`v1.2.0`) em produção — evite depender apenas de `:latest`
- **Configure Webhooks em vez de Polling** quando o Registry suportar — reduz latência de atualização e carga no Portainer
- **Separe ambientes de staging e produção** com Registries ou tags diferentes para evitar deploys acidentais

---

## 🐛 Troubleshooting

| Erro | Causa provável | Solução |
|---|---|---|
| `Image Not Found` | Registry privado não cadastrado no Portainer | Settings → Registries → Add Registry antes do deploy |
| `unauthorized: authentication required` | Token expirado ou escopo insuficiente | Gerar novo PAT com escopo `read:packages` / `read_registry` |
| Build trava ou VPS fica indisponível durante deploy | Build de Vite/React rodando direto na VPS com RAM insuficiente | Migrar para Docker Hub ou GHCR — build fora da VPS |
| Portainer não lê o `docker-compose.yml` do Git | Arquivo fora da raiz do repositório (limitação do CE) | Mover o compose para a raiz ou migrar para Portainer Business |
| Webhook não atualiza a Stack automaticamente | Configuração de autenticação complexa em repo privado | Revisar credenciais do Registry e formato da URL |

---

## 📚 Referências

- [Docker Hub Documentation](https://docs.docker.com/docker-hub/)
- [GitLab Container Registry Docs](https://docs.gitlab.com/ee/user/packages/container_registry/)
- [GitHub Container Registry (GHCR) Docs](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [Portainer CE Documentation](https://docs.portainer.io/)
- [Portainer Business Edition — Comparativo de Planos](https://www.portainer.io/pricing)
- [Docker Compose Specification](https://docs.docker.com/compose/compose-file/)

---

<div align="center">

*Guia mantido como referência viva de arquitetura DevOps para deploys self-hosted.*

</div>