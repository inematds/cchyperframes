# Como foi feito e como modificar — linear-promo-30s

> **Guia prático para pegar o primeiro exemplo da Trilha 3, entender cada peça e adaptar para sua marca em 60–90 minutos.**

---

## Por que este projeto (e não `clickup-demo`)

Na Trilha 3, os dois exemplos mostrados são `clickup-demo` (60s) e `linear-promo-30s` (30s). Para **aprender e modificar**, comece pelo `linear-promo-30s`. Motivos:

| | `clickup-demo` | `linear-promo-30s` |
|---|---|---|
| Duração | 60s | 30s |
| Arquitetura | Monolito | 8 sub-composições |
| `index.html` | 1011 linhas | 133 linhas |
| Assets | Pesado | 4 SFX + 1 pad + 3 screenshots + 2 SVGs |
| Como modificar | Editar 1000+ linhas | Trocar 1 sub-comp por vez |

Depois que você dominar o padrão modular do linear-promo, reabra o clickup-demo com outros olhos.

---

## Estrutura do projeto

```
video-projects/linear-promo-30s/
├── index.html               ← root composition (master timeline, 133 linhas)
├── meta.json                ← id, name, dimensions, fps
├── hyperframes.json         ← CLI config (paths relativos)
├── NOTES.md                 ← notas do autor sobre estado do projeto
├── final.mp4                ← render draft existente (3 MB)
├── assets/
│   ├── linear-logo.svg      ← logo vetorial
│   ├── linear-symbol.svg    ← símbolo (L-bracket)
│   ├── screenshot-plan.png  ← screenshot do produto
│   ├── screenshot-build.png
│   ├── screenshot-monitor.png
│   ├── warm-pad.mp3         ← trilha ambiente (30s)
│   ├── sfx-whoosh-1.mp3     ← SFX transição 01→02
│   ├── sfx-whoosh-2.mp3     ← SFX transição 02→03
│   └── sfx-twinkle.mp3      ← SFX brand reveal
├── compositions/            ← 8 beats em arquivos separados
│   ├── 01-problem-type.html     (0–4s)   kinetic type do problema
│   ├── 02-card-to-logo.html     (4–8s)   card vermelho morphando em L-bracket
│   ├── 03-brand-reveal.html     (8–10s)  reveal da marca
│   ├── 04-benefits-flowchart.html (10–14.8s) flowchart de benefícios
│   ├── 05-product-surfaces.html (14.8–19.5s) 3 screenshots do produto
│   ├── 06-wheel-pillars.html    (19.5–22s)   wheel chrome + pilares
│   ├── 07-foundation.html       (22–25s)     callback + cluster
│   └── 08-cta-outro.html        (25–30s)     outro com logo + CTA
└── scripts/                 ← utilitários do projeto
```

---

## Como a timeline mestre funciona (`index.html`)

Abra `index.html` no editor. Vai ver uma estrutura muito enxuta:

```html
<div id="root" data-composition-id="linear-promo-30s"
     data-start="0" data-duration="30"
     data-width="1920" data-height="1080">

  <!-- Cada beat é uma sub-composição carregada via data-composition-src -->
  <div class="beat-layer"
    data-composition-id="01-problem-type"
    data-composition-src="compositions/01-problem-type.html"
    data-start="0" data-duration="4" data-track-index="0"
    data-width="1920" data-height="1080"></div>

  <!-- ... 7 outras sub-composições sequenciais ... -->

  <!-- Áudio global (trilha de fundo) em track separada -->
  <audio id="warm-pad"
    data-start="0" data-duration="30" data-track-index="1"
    data-volume="0.15"
    src="assets/warm-pad.mp3"></audio>

  <!-- SFX spotados -->
  <audio id="sfx-whoosh-1"
    data-start="3.8" data-duration="0.4" data-track-index="2"
    data-volume="0.20"
    src="assets/sfx-whoosh-1.mp3"></audio>
  <!-- ... -->
</div>

<script>
  window.__timelines = window.__timelines || {};
  const tl = gsap.timeline({ paused: true });
  tl.to({}, { duration: 30 }, 0); // anchor de 30s
  window.__timelines["linear-promo-30s"] = tl;
</script>
```

**Regras que o `index.html` respeita** (são as 11 do Render Contract):
1. Root div tem `data-composition-id`, `data-start`, `data-duration`, `data-width`, `data-height`
2. Cada sub-composição tem **seu próprio `data-track-index=0`** (mesma track porque são sequenciais e não se sobrepõem)
3. Áudios em **tracks diferentes** (1, 2, 3, 4) porque tocam em paralelo
4. Timeline GSAP pausada, registrada em `window.__timelines` com a chave idêntica ao `data-composition-id`
5. `tl.to({}, { duration: 30 }, 0)` estende a timeline do root para os 30s completos

---

## Como uma sub-composição funciona (`08-cta-outro.html`)

Toda sub-composição segue este shape:

