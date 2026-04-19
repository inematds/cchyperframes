# Plano de Curso — Design + Vídeo com IA

**Combinando Claude Design (claude.ai/design) + Hyperframes Student Kit**
**Duração estimada:** 8 módulos × 2–3h = 20–24h de estudo ativo
**Formato:** autodirigido com projetos práticos progressivos
**Idioma:** Português (BR)
**Data do plano:** 2026-04-19

---

## Visão geral

Este curso ensina a produção visual ponta-a-ponta com IA — do briefing estático (landing page, deck, protótipo) ao vídeo motion-graphics finalizado — integrando duas ferramentas complementares:

1. **Claude Design** (claude.ai/design) — gera designs estáticos ship-ready: landing pages, pitch decks, scroll-driven sites, protótipos clicáveis de app.
2. **Hyperframes Student Kit** (cchyperframes) — pipeline de vídeo em HTML + GSAP com 12 projetos finalizados para engenharia reversa (short-form TikTok, promos SaaS, aulas educacionais, hype films de marca).

A lógica é clara: **primeiro o aluno aprende a pensar visualmente em estático**, onde o feedback é rápido e o custo de iteração é baixo. **Depois leva o mesmo raciocínio para movimento**, onde o ritmo, o áudio e a edição adicionam camadas. No final, produz uma campanha completa: landing page + pitch deck + vídeo promo, tudo consistente no sistema de marca.

---

## Público-alvo

- Empreendedores e fundadores que precisam produzir materiais visuais profissionais sem contratar estúdio
- Estudantes de design, marketing digital e produção de conteúdo
- Automatizadores da AI Automation Society que querem fechar o ciclo "estratégia → arte → vídeo" com IA
- Criadores de curso e edtechs que precisam de vídeo-aulas com motion graphics

**Não é para:** quem só quer editar vídeo em timeline tradicional (Premiere, CapCut). Este curso ensina um pipeline *programático* — HTML + GSAP — que permite iteração determinística e reuso de componentes.

---

## Pré-requisitos

### Técnicos
- **Node 20+** instalado
- **FFmpeg** no PATH
- **Chrome** atualizado
- **~5 GB de disco livre**
- Conta em **claude.ai** (acesso ao Claude Design)
- Editor de código (**VS Code** recomendado)
- Git básico

### Conhecimento
- HTML/CSS básico (saber o que é uma tag, uma classe, um seletor)
- JavaScript básico (saber ler um arquivo sem assustar)
- Terminal básico (`cd`, `ls`, `mkdir`)
- Sem conhecimento de motion graphics é OK — o curso constrói isso do zero

### Opcional (recomendado)
- **Claude Code** instalado — desbloqueia as skills `/hyperframes`, `/design-designer`, `/gsap` automaticamente
- Conhecimento prévio de Figma ou Canva (ajuda a comparar com o fluxo Claude Design)

---

## Filosofia pedagógica

1. **Aprender pela engenharia reversa.** Todo módulo começa com um artefato finalizado (um `final.mp4`, uma landing page rendered). O aluno assiste → abre o código → modifica → re-renderiza → compara.
2. **Design antes de movimento.** Estático primeiro — porque ritmo e sincronia são camadas sobre uma base visual que já precisa funcionar parada.
3. **Marca como sistema, não decoração.** O aluno constrói um `DESIGN.md` próprio no módulo 2 e carrega ele por todo o curso.
4. **Iteração rápida > perfeição.** Draft render → feedback → re-render. Nunca um render "final" que nunca foi preview-ado ao vivo.
5. **MOTION_PHILOSOPHY.md é a bíblia.** Antes de qualquer brainstorm de vídeo, lê-se as 10 Leis e o checklist pré-voo.

---

## Estrutura do curso

### Módulo 0 — Setup e Fundamentos (2h)

**Objetivos:**
- Instalar e verificar o ambiente (`npx hyperframes doctor`)
- Clonar o repo `cchyperframes` e rodar o primeiro preview
- Entender a diferença entre Claude Design (estático) e Hyperframes (vídeo)
- Visão geral do que são motion graphics, kinetic typography, shaders, GSAP

**Entregas:**
- Ambiente rodando com `npx hyperframes preview` num dos projetos
- Screenshot da Studio aberta em `may-shorts-19`
- Breve texto (1 parágrafo): em quais projetos reais você usará esse pipeline?

