# Expresso Local Visual Refinement Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Refinar as sete páginas da EXPRESSO LOCAL com o sistema Duna Atlântica, mantendo a autoridade visual atual e elevando tipografia, componentes, conteúdo, movimento e responsividade ao nível de acabamento da referência Surfing sem copiá-la.

**Architecture:** O site continua estático e sem framework: HTML semântico, uma folha de estilos compartilhada e JavaScript progressivo. O refinamento será aplicado em camadas — validação, fundação visual, shell global, conteúdo por jornada e movimento — para que cada etapa produza um estado utilizável e revisável.

**Tech Stack:** HTML5, CSS moderno, JavaScript ES2022, Node.js 20+ com `node:test`, navegador real para QA visual.

**Spec:** `DESIGN.md`

## Global Constraints

- Preservar as sete páginas, o hero fotográfico dominante, a escala de “EXPRESSO LOCAL”, o ticker, as tabs, o FAQ e o rodapé tipográfico.
- Usar Manrope para interface e texto; Instrument Serif somente em contrastes editoriais curtos.
- Usar a paleta exata de `DESIGN.md`, com azul-petróleo funcional e terracota apenas como acento.
- Não inventar preços, horários, duração, equipamentos, estoque, disponibilidade, endereço, telefone, credenciais, depoimentos, nomes de equipe ou regras operacionais.
- Toda ação “Agendar” ou “Consultar” inicia uma conversa; nunca confirma preço ou disponibilidade.
- Sem WhatsApp oficial configurado, usar apenas o Instagram confirmado ou cópia local da mensagem; nunca fabricar número.
- O conteúdo principal deve permanecer visível se o JavaScript falhar.
- Em `prefers-reduced-motion: reduce`, remover animações, ticker contínuo, parallax, rolagem suave e transições não essenciais.
- Validar desktop em 1440×900 e mobile em 375×812, sem rolagem horizontal e com alvos de toque de pelo menos 44×44px.
- Não copiar HTML, CSS, scripts, textos ou ativos do template Framer; reutilizar somente padrões abstratos de ritmo e movimento.
- Não publicar, fazer deploy, push ou alterar produção sem autorização separada.

## Review Focus

1. WhatsApp vazio: CTAs não podem produzir `wa.me` inválido; Task 6 fixa o comportamento com teste.
2. JavaScript desabilitado ou falhando: reveals não podem esconder conteúdo; Task 7 fixa fallback CSS e teste.
3. `prefers-reduced-motion: reduce`: conteúdo visível e ticker parado; Task 7 fixa e testa.
4. Viewport de 375px e zoom de 200%: navegação, títulos, cards e formulários sem overflow; Task 8 verifica em navegador.
5. Conteúdo herdado não confirmado: preços, nomes de equipe e promessas não sobrevivem; Tasks 5 e 6 fixam com regressões.

---

## File Map

- `package.json` — comandos de validação e teste sem dependências de produção.
- `scripts/validate-site.mjs` — valida documentos, referências locais, headings e configuração.
- `tests/site.test.mjs` — regressões estruturais, de conteúdo, movimento e integração.
- `dist/styles.css` — tokens, layout, componentes, responsividade e movimento.
- `dist/app.js` — menu, tabs, reveals, filtros e mensagens de consulta.
- `dist/config.js` — canais confirmados, sem dados fictícios.
- `dist/*.html` — conteúdo semântico das sete páginas.
- `dist/assets/expresso-logo.jpeg` e `dist/assets/frente.jpeg` — cópias preservadas dos ativos fornecidos.

### Task 1: Site validation baseline

**Files:**
- Create: `package.json`
- Create: `scripts/validate-site.mjs`
- Create: `tests/site.test.mjs`
- Modify: `dist/*.html`
- Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: arquivos sob `dist/`.
- Produces: `validateSite(rootDir: string): Promise<string[]>`, `npm test` e `npm run validate`.

