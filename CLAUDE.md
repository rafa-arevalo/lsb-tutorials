# CLAUDE.md — Link School of Business Tutorials

## Overview

Static tutorial platform for **Link School of Business (LSB)**. Each tutorial is a self-contained HTML file. No build step, no bundler, no framework.

## Tech Stack

- **HTML5 / CSS3 / Vanilla JS (ES6+)** — zero external dependencies except Google Fonts (Inter)
- **GitHub Pages** — static hosting (repo: `lsb-tutorials`)
- **Local dev:** `python3 -m http.server 8080`

## Project Structure

```
├── index.html                        # Landing page — tutorial catalog (card grid)
├── tutorial-{slug}.html              # One file per tutorial
├── logo-lsb.svg                      # LSB logo (dark version for light backgrounds)
└── .claude/
    └── commands/
        └── criar-nova-aula-Link.md   # Slash command to create new tutorials
```

## Brand Identity — Link School of Business

### Color Palette

```css
--accent: #C8A84B;          /* Dourado — cor de ação e destaque */
--accent2: #B09040;
--accent-light: #F8F2E0;
--accent-lighter: #FBF8F0;
--accent-border: #DEC878;
--accent-text: #8B6F28;

--bg: #F5F3EE;              /* Warm White */
--white: #FFFFFF;
--text: #1A1A18;            /* Off-Black */
--text2: #6B6860;           /* Warm Gray */
--text3: #9B9890;
--text4: #CBCAC7;
--border: #E5E1D8;
--border2: #D8D4CB;
```

### Logo

`logo-lsb.svg` — versão escura para fundos claros (Warm White / branco).

---

## Tutorial Architecture

Estrutura idêntica à Delta Academy. Ver padrão completo em:
`/Users/rafael-arevalo/Documents/delta-board-main/CLAUDE.md`

### Layout: 3 colunas com stepper sticky

```
┌─────────────────────────────────────────────────────────┐
│  Stepper Header (52px, sticky top)  — accent: dourado   │
├──────────┬────────────────────────┬─────────────────────┤
│ Sidebar  │  Main Content          │  Right Sidebar      │
│ (256px)  │  (flex, max 700px)     │  (200-220px)        │
└──────────┴────────────────────────┴─────────────────────┘
```

### Responsive

- **>1024px:** 3 colunas
- **768–1024px:** 2 colunas (sem right sidebar)
- **<768px:** 1 coluna (sem left sidebar)

### Convenções

- Arquivo: `tutorial-{slug}.html` (kebab-case)
- Idioma: `lang="pt-BR"`
- Todo CSS/JS inline — sem arquivos externos
- IDs de seção: `sec-home`, `sec-step1`, `sec-step2`...
- Título numerado: `<span class="snum">01.</span>`
- Subtitle da sidebar: `{TAG} · Link School of Business`
- Tag colors no index: `tag-gold`, `tag-blue`, `tag-green`, `tag-red`, `tag-gray`

## Criando um novo tutorial

Use o slash command `/criar-nova-aula-Link` com o roteiro como argumento.
O comando guia todo o fluxo: análise do roteiro → geração do HTML → card no index.