**Materiais:**
- README.md (seção "Como rodar os exemplos passo a passo")
- `npx hyperframes doctor`
- Vídeo introdutório ao motion graphics (link externo sugerido)

---

### Módulo 1 — Claude Design: do briefing ao primeiro render (3h)

**Objetivos:**
- Dominar a skill `/design-designer` como ferramenta de estruturação de briefing
- Entender o que Claude Design pede nos bastidores: feeling, audience, fidelity, option count, design system, brand context
- Gerar sua primeira landing page no claude.ai/design
- Usar o recurso **Tweaks** para iterar sem re-prompt

**Conceitos-chave:**
- Fidelidade: lo-fi vs hi-fi
- Variações "safe to wild" (4 variações padrão do Claude Design)
- Context imports: Figma, GitHub, screenshots — por que Claude Design pede
- Os 7 banimentos padrão: sem lorem ipsum, sem ícones genéricos de startup de IA, sem referências a concorrentes

**Exercício prático:**
1. Escolha um produto/serviço real (seu ou de cliente fictício)
2. Rode o interview do `/design-designer` respondendo as 10 perguntas
3. Cole o prompt gerado no claude.ai/design
4. Peça 4 variações (safe → wild)
5. Escolha uma e itere com Tweaks (3 tweaks: cor, tipo, intensidade de motion)

**Entregas:**
- Screenshot das 4 variações
- PNG ou link da versão final escolhida
- Breve reflexão: qual variação ficou mais ousada? Por quê?

**Materiais:**
- Skill `/design-designer` (SKILL.md)
- Sistema de prompts do Claude Design (citado na skill)
- Site: https://claude.ai/design

---

### Módulo 2 — Construindo seu Sistema de Marca (2h)

**Objetivos:**
- Escrever seu próprio `DESIGN.md` (não editar o `DESIGN.ais-example.md`)
- Definir: paleta de cores em hex, tipografia (headline + body), regras de motion, "o que NÃO fazer"
- Criar seu `assets/brand-tokens.css` com custom properties CSS
- Entender o acoplamento de marca: quais projetos do kit têm AIS hardcoded (pesado) e quais são neutros (mínimos)

**Conceitos-chave:**
- Design tokens: cores, fontes, espaçamento, raios, sombras
- CSS custom properties (`--brand-primary`, `--brand-accent`, etc.)
- Por que Claude Design precisa de hex codes reais (não "azul bonito")
- Varredura de find-and-fix: `grep -rEn` para caçar hardcoding antigo

**Exercício prático:**
1. Leia `DESIGN.ais-example.md` como referência
2. Escreva seu próprio `DESIGN.md` com: paleta, tipografia, motion rules, banimentos
3. Crie `assets/brand-tokens.css` com suas custom properties
4. Teste: cole no Claude Design o hex da sua marca e peça uma nova landing page
5. Compare — ela respeitou o sistema?

**Entregas:**
- `DESIGN.md` da sua marca (mínimo 80 linhas)
- `brand-tokens.css` com ao menos 6 custom properties
- Nova landing page no Claude Design aplicando seu sistema

**Materiais:**
- `DESIGN.ais-example.md` (referência)
- `assets/brand-tokens.css` (exemplo AIS)
- README seção "Customizando para sua marca"

---

### Módulo 3 — MOTION_PHILOSOPHY: a estética do movimento (3h)

**Objetivos:**
- Ler o `MOTION_PHILOSOPHY.md` completo — inclusive seções 0 (10 Leis) e 4 (checklist pré-voo)
- Entender por que a média de cena é 1.5s, fundo preto + grid + vinheta + grain, texto chrome-gradient com halo glow, transições com motion blur
- Desconstruir o vídeo-gold-standard (Infinite Global Payments 30s) frame a frame
- Aplicar as "7 regras mínimas" numa cena de teste

**Conceitos-chave:**
- As 10 Leis (seção 0 do MOTION_PHILOSOPHY)
- Regra dos três (sempre em trios: três beats, três cores, três cortes)
- Callback narrativo: o fim referencia o começo
- Outro breathing (4–6s de respiro no final)
- "What Would Infinite Do?" — o teste de 10 perguntas antes de mostrar ao cliente