- [ ] **Step 1: Create the test runner and validation command**

```json
{"name":"expresso-local-site","private":true,"type":"module","scripts":{"test":"node --test tests/*.test.mjs","validate":"node scripts/validate-site.mjs"},"engines":{"node":">=20"}}
```

- [ ] **Step 2: Write the structural validator**

```js
import { access, readFile } from 'node:fs/promises';
import path from 'node:path';
import { fileURLToPath } from 'node:url';

export const HTML_FILES = ['index.html','aulas.html','aluguel.html','loja.html','historia.html','condicoes.html','dicas.html'];
const localRefs = html => [...html.matchAll(/(?:href|src)="([^"]+)"/g)].map(m => m[1]).filter(ref => !/^(?:https?:|mailto:|tel:|#|data:)/.test(ref));

export async function validateSite(rootDir = 'dist') {
  const errors = [];
  for (const file of HTML_FILES) {
    const html = await readFile(path.join(rootDir, file), 'utf8');
    if (!/^<!doctype html>/i.test(html)) errors.push(`${file}: missing doctype`);
    if (!/<html[^>]+lang="pt-BR"/i.test(html)) errors.push(`${file}: lang must be pt-BR`);
    if (!/<meta[^>]+name="viewport"/i.test(html)) errors.push(`${file}: missing viewport`);
    if (!/<meta[^>]+name="description"/i.test(html)) errors.push(`${file}: missing description`);
    if ((html.match(/<main\b/gi) ?? []).length !== 1) errors.push(`${file}: expected one main`);
    if ((html.match(/<h1\b/gi) ?? []).length !== 1) errors.push(`${file}: expected one h1`);
    for (const ref of localRefs(html)) {
      const clean = ref.split(/[?#]/)[0];
      if (!clean) continue;
      try { await access(path.resolve(rootDir, clean)); }
      catch { errors.push(`${file}: missing local reference ${clean}`); }
    }
  }
  return errors;
}

if (process.argv[1] === fileURLToPath(import.meta.url)) {
  const errors = await validateSite();
  if (errors.length) { console.error(errors.join('\n')); process.exitCode = 1; }
  else console.log(`Validated ${HTML_FILES.length} pages.`);
}
```

- [ ] **Step 3: Write baseline tests**

```js
import test from 'node:test';
import assert from 'node:assert/strict';
import { readFile } from 'node:fs/promises';
import { HTML_FILES, validateSite } from '../scripts/validate-site.mjs';

test('all seven pages satisfy the structural contract', async () => assert.deepEqual(await validateSite(), []));
test('configuration never contains a fabricated WhatsApp number', async () => {
  const config = await readFile('dist/config.js', 'utf8');
  assert.match(config, /whatsapp\s*:\s*["']{2}/);
  assert.match(config, /instagram\.com\/tribo_local/);
});
test('all images declare alt text', async () => {
  for (const file of HTML_FILES) {
    const html = await readFile(`dist/${file}`, 'utf8');
    for (const image of html.match(/<img\b[^>]*>/gi) ?? []) assert.match(image, /\balt="[^"]*"/, `${file}: ${image}`);
  }
});
```

- [ ] **Step 4: Run the tests and verify the baseline failure**

Run: `npm test`

Expected: FAIL only on real structural gaps, especially subpages without exactly one `<h1>`.

- [ ] **Step 5: Make the minimum semantic corrections**

Promote each page’s primary visible title to `<h1>` without changing copy or layout class. Add missing descriptions and `alt` attributes only where reported.

- [ ] **Step 6: Verify and commit**

Run: `npm test && npm run validate`

Expected: PASS and `Validated 7 pages.`

```bash
git add package.json scripts/validate-site.mjs tests/site.test.mjs dist/*.html
git commit -m "test: add static site validation baseline"
```

### Task 2: Duna Atlântica foundation and real assets

