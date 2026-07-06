# Adalink Onboarding Tour

Protótipo interativo de **onboarding guiado** da plataforma Adalink (Adaflow). Recria o shell real do produto (rail com as abas Inteligência / Gestão / Empresa, menu com os módulos reais, header e o assistente **Adara**) e oferece um tour que **entra em cada tela** e explica função por função.

- **27 módulos** mapeados nas 3 abas.
- **92 passos** de tour: cada módulo abre sua tela real, ativa as abas internas (ex.: Memória → Memórias / Mapa / Execuções) e destaca cada função (filtros, botões, KPIs, grafo, busca semântica...).
- **Chat da Adara**: clique no ícone de IA (canto inferior direito) para perguntar sobre a plataforma.
- **Modo claro/escuro** e navegação livre (clique em qualquer módulo).

## Como rodar

É um único arquivo estático, sem build. Basta abrir `index.html` no navegador.

Para servir localmente com live reload (opcional):

```bash
npx serve .
# ou
python3 -m http.server 8000
```

## Estrutura

```
adalink-onboarding-tour/
├── index.html      # aplicação inteira (HTML + CSS + JS num só arquivo)
├── README.md
├── CLAUDE.md       # contexto para o Claude Code
└── .gitignore
```

## Publicar (GitHub Pages)

1. Suba o repositório no GitHub (veja abaixo).
2. Em **Settings → Pages**, escolha **Deploy from a branch**, branch `main` e pasta `/ (root)`.
3. O tour fica disponível em `https://SEU-USUARIO.github.io/adalink-onboarding-tour/`.

## Desenvolver com Claude Code

```bash
cd adalink-onboarding-tour
claude
```

Peça, por exemplo: *"adicione um módulo Estúdio Criativo com abas Imagem, Vídeo e Áudio e passos de tour para cada uma"*. O `CLAUDE.md` explica a arquitetura para o agente.

---

Feito para a plataforma Adalink · IA Corporativa