**Exercício prático:**
1. Assista o `final.mp4` de `may-shorts-19` com o `MOTION_PHILOSOPHY.md` aberto ao lado
2. Identifique: onde aparece cada uma das 10 Leis? Anote timestamps
3. Rode o checklist pré-voo contra o vídeo
4. Agora crie 1 cena nova (6s) em HTML + GSAP aplicando as 7 regras mínimas

**Entregas:**
- Anotação timestamp-a-timestamp do `may-shorts-19` com 10 Leis mapeadas
- Arquivo `minha-cena-teste.html` com uma cena de 6s renderizada
- Resposta ao "What Would Infinite Do?" (10 perguntas) sobre sua cena

**Materiais:**
- `MOTION_PHILOSOPHY.md` — leitura completa
- `video-projects/may-shorts-19/final.mp4`
- Skill `/hyperframes` (para sintaxe)

---

### Módulo 4 — Primeiro vídeo completo (3h)

**Objetivos:**
- Clonar o projeto mais neutro (`claude-edit-intro`) e adaptá-lo para SUA marca
- Executar o loop completo: edit → lint → preview (Studio) → draft render → verify frames → final render
- Usar o Contrato de Render (11 regras) sem quebrar nada
- Fazer a verificação visual de frames antes de entregar

**Conceitos-chave:**
- As 11 regras do Render Contract (CLAUDE.md)
- `window.__timelines` — como cada composição registra sua timeline GSAP
- `data-start`, `data-duration`, `data-track-index` — semântica de timing
- Sub-composições via `<template>` + `data-composition-src`
- Por que `<video>` nunca leva `class="clip"` nem anima width/height diretamente

**Exercício prático:**
1. `cd video-projects` e `cp -r claude-edit-intro meu-primeiro-video`
2. Rode `npx hyperframes preview` — veja funcionar como base
3. Troque: logo, cores (via brand-tokens.css), tipografia, texto dos títulos
4. Rode `npx hyperframes lint` — conserte todos os erros
5. `npx hyperframes render --quality draft` → verifique 3 frames com FFmpeg
6. `npx hyperframes render --quality standard` — entrega o `final.mp4`

**Entregas:**
- Projeto `meu-primeiro-video/` completo dentro de `video-projects/`
- `renders/final.mp4` rodando
- Screenshots de 3 frames-chave com a marca aplicada
- Auto-avaliação: rodou o pre-flight do MOTION_PHILOSOPHY?

**Materiais:**
- CLAUDE.md (seções "Render Contract" e "Visual Verification")
- Skill `/hyperframes` e `/hyperframes-cli`
- Projeto base: `claude-edit-intro`

---

### Módulo 5 — Pitch Deck + Vídeo Sizzle (3h)

**Objetivos:**
- Criar um pitch deck de 7 slides no Claude Design usando seu sistema de marca
- Criar um sizzle reel de 15s no Hyperframes que vende o mesmo produto
- Entender como os dois outputs conversam: visual consistency entre estático e movimento
- Usar o fluxo `/website-to-hyperframes` para transformar sua landing page em base de vídeo

**Conceitos-chave:**
- Pitch deck architecture: Título → Problema → Why Now → O que fazemos → Tração → Time → Próximos passos
- Aspect ratio: 16:9 para deck, 16:9 ou 9:16 para sizzle
- Scroll-stop hook nos 2 primeiros segundos
- Callback visual: slide 1 e cena 1 do vídeo compartilham uma motif

**Exercício prático:**
1. Use `/design-designer` para gerar um prompt de pitch deck 7-slides
2. Rode no claude.ai/design e finalize
3. Extraia: palette, headline typeface, body typeface, um visual motif
4. Abra `video-projects/hyperframes-sizzle` como referência
5. Crie um novo projeto `sizzle-<seu-produto>/` e adapte
6. Inclua o motif do deck em pelo menos 1 cena do vídeo
7. Render draft → verify → render standard

**Entregas:**
- PDF ou PNGs dos 7 slides
- `sizzle-<seu-produto>/renders/final.mp4` (15s)
- Documento curto: "como o deck conversa com o vídeo?" (1 parágrafo)

**Materiais:**
- Skill `/website-to-hyperframes`
- `video-projects/hyperframes-sizzle/` (referência)
- Skill `/design-designer`

---

### Módulo 6 — Short-form vertical com talking-head (3h)