**Files:**
- Modify: `dist/styles.css`
- Create: `dist/assets/expresso-logo.jpeg`
- Create: `dist/assets/frente.jpeg`
- Modify/Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: `expresso.jpeg`, `frente.jpeg`, primitive values from `DESIGN.md`.
- Produces: semantic tokens, `--font-sans`, `--font-serif` and stable asset paths.

- [ ] **Step 1: Add the failing token and asset test**

```js
import { access, readFile } from 'node:fs/promises';
test('approved Duna Atlantica foundation exists', async () => {
  const css = await readFile('dist/styles.css', 'utf8');
  for (const value of ['#FBFAF6','#F4F0E7','#527A7E','#315B61','#193A3E','#B86857','#17211E']) assert.ok(css.toUpperCase().includes(value), `missing ${value}`);
  assert.match(css, /--font-sans:\s*["']?Manrope/i);
  assert.match(css, /--font-serif:\s*["']?Instrument Serif/i);
  await access('dist/assets/expresso-logo.jpeg');
  await access('dist/assets/frente.jpeg');
});
```

- [ ] **Step 2: Run the focused test**

Run: `node --test --test-name-pattern="approved Duna" tests/site.test.mjs`

Expected: FAIL because the tokens and copied assets do not exist.

- [ ] **Step 3: Copy supplied assets without altering root originals**

```powershell
Copy-Item -LiteralPath expresso.jpeg -Destination dist/assets/expresso-logo.jpeg
Copy-Item -LiteralPath frente.jpeg -Destination dist/assets/frente.jpeg
```

- [ ] **Step 4: Replace flat variables with the approved token layer**

```css
@import url("https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=Manrope:wght@400;500;600;700;800&display=swap");
:root {
  --color-foam-50:#FBFAF6; --color-foam-100:#F4F0E7; --color-sand-200:#E7DFD2;
  --color-line-300:#CEC8BB; --color-mist-100:#DCE7E3; --color-sea-500:#527A7E;
  --color-sea-700:#315B61; --color-sea-900:#193A3E; --color-clay-500:#B86857;
  --color-clay-700:#8E4337; --color-sun-500:#C3A35E; --color-ink-900:#17211E;
  --color-background:var(--color-foam-100); --color-surface:var(--color-foam-50);
  --color-surface-muted:var(--color-mist-100); --color-foreground:var(--color-ink-900);
  --color-primary:var(--color-sea-700); --color-primary-hover:var(--color-sea-900);
  --color-accent:var(--color-clay-500); --color-border:var(--color-line-300);
  --font-sans:"Manrope",Arial,Helvetica,sans-serif;
  --font-serif:"Instrument Serif",Georgia,"Times New Roman",serif;
  --content-gutter:clamp(1.25rem,5vw,5.5rem); --section-space:clamp(4rem,8vw,8rem);
  --button-height:48px; --button-radius:999px; --card-radius:4px;
  --motion-fast:180ms; --motion-normal:500ms; --motion-slow:750ms;
  --motion-ease-out:cubic-bezier(.22,1,.36,1);
}
```

Map legacy variables to semantic tokens, normalize body typography, visible focus and minimum 48px controls. Keep existing selectors and layout in this task.

- [ ] **Step 5: Verify and commit**

Run: `npm test && npm run validate`

```bash
git add dist/styles.css dist/assets/expresso-logo.jpeg dist/assets/frente.jpeg tests/site.test.mjs
git commit -m "feat: establish Duna Atlantica design foundation"
```

### Task 3: Shared header, navigation and footer

**Files:**
- Modify: `dist/*.html`, `dist/styles.css`, `tests/site.test.mjs`
- Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: tokens and asset paths from Task 2.
- Produces: shared `.site-header`, `.primary-nav`, `.menu-toggle`, `.site-footer` and one `aria-current="page"` per route.

- [ ] **Step 1: Add and run the failing shell test**

