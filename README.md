# Gupy Skills para Candidatos

Coleção pública de **Agent Skills** da Gupy para apoiar candidatos na jornada de emprego — desde melhorar o perfil no portal até entender como a triagem por IA funciona e receber orientações práticas de empregabilidade.

As skills seguem o padrão aberto [`SKILL.md`](https://agentskills.io): um arquivo Markdown com instruções que o agente de IA segue quando a skill é acionada.

> **Em evolução:** este repositório crescerá com novas skills. Hoje estão disponíveis **Melhorar meu currículo**, **Guia de triagem** e **Mapa de atratividade de vagas**.

---

## Skills disponíveis

| Skill | Descrição | MCP opcional |
|-------|-----------|--------------|
| [`analise-de-curriculo`](skills/analise-de-curriculo/SKILL.md) | Checklist de perfil Gupy, diagnóstico de experiências e habilidades, entrevista para redigir resultados reais e sugestões de palavras-chave por área | Sim — busca padrões de vagas no portal |
| [`guia-de-triagem`](skills/guia-de-triagem/SKILL.md) | Explica como a IA da Gupy ordena perfis, o que não analisa (dados sensíveis), quem decide aprovação e como lidar com frustração por reprovações | Não |
| [`radar-de-vagas`](skills/radar-de-vagas/SKILL.md) | Compara oportunidades no portal Gupy com tabela de 6 dimensões (0–5), destaques e próximos passos; radar exportável quando o ambiente permitir | Sim — coleta e pontua vagas públicas |

### Como usar uma skill

Depois de instalada, **peça em linguagem natural** o que você precisa. Exemplos:

- *"Quero melhorar meu currículo na Gupy"*
- *"Me ajuda a completar as experiências do meu perfil"*
- *"O que está faltando no meu cadastro para aparecer melhor nas vagas de atendimento?"*
- *"A IA da Gupy está me recusando automaticamente?"*
- *"Como funciona a triagem da Gupy?"*
- *"A IA olha minha idade ou onde eu moro?"*
- *"Quem decide se eu passo de fase no processo seletivo?"*
- *"Estou indeciso entre aplicar no Itaú ou no Nubank — dev Java pleno em SP"*
- *"Quais hospitais em SC estão mais atrativos no portal Gupy?"*
- *"Compara vagas de analista de dados sênior remoto"*

O agente deve reconhecer o contexto e aplicar a skill automaticamente. Se não aplicar, invoque pelo nome:

- *"Use a skill analise-de-curriculo para revisar meu perfil"*
- *"Use a skill guia-de-triagem para explicar a triagem"*
- *"Use a skill radar-de-vagas para comparar essas empresas"*

---

## Instalação rápida (recomendada)

A forma mais simples é o [Skills CLI](https://github.com/vercel-labs/skills) (`npx skills`), que instala as skills no diretório correto de cada ferramenta.

**Pré-requisito:** [Node.js 18+](https://nodejs.org/) (para o `npx`).

```bash
# Ver skills disponíveis neste repositório
npx skills add gupy-io/gupy-skills-candidato --list

# Instalar todas as skills (modo interativo)
npx skills add gupy-io/gupy-skills-candidato

# Instalar uma skill específica, globalmente, para o Cursor
npx skills add gupy-io/gupy-skills-candidato \
  --skill analise-de-curriculo \
  -g -a cursor -y

npx skills add gupy-io/gupy-skills-candidato \
  --skill guia-de-triagem \
  -g -a cursor -y

npx skills add gupy-io/gupy-skills-candidato \
  --skill radar-de-vagas \
  -g -a cursor -y

# Instalar todas as skills para Claude Code e Gemini CLI
npx skills add gupy-io/gupy-skills-candidato -g -a claude-code -a gemini-cli -y
```

| Comando | O que faz |
|---------|-----------|
| `npx skills list` | Lista skills instaladas |
| `npx skills update` | Atualiza para a versão mais recente do repositório |
| `npx skills remove <nome-da-skill>` | Remove uma skill (ex.: `analise-de-curriculo`, `guia-de-triagem`, `radar-de-vagas`) |
| `npx skills use gupy-io/gupy-skills-candidato@<nome-da-skill>` | Usa sem instalar (gera prompt temporário) |

**Escopo da instalação:**

| Flag | Onde instala | Quando usar |
|------|--------------|-------------|
| *(padrão)* | Pasta do projeto (ex.: `.agents/skills/`) | Compartilhar com o time ou versionar no repo |
| `-g` / `--global` | Pasta do usuário (ex.: `~/.cursor/skills/`) | Disponível em qualquer projeto |

---

## Instalação por ferramenta

Nem toda ferramenta suporta o `npx skills`. A tabela abaixo resume o método e o suporte a MCP.

| Ferramenta | `npx skills` | Instalação manual | MCP Gupy (opcional) |
|------------|:------------:|:-----------------:|:-------------------:|
| [Cursor](#cursor) | Sim | Sim | Sim |
| [Claude Code](#claude-code) | Sim | Sim | Sim |
| [Gemini CLI](#gemini-cli) | Sim | Sim | Depende do host |
| [GitHub Copilot](#github-copilot) | Sim | Sim | No VS Code |
| [Windsurf / Cline / OpenCode](#outras-ferramentas-de-código) | Sim | Sim | Depende do host |
| [Claude Desktop](#claude-desktop) | Não | ZIP | Sim (conectores) |
| [ChatGPT](#chatgpt) | Não | Custom GPT / colar instruções | Limitado |
| [Gemini (web/app)](#gemini-web-e-app) | Não | Gems / instruções | Não |

---

### Cursor

**Via CLI (recomendado):**

```bash
npx skills add gupy-io/gupy-skills-candidato -g -a cursor -y
```

**Manual:** copie as pastas em `skills/` para o diretório de skills do Cursor:

- Global: `~/.cursor/skills/<nome-da-skill>/`
- Por projeto: `.agents/skills/<nome-da-skill>/`

Ex.: `analise-de-curriculo`, `guia-de-triagem`

Reinicie o Cursor ou abra um novo chat de agente. As skills aparecem em **Cursor Settings → Rules → Agent Skills**.

**MCP opcional:** em **Cursor Settings → MCP**, adicione o servidor MCP do portal Gupy para candidatos (veja [Integração MCP](#integração-mcp-opcional)).

---

### Claude Code

**Via CLI (recomendado):**

```bash
npx skills add gupy-io/gupy-skills-candidato -g -a claude-code -y
```

**Manual:** copie cada pasta de `skills/` para `~/.claude/skills/<nome-da-skill>/` (global) ou `.claude/skills/<nome-da-skill>/` (projeto).

O Claude Code descobre skills automaticamente na inicialização. Para forçar o uso:

```text
/skill analise-de-curriculo
/skill guia-de-triagem
```

**MCP opcional:** configure em `~/.claude.json` ou `.mcp.json` do projeto.

---

### Claude Desktop

O Claude Desktop **não** usa o `npx skills`. É preciso fazer upload de um pacote ZIP.

1. Baixe ou clone este repositório.
2. Compacte **apenas** a pasta da skill (não o repositório inteiro). Repita para cada skill que quiser instalar:

   ```text
   analise-de-curriculo.zip
   └── analise-de-curriculo/
       ├── SKILL.md
       └── references/
           └── manual-empregabilidade.md

   guia-de-triagem.zip
   └── guia-de-triagem/
       ├── SKILL.md
       └── evals/
           └── evals.json
   ```

   > O nome da pasta no ZIP deve ser **idêntico** ao campo `name` no frontmatter do `SKILL.md` (`analise-de-curriculo` ou `guia-de-triagem`).

3. No Claude Desktop: **Settings → Capabilities → Skills → Upload Skill**.
4. Ative a skill após o upload.

**MCP opcional:** em **Settings → Connectors**, adicione o MCP do portal Gupy (veja abaixo).

---

### Gemini CLI

**Via CLI (recomendado):**

```bash
npx skills add gupy-io/gupy-skills-candidato -g -a gemini-cli -y
```

**Manual:** copie para `~/.gemini/skills/<nome-da-skill>/` ou `.agents/skills/<nome-da-skill>/`.

Reinicie o Gemini CLI e peça ajuda em linguagem natural (ex.: melhorar currículo na Gupy ou entender como a IA avalia candidatos).

---

### GitHub Copilot

**Via CLI:**

```bash
npx skills add gupy-io/gupy-skills-candidato -g -a github-copilot -y
```

**Manual:** copie para `~/.copilot/skills/<nome-da-skill>/` ou `.agents/skills/<nome-da-skill>/` no repositório.

No VS Code, o Copilot Agent pode carregar skills do diretório configurado. MCP é configurável nas settings do VS Code.

---

### ChatGPT

O ChatGPT **não** implementa o padrão `SKILL.md` nativamente. Alternativas:

**Opção A — Custom GPT (recomendada para uso recorrente)**

1. Crie um GPT em [chatgpt.com/gpts](https://chatgpt.com/gpts) para cada caso de uso (ou combine as instruções em um só).
2. Em **Instructions**, cole o corpo markdown (abaixo do frontmatter) de:
   - `skills/analise-de-curriculo/SKILL.md` — revisão de perfil
   - `skills/guia-de-triagem/SKILL.md` — triagem e papel da IA
3. Para `analise-de-curriculo`, em **Knowledge**, faça upload de `references/manual-empregabilidade.md`.
4. Salve e use o GPT conforme a necessidade.

**Opção B — Conversa avulsa**

Cole o conteúdo do `SKILL.md` no início da conversa e diga: *"Siga estas instruções como orientador de carreira Gupy"*.

**MCP:** o ChatGPT web não expõe MCP customizado da mesma forma que Cursor ou Claude Desktop. As skills funcionam **sem MCP**; apenas `analise-de-curriculo` pode enriquecer sugestões com busca de vagas quando o MCP estiver disponível em outras ferramentas.

---

### Gemini (web e app)

Assim como o ChatGPT, o Gemini web não instala `SKILL.md` diretamente.

- Crie um **Gem** com as instruções da skill.
- Ou cole o conteúdo do `SKILL.md` na conversa.

**MCP:** não suportado no Gemini web/app.

---

### Outras ferramentas de código

O Skills CLI suporta dezenas de agentes ([lista completa](https://github.com/vercel-labs/skills#supported-agents)), incluindo Windsurf, Cline, OpenCode, Codex, Roo Code, Kiro CLI e outras.

```bash
# Exemplo: Windsurf
npx skills add gupy-io/gupy-skills-candidato -g -a windsurf -y

# Exemplo: instalar em todos os agentes detectados
npx skills add gupy-io/gupy-skills-candidato --all -y
```

Consulte a documentação da sua ferramenta para o caminho exato de skills e suporte a MCP.

---

## Integração MCP (opcional)

Algumas skills podem usar o **MCP do Portal Gupy para candidatos** para enriquecer sugestões com padrões do mercado (habilidades recorrentes, modalidades, termos de descrição). Hoje, apenas `analise-de-curriculo` usa o MCP (`search_jobs` quando você informa uma área). A skill `guia-de-triagem` **não depende de MCP**.

**Importante:**

- O MCP é **opcional**. Sem ele, `analise-de-curriculo` continua funcionando com o manual de empregabilidade e o checklist; `guia-de-triagem` funciona normalmente; `radar-de-vagas` oferece comparativo qualitativo se o candidato colar descrições de vagas.
- O MCP lê **dados públicos** do portal de vagas — não acessa seu currículo logado nem dados privados da conta.
- Nem toda ferramenta suporta MCP.

### Ferramentas com suporte a MCP

| Ferramenta | Como configurar |
|------------|-----------------|
| **Cursor** | Settings → MCP → adicionar servidor |
| **Claude Code** | `~/.claude.json` ou `.mcp.json` no projeto |
| **Claude Desktop** | Settings → Connectors → Add custom connector |
| **VS Code + Copilot** | `mcp.json` / configuração MCP da extensão |

### Exemplo de configuração

Adicione o servidor MCP em `mcp.json`, `~/.claude.json` ou `claude_desktop_config.json` (conforme a ferramenta):

```json
{
  "mcpServers": {
    "gupy-candidates-mcp": {
      "url": "https://candidates.mcp.api.gupy.io/mcp",
      "type": "http"
    }
  }
}
```

**Cursor:** Settings → MCP → adicionar servidor com a URL acima.

**Claude Desktop:** Settings → Connectors → Add custom connector → informe a mesma URL.

Ferramentas expostas pelo MCP (ótica do candidato), usadas pelas skills:

| Ferramenta | Uso nas skills |
|------------|----------------|
| `search_jobs` | Busca vagas por área, cargo ou recorte comparativo |
| `get_job_by_id` | Detalhes de uma vaga pública (descrição, benefícios, modalidade) |
| `list_companies` | Descoberta de empresas e pares do mesmo nicho no portal |
| `get_company_by_id` | Página de carreiras de uma empresa |

Após configurar, reinicie o agente. As skills detectam automaticamente se o MCP está disponível (`search_jobs`).

### O que o MCP **não** faz

- Não edita seu currículo na Gupy
- Não envia candidaturas
- Não substitui o login no portal — você continua atualizando o perfil manualmente em [portal.gupy.io](https://portal.gupy.io/)

---

## Instalação manual (sem Node.js)

Se não puder usar `npx skills`:

1. Clone ou baixe este repositório.
2. Copie a pasta `skills/<nome-da-skill>/` para o diretório de skills da sua ferramenta (veja tabelas acima).
3. Mantenha a estrutura interna (`SKILL.md`, pasta `references/`, etc.).

```bash
git clone https://github.com/gupy-io/gupy-skills-candidato.git
cp -r gupy-skills-candidato/skills/analise-de-curriculo ~/.cursor/skills/
cp -r gupy-skills-candidato/skills/guia-de-triagem ~/.cursor/skills/
cp -r gupy-skills-candidato/skills/radar-de-vagas ~/.cursor/skills/
```

---

## Atualizar skills

```bash
# Atualizar todas as skills instaladas
npx skills update -y

# Atualizar skills desta coleção
npx skills update analise-de-curriculo -y
npx skills update guia-de-triagem -y
npx skills update radar-de-vagas -y
```

Para instalação manual, baixe novamente o repositório e substitua as pastas das skills.

---

## Estrutura do repositório

```text
gupy-skills-candidato/
├── README.md
└── skills/
    ├── analise-de-curriculo/
    │   ├── SKILL.md              # Checklist e diagnóstico de perfil
    │   ├── references/
    │   │   └── manual-empregabilidade.md
    │   └── evals/
    │       └── evals.json
    ├── guia-de-triagem/
    │   ├── SKILL.md              # Triagem, dados sensíveis e papel humano
    │   └── evals/
    │       └── evals.json
    └── radar-de-vagas/
        ├── SKILL.md              # Radar comparativo de oportunidades (ótica candidato)
        ├── references/
        │   └── scoring-rubric.md
        ├── assets/
        │   └── radar-export.html
        ├── scripts/
        │   └── render_radar.py
        └── evals/
            └── evals.json
```

---

## Perguntas frequentes

### Preciso ter conta na Gupy?

Não para instalar e usar as skills. Para **aplicar** as sugestões no seu perfil, você precisa de cadastro em [portal.gupy.io](https://portal.gupy.io/).

### As skills garantem que vou ser aprovado em vagas?

Não. As skills orientam a melhorar visibilidade e qualidade do perfil, mas **não garantem** aprovação em processos seletivos.

### Posso usar para LinkedIn ou outros ATS?

A skill `analise-de-curriculo` é específica para o **perfil Gupy**. Princípios gerais de empregabilidade podem ajudar, mas o fluxo e o checklist são do portal Gupy. A skill `guia-de-triagem` explica o processo de triagem **na plataforma Gupy** — não generaliza para outros ATS.

### A IA da Gupy me recusou automaticamente?

Use a skill `guia-de-triagem`. Ela esclarece que a IA **ordena** perfis por compatibilidade e que **recrutadores humanos** decidem quem avança — sem prometer aprovação nem confirmar eliminação injusta. Para melhorar o perfil depois, a skill indica o handoff para `analise-de-curriculo`.

### Minha ferramenta não está na lista. E agora?

1. Verifique se ela suporta arquivos `SKILL.md` ([agentskills.io](https://agentskills.io)).
2. Tente `npx skills add gupy-io/gupy-skills-candidato --list` — o CLI pode detectar seu agente.
3. Use a [instalação manual](#instalação-manual-sem-nodejs) ou cole o conteúdo do `SKILL.md` nas instruções do agente.

### Como contribuir ou sugerir uma nova skill?

Abra uma issue ou pull request neste repositório descrevendo o caso de uso do candidato.

---

## Links úteis

- [Portal de vagas Gupy](https://portal.gupy.io/)
- [Central de Empregabilidade](https://centraldeempregabilidade.com.br/)
- [Manual de Empregabilidade Gupy (PDF)](https://conteudos.gupy.io/hubfs/Ebook_Manual_completo_para_conquistar_a_vaga_dos_sonhos_2.pdf)
- [Blog Gupy IA — como a tecnologia funciona](https://www.gupy.io/blog-do-emprego/gupy-ia)
- [Suporte ao candidato Gupy](https://suporte-candidatos.gupy.io/s/)
- [Skills CLI](https://github.com/vercel-labs/skills)
- [Catálogo de skills (skills.sh)](https://skills.sh)
- [Padrão Agent Skills](https://agentskills.io)

---
