# Hyperframes Editor — Edição Estudante

Uma bancada de trabalho para construir pipelines de vídeo motion-graphics em **HTML + GSAP puros**, alimentada pelo [Hyperframes](https://hyperframes.heygen.com). Doze projetos de vídeo finalizados que você pode clonar, varrer a timeline, destrinchar e reconstruir como seus.

> Isto **não** é um stack de vídeo Remotion / React / Next.js. Cada composição neste repositório é um arquivo HTML comum com uma timeline GSAP pausada anexada a `window.__timelines`. A CLI do Hyperframes cuida de lint, preview e render.

---

## Para quem é isso

Estudantes que querem aprender como vídeos promocionais e short-form profissionais são construídos de ponta a ponta — storyboard, sistema de marca, motion graphics, sincronia de áudio, pipeline de render — através de engenharia reversa de projetos reais. Todo projeto aqui entregou (ou quase entregou). Varra as composições, leia os docs de HANDOFF/STORYBOARD, assista ao `final.mp4`, depois mude coisas e veja o que quebra.

## Pré-requisitos

- **Node 20+** — rode `node --version` para verificar
- **FFmpeg** no seu `PATH` — necessário para extração e reencode de áudio
- **Chrome (mais recente)** — Hyperframes renderiza via Chromium headless
- **~5 GB de disco livre** — node_modules é grande; renders são maiores ainda
- **16 GB de RAM recomendado** para preview fluido no Studio com múltiplos blocos de shader

Rode `npx hyperframes doctor` após `npm install` — ele reporta o que está faltando.

## Início rápido

```bash
git clone <url-do-seu-fork> hyperframes-editor
cd hyperframes-editor
npm install

# Opcional — apenas se quiser usar as integrações ClickUp / OpenAI
cp .env.example .env
# ...depois edite .env com suas próprias chaves

# Abra o Studio em um dos projetos inclusos
cd video-projects/may-shorts-19
npx hyperframes preview    # http://localhost:3002
```

O Studio recarrega automaticamente ao salvar o arquivo. Varra a timeline, inspecione cenas, mude cores, veja re-renderizar ao vivo.

## Como rodar os exemplos passo a passo

Guia prático para quem está começando do zero com o Hyperframes.

### 1. Instalar dependências (uma vez só, na raiz)

```bash
cd cchyperframes
npm install
```

### 2. Verificar o ambiente

Confirma se Node 20+, FFmpeg e Chrome estão instalados e acessíveis:

```bash
npx hyperframes doctor
```

### 3. Entrar num projeto

Cada pasta em `video-projects/` é um projeto independente com seu próprio `index.html`, `assets/` e `compositions/`.

```bash
cd video-projects/may-shorts-19      # ou qualquer outro projeto
```

### 4. Preview ao vivo no Studio (hot reload)

```bash
npx hyperframes preview              # abre http://localhost:3002
```

O Studio recarrega automaticamente ao salvar qualquer arquivo. Varra a timeline, mude cores, teste animações em tempo real.

### 5. Lint antes de renderizar

```bash
npx hyperframes lint
```

Zero erros antes de partir para o render. Warnings você pode sobreviver com.

### 6. Render rápido de rascunho (1–3 min, pixelado)

```bash
npx hyperframes render --quality draft --output renders/draft.mp4
```

Use para validação rápida de pacing e sincronia.

### 7. Render final (1080p visualmente lossless)

```bash
npx hyperframes render --quality standard --output renders/final.mp4
```

Este é o render que se entrega.

### Por onde começar (ordem recomendada)

| Nível | Projeto | Por quê |
|---|---|---|
| **Iniciante** | `claude-edit-intro` | Intro estilo promo com mínimo hardcoding de marca — template inicial fácil de entender. |
| **Polido** | `may-shorts-19` | Vertical TikTok com talking-head + motion graphics + legendas karaokê. A skill `/short-form-video` foi escrita em torno dele. |
| **Produto SaaS** | `clickup-demo` | Demo de 60s com uso pesado de blocos do registry (x-post, ui-3d-reveal). Mostra a curva de iteração. |
| **Motion avançado** | `linear-promo-30s` | Promo de 30s na estética Infinite Payments. Entregue como draft — terminá-lo é um bom exercício. |

Abra o `final.mp4` de cada projeto primeiro para ver o alvo, depois o `index.html` lado a lado para entender como foi construído.

### Dicas importantes

- **Sempre rode os comandos de dentro da pasta do projeto** (não da raiz do workspace). A CLI lê `hyperframes.json` e `meta.json` do diretório atual.
- **Leia `MOTION_PHILOSOPHY.md`** antes de brainstormar sua primeira cena — as 10 Leis (seção 0) e o checklist pré-voo (seção 4) são o que separa "renderizou" de "está bom".
- **Se o Studio travar em 0s**, force refresh no navegador (Ctrl+Shift+R) ou abra uma sub-composição específica: `http://localhost:3002/?comp=<sub-comp-id>`.
- **Se um render falhar no meio**, rode `npx hyperframes doctor` para ver o que está faltando.

## Estrutura do repositório

```
hyperframes-editor/
├── README.md                    ← você está aqui
├── LICENSE                      ← MIT (veja nota sobre assets de marca)
├── .env.example                 ← copie para .env, preencha com suas chaves
├── CLAUDE.md                    ← guia completo do workspace para usuários do Claude Code
├── AGENTS.md                    ← notas sobre delegação a agentes
├── MOTION_PHILOSOPHY.md         ← a estética de motion que este repo almeja
├── DESIGN.ais-example.md        ← a spec de marca da AIS — seu exemplo trabalhado
├── assets/                      ← exemplos de marca compartilhados (logo AIS + tokens)
│   ├── AIS Logo PNG.png
│   ├── AIS Background.png
│   ├── AIS Brand Guideline Small.jpg
│   └── brand-tokens.css         ← custom props CSS que toda comp pode importar
├── docs/                        ← specs e planos mais longos
├── scripts/                     ← scripts de preflight no nível do workspace
├── .claude/                     ← skills do Claude Code (slash commands drop-in)
│   ├── launch.json
│   └── skills/                  ← /hyperframes, /gsap, /make-a-video, etc.
├── package.json
└── video-projects/              ← os 13 projetos
    └── <projeto>/
        ├── index.html           ← ponto de entrada da composição raiz
        ├── compositions/        ← sub-comps carregadas via data-composition-src
        ├── assets/              ← vídeo, áudio, imagens, transcrições
        ├── final.mp4            ← a saída alvo — assista isto primeiro
        ├── renders/             ← seu rascunho local de render (no gitignore)
        ├── hyperframes.json     ← config da CLI (caminhos relativos a esta pasta)
        ├── meta.json            ← id / nome / dimensões / fps
        └── (STORYBOARD.md, HANDOFF.md, NOTES.md conforme aplicável)
```

## Os 12 projetos, em um relance

Comece abrindo cada `final.mp4` para ver o alvo, depois abra `index.html` para ver como foi construído.

### Vertical short-form (9:16, 1080×1920)
| Projeto | O que é |
|---|---|
| `may-shorts-19` | Talking-head estilo TikTok + motion graphics + legendas karaokê. Este tem o maior polimento — a skill `/short-form-video` foi escrita em torno dele. |
| `may-shorts-18` | Short anterior na mesma série. Compare v2 com may-shorts-19 para ver o que foi refinado. |

### Landscape short-form (16:9)
| Projeto | O que é |
|---|---|
| `may-shorts-6` | Corte landscape de um talking-head short, mesmo padrão de produção da série vertical. |

### Promos de produto
| Projeto | O que é |
|---|---|
| `clickup-demo` | Demo de produto SaaS de 60s — uso pesado de blocos do registry (x-post, ui-3d-reveal). Cinco versões de render mostram a curva de iteração. |
| `linear-promo-30s` | Promo de 30s estilo Linear na estética Infinite Payments. Foi entregue como draft — terminá-lo é um bom exercício para estudante. Veja `NOTES.md`. |
| `hyperframes-sizzle` | Sizzle reel Hyperframes × Claude Code. Usa o fluxo `/website-to-hyperframes`. |
| `first-agent-promo` | Filme de lançamento "Your First AI Agent" de 32s. Usa abordagem React-via-Babel em vez do padrão HTML — um contra-exemplo útil. |

### Aulas educacionais
| Projeto | O que é |
|---|---|
| `aisoc-lesson-5-1` | Vídeo-aula completo (face-cam + motion graphics). Veja CLAUDE.md para o pipeline de nova aula (transcrever → MG sincronizado por palavra → seções). |
| `golden-ratio-demo` | Aula AIS sobre proporção em layout. Entregue como draft polido — veja `NOTES.md` para o que ficou em aberto. |
| `claude-edit-intro` | Intro estilo promo para um workflow de edição; mínimo hardcoding de marca — template inicial fácil. |

### Hype / lançamento de marca
| Projeto | O que é |
|---|---|
| `aisoc-hype` | Filme de hype da marca AI Automation Society de 30s — o scaffold que muitos outros projetos AIS referenciam. |
| `aisoc-app-release` | Promo de 30s do lançamento do app mobile AIS. Leia `HANDOFF.md` — o Nate documentou cada pegadinha. |

## ⚠️ Customizando para sua marca

**Esta é a seção mais importante.** O repositório vem com o branding da AI Automation Society (AIS) embutido como exemplo trabalhado. Antes de usar qualquer coisa disto para seu próprio trabalho, troque isto:

### A lista de troca global

| Arquivo | O que é | O que fazer |
|---|---|---|
| `assets/brand-tokens.css` | Define `--ais-bg`, `--ais-accent`, `--ais-warn`, variáveis de fonte | Substitua os valores hex + famílias de fonte pelos seus. Considere renomear o prefixo das custom props para sua marca (`--acme-bg`, etc.) — mas aí você vai precisar fazer grep em toda composição pelos nomes antigos. |
| `assets/AIS Logo PNG.png` | Logo usado pelos projetos AIS | Jogue seu próprio logo; ou mantenha o nome do arquivo para que as referências existentes funcionem, ou renomeie e faça grep-replace. |
| `assets/AIS Background.png` | Imagem de fundo ocasionalmente usada em cenas AIS | Mesmo padrão do logo. |
| `assets/AIS Brand Guideline Small.jpg` | Imagem de referência para a marca AIS | Delete; substitua por sua própria imagem de guideline se tiver uma. |
| `DESIGN.ais-example.md` | Spec completa da marca AIS | **Não edite.** Use como seu template: copie para a pasta do seu novo projeto como `DESIGN.md` e reescreva cores, fontes, regras de motion e "O que NÃO fazer" para sua marca. |

### Acoplamento AIS por projeto

| Projeto | Acoplamento AIS | Tratar como |
|---|---|---|
| `aisoc-hype`, `aisoc-app-release`, `aisoc-lesson-5-1`, `golden-ratio-demo` | **Pesado** — valores hex hardcoded nas composições, handle `@aiautomationsociety`, receita de glow do logo AIS, identidade on-camera do Nate em algumas cenas | Apenas referência. Reconstrua do zero para sua marca. |
| `clickup-demo`, `first-agent-promo`, `hyperframes-sizzle`, `linear-promo-30s`, `may-shorts-*`, `context-session`, `claude-edit-intro` | **Mínimo** | Bons templates iniciais — troque os tokens de marca e você está quase lá. |

### Varredura de busca-e-correção

Depois de trocar os assets globais, rode este grep para achar quaisquer referências AIS ainda vivendo nas composições:

```bash
# Acha valores hex hardcoded e strings AIS em qualquer lugar de video-projects/
grep -rEn "(#37bdf8|#f09025|#07121c|#195066|aisoc|AIS Logo|@aiautomationsociety)" video-projects/
```

Substitua cada resultado pela custom prop CSS correspondente do seu novo `brand-tokens.css`, ou pelo seu próprio hex/handle.

## Criando seu próprio novo projeto de vídeo

1. Escolha um nome em kebab-case: `mkdir video-projects/meu-promo-marca`
2. Faça o scaffold com a CLI ou copie um irmão:
   ```bash
   cd video-projects/meu-promo-marca
   npx hyperframes init
   ```
   Ou, mais rápido: copie `hyperframes.json` + `meta.json` de um projeto irmão que você goste, edite `meta.json` com seu novo id/nome/dimensões, e comece o `index.html` do zero.
3. Instale os assets de marca compartilhados no seu projeto:
   ```bash
   cp ../../assets/brand-tokens.css assets/
   cp ../../assets/SeuLogo.png assets/
   ```
4. Escreva seu `DESIGN.md` (copie o formato de `DESIGN.ais-example.md` da raiz).
5. Construa. Preview. Lint. Render.

## O loop de autoria

```
editar → lint → preview (Studio, ao vivo) → render draft → verificar frames → render final
```

| Passo | Comando | O que verificar |
|---|---|---|
| Lint | `npx hyperframes lint` | Zero erros antes do preview. Warnings são sobreviventes. |
| Preview | `npx hyperframes preview` | Varra a timeline, conserte qualquer coisa estranha ao vivo. Hot reload funciona. |
| Render draft | `npx hyperframes render --quality draft --output renders/draft.mp4` | ~1–3 minutos. CRF 28 — pixelado mas rápido. |
| Verificar frames | `ffmpeg -ss <t> -i renders/draft.mp4 -frames:v 1 out.png` | Puxe um frame por cena no momento-herói. Procure por rostos cortados, texto desalinhado, frames em branco. |
| Render final | `npx hyperframes render --quality standard --output renders/final.mp4` | Visualmente lossless 1080p. É este que se entrega. |

> **`MOTION_PHILOSOPHY.md` é sua academia estética.** Antes de construir qualquer coisa, leia a seção 0 (as 10 Leis) e a seção 4 (checklist pré-voo). Este doc é a diferença entre "renderizou" e "está bom".

## Ordem de leitura recomendada

1. **Este README** (você está aqui)
2. **`CLAUDE.md`** — guia completo do workspace, convenções, skills, contrato de render. Útil mesmo se você não usar Claude Code — as 11 regras do Contrato de Render valem para qualquer um editando uma composição.
3. **`MOTION_PHILOSOPHY.md`** — regras estéticas. Leia antes de brainstormar sua primeira cena.
4. **`DESIGN.ais-example.md`** — exemplo trabalhado de uma spec de marca.
5. **Escolha um projeto genérico** (`claude-edit-intro` é um bom começo). Abra `index.html`, `final.mp4` lado a lado, e a pasta `compositions/`. Leia, varra, modifique, re-renderize.
6. **Depois abra um projeto AIS** — você terá o vocabulário para ver quão longe eles empurram o framework.

## Usando Claude Code com este repositório

A pasta `.claude/skills/` traz um conjunto de slash commands que encodam padrões específicos do framework (registro `window.__timelines`, semântica de atributos `data-*`, CSS compatível com shader). Se você usa o [Claude Code](https://claude.com/claude-code), eles ativam automaticamente:

- `/hyperframes` — autoria/edição de composições, legendas, TTS, animação audio-reativa
- `/hyperframes-cli` — referência da CLI (init, add, lint, preview, render, transcribe, tts)
- `/gsap` — animação GSAP: timelines, easing, stagger, plugins
- `/hyperframes-registry` — instalar blocks/components do catálogo
- `/website-to-hyperframes` — transformar uma URL em composição (captura-para-vídeo em 7 passos)
- `/make-a-video` — fluxo iniciante ponta-a-ponta
- `/short-form-video` — playbook 9:16 talking-head + motion graphics

Não usa Claude Code? As skills são só markdown — abra e leia como documentação.

## Solução de problemas

| Sintoma | Primeira coisa a tentar |
|---|---|
| `npx hyperframes` — comando não encontrado | Rode `npm install` na raiz do repo primeiro |
| Render falha no meio | `npx hyperframes doctor` — verifica Node, FFmpeg, Chrome |
| Preview do Studio travado em 0s | Hard-refresh no navegador (Ctrl+Shift+R). Se falhar, tente uma URL de sub-composição específica: `http://localhost:3002/?comp=<sub-comp-id>` |
| Erros de lint sobre clips sobrepostos | Dois clips no mesmo `data-track-index` sobrepostos no tempo — atribua track indices diferentes ou ajuste `data-start` / `data-duration` |
| Erros de lint sobre `missing_gsap_script` | Todo HTML de sub-composição precisa do seu próprio `<script src="https://cdn.jsdelivr.net/npm/gsap@3.14.2/dist/gsap.min.js"></script>` antes do seu IIFE — GSAP não herda do pai |
| Vídeo congelado num render, áudio continua | Um elemento `<video>` foi animado diretamente (não anime `width`/`height`/`top`/`left` num `<video>`). Envolva num `<div>` e anime o wrapper. |

Mais: `npx hyperframes docs <tópico>` (tópicos: `data-attributes`, `gsap`, `rendering`, `examples`, `troubleshooting`, `compositions`).

## Créditos e licença

- **Código e composições** — MIT, veja `LICENSE`.
- **Assets de marca AIS** (logo, background, imagem de guideline, tokens AIS) — permanecem propriedade da AI Automation Society. Inclusos como exemplo trabalhado; não licenciados para reuso. Substitua pelos seus próprios antes de entregar.
- **Hyperframes** — framework © HeyGen, docs em https://hyperframes.heygen.com.

Construído por [Nate Herk](https://aiautomationsociety.ai). Divirta-se destrinchando.