```js
test('all pages use the official logo and one current navigation item', async () => {
  for (const file of HTML_FILES) {
    const html = await readFile(`dist/${file}`, 'utf8');
    assert.match(html, /assets\/expresso-logo\.jpeg/);
    assert.equal((html.match(/aria-current="page"/g) ?? []).length, 1, file);
    assert.match(html, /class="menu-toggle"[^>]+aria-expanded="false"/);
  }
});
```

Run: `node --test --test-name-pattern="official logo" tests/site.test.mjs`

Expected: FAIL on the improvised wordmark or missing current state.

- [ ] **Step 2: Apply the shared header contract to all pages**

```html
<header class="site-header" data-site-header>
  <a class="brand" href="index.html" aria-label="EXPRESSO LOCAL — Início">
    <img src="assets/expresso-logo.jpeg" width="56" height="56" alt=""><span>EXPRESSO LOCAL</span>
  </a>
  <button class="menu-toggle" type="button" aria-expanded="false" aria-controls="primary-nav"><span class="sr-only">Abrir menu</span><span aria-hidden="true">Menu</span></button>
  <nav class="primary-nav" id="primary-nav" aria-label="Principal">
    <a href="aulas.html">Aulas</a><a href="aluguel.html">Aluguel</a><a href="loja.html">Loja</a>
    <a href="historia.html">A escola</a><a href="condicoes.html">O mar</a><a href="dicas.html">Dicas &amp; bases</a>
  </nav>
  <a class="button button--small header-cta" href="aulas.html#consulta">Consultar aula</a>
</header>
```

Set `aria-current="page"` only on the matching link; on `index.html`, use the brand link.

- [ ] **Step 3: Consolidate and style the shell**

Footer: official brand, seven routes, `Itapema / SC`, confirmed Instagram only. Header: full nav above 1180px; from 900–1179px keep primary routes visible and move secondary routes into an accessible “Mais” disclosure; mobile menu below 900px, internal scroll and 44px links.

- [ ] **Step 4: Verify and commit**

Run: `npm test && npm run validate`

```bash
git add dist/*.html dist/styles.css tests/site.test.mjs
git commit -m "feat: unify responsive site navigation"
```

### Task 4: Homepage editorial refinement

**Files:**
- Modify: `dist/index.html`, `dist/styles.css`, `tests/site.test.mjs`
- Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: shared shell and Duna tokens.
- Produces: approved section order and `data-reveal`, `data-reveal-group`, `data-ticker` hooks.

- [ ] **Step 1: Add and run the failing homepage contract**

```js
test('homepage preserves authority and uses the real headquarters photo once', async () => {
  const html = await readFile('dist/index.html', 'utf8');
  for (const marker of ['data-hero','data-experience-tabs','data-ticker','<details','class="site-footer"']) assert.ok(html.includes(marker), `missing ${marker}`);
  assert.equal((html.match(/assets\/frente\.jpeg/g) ?? []).length, 1);
  assert.doesNotMatch(html, /desde\s+2012/i);
});
```

Run: `node --test --test-name-pattern="homepage preserves" tests/site.test.mjs`

Expected: FAIL on normalized hooks and real headquarters photo.

- [ ] **Step 2: Refine hero and page order**

Keep `ocean.jpg`, near-full viewport crop and dominant H1. Use this order: hero → ticker → manifesto → experience tabs → real headquarters story → principles → consultation CTA → FAQ → commercial paths → gallery/Instagram → footer. Preserve confirmed copy; rewrite only unsafe claims.

- [ ] **Step 3: Apply editorial layout and motion hooks**

Use 12/8/4-column composition; alternate ivory, white, one sea-dark and one mist section. Add `data-reveal="text"`, `data-reveal="media"` and `data-reveal-group` without inline hidden styles. Keep radii 0–8px except pill controls.

- [ ] **Step 4: Verify and commit**

Run: `npm test && npm run validate`

