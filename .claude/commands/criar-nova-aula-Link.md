Você foi acionado para criar uma nova aula no site da **Link School of Business (LSB)**.

O usuário vai colar um roteiro. Sua missão é transformar esse roteiro em um arquivo HTML completo seguindo exatamente o padrão do projeto, depois adicionar o card em `index.html`.

---

## PASSO 1 — Receber e analisar o roteiro

O roteiro fornecido é: $ARGUMENTS

Se `$ARGUMENTS` estiver vazio, peça ao usuário que cole o roteiro agora antes de continuar.

Ao analisar o roteiro, extraia obrigatoriamente:
- **Título da aula** (ex: "Estratégia com IA Generativa")
- **Código/tag** (ex: M01, EXERCICIO, CASE, PITCH — pergunte ao usuário se não estiver claro)
- **Cor da tag** para o index: gold, blue, green, red ou gray
- **Descrição curta** (1–2 frases para o card do index)
- **Lista de seções/steps** com seus títulos e conteúdo
- **Slug para o nome do arquivo** (kebab-case, ex: `tutorial-estrategia-ai.html`)

Confirme com o usuário o nome do arquivo e a tag antes de gerar o HTML.

---

## PASSO 2 — Gerar o arquivo HTML

Crie o arquivo `tutorial-{slug}.html` na raiz do repositório (`/Users/rafael-arevalo/Documents/lsb-tutorials/`).

### Identidade visual — Link School of Business

```css
/* Accent: Dourado */
--accent: #C8A84B;
--accent2: #B09040;
--accent-light: #F8F2E0;
--accent-lighter: #FBF8F0;
--accent-border: #DEC878;
--accent-text: #8B6F28;

/* Backgrounds */
--bg: #F5F3EE;       /* Warm White */
--white: #FFFFFF;
--surface2: #F5F3EE;

/* Texto */
--text: #1A1A18;     /* Off-Black */
--text2: #6B6860;    /* Warm Gray */
--text3: #9B9890;
--text4: #CBCAC7;

/* Bordas */
--border: #E5E1D8;
--border2: #D8D4CB;

/* Sombras */
--shadow-xs: 0 1px 2px rgba(26,26,24,0.04);
--shadow-sm: 0 1px 3px rgba(26,26,24,0.06), 0 1px 2px rgba(26,26,24,0.04);
--shadow-md: 0 4px 6px -1px rgba(26,26,24,0.07), 0 2px 4px -2px rgba(26,26,24,0.05);

/* Semânticas */
--green: #059669;  --green-bg: #ECFDF5;  --green-border: #A7F3D0;  --green-text: #065F46;
--red: #DC2626;    --red-bg: #FEF2F2;    --red-border: #FECACA;
--orange: #EA580C;
--yellow: #C8A84B; /* reusa o accent */
```

Logo: `logo-lsb.svg`

### Template HTML obrigatório

Siga esta estrutura **exatamente** — não invente componentes novos:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Tutorial — {TÍTULO DA AULA}</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;450;500;600;700;800&display=swap" rel="stylesheet">
<style>
  /* CSS completo inline — use as variáveis LSB acima.
     Estrutura idêntica à Delta, apenas as variáveis --accent mudam. */
</style>
</head>
<body>

<!-- STEPPER HEADER: uma bolinha por flow, separadas por setas -->
<div class="stepper-header">
  <div class="stepper-node active" onclick="showSection('sec-home')" id="sn-home">
    <span class="sn-check">✓</span>
    <span class="sn-label">Início</span>
  </div>
  <!-- Repita para cada flow/step: arrow + stepper-node -->
</div>

<div class="layout">

  <!-- SIDEBAR ESQUERDA (256px) -->
  <div class="sidebar">
    <div class="sidebar-header">
      <h2>{TÍTULO DA AULA}</h2>
      <p>{TAG} · Link School of Business</p>
    </div>
    <nav>
      <div class="nav-group">
        <div class="nav-group-title">Conteúdo</div>
        <div class="nav-item active" onclick="showSection('sec-home')" data-section="sec-home">
          <span class="ni-num">00</span>Visão Geral<span class="ni-check">✓</span>
        </div>
        <!-- um nav-item por seção -->
      </div>
    </nav>
    <div class="sidebar-progress">
      <div class="sp-label">Progresso — <span id="pct">0</span>%</div>
      <div class="sp-bar"><div class="sp-fill" id="sp-fill"></div></div>
    </div>
  </div>

  <!-- MAIN CONTENT -->
  <div class="main">
    <div class="section active" id="sec-home">
      <div class="sec-title"><span class="snum">00.</span> {Título}</div>
      <div class="sec-desc">{Subtítulo}</div>
      <!-- conteúdo -->
      <div class="nav-buttons">
        <button class="nav-btn secondary" onclick="goHome()">↩ Início</button>
        <button class="nav-btn primary" onclick="showSection('sec-step1')">Próximo →</button>
      </div>
    </div>
    <!-- demais seções (class="section", sem active) -->
  </div>

  <!-- RIGHT SIDEBAR (200px, referências contextuais) -->
  <div class="right-sidebar">
    <div class="rs-section-title">Referência Rápida</div>
    <!-- rs-table, links, etc. -->
  </div>