**Objetivos:**
- Produzir um short 9:16 estilo TikTok/Reels/Shorts com rosto + motion graphics sincronizado
- Usar a skill `/short-form-video` com o playbook do May Shorts 19
- Sincronizar legendas karaokê com transcrição automática (`npx hyperframes transcribe`)
- Gerar narração on-device com TTS (`npx hyperframes tts`)

**Conceitos-chave:**
- Face-mode choreography: full-screen vs bottom-half
- Áudio-sync scene timing: motion graphics colam em beats da fala
- Karaoke captions: cada palavra acende no timing correto
- As 10 regras de qualidade do `/short-form-video`

**Exercício prático:**
1. Grave um talking-head de 30–45s em MP4 (celular serve)
2. Re-encode com FFmpeg: `ffmpeg -i raw.mov -c:v libx264 -crf 20 -c:a aac -movflags +faststart assets/clip.mp4`
3. `npx hyperframes transcribe assets/clip.mp4 --model small.en --json` → timestamps por palavra
4. `cp -r may-shorts-19 meu-short` e adapte
5. Monte legendas karaokê sincronizadas
6. Adicione 3–5 cenas de motion graphics entre beats do discurso
7. Render draft → frames verify → render standard

**Entregas:**
- `meu-short/renders/final.mp4` (vertical 1080×1920, 30–45s)
- Screenshots de 5 frames mostrando: talking-head, motion cena, legenda karaokê, call-to-action
- Auto-avaliação: as 10 regras da skill foram respeitadas?

**Materiais:**
- Skill `/short-form-video`
- `video-projects/may-shorts-19/` (referência máxima)
- Comandos: `npx hyperframes transcribe` e `npx hyperframes tts`

---

### Módulo 7 — Promo de produto SaaS 30–60s (4h)

**Objetivos:**
- Produzir um promo de produto SaaS no estilo Infinite Payments / Linear
- Usar intensivamente o catálogo do registry: `x-post`, `ui-3d-reveal`, `app-showcase`, shader transitions
- Aplicar o MOTION_PHILOSOPHY.md integralmente — esta é a prova final da estética
- Renderizar em qualidade `high` com `--docker` para output determinístico arquivável

**Conceitos-chave:**
- Estrutura típica de SaaS promo: hook → problema → solução (produto em ação) → prova social → CTA
- Uso de blocos do registry: quando um bloco pronto é melhor que reinventar
- Shader transitions: `glitch`, `whip-pan`, `cinematic-zoom`, `light-leak`, `sdf-iris`
- Kinetic typography: chrome gradient + halo glow + motion blur
- Outro breathing: hold de 4–6s com o logo + URL

**Exercício prático:**
1. Escolha: `clickup-demo` (mais longo) ou `linear-promo-30s` (mais apertado) como base
2. Substitua o produto pelo seu
3. Instale 3+ blocos do catálogo que você não usou ainda: `npx hyperframes add <name>`
4. Rode o pre-flight do MOTION_PHILOSOPHY **três vezes** durante a produção
5. Draft render → preview gate com alguém que nunca viu o projeto
6. Final render em `--quality high --docker`

**Entregas:**
- Projeto completo em `video-projects/promo-<seu-produto>/`
- `renders/final-high.mp4` em qualidade archival
- Lista dos 3+ blocos do registry que você adicionou
- Resposta ao "What Would Infinite Do?" (10 perguntas) sobre o resultado

**Materiais:**
- `MOTION_PHILOSOPHY.md` (relê tudo)
- Skills `/hyperframes`, `/gsap`, `/hyperframes-registry`
- `video-projects/clickup-demo/` ou `linear-promo-30s/` (referências)

---

### Módulo 8 — Projeto Final Integrado (4h)

**Objetivos:**
- Produzir uma **campanha completa** para um produto/serviço (real ou ficcional) com 3 peças consistentes:
  1. **Landing page** (Claude Design)
  2. **Pitch deck** 7–10 slides (Claude Design)
  3. **Vídeo promo** 30–60s (Hyperframes)
- Garantir consistência visual e narrativa entre as 3 peças
- Publicar as peças: landing no Vercel, deck em PDF, vídeo no YouTube ou social

**Rubrica de avaliação (autoavaliação):**