```html
<template id="08-cta-outro-template">
  <!-- 1. Imports (GSAP + fontes) -->
  <script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:..." rel="stylesheet" />

  <!-- 2. HTML da cena, scopado pelo data-composition-id -->
  <div data-composition-id="08-cta-outro"
       data-start="0" data-duration="5"
       data-width="1920" data-height="1080">
    <div class="stage-08">
      <div class="grid-floor"></div>
      <div class="outro-lockup">
        <div class="o-symbol"><svg>...</svg></div>
        <div class="o-wordmark">Linear</div>
        <div class="o-tagline">The product development system for teams and agents.</div>
        <div class="o-cta">Get started — linear.app</div>
      </div>
      <div class="vignette"></div>
      <div class="grain"></div>
    </div>

    <!-- 3. CSS scopado pelo data-composition-id -->
    <style>
      [data-composition-id="08-cta-outro"] .o-wordmark {
        font-size: 144px;
        background: linear-gradient(180deg, #fff 0%, #ccc 60%, #888 100%);
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        text-shadow: 0 0 60px rgba(255,255,255,0.3);
      }
      /* ... mais styles ... */
    </style>

    <!-- 4. Timeline GSAP da cena, pausada -->
    <script>
      (() => {
        const SLOT = 5; // duração da cena
        const tl = gsap.timeline({ paused: true });
        const sel = (q) => `[data-composition-id="08-cta-outro"] ${q}`;

        tl.to(sel('.o-symbol'), { opacity: 1, duration: 0.5, ease: 'power2.out' }, 0);
        tl.to(sel('.o-wordmark'), { opacity: 1, duration: 0.5 }, 0.1);
        tl.fromTo(sel('.o-tagline'),
          { opacity: 0, y: 20 },
          { opacity: 1, y: 0, duration: 0.6 }, 0.5);
        // ... mais tweens ...

        tl.to({}, { duration: SLOT }, 0); // estende para 5s
        window.__timelines = window.__timelines || {};
        window.__timelines['08-cta-outro'] = tl;
      })();
    </script>
  </div>
</template>
```

**Padrões consistentes em todas as sub-comps**:
- Tudo dentro de `<template>` (o framework injeta na DOM no momento certo)
- CSS usa seletor `[data-composition-id="..."] .classe` para evitar vazamento entre cenas
- IIFE envolve o script (não polui escopo global)
- `SLOT` local guarda a duração; `tl.to({}, { duration: SLOT }, 0)` garante timeline do tamanho certo
- Chave em `window.__timelines` é **exatamente** igual ao `data-composition-id` da root div

---

## Passo a passo: adaptar para sua marca

Vou usar o exemplo de você querer transformar o `linear-promo-30s` em `nova-marca-promo-30s`.

### 1. Copiar o projeto

```bash
cd video-projects
cp -r linear-promo-30s nova-marca-promo-30s
cd nova-marca-promo-30s
```

### 2. Atualizar metadados

Abra `meta.json` e troque os campos:

```json
{
  "id": "nova-marca-promo-30s",
  "name": "Nova Marca — 30s Promo",
  "width": 1920,
  "height": 1080,
  "fps": 30
}
```

Abra `index.html` e ajuste o `data-composition-id` do root e o `<title>`:

```html
<title>Nova Marca — 30s Promo</title>
...
<div id="root" data-composition-id="nova-marca-promo-30s" ...>
```

### 3. Trocar assets

```bash
# Remove logo antigo, coloca o seu
rm assets/linear-logo.svg assets/linear-symbol.svg
cp ~/caminho/para/seu-logo.svg assets/brand-logo.svg
cp ~/caminho/para/seu-simbolo.svg assets/brand-symbol.svg

# Substitua os screenshots por prints do seu produto
rm assets/screenshot-plan.png assets/screenshot-build.png assets/screenshot-monitor.png
cp ~/caminho/para/print-1.png assets/screenshot-1.png
cp ~/caminho/para/print-2.png assets/screenshot-2.png
cp ~/caminho/para/print-3.png assets/screenshot-3.png
```