```bash
git add dist/index.html dist/styles.css tests/site.test.mjs
git commit -m "feat: refine homepage editorial composition"
```

### Task 5: Safe lessons, rental and shop journeys

**Files:**
- Modify: `dist/aulas.html`, `dist/aluguel.html`, `dist/loja.html`, `dist/styles.css`, `tests/site.test.mjs`
- Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: shared forms, cards, buttons and reveals.
- Produces: consultation-first pages without checkout, calculation, inventory or guaranteed availability.

- [ ] **Step 1: Add and run the failing content-safety test**

```js
test('service pages contain no unconfirmed commerce claims', async () => {
  for (const file of ['aulas.html','aluguel.html','loja.html']) {
    const html = await readFile(`dist/${file}`, 'utf8');
    assert.doesNotMatch(html, /R\$|comprar agora|em estoque|dispon[ií]vel agora|pagamento aprovado/i, file);
    assert.match(html, /consulta|conversar|confirmar com a equipe/i, file);
  }
  const js = await readFile('dist/app.js', 'utf8');
  assert.doesNotMatch(js, /\brates\b|\bmoney\b|weekend|totalPrice/);
});
```

Run: `node --test --test-name-pattern="commerce claims" tests/site.test.mjs`

Expected: FAIL on inherited rental arithmetic or shop inventory language.

- [ ] **Step 2: Refine `aulas.html`**

Keep compact editorial hero; explain the decision flow without duration or inclusions. Set the form section to `id="consulta"`, submit text to `Preparar mensagem`, and state that format and availability are confirmed by the team.

- [ ] **Step 3: Replace rental calculator with inquiry builder**

Use generic needs — primeira experiência, evolução, prancha própria, acessórios — without stock claims. Collect date, people, experience and free text. Display `Valores e disponibilidade são confirmados pela equipe.`

- [ ] **Step 4: Turn the shop into a contact catalogue**

Use only generic categories `Pranchas`, `Acessórios`, `Usados`; remove product names, prices and purchase states. Each action sets consultation interest and scrolls to the form.

- [ ] **Step 5: Unify forms and verify**

Visible labels, 48px fields, adjacent help/errors, focus ring and one hover effect per card. Ensure every input has matching `for`/`id`.

Run: `npm test && npm run validate`

```bash
git add dist/aulas.html dist/aluguel.html dist/loja.html dist/styles.css tests/site.test.mjs
git commit -m "feat: make service journeys consultation first"
```

### Task 6: Institutional pages and safe contact core

**Files:**
- Modify: `dist/historia.html`, `dist/condicoes.html`, `dist/dicas.html`, `dist/app.js`, `dist/styles.css`, `tests/site.test.mjs`
- Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: `window.EXPRESSO_CONFIG` and shared controls.
- Produces: `resolveContact(config, message)` and safe institutional/editorial content.

- [ ] **Step 1: Add and run failing regressions**

```js
test('institutional pages contain no unconfirmed people, history or live conditions', async () => {
  const history = await readFile('dist/historia.html', 'utf8');
  assert.doesNotMatch(history, /Matheus|Teteu|Camila Duarte|Lucas|Bicudo|desde\s+2012/i);
  const conditions = await readFile('dist/condicoes.html', 'utf8');
  assert.doesNotMatch(conditions, /agora|hoje|tempo real|ondas de \d/i);
});
test('empty WhatsApp has a safe fallback', async () => {
  const js = await readFile('dist/app.js', 'utf8');
  assert.match(js, /if\s*\(whatsapp\)/); assert.match(js, /instagram/); assert.match(js, /clipboard/);
});
```

Run: `node --test --test-name-pattern="institutional pages|safe fallback" tests/site.test.mjs`

Expected: FAIL on inherited staff names and unsafe contact branching.

- [ ] **Step 2: Ground the three pages in confirmed information**