| Critério | Pontos | O que significa |
|---|---|---|
| **Sistema de marca aplicado consistentemente** | 25 | Mesmas cores, mesma tipografia, mesmo motif visual nas 3 peças |
| **Narrativa coerente** | 20 | Landing → Deck → Vídeo contam a mesma história, cada um no seu formato |
| **Motion discipline** | 20 | Pre-flight do MOTION_PHILOSOPHY passou; nenhuma das 10 Leis quebrada |
| **Qualidade técnica** | 15 | Lint zero erros; render em `standard`; nenhum frame quebrado |
| **Scroll-stop hook** | 10 | Primeiros 2s do vídeo e primeira dobra da landing prendem atenção |
| **Callback narrativo** | 10 | Fim referencia início em pelo menos 2 peças |

**Entregas:**
- Link da landing page publicada
- PDF do pitch deck
- MP4 do vídeo em qualidade `standard` (1080p lossless)
- Documento de 1 página: "decisões de design explicadas" — por que essa paleta, por que esse ritmo, por que esse hook
- Repo no GitHub com todos os arquivos (exceto `renders/` pesados, que ficam no gitignore)

**Materiais:**
- Tudo que foi aprendido nos módulos 1–7
- Todas as skills relevantes: `/design-designer`, `/hyperframes`, `/short-form-video`, `/website-to-hyperframes`, `/gsap`, `/hyperframes-registry`, `/hyperframes-cli`
- CLAUDE.md completo (render contract + visual verification)

---

## Recursos e referências

### Documentação oficial
- Hyperframes: https://hyperframes.heygen.com (`llms.txt` para índice completo)
- Claude Design: https://claude.ai/design (sem docs públicas; skill `/design-designer` resume o system prompt)
- GSAP: https://greensock.com/docs
- FFmpeg: https://ffmpeg.org/documentation.html

### Skills do Claude Code usadas no curso
| Skill | Módulos |
|---|---|
| `/design-designer` | 1, 2, 5, 8 |
| `/hyperframes` | 3–8 |
| `/hyperframes-cli` | 0, 4, 6, 7, 8 |
| `/gsap` | 3, 4, 7 |
| `/hyperframes-registry` | 5, 7, 8 |
| `/short-form-video` | 6, 8 |
| `/website-to-hyperframes` | 5, 8 |

### Projetos do kit usados como referência
| Módulo | Projeto de referência |
|---|---|
| 3 | `may-shorts-19` (gold standard curto) |
| 4 | `claude-edit-intro` (base neutra) |
| 5 | `hyperframes-sizzle` (sizzle reel) |
| 6 | `may-shorts-19` e `may-shorts-18` (shorts verticais) |
| 7 | `clickup-demo`, `linear-promo-30s` (promos SaaS) |
| 8 | qualquer combinação — é projeto original |

### Leituras obrigatórias (por ordem)
1. `README.md` (este repo)
2. `CLAUDE.md` — especialmente seções "Render Contract" e "Visual Verification"
3. `MOTION_PHILOSOPHY.md` — integral
4. `DESIGN.ais-example.md` — como exemplo para escrever o seu
5. Skill `/design-designer` (SKILL.md) — entender a mente do Claude Design

---

## Formato de entrega e suporte

**Modalidade sugerida:** cohort de 4–8 semanas com encontros semanais de 90min + trabalho autodirigido nos módulos.

**Ferramentas de suporte:**
- Discord ou Slack da turma para revisões entre pares
- Office hours semanais com o instrutor (1h)
- GitHub Classroom para entregas (cada aluno tem fork do kit)
- Reviews cruzadas: cada aluno avalia 2 projetos de colegas por módulo

**Certificação (opcional):**
Aluno que entregar os 8 projetos (M0–M8) com nota ≥70 na rubrica do M8 recebe certificado de conclusão.

---

## Próximos passos após o curso

- **Nível avançado:** estudar os projetos AIS pesados (`aisoc-hype`, `aisoc-app-release`, `golden-ratio-demo`) e reconstruir um do zero para sua marca
- **Produção em escala:** montar pipeline com `hyperframes producer` para renders em batch via API
- **Integração com n8n/ClickUp:** automatizar geração de briefings + renders via webhook (ver skills `/n8n-*`)
- **Formação de instrutor:** pegar este plano e adaptar para seu próprio público

---

*Plano construído combinando o conteúdo do `cchyperframes` (Hyperframes Student Kit) com a skill `design-designer` para Claude Design (claude.ai/design). Iterável — contribuições e ajustes bem-vindos.*