</div>

<script>
const stepToFlow = { 'sec-home':'home', 'sec-step1':'step1' /* ... */ }
const flowOrder = ['home','step1' /* ... */]
const visited = new Set(['sec-home'])
const completedFlows = new Set()

function showSection(id) {
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'))
  document.getElementById(id)?.classList.add('active')
  document.querySelectorAll('.nav-item').forEach(n => {
    n.classList.toggle('active', n.dataset.section === id)
    if (visited.has(n.dataset.section)) n.classList.add('completed')
  })
  visited.add(id)
  const flow = stepToFlow[id]
  if (flow) {
    const idx = flowOrder.indexOf(flow)
    if (idx > 0) completedFlows.add(flowOrder[idx-1])
  }
  updateStepper(flow)
  updateProgress()
  window.scrollTo({top:0,behavior:'smooth'})
}

function updateStepper(activeFlow) {
  document.querySelectorAll('.stepper-node').forEach(n => {
    const f = n.id.replace('sn-','')
    const idx = flowOrder.indexOf(f)
    const activeIdx = flowOrder.indexOf(activeFlow)
    n.classList.remove('active','completed','future')
    if (f === activeFlow) n.classList.add('active')
    else if (completedFlows.has(f) || idx < activeIdx) n.classList.add('completed')
    else n.classList.add('future')
  })
}

function updateProgress() {
  const pct = Math.round((visited.size / document.querySelectorAll('.section').length) * 100)
  document.getElementById('pct').textContent = pct
  document.getElementById('sp-fill').style.width = pct + '%'
}

function goHome() { showSection('sec-home') }

function copyText(btn, text) {
  navigator.clipboard?.writeText(text).then(() => {
    btn.textContent = 'Copiado!'
    setTimeout(() => btn.textContent = 'Copiar', 1800)
  }).catch(() => {
    const ta = document.createElement('textarea')
    ta.value = text; document.body.appendChild(ta)
    ta.select(); document.execCommand('copy')
    document.body.removeChild(ta)
    btn.textContent = 'Copiado!'
    setTimeout(() => btn.textContent = 'Copiar', 1800)
  })
}

updateStepper('home')
updateProgress()
</script>
</body>
</html>
```

### Regras de conteúdo

- Títulos de seção: `<div class="sec-title"><span class="snum">01.</span> Título</div>`
- Passos numerados: use `.instructions` > `.inst` > `.inst-num` + `.inst-text`
- Prompts/código para copiar: use `.copy-block` com `.copy-btn` e `onclick="copyText(this, '...')"`
- Dicas: `.tip-box` (verde); alertas: `.warning-box` (vermelho)
- Cada seção termina com `.nav-buttons` contendo "← Anterior" e "Próximo →"
- Numeração das seções: 00 (home), 01, 02, 03...
- IDs das seções: `sec-home`, `sec-step1`, `sec-step2`, etc.
- Todo o CSS vai inline no `<style>` — **não crie arquivos externos**

---

## PASSO 3 — Adicionar card em `index.html`

Abra `/Users/rafael-arevalo/Documents/lsb-tutorials/index.html` e insira o novo card **após o último `</a>` existente dentro do container**, mantendo a ordem numérica da lista:

```html
<a class="card" href="tutorial-{slug}.html">
  <span class="card-tag tag-{cor}">{TAG}</span>
  <div class="card-title">{Título da Aula}</div>
  <div class="card-desc">{Descrição curta de 1-2 frases}</div>
</a>
```

Cores disponíveis: `tag-gold`, `tag-blue`, `tag-green`, `tag-red`, `tag-gray`

---

## PASSO 4 — Publicar e retornar URL

Execute os comandos abaixo **na ordem**, a partir do diretório `/Users/rafael-arevalo/Documents/lsb-tutorials/`:

```bash
git add tutorial-{slug}.html index.html
git commit -m "Add {TAG}: {Título da Aula}"
git push origin main
```

Após o push ser concluído com sucesso, retorne ao usuário a mensagem:

```
✅ Aula publicada!

📄 Página: https://rafa-arevalo.github.io/lsb-tutorials/tutorial-{slug}.html
📋 Índice: https://rafa-arevalo.github.io/lsb-tutorials/

⏱ O GitHub Pages pode levar até 1 minuto para refletir a atualização.
```