Audio (whoosh, twinkle, warm-pad) pode manter — é royalty-free e genérico o suficiente. Se quiser trocar, use [freesound.org](https://freesound.org) ou [pixabay.com/music](https://pixabay.com/music).

### 4. Atualizar copy e cores em cada sub-composição

**Comece pelo `08-cta-outro.html`** (outro) — é o mais simples e dá dopamina rápido ver seu logo ali.

Edite:
- Texto do wordmark (linha `<div class="o-wordmark">Linear</div>` → seu nome de marca)
- Tagline (`<div class="o-tagline">The product development system...</div>` → sua proposta de valor)
- CTA (`<div class="o-cta">Get started — linear.app</div>` → seu URL)
- SVG do símbolo (`<path d="M 25 18 L 75 18..."` → seu símbolo ou simplesmente `<image href="assets/brand-symbol.svg"/>`)
- Cores do gradiente chrome no `.o-wordmark` CSS (linha `linear-gradient(180deg, #fff 0%, #ccc 60%, #888 100%)`)
- Gradiente do CTA (linha `background: linear-gradient(135deg, rgba(94,106,210,0.3), rgba(169,176,240,0.2))`)

Trabalhe sub-comp por sub-comp. **Uma por sessão** é ritmo bom para começar.

### 5. Testar ao vivo no Studio

```bash
cd video-projects/nova-marca-promo-30s
npx hyperframes preview
```

Abre em `http://localhost:3002`. Hot reload funciona — toda vez que salvar um arquivo, o Studio recarrega. Varra a timeline, veja cada beat.

**Dica**: se o root stalling (pode acontecer quando há shader blocks e WebGL software), abra sub-composições isoladamente pela URL:
```
http://localhost:3002/?comp=08-cta-outro
http://localhost:3002/?comp=01-problem-type
```

### 6. Lint antes de renderizar

```bash
npx hyperframes lint
```

Se aparecer `missing_gsap_script` em alguma sub-comp, adicione `<script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>` antes do IIFE.

Se aparecer sobreposição de clips, verifique que `data-track-index` está diferente para elementos que tocam ao mesmo tempo.

### 7. Render draft e verificação visual

```bash
npx hyperframes render --quality draft --output renders/draft.mp4
```

Puxe frames-chave com FFmpeg:

```bash
mkdir -p renders/frames
for t in 2 6 9 12 17 20 23 27; do
  ffmpeg -y -ss $t -i renders/draft.mp4 -frames:v 1 -q:v 2 -vf scale=960:-1 "renders/frames/t${t}.jpg"
done
```

Abra cada JPG e verifique:
- Texto não está cortado
- Cores da marca aparecem corretas
- Logo está legível
- Outros elementos visuais não sumiram

### 8. Render final

```bash
npx hyperframes render --quality standard --output renders/final.mp4
```

Output 1080p visualmente lossless. É o que você entrega.

---

## O que cada beat faz (para decidir o que trocar)

| Beat | Papel narrativo | O que NA sua versão provavelmente muda |
|---|---|---|
| **01 — problem-type** (0–4s) | Hook: afirma problema do público | Texto (substitua "Building software..." pela dor específica do seu ICP) |
| **02 — card-to-logo** (4–8s) | Transição visual, cria expectativa | Cor do card, formato do morph (se sua marca não usa L-bracket) |
| **03 — brand-reveal** (8–10s) | Revela a marca com twinkle | Logo SVG, paleta do reveal |
| **04 — benefits-flowchart** (10–14.8s) | Mostra 3 benefícios em flowchart | Textos dos benefícios, ícones |
| **05 — product-surfaces** (14.8–19.5s) | 3 screenshots do produto em ângulo 3D | Screenshots (use prints reais do seu produto) |
| **06 — wheel-pillars** (19.5–22s) | Wheel chrome com pilares do produto | Nomes dos pilares, cores |
| **07 — foundation** (22–25s) | Callback ao beat 01 + cluster de ícones | Textos de foundation, cluster |
| **08 — cta-outro** (25–30s) | Outro com logo + CTA | Tudo: nome, tagline, URL, logo |

---

## Erros comuns e como resolver

| Erro | Causa | Correção |
|---|---|---|
| Lint falha em `missing_gsap_script` | Sub-comp sem `<script src="gsap...">` antes do IIFE | Adicione o script no `<template>` |
| Sub-comp não aparece no preview | `data-composition-id` da root div não bate com chave em `window.__timelines` | Confira os dois — devem ser strings idênticas |
| Vídeo trava em um frame específico | Você animou width/height direto num `<video>` | Envolva o `<video>` num `<div>` e anime o wrapper |
| Áudio fora de sincronia | `data-start` do áudio não bate com o beat visual | Cheque timestamps nos `<audio>` do `index.html` |
| Render incompleto | Timeline GSAP mais curta que `data-duration` | Adicione `tl.to({}, { duration: SLOT }, 0)` no final do script |

---

## Próximos passos

Depois de entregar sua primeira adaptação:

1. **Variação A:** mesma estrutura de 8 beats, troque paleta completa e assets. Cliente 1.
2. **Variação B:** altere a duração dos beats (ex: `data-start="2"` no beat 02, cortando o beat 01 em 2s). Ritmo diferente.
3. **Variação C:** reordene os beats. Coloque o beat 08 (outro CTA) como beat 01 (hook) — tipo teaser que revela primeiro.
4. **Avançado:** substitua um beat por um bloco do registry. Ex: `npx hyperframes add ui-3d-reveal` no lugar do beat 05.

---

## Arquivos de referência

- Render contract completo: `CLAUDE.md` (seção "Render Contract")
- Estética de motion: `MOTION_PHILOSOPHY.md` (10 Leis + checklist pré-voo)
- Exemplo monolítico (mais avançado): `video-projects/clickup-demo/index.html`
- Outro exemplo modular: `video-projects/may-shorts-19/` (short-form vertical 9:16)

---

*Documento criado em 2026-04-19. Se encontrar inconsistência, verifique se o projeto não mudou desde então.*
