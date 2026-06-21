# Redesign Fintech — Quanttia Site

**Data:** 2026-06-20  
**Referência:** Puzzle Fintech Website (Dribbble)  
**Escopo:** Todas as páginas (index, servicos, sobre, cases, blog, blog posts, contato)  
**Restrição de cor:** Manter azul Quanttia (`#3B82F6` + `#22D3EE`) no lugar do verde da referência

---

## 1. Sistema Visual Global

### Paleta
- `--bg: #0A0F1C` — fundo principal
- `--surface: #111827` — superfície de cards
- `--surface-2: #0D1220` — superfície alternada (seções zebradas)
- `--primary: #3B82F6` — azul principal
- `--cyan: #22D3EE` — cyan accent
- `--gradient-data: linear-gradient(135deg, #3B82F6, #22D3EE)` — gradiente principal

### Tipografia
- Display: Space Grotesk 700
- Body: Inter 400/500/600
- Mono: JetBrains Mono 500/600
- Escala de hero aumentada: `clamp(48px, 6vw, 72px)` na homepage
- Páginas internas: `clamp(34px, 4.6vw, 52px)`

### Cards
- `border-radius: 20px` (aumentado de 14px)
- Hover: borda com gradiente `rgba(59,130,246,0.4)` + sombra dramática
- Transição: `transform .25s ease, border-color .25s ease, box-shadow .25s ease`

### Padrão de fundo
- Grid sutil (`rgba(255,255,255,0.035)`, 46px) mantido nas seções `grid-bg`
- Glow spots radiais azuis em heróis e CTAs

---

## 2. Navegação — Pill Flutuante

**Estrutura:**
- Container centralizado, `max-width: 780px`, `position: fixed`, `top: 20px`, `left: 50%`, `transform: translateX(-50%)`
- Shape: `border-radius: 999px`
- Background: `rgba(10,15,28,0.75)`, `backdrop-filter: blur(16px)`
- Borda: `1px solid rgba(255,255,255,0.1)`
- Sombra: `0 8px 32px rgba(0,0,0,0.4)`

**Conteúdo interno (flex, justify-content: space-between):**
- Logo à esquerda (ícone Q + "QUANTTIA")
- Links no centro (Serviços · Cases · Sobre · Blog)
- Botão CTA à direita: "Agendar diagnóstico" em gradiente, `border-radius: 999px`

**Estado scrolled:** já ativo desde o início (pill sempre visível com o estilo acima, sem transição de fundo).

**Mobile (< 680px):** pill vira barra full-width com toggle hamburguer; menu desliza do lado direito.

---

## 3. Homepage (index.html)

### 3.1 Hero
**Layout:** grid `55fr 45fr`, gap 48px, `align-items: center`, padding `180px 0 100px`.

**Coluna esquerda:**
- Badge animado: `[ ● BI + IA ]` — ponto pulsa em cyan, fonte mono, borda sutil, `border-radius: 999px`
- `<h1>` com `font-size: clamp(48px, 6vw, 72px)`, `line-height: 1.08`; palavra-chave "decisões" em gradiente
- Lead 18px, max-width 480px
- Hero CTAs: btn-primary "Agendar diagnóstico gratuito" + btn-ghost "Ver serviços →"
- Tags mono: Power BI · SQL & ETL · Python · Analytics

**Coluna direita — Dashboard Mockup:**
- Card principal: `transform: perspective(1200px) rotateY(-8deg) rotateX(3deg)`, sombra dramática
- Três **floating metric cards** (`position: absolute`) sobrepostos na frente do dashboard:
  - Card "Receita" (canto superior esquerdo do mockup): valor + variação verde
  - Card "Ticket Médio" (canto inferior direito): valor + variação
  - Card "Churn" (lateral): valor + variação vermelha
  - Cada card: `background: rgba(17,24,39,0.9)`, `backdrop-filter: blur(8px)`, `border: 1px solid rgba(255,255,255,0.12)`, `border-radius: 12px`, `padding: 12px 16px`
- Glow spot azul atrás do card principal

### 3.2 Seção "Para quem trabalhamos"
- Substituir grid 3×2 por **tags horizontais** em flex wrap
- Cada tag: `border: 1px solid var(--border-strong)`, `border-radius: 999px`, `padding: 8px 20px`, ícone SVG inline + label
- Linha descritiva abaixo, centralizada

### 3.3 Bento Grid "O que fazemos"
**Grid CSS:**
```css
display: grid;
grid-template-columns: 1.6fr 1fr;
grid-template-rows: 1fr 1fr;
gap: 20px;
```

**Card A (BI/Dashboards) — row-span 2 (coluna esquerda, altura dupla):**
- Ícone 48px com glow azul radial atrás
- Badge `BUSINESS INTELLIGENCE`
- Título e descrição completa
- Mini SVG preview de gráfico de barras no canto inferior (decorativo, baixa opacidade)
- `background: var(--surface)`, borda com gradiente sutil

**Card B (Análise de Dados) — canto superior direito:**
- Ícone, badge, título, descrição curta

**Card C (Consultoria) — canto inferior direito:**
- Ícone, badge, título, descrição curta

### 3.4 Métricas (KPI Grid)
- Mantém 4 colunas
- Divisores verticais `rgba(255,255,255,0.08)` entre cada KPI
- Fundo `var(--surface-2)` para destacar a seção
- Números em gradiente com animação de contagem