`historia.html`: lead with `assets/frente.jpeg`, local presence in Itapema, no named team or dated milestones. `condicoes.html`: general tide/wind/swell education and explicit “not live forecast” note. `dicas.html`: accessible `<details>` for preparation, etiquette and post-session care; location only `Itapema / SC`.

- [ ] **Step 3: Implement contact resolution**

```js
function resolveContact(config, message) {
  const whatsapp = String(config?.whatsapp ?? '').replace(/\D/g, '');
  if (whatsapp) return { mode:'whatsapp', url:`https://wa.me/${whatsapp}?text=${encodeURIComponent(message)}` };
  const instagram = String(config?.instagram ?? '').trim();
  if (instagram) return { mode:'instagram', url:instagram };
  return { mode:'copy' };
}
async function deliverInquiry(message) {
  const contact = resolveContact(window.EXPRESSO_CONFIG, message);
  if (contact.mode === 'whatsapp') window.open(contact.url, '_blank', 'noopener');
  else { await navigator.clipboard?.writeText(message); if (contact.url) window.open(contact.url, '_blank', 'noopener'); }
  return contact.mode;
}
```

Bind features only when their elements exist. Preserve tabs, filters and menu; delete rental price arithmetic.

- [ ] **Step 4: Verify and commit**

Run: `npm test && npm run validate`

```bash
git add dist/historia.html dist/condicoes.html dist/dicas.html dist/app.js dist/styles.css tests/site.test.mjs
git commit -m "feat: ground institutional content in confirmed facts"
```

### Task 7: Progressive motion inspired by the reference

**Files:**
- Modify: `dist/app.js`, `dist/styles.css`, `tests/site.test.mjs`
- Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: `data-reveal`, `data-reveal-group`, `data-ticker`.
- Produces: `initReveals()`, `.is-revealed`, controlled ticker and reduced-motion fallback.

- [ ] **Step 1: Add and run the failing motion-safety test**

```js
test('motion is progressive and honors reduced motion', async () => {
  const css = await readFile('dist/styles.css', 'utf8'); const js = await readFile('dist/app.js', 'utf8');
  assert.match(css, /\.js\s+\[data-reveal\][\s\S]*opacity:\s*0/);
  assert.match(js, /document\.documentElement\.classList\.add\(['"]js['"]\)/);
  assert.match(js, /IntersectionObserver/);
  assert.match(css, /prefers-reduced-motion:\s*reduce[\s\S]*animation:\s*none/);
  assert.match(css, /prefers-reduced-motion:\s*reduce[\s\S]*opacity:\s*1/);
});
```

Run: `node --test --test-name-pattern="motion is progressive" tests/site.test.mjs`

Expected: FAIL until the `.js` gate and fallback exist.

- [ ] **Step 2: Implement restrained reveals**

```css
.js [data-reveal]{opacity:0;transform:translateY(18px);transition:opacity var(--motion-slow) var(--motion-ease-out),transform var(--motion-slow) var(--motion-ease-out)}
.js [data-reveal="text"]{filter:blur(8px);transition-property:opacity,transform,filter}
.js [data-reveal="media"]{transform:translateY(24px) scale(.985)}
.js [data-reveal].is-revealed{opacity:1;filter:none;transform:none}
[data-reveal-group]>*{transition-delay:calc(var(--stagger-index,0)*60ms)}
```

- [ ] **Step 3: Initialize reveals with fallback**

```js
document.documentElement.classList.add('js');
function initReveals() {
  const nodes = [...document.querySelectorAll('[data-reveal]')];
  document.querySelectorAll('[data-reveal-group]').forEach(group => [...group.children].forEach((child,index) => child.style.setProperty('--stagger-index', Math.min(index,4))));
  if (!('IntersectionObserver' in window) || matchMedia('(prefers-reduced-motion: reduce)').matches) { nodes.forEach(node => node.classList.add('is-revealed')); return; }
  const observer = new IntersectionObserver(entries => entries.forEach(entry => { if (entry.isIntersecting) { entry.target.classList.add('is-revealed'); observer.unobserve(entry.target); } }), { threshold:.16, rootMargin:'0px 0px -8% 0px' });
  nodes.forEach(node => observer.observe(node));
}
```

- [ ] **Step 4: Finish interaction and reduced-motion behavior**

Controls: 160–220ms; image scale at most 1.04; one masked ticker; short tab crossfade; no parallax. Pause ticker offscreen and while page is hidden.

```css
@media (prefers-reduced-motion:reduce){
  html{scroll-behavior:auto} *,*::before,*::after{animation:none!important;transition-duration:.01ms!important}
  .js [data-reveal]{opacity:1;filter:none;transform:none} [data-ticker]{transform:none!important}
}
```

- [ ] **Step 5: Verify and commit**

Run: `npm test && npm run validate`

```bash
git add dist/app.js dist/styles.css tests/site.test.mjs
git commit -m "feat: add restrained progressive motion"
```

### Task 8: Responsive, accessibility and visual acceptance

**Files:**
- Modify: `dist/styles.css`, `dist/*.html`, `dist/app.js` only for observed defects
- Modify/Test: `tests/site.test.mjs`

**Interfaces:**
- Consumes: completed site.
- Produces: locally validated build with recorded desktop/mobile/reduced-motion results.

- [ ] **Step 1: Add the final regression**

```js
test('final bundle has no unsafe content or broken empty links', async () => {
  const html = (await Promise.all(HTML_FILES.map(file => readFile(`dist/${file}`, 'utf8')))).join('\n');
  assert.doesNotMatch(html, /href=""|src=""|R\$|Matheus|Teteu|Camila Duarte|Bicudo/i);
  assert.doesNotMatch(html, /style="[^"]*(?:opacity\s*:\s*0|visibility\s*:\s*hidden)/i);
});
```

- [ ] **Step 2: Run the automated gate**

Run: `npm test && npm run validate`

Expected: PASS and seven pages validated.

- [ ] **Step 3: Start local review**

Run: `python -m http.server 4173 --directory dist`

Expected: homepage at `http://localhost:4173/`; local review only.

- [ ] **Step 4: Inspect desktop 1440×900**

Open all seven routes. Verify header state, one H1, font fallback, image crop, 44px targets, focus, assets, console, overlap and consultation path. Capture homepage and one representative subpage.

- [ ] **Step 5: Inspect mobile 375×812**

Open all routes. Verify menu open/close/focus return, body width equals viewport, title wrapping, forms, single-column cards, no hidden content and no horizontal scroll. Capture homepage and one form page.

- [ ] **Step 6: Inspect 200% zoom, keyboard and reduced motion**

Navigate controls with Tab/Shift+Tab/Enter/Space, verify ARIA state, zoom to 200%, emulate reduced motion, confirm visible content and stopped ticker/reveals.

- [ ] **Step 7: Correct only evidenced defects, then run the final gate**

Run:

```bash
npm test
npm run validate
git diff --check
git status --short
```

Expected: passing tests, seven pages, no whitespace errors, only intended changes plus user-owned root images if still untracked.

- [ ] **Step 8: Commit**

```bash
git add dist tests scripts package.json
git commit -m "fix: complete responsive visual acceptance"
```

Do not push, deploy or alter `.openai/hosting.json`.

## Completion Criteria

- Automated tests and seven-page validator pass.
- Shared Duna Atlântica tokens and approved typography cover all pages.
- Hero and editorial authority of the current site remain intact.
- Real logo and headquarters photo are integrated without changing root originals.
- No invented commerce, person, history, contact, availability or live-condition claim remains.
- CTAs degrade safely when WhatsApp is blank.
- Motion is restrained, progressive and disabled for reduced-motion users.
- Desktop, mobile, keyboard, 200% zoom and reduced motion are inspected in a real browser.
- Repository changes remain local; deployment requires separate authorization.