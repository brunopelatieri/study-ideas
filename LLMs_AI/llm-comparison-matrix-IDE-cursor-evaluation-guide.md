# Guia de Avaliação e Comparação de LLMs para Desenvolvimento no Cursor IDE

Este documento foi estruturado para servir como material de pesquisa técnica no GitHub. O objetivo é apoiar o time de engenharia na tomada de decisão sobre qual modelo alocar em cada etapa do ciclo de desenvolvimento de software (SDLC), maximizando a eficiência de custos e a precisão do código gerado dentro do ecossistema do **Cursor IDE**.

---

## Avaliação: O Composer 2 do Cursor é bom?

**Sim, o Composer 2 é excelente e redefine o custo-benefício dentro do Cursor.**

Diferente de LLMs genéricos, o Composer 2 é um modelo proprietário da Anysphere (criadora do Cursor) treinado especificamente para agir como um agente de engenharia de software dentro da IDE. Ele não foca em conversação longa ou redação, mas sim na geração precisa de **Diffs**, manipulação multi-arquivos e execução de comandos no terminal.

> **Veredito para o Time:** O Composer 2 deve ser o modelo padrão do time para refatorações complexas e criação de features multi-arquivos diretamente na interface do Composer. Ele supera modelos mais robustos e caros em velocidade e aplicação de patches locais porque foi treinado exatamente no mesmo *harness* (infraestrutura) da IDE.

---

## Tabela Comparativa de LLMs (Foco em Engenharia de Software)

*Ranqueado estritamente do melhor (SOTA) para o menor em capacidade de codificação e automação agentica.*

| Rank | Modelo | Custo Médio (In/Out por 1M) | Core Strengths (Foco Pareto) | Slot Recomendado no Cursor |
| --- | --- | --- | --- | --- |
| **1** | Claude Opus 4.7 | $5.00 / $25.00 | SOTA absoluto em SWE-bench (87.6%), raciocínio adaptativo extremo. | Arquitetura, Bugs Complexos de Lógica |
| **2** | GPT-5.5 | $5.00 / $30.00 | Altíssima eficiência de tokens, prompts focados em resultados (*outcome-first*). | Microserviços, Orquestração de APIs |
| **3** | Gemini 3.1 Pro | $7.00 / $21.00 | Janela de 1M+ estável, excelente raciocínio lógico (ARC-AGI-2: 77.1%). | Engenharia Reversa, Grandes Codebases |
| **4** | GPT-5.4 | $2.50 / $15.00 | Padrão ouro para agentes autônomos fora da IDE. | Scripts de CI/CD e Automações de Infra |
| **5** | **Cursor Composer 2** | **$0.50 / $2.50** | **Nativo da IDE, geração cirúrgica de Diffs multi-arquivos, custo-benefício imbatível.** | **Default Composer (Multi-file Features)** |
| **6** | Claude Sonnet 4.6 | $3.00 / $15.00 | Equilíbrio perfeito entre velocidade e inteligência geral de código. | Default Chat / Discussões de Código |
| **7** | Claude Opus 4.6 | $5.00 / $25.00 | Raciocínio profundo de contexto longo, mas superado em custo pelo 4.7. | Legado (Substituir pelo Opus 4.7) |
| **8** | Codex 5.3 | Sob Consulta | Geração direta de código bruto com baixa alucinação de assinaturas. | Geração de Boilerplates / Testes Unitários |
| **9** | GPT-5.2 | High Tier Standard | Modelo generalista forte, mas ineficiente em custo perante o 5.4. | Tarefas gerais de backend |
| **10** | Codex 5.2 | Sob Consulta | Focado em preenchimento e sintaxe pura. | Autocomplete / Correção de Sintaxe |
| **11** | Claude Opus 4.5 | $5.00 / $25.00 | Primeiro da linha 4.x com raciocínio focado em ferramentas. | Legado (Migrar para 4.6/4.7) |
| **12** | GPT-5 | Generacional Base | Janela de contexto ampla, mas com alta taxa de retentativas (*retries*). | Legado |
| **13** | Claude Sonnet 4.5 | Mid Tier Standard | Boa velocidade, mas superado pelo Sonnet 4.6. | Legado |
| **14** | Claude Sonnet 4 | Mid Tier Standard | Base da geração 4, obsoleto para código complexo. | Legado |
| **15** | GPT-5.4 Mini | $0.15 / $0.60 | Respostas rápidas e estruturadas em JSON/YAML. | Configurações, Dockerfiles, Chat rápido |
| **16** | GPT-5 Mini | Low Tier Standard | Execução de tarefas simples em lote. | Scripts auxiliares de migração |
| **17** | Grok 4.3 | Custom API | Respostas diretas e excelente extração de dados brutos. | Análise de Logs densos / Scripting rápido |
| **18** | Gemini 3 Flash | Ultra-Low Cost | Latência incrivelmente baixa em contexto médio. | Inline Edit (Cmd+K) / Explicação de Erros |
| **19** | Gemini 2.5 Flash | Ultra-Low Cost | Processamento massivo de arquivos leves em lote. | Refatorações simples de Naming Docs |
| **20** | GPT-5 Nano | Micro-Cost | Ultra veloz, rodando próximo à borda. | Inline Predict / Próxima Linha (Tab) |

