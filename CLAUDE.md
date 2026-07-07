# CLAUDE.md — Adalink Onboarding Tour

> Instruções para agentes (Claude Code) trabalharem neste repositório.
> **Idioma:** sempre responder e escrever textos de UI em **português do Brasil**, com acentuação correta.

## O que é

Protótipo interativo de onboarding da plataforma **Adalink (Adaflow)**. Recria o shell real do produto e um **tour guiado que entra em cada tela e explica cada função**. É um único arquivo estático, sem backend e sem build.

## Arquitetura (importante)

Tudo vive em **`index.html`** — HTML, CSS e JavaScript no mesmo arquivo. Não há framework, bundler nem dependências (a única externa é a fonte Geist via Google Fonts, com fallback para a fonte do sistema).

O app é **dirigido por dados**. Os dois pontos centrais no `<script>`:

1. **`const M = [ ... ]`** — o array de **módulos**. Cada módulo tem:
   - `id`, `name`, `icon`, `color`, `tab` (`inteligencia` | `gestao` | `empresa`), `sec` (rótulo da seção na sidebar), `desc` (descrição/1º passo do tour).
   - `views`: uma ou mais abas internas, cada uma com `id`, `name`, `icon` e `blocks`.
   - `blocks`: componentes da tela. Cada bloco tem um `t` (tipo) e, se for alvo do tour, um `tt` (id do alvo) e `tour: { title, text }`.
   - `subtours` (opcional): passos extra que apontam para sub-elementos internos (ex.: o chat de Agentes).

2. **`const STEPS = [ ... ]`** — gerado automaticamente a partir de `M`. Para cada módulo cria: um passo de introdução (destaca o item na sidebar), um passo para a barra de abas (se houver mais de uma view) e um passo para cada bloco/subtour com `tour`.

### Tipos de bloco disponíveis (`t`)

`stats`, `collapse`, `banner`, `toolbar`, `actions`, `cards`, `kpis`, `table`, `list`, `bigsearch`, `empty`, `memcards`, `chatpanel`, `nodes`, `graph`, `chart`, `flags`, `chat`, `agents`. O renderizador é a função `C(b)`; blocos especiais têm helpers (`graphHTML`, `chatHTML`, `agentsHTML` — galeria de especialistas com `data-tt` internos `ag-search`, `ag-status`, `ag-tags`, `ag-tools`, `ag-count`, `ag-create`, `ag-card`, `ag-badge-status`, `ag-badge-model`, `ag-badge-vis`, `ag-pinned`, `ag-actions`).

### Como o tour se posiciona

`goStep(i)` abre o módulo/aba do passo e chama `placeTour(s)`, que localiza o alvo por `data-tt="<tt>"` (ou o item da sidebar, para introduções), aplica o spotlight e posiciona o card de explicação. Alvos internos são marcados com `data-tt` no HTML gerado por `C()`.

### Adara (assistente)

O FAB abre `openAdara()` (painel lateral `#adrawer`). As respostas ficam em `ADARA_QA` (correspondência por palavras-chave em `adaraMatch`) e as sugestões em `ADARA_SUGG`.

## Como adicionar um módulo novo

1. Acrescente um objeto em `M` com `id/name/icon/color/tab/sec/desc` e ao menos uma `view` com `blocks`.
2. Dê um `tt` único e um `tour: { title, text }` a cada bloco que deve ser explicado.
3. Pronto: `STEPS`, a sidebar e as telas são gerados automaticamente. Não é preciso mexer no motor.

## Convenções

- **Textos de UI em pt-BR com acento.** Ícones novos entram no mapa `I` (paths SVG, sem a tag `<svg>`).
- **Sem build**: teste abrindo o `index.html` no navegador.
- **Cores** vêm das variáveis CSS em `:root` / `.dark` (paleta violet-600/zinc, fonte Geist) — reaproveite os tokens, não use cores soltas.
- Commits em português no padrão convencional: `feat: ...`, `fix: ...`, `docs: ...`, `style: ...`.

## Verificação rápida (recomendada antes de commitar)

Um teste com DOM simulado no Node confirma sintaxe e que todo alvo de passo existe:

```bash
node -e "const fs=require('fs');const h=fs.readFileSync('index.html','utf8');const a=h.indexOf('<script>')+8,b=h.indexOf('</script>');fs.writeFileSync('/tmp/t.js',h.slice(a,b));" && node --check /tmp/t.js && echo OK
```

Idealmente, todo passo referenciado em `STEPS` deve ter seu `data-tt` presente na view renderizada correspondente.