### 3.5 Mini Cases — Bento
**Grid:**
```css
grid-template-columns: 1.5fr 1fr;
grid-template-rows: auto auto;
```
- Case A: coluna esquerda, altura simples
- Case B: coluna direita, topo
- Case C: coluna esquerda, segunda linha (ou span 2 no fundo)
- Cada card: tag segmento, título, descrição, resultado em mono cyan no rodapé
- Card "largo": linha decorativa de código/dado no background (`opacity: 0.04`, mono grande)

### 3.6 Depoimento
- Centralizado, `max-width: 680px`
- Aspas tipográficas `"` como decoração: `font-size: 96px`, gradiente, `opacity: 0.12`, `position: absolute`
- Citação em Space Grotesk 500, `clamp(19px, 2.4vw, 25px)`
- Autor + cargo abaixo com separador `—`

### 3.7 CTA Final
- Seção com `background: radial-gradient(ellipse 80% 60% at 50% 50%, rgba(59,130,246,0.18), transparent)`
- Grid de fundo ativo
- Título grande, subtítulo, botão primário centralizado

---

## 4. Páginas Internas — Hero Padrão

Aplicado em: servicos.html, sobre.html, cases.html, blog.html, contato.html

```
padding: 140px 0 60px
eyebrow tag (mono, cyan)
h1 (clamp 34px–52px)
linha decorativa gradiente (width: 180px, height: 2px, margin-top: 16px)
subtítulo (max-width: 600px)
```

---

## 5. Serviços (servicos.html)

Cada bloco de serviço (`service-block`):
- Layout split: `0.85fr 1.15fr`
- Número grande de fundo: `01`, `02`, `03` em gradiente com `opacity: 0.06`, posicionado absoluto no canto superior esquerdo da coluna de texto (`font-size: 120px`, Space Grotesk)
- Detail cards mantidos (2×2 grid) com visual atualizado: título em cyan mono, listas com `—` marker

---

## 6. Sobre (sobre.html)

**Hero split:**
- Texto à esquerda (eyebrow + h1 + subtítulo + linha decorativa)
- 3 stats flutuantes à direita: anos de experiência, clientes, dashboards — números em gradiente, labels em muted

**Seção de valores:**
- Bento grid: 1 card largo (`grid-column: span 2`) + 2 cards normais
- Card largo: ícone grande + título + descrição detalhada
- Cards normais: ícone + título + descrição curta

**Diferenciais:** mantém layout split com ícones

---

## 7. Cases (cases.html)

**Filtros por segmento:**
- Tabs horizontais: Todos · Varejo · Indústria · Saúde · Serviços · Logística
- Tab ativa: fundo gradiente, texto escuro
- Implementação: JavaScript simples com `data-segment` attribute nos cards

**Grid de cases:**
- 2 colunas, cards uniformes mas generosos (`padding: 32px`)
- Topo do card: faixa colorida (`height: 4px`, gradiente) ou ícone de segmento
- Estrutura: tag · título · desafio · resultado em mono cyan

---

## 8. Blog (blog.html)

**Layout:**
- Card destaque: 100% largura, imagem/cor de categoria à esquerda (`background: var(--surface-2)`), texto à direita
- Grid abaixo: 2 ou 3 colunas para posts secundários
- Cada card: categoria em eyebrow, título, resumo, tempo de leitura em mono muted, data

---

## 9. Contato (contato.html)

**Layout split `1fr 1fr`:**

**Coluna esquerda — formulário:**
- Card elevado com `box-shadow: var(--shadow-card)`
- Campos: nome, email, empresa, segmento, mensagem
- Botão submit full-width em gradiente

**Coluna direita — informações:**
- Título + subtítulo
- Card "O que esperar": lista de 3–4 próximos passos após o envio (ícone + texto)
- Link para Instagram
- Texto de resposta esperada

---

## 10. Blog Posts (blog-*.html)

- Hero com título do post, eyebrow com categoria, data e tempo de leitura
- Conteúdo em coluna central (`max-width: 720px`, `margin: 0 auto`)
- Tipografia de leitura: `font-size: 17px`, `line-height: 1.8`
- CTA ao final: card com "Quer aplicar isso no seu negócio?"

---

## 11. Responsividade

| Breakpoint | Mudança |
|---|---|
| `< 980px` | Hero homepage vira 1 coluna; bento grid vira 1 coluna |
| `< 680px` | Nav pill vira barra full-width com toggle; KPI grid 2×2; forms 1 coluna |

---

## 12. Arquitetura de Implementação

**Arquivos a modificar:**
- `styles.css` — refatoração completa (nav pill, card system, bento utilities, tokens atualizados)
- `index.html` — hero novo, bento grid serviços, bento mini cases, segmentos como tags
- `servicos.html` — números decorativos de fundo, visual atualizado
- `sobre.html` — hero split com stats, bento valores
- `cases.html` — filtros por segmento, grid atualizado
- `blog.html` — card destaque + grid secundário
- `contato.html` — layout split formulário/info
- `blog-dashboard-vs-planilha.html` — hero e tipografia de leitura
- `blog-bi-mais-ia.html` — hero e tipografia de leitura
- `script.js` — adicionar filtro de cases por segmento

**Ordem de implementação:**
1. `styles.css` (sistema base + nav pill)
2. `index.html` (homepage completa)
3. `servicos.html`, `sobre.html`, `cases.html`
4. `blog.html`, posts, `contato.html`