---

## O que Importa de Cada LLM (Regra de Pareto)

### 1. Claude Opus 4.7

* **O 80/20:** Maior taxa de acerto do mercado em correção de bugs de produção reais (87.6% no SWE-bench Verified). Conserta o gerenciamento de ferramentas complexas (MCP Atlas) que falhava na versão 4.6.
* **Uso no Cursor:** Ative via Chat/Composer apenas quando o Composer 2 travar em uma alucinação de lógica pura ou arquitetura multi-camadas.

### 2. GPT-5.5

* **O 80/20:** Reduziu drasticamente o número de tokens de raciocínio (*thinking tokens*) necessários para entregar a mesma qualidade das versões anteriores. Excelente para entender o "objetivo final" sem precisar receber instruções passo a passo.
* **Uso no Cursor:** Ideal para criar integrações complexas de API onde você descreve o resultado esperado e ele gerencia os *edge cases*.

### 3. Gemini 3.1 Pro

* **O 80/20:** Saltou para 77.1% no ARC-AGI-2 (dobro da inteligência do 3 Pro). Mantém a melhor ingestão de repositórios inteiros sem degradação de atenção ao longo do contexto.
* **Uso no Cursor:** Excelente para usar com o recurso `@Repository` quando você precisa mapear o impacto de uma alteração estrutural em uma base de código gigante de forma holística.

### 4. GPT-5.4

* **O 80/20:** Consolidou a estabilidade de respostas em tarefas repetitivas com preço reduzido pela metade se comparado ao GPT-5.5.
* **Uso no Cursor:** Excelente alternativa de Chat diário para documentação e entendimento de regras de negócio.

### 5. Cursor Composer 2

* **O 80/20:** Custo extremamente baixo ($0.50/$2.50 MTok) aliado a um treinamento via Aprendizado por Reforço (RL) focado em tarefas de longa duração e geração de Diffs precisos. Ele não tenta reescrever o arquivo todo, ele apenas aplica o bloco alterado.
* **Uso no Cursor:** **O cavalo de batalha do time.** Deve ser mantido ativo por padrão na janela do Composer para codificação diária.

### 6. Claude Sonnet 4.6

* **O 80/20:** É 40% mais barato que a linha Opus mantendo uma velocidade de resposta altíssima para tarefas padrões de desenvolvimento.
* **Uso no Cursor:** Modelo ideal para deixar selecionado na barra lateral de Chat para consultas rápidas sobre sintaxe, padrões de projeto e perguntas teóricas.

### 7. Claude Opus 4.6

* **O 80/20:** Introduziu o salto inicial de raciocínio e o aumento de capacidade para saídas de até 128k tokens, eliminando truncamentos em códigos longos.
* **Uso no Cursor:** Substituível pelo 4.7 devido à paridade de preço, mas ainda útil se houver gargalos de concorrência na API da Anthropic.

### 8. Codex 5.3

* **O 80/20:** Engenharia focada puramente na tradução de especificações técnicas para blocos de código limpos, livre de "conversas" e explicações de texto.
* **Uso no Cursor:** Ótimo para geração em massa de arquivos de testes unitários (`.spec`, `.test`).

### 9. GPT-5.2

