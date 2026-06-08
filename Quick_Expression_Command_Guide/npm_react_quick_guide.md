# 📦 NPM — Guia Completo de Comandos

> **The Complete NPM Command Reference for JavaScript & Node.js Developers**

[![npm](https://img.shields.io/badge/npm-CB3837?style=flat-square&logo=npm&logoColor=white)](https://www.npmjs.com)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)](https://nodejs.org)
[![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)](.)
[![Nível](https://img.shields.io/badge/Nível-Iniciante%20ao%20Avançado-blue?style=flat-square)](.)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](.)

---

## 📋 Índice

- [📦 NPM — Guia Completo de Comandos](#-npm--guia-completo-de-comandos)
  - [📋 Índice](#-índice)
  - [1️⃣ Instalação e Gerenciamento](#1️⃣-instalação-e-gerenciamento)
  - [2️⃣ Auditoria e Manutenção](#2️⃣-auditoria-e-manutenção)
  - [⚡ `npm run` — O Comando Principal](#-npm-run--o-comando-principal)
    - [📐 Sintaxe](#-sintaxe)
    - [📋 Scripts mais usados](#-scripts-mais-usados)
    - [💡 Exemplos Práticos](#-exemplos-práticos)
      - [Listar todos os scripts do projeto](#listar-todos-os-scripts-do-projeto)
      - [Build com watch mode (React / Vite)](#build-com-watch-mode-react--vite)
      - [Testes com filtro de padrão](#testes-com-filtro-de-padrão)
      - [Execução silenciosa (ideal para pipelines CI)](#execução-silenciosa-ideal-para-pipelines-ci)
      - [Dev server com porta customizada](#dev-server-com-porta-customizada)
    - [🔁 Hooks `pre` e `post` Automáticos](#-hooks-pre-e-post-automáticos)
    - [📄 Exemplo Completo `package.json`](#-exemplo-completo-packagejson)
  - [🗺️ Referência Rápida Final](#️-referência-rápida-final)

---

## 1️⃣ Instalação e Gerenciamento

| Comando | Sintaxe | Descrição |
|---|---|---|
| **Inicializar projeto** | `npm init` / `npm init -y` | Cria o `package.json`. Com `-y` aceita todos os padrões sem perguntas |
| **Instalar pacote** | `npm install <pkg>` / `npm i <pkg>` | Instala e adiciona em `dependencies` |
| **Instalar todas** | `npm install` / `npm i` | Instala todas as dependências listadas no `package.json` |
| **Versão específica** | `npm install <pkg>@<versão>` | Instala versão exata — ex: `npm i react@18.2.0` |
| **Dependência de dev** | `npm install <pkg> --save-dev` / `-D` | Adiciona em `devDependencies` (não vai para produção) |
| **Global** | `npm install -g <pkg>` | Instala globalmente, acessível em qualquer projeto do sistema |
| **Desinstalar** | `npm uninstall <pkg>` / `npm remove <pkg>` | Remove o pacote e a entrada no `package.json` |
| **Atualizar tudo** | `npm update` | Atualiza todos os pacotes respeitando os ranges do `package.json` |
| **Atualizar um** | `npm update <pkg>` | Atualiza apenas o pacote especificado |
| **Listar locais** | `npm list` / `npm ls` | Lista todas as dependências instaladas no projeto |
| **Listar globais** | `npm list -g --depth=0` | Lista pacotes instalados globalmente |
| **Instalar para CI** | `npm ci` | Instalação limpa via `package-lock.json` — ideal para pipelines |

> 💡 **Dica:** prefira `npm ci` em ambientes de CI/CD (GitHub Actions, Jenkins, etc.). Ele é mais rápido e garante reprodutibilidade exata do `package-lock.json`.

---

## 2️⃣ Auditoria e Manutenção

| Comando | Sintaxe | Descrição |
|---|---|---|
| **Auditar** | `npm audit` | Verifica vulnerabilidades nas dependências instaladas |
| **Corrigir auto** | `npm audit fix` | Aplica correções automáticas compatíveis com semver |
| **Forçar correção** | `npm audit fix --force` | Força atualizações mesmo com breaking changes — use com cautela |
| **Versão do npm** | `npm --version` / `npm -v` | Exibe a versão atual do npm |
| **Versão do Node** | `node -v` | Exibe a versão atual do Node.js |
| **Bump patch** | `npm version patch` | Incrementa versão patch: `1.0.0` → `1.0.1` |
| **Bump minor** | `npm version minor` | Incrementa versão minor: `1.0.0` → `1.1.0` |
| **Bump major** | `npm version major` | Incrementa versão major: `1.0.0` → `2.0.0` |
| **Versão manual** | `npm version 2.1.3` | Define versão exata no `package.json` + cria tag git |
| **Limpar cache** | `npm cache clean --force` | Remove o cache local do npm |
| **Info do pacote** | `npm info <pkg>` | Exibe metadados de um pacote no registry |

> 💡 **Dica:** `npm version` atualiza o `package.json`, cria um commit e uma tag git automaticamente. Muito útil em fluxos de release automatizados.

---

## ⚡ `npm run` — O Comando Principal

O `npm run` é o coração da automação em projetos JavaScript. Ele executa qualquer script definido no objeto `"scripts"` do seu `package.json`, funcionando como um **task runner nativo** sem dependências externas.

```
Se nenhum script for passado → lista todos os scripts disponíveis
Se um script for passado     → executa o script correspondente
```

---

### 📐 Sintaxe

```bash
# Executa um script pelo nome
npm run <nome-do-script>

# Lista todos os scripts disponíveis no projeto
npm run

# Passa flags e argumentos para o script subjacente
npm run <script> -- --flag --opcao=valor

# Executa em modo silencioso (reduz logs do npm)
npm run <script> --silent
```

> 🔑 **Tudo após `--` é encaminhado diretamente ao script.** Isso permite ajustar comportamentos sem modificar o `package.json`.

---

### 📋 Scripts mais usados

| Script | Definição no `package.json` | Comando | Quando usar |
|---|---|---|---|
| **start** | `"node server.js"` | `npm start` | Inicia a aplicação em produção |
| **dev** | `"nodemon server.js"` | `npm run dev` | Servidor local com hot-reload |
| **build** | `"react-scripts build"` | `npm run build` | Gera bundle otimizado para produção |
| **test** | `"jest"` | `npm test` / `npm run test` | Executa a suíte de testes |
| **lint** | `"eslint src/"` | `npm run lint` | Verifica padrões e erros de código |
| **format** | `"prettier --write src/"` | `npm run format` | Formata o código automaticamente |
| **clean** | `"rm -rf dist/"` | `npm run clean` | Remove artefatos de build anteriores |
| **deploy** | `"gh-pages -d build"` | `npm run deploy` | Publica em ambiente de produção |
| **serve** | `"parcel src/index.html"` | `npm run serve` | Serve a aplicação localmente |
| **eject** | `"react-scripts eject"` | `npm run eject` | Expõe configurações internas (irreversível) |

---

### 💡 Exemplos Práticos

#### Listar todos os scripts do projeto

```bash
npm run
```

```
Scripts available in my-project via `npm run-script`:

  dev
    nodemon server.js
  build
    react-scripts build
  test
    jest
  lint
    eslint src/
```

---

#### Build com watch mode (React / Vite)

```bash
# Recompila automaticamente a cada alteração
npm run build -- --watch --mode=development
```

---

#### Testes com filtro de padrão

```bash
# Executa apenas testes cujo nome contém "LoginPage"
npm run test -- --grep="LoginPage"

# Equivalente com Jest
npm run test -- --testNamePattern="LoginPage"
```

---

#### Execução silenciosa (ideal para pipelines CI)

```bash
# Suprime logs internos do npm, mantém só a saída do script
npm run deploy --silent
```

---

#### Dev server com porta customizada

```bash
# Passa a porta diretamente para o Vite/React sem editar o package.json
npm run dev -- --port=3001
```

---

### 🔁 Hooks `pre` e `post` Automáticos

O npm executa automaticamente scripts com prefixo `pre` e `post` antes e depois de qualquer script principal — sem nenhuma configuração adicional.

```json
{
  "scripts": {
    "prelint":    "echo '🔍 Verificando código...'",
    "lint":       "eslint src/",
    "postlint":   "echo '✅ Lint concluído'",

    "prebuild":   "npm run lint",
    "build":      "react-scripts build",
    "postbuild":  "npm run test",

    "pretest":    "npm run clean",
    "test":       "jest --coverage",
    "posttest":   "echo '📊 Relatório gerado em coverage/'"
  }
}
```

**Fluxo de execução ao rodar `npm run build`:**

```
1. prebuild  → npm run lint     (valida o código)
2. build     → react-scripts build  (gera o bundle)
3. postbuild → npm run test     (confirma que os testes passam)
```

> 💡 **Boas práticas com hooks:**
> - Use `prebuild` para garantir que o código está limpo antes de buildar
> - Use `postbuild` para executar testes de integração ou notificações
> - Mantenha hooks rápidos para não impactar o tempo de desenvolvimento

---

### 📄 Exemplo Completo `package.json`

Estrutura de referência para um projeto React com todas as automações configuradas:

```json
{
  "name": "my-react-app",
  "version": "1.0.0",
  "description": "Aplicação React com scripts de automação completos",
  "scripts": {
    "start":      "react-scripts start",
    "dev":        "react-scripts start",
    "build":      "react-scripts build",
    "test":       "react-scripts test",
    "test:ci":    "react-scripts test --watchAll=false --coverage",
    "eject":      "react-scripts eject",
    "lint":       "eslint src/ --ext .js,.jsx,.ts,.tsx",
    "lint:fix":   "eslint src/ --ext .js,.jsx,.ts,.tsx --fix",
    "format":     "prettier --write 'src/**/*.{js,jsx,ts,tsx,css,md}'",
    "clean":      "rm -rf build/ coverage/",
    "prebuild":   "npm run lint",
    "postbuild":  "echo '✅ Build concluído com sucesso'",
    "predeploy":  "npm run build",
    "deploy":     "gh-pages -d build"
  },
  "dependencies": {
    "react":     "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "eslint":    "^8.0.0",
    "prettier":  "^3.0.0",
    "gh-pages":  "^6.0.0"
  }
}
```

---

## 🗺️ Referência Rápida Final

```bash
# ── PROJETO ──────────────────────────────────────────────
npm init -y                        # Cria package.json com padrões
npm install                        # Instala todas as dependências
npm ci                             # Instalação limpa (CI/CD)

# ── PACOTES ──────────────────────────────────────────────
npm i <pkg>                        # Instala como dependência
npm i <pkg> -D                     # Instala como devDependency
npm i <pkg>@18.2.0                 # Versão específica
npm i -g <pkg>                     # Instala globalmente
npm uninstall <pkg>                # Remove pacote
npm update                         # Atualiza todos

# ── SCRIPTS ──────────────────────────────────────────────
npm run                            # Lista scripts disponíveis
npm start                          # Atalho para npm run start
npm test                           # Atalho para npm run test
npm run dev                        # Inicia servidor de desenvolvimento
npm run build                      # Gera build de produção
npm run lint                       # Verifica qualidade do código
npm run format                     # Formata o código
npm run deploy                     # Publica em produção
npm run <script> -- --flag=valor   # Passa argumento para o script

# ── MANUTENÇÃO ───────────────────────────────────────────
npm audit                          # Verifica vulnerabilidades
npm audit fix                      # Corrige automaticamente
npm version patch                  # Bump de versão (patch)
npm version minor                  # Bump de versão (minor)
npm version major                  # Bump de versão (major)
npm cache clean --force            # Limpa cache local
npm list                           # Lista dependências do projeto
npm list -g --depth=0              # Lista pacotes globais
```

---

<div align="center">

*Referência mantida com foco em precisão técnica e uso real em produção.*

[![npm Docs](https://img.shields.io/badge/Documentação%20Oficial-npm-CB3837?style=for-the-badge&logo=npm)](https://docs.npmjs.com)
[![Node.js](https://img.shields.io/badge/Node.js-nodejs.org-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org)

</div>
