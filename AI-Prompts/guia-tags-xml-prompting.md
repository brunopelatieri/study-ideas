# Guia de Tags XML para Prompt Engineering — Referência (Claude e LLMs em geral)

## Objetivo

Documento de referência rápida para estruturar prompts com tags XML: quais tags usar, como colocar o objetivo, como lidar com múltiplos objetivos, e um template pronto para copiar e colar.

---

## Por que tags XML funcionam tão bem com Claude

Claude foi treinado com grandes volumes de dados contendo XML, então o modelo tem uma sensibilidade natural a esse formato — inclusive os próprios prompts internos da Anthropic usam XML pesadamente. Na prática, tags XML resolvem um problema específico: quando um prompt mistura contexto, instruções, exemplos e dados de entrada em texto corrido, o modelo pode confundir onde termina uma coisa e começa outra. Cada bloco em sua própria tag elimina essa ambiguidade.

Ponto importante da documentação oficial: **não existe um conjunto "canônico" de tags que o Claude foi treinado especificamente para reconhecer.** O que importa é que o nome da tag faça sentido com o conteúdo que ela envolve, e que você seja consistente — use o mesmo nome de tag em todo o prompt, e refira-se a ele no texto (ex: "usando o contrato dentro das tags `<contract>`...").

**Regra de ouro:** mostre seu prompt para um colega com pouco contexto sobre a tarefa e peça para ele seguir as instruções. Se ele ficaria confuso, o Claude também ficará.

---

## O conjunto essencial de tags

Não existe lista oficial obrigatória, mas este é o conjunto que cobre praticamente qualquer prompt técnico:

| Tag | Para que serve | Quando usar |
|---|---|---|
| `<role>` | Define a persona/expertise do modelo | Geralmente no system prompt; foca tom e comportamento |
| `<context>` | Informações de fundo, estado do sistema, arquivos referenciados | Sempre que houver background necessário para entender a tarefa |
| `<task>` ou `<objective>` | O objetivo principal — o que deve ser feito | Um por prompt, idealmente (ver seção abaixo) |
| `<instructions>` | Passos sequenciais de execução | Quando a ordem ou a completude dos passos importa |
| `<constraints>` ou `<rules>` | Restrições, limites de escopo, o que NÃO fazer | Evita que o modelo extrapole o pedido |
| `<example>` / `<examples>` | Exemplos de entrada/saída (multishot) | 3–5 exemplos melhoram muito consistência e formato |
| `<output_format>` ou `<format>` | Estrutura esperada da resposta final | JSON, markdown com seções fixas, schema específico |
| `<document>` / `<documents>` | Documentos de referência, com subtags `<source>` e `<document_content>` | Contexto longo, múltiplos arquivos/PRDs/specs |
| `<data>` ou `<input>` | Dados brutos e variáveis da tarefa | Quando há dados dinâmicos separados das instruções fixas |
| `<thinking>` / `<answer>` | Separa raciocínio de resposta final | Chain-of-thought manual, quando o "pensar" nativo está desligado |

---

## Como colocar o objetivo em tag

Use `<task>` ou `<objective>` — os dois são válidos e comuns; o importante é escolher **um só nome e manter consistência no resto do prompt**. Prefiro `<task>` porque é mais curto e mapeia direto para "o que fazer", deixando `<context>` para "o que você precisa saber antes".

```xml
<task>
Revisar a spec-048 e validar se ela atende integralmente aos requisitos do prd-047,
apontando lacunas e riscos técnicos.
</task>
```

Boas práticas dentro de `<task>`:
- Uma frase objetiva, no imperativo, dizendo exatamente o resultado esperado.
- Não misture instruções passo a passo aqui — isso vai em `<instructions>`.
- Não misture contexto/background aqui — isso vai em `<context>`.

---

## Posso colocar mais de uma tag `<task>`?

Tecnicamente sim — XML não impede tags repetidas, e o Claude não vai "quebrar" com isso. Mas na prática, múltiplas tags `<task>` soltas e sem hierarquia tendem a gerar ambiguidade sobre qual é a prioridade ou se são sequenciais/paralelas. Existem três padrões melhores, dependendo do caso:

**1. Um objetivo com sub-passos → mantenha uma única `<task>` com lista**
```xml
<task>
1. Analisar o PRD e extrair os requisitos.
2. Mapear cada requisito à spec correspondente.
3. Listar lacunas encontradas.
</task>
```

**2. Objetivos genuinamente distintos e sequenciais → tags indexadas**
```xml
<task_1 name="analise">
Revisar a spec-048 contra o prd-047 e listar lacunas.
</task_1>

<task_2 name="implementacao">
A partir das lacunas encontradas, propor os ajustes necessários na spec.
</task_2>
```

**3. Objetivos com hierarquia clara → agrupe em um container**
```xml
<tasks>
<task id="1">Analisar aderência da spec ao PRD</task>
<task id="2">Apontar riscos técnicos da integração com Evolution API</task>
</tasks>
```

Recomendação prática: se as tarefas rodam em sequência dentro da mesma resposta, prefira o padrão 1 (mais simples). Se são etapas que você quer poder referenciar separadamente (ex: em um pipeline de prompts encadeados, ou pedindo pra Claude tratar cada uma com um nível de profundidade diferente), use o padrão 2 ou 3.

---

## Nesting (aninhamento)