* **O 80/20:** Resolveu os problemas crônicos de "preguiça" de código que afetavam a geração 4 do GPT, fornecendo scripts completos sem omissões (`// todo: implement here`).
* **Uso no Cursor:** Intermediário para automações gerais.

### 10. Codex 5.2

* **O 80/20:** Baixíssima latência para preenchimento de funções isoladas baseadas em comentários explicativos JSDoc/Docstrings.
* **Uso no Cursor:** Utilizado em pipelines de geração automática de código legado.

### 11. Claude Opus 4.5

* **O 80/20:** Trouxe a primeira janela estável de 200k com foco em *Computer Use*, abrindo espaço para testes automáticos de interface (UI).
* **Uso no Cursor:** Modelo legado. Recomendado migrar para garantir maior precisão de tipos (TypeScript/Go).

### 12. GPT-5

* **O 80/20:** Modelo que inaugurou a era do raciocínio complexo nativo na OpenAI, porém pesado e propenso a loops de retentativa em tarefas agenticas complexas.
* **Uso no Cursor:** Despriorizar em favor do GPT-5.4/5.5 ou Composer 2.

### 13 a 14. Claude Sonnet 4.5 / Sonnet 4

* **O 80/20:** Modelos de transição com foco em velocidade de listagem de arquivos.
* **Uso no Cursor:** Obsoletos para o fluxo atual de desenvolvimento em IDE.

### 15. GPT-5.4 Mini

* **O 80/20:** Velocidade extrema com custo quase nulo e suporte impecável a *Structured Outputs* (JSON Schema estrito).
* **Uso no Cursor:** Excelente para gerar payloads de teste, mocks de dados e arquivos de configuração (`.yaml`, `.json`).

### 16. GPT-5 Mini

* **O 80/20:** Primeiro modelo "Mini" a lidar de forma aceitável com lógica condicional simples sem quebrar identação.
* **Uso no Cursor:** Legado em relação ao 5.4 Mini.

### 17. Grok 4.3

* **O 80/20:** Forte capacidade de parsing de logs textuais não estruturados e depuração rápida de saídas de console complexas.
* **Uso no Cursor:** Copie o dump de erro do terminal e use no Chat para um diagnóstico rápido e direto.

### 18. Gemini 3 Flash

* **O 80/20:** Latência imbatível. Perfeito para pequenas alterações de código onde o tempo de resposta do desenvolvedor dita o ritmo de trabalho.
* **Uso no Cursor:** Ideal para o recurso inline `Cmd+K` para refatorar métodos pequenos ou alterar nomes de variáveis em massa.

### 19. Gemini 2.5 Flash

* **O 80/20:** Processamento rápido de tarefas paralelas simples.
* **Uso no Cursor:** Útil se configurado para rotinas de linting customizadas via extensões.

### 20. GPT-5 Nano

* **O 80/20:** Otimizado ao extremo para predição local de tokens (o "próximo caractere").
* **Uso no Cursor:** Roda implicitamente por trás do motor de Autocomplete nativo da IDE (Cursor Tab).

---

## Mapeamento Prático para o Time (Decisão por Etapa)

Para maximizar a produtividade e otimizar os créditos de API do time, adote a seguinte matriz de decisão dentro do Cursor IDE:

1. **Fase de Planejamento e Arquitetura:**
* *Ferramenta:* Cursor Chat Lateral (`Ctrl+L`)
* *Modelo:* **Claude Opus 4.7** ou **Gemini 3.1 Pro** (com `@Repository`)


2. **Desenvolvimento de Features e Refatoração Multi-arquivos:**
* *Ferramenta:* Cursor Composer (`Ctrl+I`)
* *Modelo:* **Cursor Composer 2** (Padrão)


3. **Pequenos Ajustes de Escopo Local (Inline Edit):**
* *Ferramenta:* Edição Direta (`Cmd+K` / `Ctrl+K`)
* *Modelo:* **Claude Sonnet 4.6** ou **Gemini 3 Flash**


4. **Escrita de Testes e Mocks:**
* *Ferramenta:* Cursor Composer ou Chat
* *Modelo:* **Codex 5.3** ou **GPT-5.4 Mini**
  
---

# 👤 Autor

Bruno Pelatieri Goulart  
Enterprise Automation Architect • AI • DevOps • n8n Specialist

---