Tags devem ser aninhadas quando o conteúdo tem hierarquia natural — por exemplo, vários documentos dentro de `<documents>`, ou vários exemplos dentro de `<examples>`:

```xml
<documents>
<document>
<source>docs/prd-047-canais-e-whatsapp-qrcode.md</source>
<document_content>
[conteúdo do PRD]
</document_content>
</document>
<document>
<source>docs/spec-048-canal-whatsapp-qrcode.md</source>
<document_content>
[conteúdo da spec]
</document_content>
</document>
</documents>
```

```xml
<examples>
<example>
<input>...</input>
<output>...</output>
</example>
<example>
<input>...</input>
<output>...</output>
</example>
</examples>
```

---

## Exemplo completo aplicado (usando seu próprio caso)

Aplicando ao contexto do Zap CRM BR que você trouxe:

```xml
<role>
Você é um Engenheiro de Software Sênior especializado em integrações de API
de mensageria e arquitetura de sistemas.
</role>

<context>
O sistema Zap CRM BR possui a capacidade de cadastrar e gerenciar instância
WhatsApp não oficial via QRCode, para chat de conversation integrado ao
software externo opensource via API Evolution GO.
</context>

<documents>
<document>
<source>docs/prd-047-canais-e-whatsapp-qrcode.md</source>
</document>
<document>
<source>docs/spec-048-canal-whatsapp-qrcode.md</source>
</document>
</documents>

<task>
Revisar a spec-048 e validar se ela atende integralmente aos requisitos
funcionais e não-funcionais definidos no prd-047.
</task>

<instructions>
1. Extraia os requisitos do PRD.
2. Mapeie cada requisito a uma seção correspondente da spec.
3. Liste requisitos do PRD não cobertos pela spec.
4. Aponte riscos técnicos específicos da integração via QR Code com a
   Evolution API (ex: reconexão de sessão, rate limits, tratamento de webhook).
</instructions>

<constraints>
- Não sugira mudança de escopo do PRD — apenas avalie aderência.
- Use somente informações presentes nos documentos fornecidos.
</constraints>

<output_format>
Markdown com as seções: Requisitos Cobertos, Lacunas Encontradas,
Riscos Técnicos, Recomendações.
</output_format>
```

---

## Boas práticas

- **Consistência de nomes:** se chamou de `<context>` no início, não troque para `<background>` depois no mesmo prompt.
- **Nomes descritivos:** `<constraints>` diz mais que `<c>`; `<output_format>` diz mais que `<fmt>`.
- **Combine com few-shot:** `<examples>` com 3–5 exemplos bem escolhidos (relevantes, diversos, cobrindo casos de borda) melhora mais o resultado do que instruções longas em prosa.
- **Combine com chain-of-thought:** se você desativou o "thinking" nativo ou quer controlar o raciocínio manualmente, use `<thinking>` para o raciocínio e `<answer>` para a resposta final — isso separa claramente o processo do resultado.
- **Dados longos no topo:** em prompts com muito contexto (documentos grandes, 20k+ tokens), coloque `<documents>`/`<context>` no início do prompt e a `<task>` por último — isso melhora a qualidade da resposta.
- **Prefira dizer o que fazer, não o que não fazer:** ao invés de "não use markdown", prefira "escreva a resposta em parágrafos de prosa corrida".

---

## Erros comuns

- Colocar instruções passo a passo dentro de `<context>` (deveriam estar em `<instructions>`).
- Usar tags diferentes para a mesma coisa ao longo do prompt (`<goal>` em um lugar, `<task>` em outro).
- Empilhar múltiplas `<task>` soltas sem indicar se são sequenciais, paralelas ou alternativas.
- Aninhar demais sem necessidade — nesting deve refletir hierarquia real do conteúdo, não ser decorativo.
- Confundir `<constraints>` (restrições rígidas) com `<instructions>` (passos a seguir).

---

## Isso funciona em outros LLMs?

Sim, com uma ressalva. Delimitadores estruturados (XML, mas também Markdown com headers, ou JSON) ajudam **qualquer** modelo a separar contexto de instrução — isso é um princípio geral de clareza, não exclusivo do Claude. A diferença é que o Claude foi exposto a grandes volumes de XML durante o treinamento e reconhece tags mesmo sem nomes "oficiais", enquanto outros modelos (GPT, Gemini) tendem a responder igualmente bem a Markdown com headers (`## Contexto`, `## Tarefa`) ou JSON estruturado. Ou seja: a estrutura deste guia é portável — só troque tags XML por headers Markdown se estiver mirando um modelo menos habituado a XML.

---

## Template rápido para copiar e colar

```xml
<role>
[Persona/expertise do modelo]
</role>

<context>
[Contexto, background, estado atual, arquivos referenciados]
</context>

<task>
[Objetivo único e claro — o que deve ser entregue]
</task>

<instructions>
1. [passo 1]
2. [passo 2]
3. [passo 3]
</instructions>

<constraints>
- [restrição 1]
- [restrição 2]
</constraints>

<examples>
<example>
<input>[entrada de exemplo]</input>
<output>[saída esperada]</output>
</example>
</examples>

<output_format>
[Formato exato esperado da resposta]
</output_format>
```

---

## Referências

- Prompting best practices — Claude Platform Docs: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
- Use XML tags to structure your prompts: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/use-xml-tags
- Prompt engineering overview: https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview
