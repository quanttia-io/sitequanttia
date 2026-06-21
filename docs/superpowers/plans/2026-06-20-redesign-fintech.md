# Redesign Fintech — Quanttia Site — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesenhar todas as páginas do site Quanttia com estética fintech moderna — nav pill flutuante, hero split mais impactante, bento grids, floating metric cards e layout refinado em todas as páginas internas.

**Architecture:** Static HTML/CSS/JS puro. Cada página tem seu próprio HTML com `<style>` inline para estilos específicos de página, compartilhando `styles.css` global. O redesign refatora `styles.css` completamente e reescreve cada arquivo HTML preservando todo o conteúdo existente.

**Tech Stack:** HTML5, CSS3 (Grid, Flexbox, custom properties), Vanilla JS, Google Fonts (Space Grotesk, Inter, JetBrains Mono)

## Global Constraints

- Paleta: `--primary: #3B82F6`, `--cyan: #22D3EE`, `--gradient-data: linear-gradient(135deg,#3B82F6,#22D3EE)`, `--bg: #0A0F1C`
- Fontes: Space Grotesk (display), Inter (body), JetBrains Mono (mono)
- Nav pill: `max-width: 780px`, centralizada, `position: fixed`, `top: 20px`
- Cards: `border-radius: 20px`, hover com borda gradiente
- Sem frameworks externos — CSS puro e JS vanilla
- Todo conteúdo textual existente deve ser preservado
- Spec completa em `docs/superpowers/specs/2026-06-20-redesign-fintech-design.md`

---

### Task 1: Refatorar styles.css — Sistema base + Nav Pill

**Files:**
- Modify: `styles.css` (reescrita completa)

**Interfaces:**
- Produces: todas as classes base usadas em tasks 2–7: `.nav`, `.nav-pill`, `.nav-pill-inner`, `.btn-primary`, `.btn-secondary`, `.btn-ghost`, `.card`, `.badge-pill`, `.eyebrow`, `.section-head`, `.kpi-grid`, `.kpi`, `.grid-bg`, `.glow-spot`, `.container`, `.footer-*`, `[data-reveal]`

- [ ] **Step 1: Reescrever styles.css com novo sistema de tokens e nav pill**

Substituir o conteúdo completo de `styles.css` pelo seguinte:

```css
/* ===========================================================
   QUANTTIA — Design System v2 (Fintech Redesign)
   =========================================================== */

:root{
  --bg:#0A0F1C;
  --surface:#111827;
  --surface-2:#0D1220;
  --border:rgba(255,255,255,0.08);
  --border-strong:rgba(255,255,255,0.14);
  --border-hover:rgba(59,130,246,0.45);

  --primary:#3B82F6;
  --cyan:#22D3EE;
  --gradient-data:linear-gradient(135deg,#3B82F6,#22D3EE);

  --text:#E5E7EB;
  --muted:#94A3B8;
  --muted-2:#64748B;
  --white:#FFFFFF;

  --radius-sm:10px;
  --radius:14px;
  --radius-lg:20px;
  --radius-xl:28px;

  --shadow-card:0 20px 60px rgba(0,0,0,0.5);
  --shadow-pill:0 8px 32px rgba(0,0,0,0.45);
  --glow-primary:0 0 0 1px rgba(59,130,246,0.4), 0 18px 50px rgba(59,130,246,0.18);

  --ff-display:'Space Grotesk',sans-serif;
  --ff-body:'Inter',sans-serif;
  --ff-mono:'JetBrains Mono',monospace;

  --container:1180px;
}

*{box-sizing:border-box;margin:0;padding:0;}
html{scroll-behavior:smooth;}
body{
  background:var(--bg);
  color:var(--text);
  font-family:var(--ff-body);
  line-height:1.65;
  -webkit-font-smoothing:antialiased;
  overflow-x:hidden;
}
img,svg{display:block;max-width:100%;}
a{color:inherit;text-decoration:none;}
ul{list-style:none;}
button{font-family:inherit;cursor:pointer;border:none;background:none;}
:focus-visible{outline:2px solid var(--cyan);outline-offset:3px;}

.container{width:100%;max-width:var(--container);margin:0 auto;padding:0 24px;}
section{padding:clamp(64px,9vw,120px) 0;position:relative;}

h1,h2,h3,h4{
  font-family:var(--ff-display);
  font-weight:700;
  line-height:1.1;
  letter-spacing:-0.025em;
  color:var(--white);
}
h2{font-size:clamp(28px,3.6vw,44px);}
h3{font-size:20px;}
p{color:var(--muted);}

.eyebrow{
  font-family:var(--ff-mono);font-size:12px;font-weight:600;
  letter-spacing:0.14em;text-transform:uppercase;color:var(--cyan);
  display:flex;align-items:center;gap:10px;margin-bottom:16px;
}
.eyebrow::before{content:"";width:20px;height:2px;background:var(--cyan);display:inline-block;flex-shrink:0;}

.section-head{max-width:640px;margin-bottom:52px;}
.section-head p{font-size:16.5px;margin-top:14px;}
.section-head.center{margin-left:auto;margin-right:auto;text-align:center;}
.section-head.center .eyebrow{justify-content:center;}

/* ---------- grid backdrop ---------- */
.grid-bg{position:relative;}
.grid-bg::before{
  content:"";position:absolute;inset:0;z-index:0;pointer-events:none;
  background-image:
    linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);
  background-size:46px 46px;
  -webkit-mask-image:radial-gradient(ellipse 80% 70% at 50% 0%, black 30%, transparent 100%);
          mask-image:radial-gradient(ellipse 80% 70% at 50% 0%, black 30%, transparent 100%);
}
.grid-bg > .container{position:relative;z-index:1;}
.glow-spot{
  position:absolute;border-radius:50%;filter:blur(100px);pointer-events:none;z-index:0;
  background:radial-gradient(circle, rgba(59,130,246,0.28), transparent 70%);
}

/* ---------- buttons ---------- */
.btn-primary{
  background:var(--gradient-data);color:#04101F;
  padding:14px 28px;border-radius:999px;font-weight:700;font-size:15px;
  display:inline-flex;align-items:center;gap:9px;
  box-shadow:0 12px 32px rgba(59,130,246,0.35);
  transition:transform .2s ease, box-shadow .2s ease;
  white-space:nowrap;
}
.btn-primary:hover{transform:translateY(-2px);box-shadow:0 20px 48px rgba(34,211,238,0.4);}
.btn-secondary{
  border:1.5px solid var(--border-strong);color:var(--text);
  padding:13px 26px;border-radius:999px;font-weight:600;font-size:15px;
  display:inline-flex;align-items:center;gap:9px;
  transition:border-color .2s ease, background .2s ease, transform .2s ease;
  white-space:nowrap;
}
.btn-secondary:hover{border-color:var(--border-hover);background:rgba(59,130,246,0.08);transform:translateY(-2px);}
.btn-ghost{
  font-weight:600;font-size:14.5px;color:var(--muted);
  display:inline-flex;align-items:center;gap:7px;transition:color .2s ease;
}
.btn-ghost:hover{color:var(--text);}

/* ---------- nav pill ---------- */
.nav{
  position:fixed;top:20px;left:0;right:0;z-index:100;
  display:flex;justify-content:center;
  pointer-events:none;
}
.nav-pill{
  pointer-events:all;
  width:100%;max-width:800px;margin:0 24px;
  background:rgba(10,15,28,0.82);backdrop-filter:blur(18px);-webkit-backdrop-filter:blur(18px);
  border:1px solid rgba(255,255,255,0.1);border-radius:999px;
  box-shadow:var(--shadow-pill);
  padding:10px 14px 10px 20px;
  display:flex;align-items:center;justify-content:space-between;gap:12px;
  transition:background .3s ease, box-shadow .3s ease;
}
.nav.scrolled .nav-pill{
  background:rgba(10,15,28,0.92);
  box-shadow:0 12px 40px rgba(0,0,0,0.55);
}
.nav-logo{display:flex;align-items:center;gap:9px;font-family:var(--ff-display);font-weight:700;font-size:17px;color:var(--white);flex-shrink:0;}
.nav-logo .q-mark{width:22px;height:22px;}
.nav-links{display:flex;align-items:center;gap:4px;}
.nav-links a{
  font-size:14px;font-weight:500;color:var(--muted);
  padding:7px 14px;border-radius:999px;
  transition:color .2s ease, background .2s ease;
}
.nav-links a:hover,.nav-links a.active{color:var(--white);background:rgba(255,255,255,0.07);}
.nav-cta{
  background:var(--gradient-data);color:#04101F;padding:9px 20px;border-radius:999px;
  font-size:13.5px;font-weight:700;
  transition:transform .2s ease, box-shadow .2s ease;
  box-shadow:0 4px 16px rgba(34,211,238,0.25);
  flex-shrink:0;
}
.nav-cta:hover{transform:translateY(-1px);box-shadow:0 8px 24px rgba(34,211,238,0.4);}
.nav-toggle{display:none;width:36px;height:36px;align-items:center;justify-content:center;border-radius:8px;border:1px solid var(--border-strong);}
.nav-toggle span{display:block;width:18px;height:1.5px;background:#fff;position:relative;transition:background .2s;}
.nav-toggle span::before,.nav-toggle span::after{content:"";position:absolute;width:18px;height:1.5px;background:#fff;transition:transform .25s ease;}
.nav-toggle span::before{top:-6px;}
.nav-toggle span::after{top:6px;}

/* ---------- cards ---------- */
.card{
  background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-lg);
  padding:28px;
  transition:transform .25s ease, border-color .25s ease, box-shadow .25s ease;
  position:relative;overflow:hidden;
}
.card:hover{transform:translateY(-4px);border-color:rgba(59,130,246,0.35);box-shadow:0 24px 60px rgba(0,0,0,0.4);}
.card .icon{
  width:44px;height:44px;border-radius:12px;
  background:rgba(59,130,246,0.12);color:var(--cyan);
  display:flex;align-items:center;justify-content:center;margin-bottom:18px;
}
.card h3{margin-bottom:10px;font-size:19px;}
.card p{font-size:14.5px;}

.badge-pill{
  font-family:var(--ff-mono);font-size:11px;font-weight:600;letter-spacing:.09em;
  padding:4px 12px;border-radius:999px;display:inline-block;
  background:rgba(59,130,246,0.12);color:var(--primary);text-transform:uppercase;
}
.badge-pill.cyan{background:rgba(34,211,238,0.12);color:var(--cyan);}

/* ---------- kpi / counters ---------- */
.kpi-grid{
  display:grid;grid-template-columns:repeat(4,1fr);
  background:var(--surface-2);border:1px solid var(--border);border-radius:var(--radius-lg);
  overflow:hidden;
}
.kpi{
  padding:32px 28px;
  border-right:1px solid var(--border);
}
.kpi:last-child{border-right:none;}
.kpi .num{font-family:var(--ff-display);font-size:clamp(32px,3.4vw,46px);font-weight:700;color:var(--white);line-height:1;}
.kpi .num .grad{background:var(--gradient-data);-webkit-background-clip:text;background-clip:text;color:transparent;}
.kpi .label{font-size:13px;color:var(--muted);margin-top:8px;}

/* ---------- process ---------- */
.process-track{display:grid;grid-template-columns:repeat(5,1fr);gap:18px;position:relative;}
.process-track::before{content:"";position:absolute;top:22px;left:8%;right:8%;height:1px;background:var(--border-strong);z-index:0;}
.process-step{position:relative;z-index:1;}
.process-step .num{
  width:44px;height:44px;border-radius:50%;background:var(--surface-2);border:1px solid var(--border-strong);color:var(--cyan);
  display:flex;align-items:center;justify-content:center;font-family:var(--ff-mono);font-weight:700;font-size:14px;margin-bottom:18px;
}
.process-step h3{font-size:15.5px;margin-bottom:8px;}
.process-step p{font-size:13.5px;}

/* ---------- forms ---------- */
.form-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-lg);padding:36px;box-shadow:var(--shadow-card);}
.form-row{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-bottom:16px;}
.field{display:flex;flex-direction:column;gap:7px;}
.field.full{grid-column:1 / -1;}
.field label{font-size:13px;font-weight:600;color:var(--muted);}
.field input,.field select,.field textarea{
  border:1.5px solid var(--border-strong);border-radius:10px;padding:12px 14px;
  font-size:14.5px;font-family:inherit;color:var(--text);background:var(--surface-2);
  transition:border-color .2s ease, box-shadow .2s ease;
}
.field input:focus,.field select:focus,.field textarea:focus{outline:none;border-color:var(--primary);box-shadow:0 0 0 3px rgba(59,130,246,0.18);}
.field textarea{resize:vertical;min-height:90px;}
.field select{appearance:none;background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='14' height='14' viewBox='0 0 24 24' fill='none' stroke='%2394A3B8' stroke-width='2.4'%3E%3Cpath d='M6 9l6 6 6-6'/%3E%3C/svg%3E");background-repeat:no-repeat;background-position:right 14px center;}
.submit-btn{
  width:100%;background:var(--gradient-data);color:#04101F;padding:15px;border-radius:12px;
  font-weight:700;font-size:15.5px;margin-top:6px;
  transition:transform .2s ease, box-shadow .2s ease;
  display:flex;align-items:center;justify-content:center;gap:10px;
}
.submit-btn:hover{transform:translateY(-2px);box-shadow:0 16px 36px rgba(34,211,238,0.3);}
.submit-btn:disabled{opacity:.6;cursor:not-allowed;transform:none;}
.form-note{font-size:12px;color:var(--muted-2);margin-top:12px;text-align:center;}
.form-status{margin-top:14px;padding:13px 16px;border-radius:10px;font-size:14px;font-weight:600;display:none;}
.form-status.success{display:block;background:rgba(34,211,238,0.12);color:#67E8F9;}
.form-status.error{display:block;background:rgba(248,113,113,0.12);color:#FCA5A5;}

/* ---------- page hero (inner pages) ---------- */
.page-hero{padding:140px 0 64px;}
.page-hero h1{font-size:clamp(34px,4.6vw,52px);margin-bottom:16px;}
.page-hero p.lead{font-size:17px;max-width:600px;color:var(--muted);}
.page-hero .deco-line{width:180px;height:2px;background:var(--gradient-data);border-radius:2px;margin:18px 0 24px;}

/* ---------- footer ---------- */
footer{padding:56px 0 32px;border-top:1px solid var(--border);}
.footer-top{display:flex;justify-content:space-between;flex-wrap:wrap;gap:32px;margin-bottom:40px;}
.footer-brand{display:flex;align-items:center;gap:10px;margin-bottom:14px;}
.footer-brand .q-mark{width:22px;height:22px;}
.footer-brand strong{font-family:var(--ff-display);color:#fff;font-size:15px;}
.footer-brand-col p{font-size:13.5px;max-width:260px;}
.footer-links-col h4{font-size:12px;color:var(--muted-2);margin-bottom:16px;font-family:var(--ff-mono);font-weight:600;letter-spacing:.1em;text-transform:uppercase;}
.footer-links-col a{display:block;font-size:14px;color:var(--muted);margin-bottom:11px;transition:color .2s ease;}
.footer-links-col a:hover{color:var(--white);}
.footer-bottom{font-size:12px;color:var(--muted-2);text-align:center;padding-top:24px;border-top:1px solid var(--border);}

/* ---------- reveal animation ---------- */
[data-reveal]{opacity:0;transform:translateY(20px);transition:opacity .65s ease, transform .65s ease;}
[data-reveal].is-visible{opacity:1;transform:translateY(0);}

@media (prefers-reduced-motion: reduce){*{animation:none !important;transition:none !important;}}

/* ---------- responsive ---------- */
@media (max-width:980px){
  .kpi-grid{grid-template-columns:1fr 1fr;}
  .kpi{border-right:none;border-bottom:1px solid var(--border);}
  .kpi:nth-child(2n){border-right:none;}
  .kpi:last-child{border-bottom:none;}
  .process-track{grid-template-columns:1fr 1fr;}
  .process-track::before{display:none;}
}
@media (max-width:680px){
  .nav{top:12px;}
  .nav-links{
    position:fixed;top:0;right:0;height:100vh;width:78%;max-width:300px;
    background:rgba(10,15,28,0.98);backdrop-filter:blur(16px);
    flex-direction:column;align-items:flex-start;padding:90px 28px 28px;gap:4px;
    border-left:1px solid var(--border);
    transform:translateX(100%);transition:transform .3s ease;
    border-radius:0;
  }
  .nav-links a{padding:10px 16px;width:100%;border-radius:10px;}
  .nav-links.open{transform:translateX(0);}
  .nav-toggle{display:flex;}
  .nav-cta{display:none;}
  .nav-links .nav-cta{display:inline-flex;margin-top:12px;}
  .kpi-grid{grid-template-columns:1fr 1fr;}
  .process-track{grid-template-columns:1fr;}
  .form-row{grid-template-columns:1fr;}
  .footer-top{flex-direction:column;}
}
```

- [ ] **Step 2: Verificar visualmente no browser**

Abrir qualquer página HTML existente no browser. A nav deve aparecer como pill centralizada flutuando no topo. Cards e botões devem ter o novo visual. Verificar que nenhum estilo quebrou.

- [ ] **Step 3: Commit**

```bash
git add styles.css
git commit -m "feat: refatorar design system — nav pill, tokens v2, card system"
```

---

### Task 2: Homepage — Hero + Nav markup

**Files:**
- Modify: `index.html` (hero section + nav markup)

**Interfaces:**
- Consumes: `.nav`, `.nav-pill`, `.nav-logo`, `.nav-links`, `.nav-cta`, `.nav-toggle` (Task 1), `.btn-primary`, `.btn-secondary`, `.grid-bg`, `.glow-spot`
- Produces: hero completo com floating metric cards, badge animado, dashboard mockup

- [ ] **Step 1: Atualizar a tag `<style>` inline do index.html e o markup do `<nav>`**

Substituir o bloco `<style>` inline existente e o `<nav>` pelo seguinte:

**`<style>` inline (dentro do `<head>`):**
```html
<style>
  /* hero */
  .hero{padding:180px 0 110px;}
  .hero .container{display:grid;grid-template-columns:55fr 45fr;gap:56px;align-items:center;}
  .hero h1{font-size:clamp(46px,6vw,72px);margin-bottom:22px;letter-spacing:-0.03em;line-height:1.06;}
  .hero h1 .hl{background:var(--gradient-data);-webkit-background-clip:text;background-clip:text;color:transparent;}
  .hero p.lead{font-size:18px;max-width:480px;margin-bottom:36px;line-height:1.7;}
  .hero-ctas{display:flex;align-items:center;gap:20px;flex-wrap:wrap;}
  .hero-tags{display:flex;gap:8px;flex-wrap:wrap;margin-top:24px;}
  .hero-tags span{font-family:var(--ff-mono);font-size:11.5px;font-weight:600;letter-spacing:.06em;padding:5px 14px;border-radius:999px;border:1px solid var(--border-strong);color:var(--muted-2);}

  /* hero badge */
  .hero-badge{
    display:inline-flex;align-items:center;gap:8px;
    font-family:var(--ff-mono);font-size:12px;font-weight:600;letter-spacing:.08em;
    color:var(--muted);border:1px solid var(--border-strong);border-radius:999px;
    padding:6px 14px 6px 10px;margin-bottom:22px;
  }
  .hero-badge .dot{
    width:7px;height:7px;border-radius:50%;background:var(--cyan);
    animation:pulse 2s infinite;flex-shrink:0;
  }
  @keyframes pulse{0%,100%{opacity:1;box-shadow:0 0 0 0 rgba(34,211,238,0.5);}50%{opacity:.7;box-shadow:0 0 0 5px rgba(34,211,238,0);}}

  /* dashboard mockup */
  .hero-visual{position:relative;padding:20px 0 20px 0;}
  .dash{
    background:var(--surface);border:1px solid var(--border-strong);border-radius:18px;
    box-shadow:0 30px 80px rgba(0,0,0,0.6);padding:0;overflow:hidden;
    transform:perspective(1200px) rotateY(-8deg) rotateX(3deg);
    position:relative;z-index:1;
  }
  .dash-bar{display:flex;gap:6px;padding:14px 16px;border-bottom:1px solid var(--border);}
  .dash-bar span{width:8px;height:8px;border-radius:50%;background:var(--border-strong);}
  .dash-body{padding:20px;}
  .dash-kpis{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:16px;}
  .dash-kpi{background:var(--surface-2);border:1px solid var(--border);border-radius:9px;padding:11px;}
  .dash-kpi .v{font-family:var(--ff-display);font-weight:700;font-size:17px;color:#fff;}
  .dash-kpi .l{font-size:10px;color:var(--muted);margin-top:2px;}
  .dash-kpi .up{color:var(--cyan);font-size:10px;}
  .dash-chart{background:var(--surface-2);border:1px solid var(--border);border-radius:9px;padding:14px;margin-bottom:12px;}
  .dash-bars{background:var(--surface-2);border:1px solid var(--border);border-radius:9px;padding:14px;display:flex;align-items:flex-end;gap:9px;height:80px;}
  .dash-bars .col{flex:1;border-radius:3px 3px 0 0;background:var(--gradient-data);opacity:.8;}

  /* floating metric cards */
  .float-card{
    position:absolute;z-index:2;
    background:rgba(13,18,32,0.92);backdrop-filter:blur(10px);-webkit-backdrop-filter:blur(10px);
    border:1px solid rgba(255,255,255,0.12);border-radius:14px;
    padding:12px 16px;box-shadow:0 12px 36px rgba(0,0,0,0.4);
  }
  .float-card .fc-label{font-size:10.5px;color:var(--muted);font-family:var(--ff-mono);letter-spacing:.06em;text-transform:uppercase;margin-bottom:4px;}
  .float-card .fc-val{font-family:var(--ff-display);font-size:18px;font-weight:700;color:#fff;line-height:1;}
  .float-card .fc-delta{font-size:11px;font-family:var(--ff-mono);margin-top:3px;}
  .float-card.fc-receita{top:8%;left:-14%;}
  .float-card.fc-ticket{bottom:12%;right:-12%;}
  .float-card.fc-churn{top:50%;right:-14%;transform:translateY(-50%);}

  /* bento grid serviços */
  .bento{display:grid;grid-template-columns:1.6fr 1fr;grid-template-rows:1fr 1fr;gap:20px;margin-top:8px;}
  .bento-main{grid-row:span 2;padding:36px;}
  .bento-main .icon{width:52px;height:52px;border-radius:14px;background:rgba(59,130,246,0.15);
    color:var(--cyan);display:flex;align-items:center;justify-content:center;margin-bottom:22px;
    box-shadow:0 0 24px rgba(59,130,246,0.2);}
  .bento-main h3{font-size:22px;margin-bottom:12px;}
  .bento-main p{font-size:15px;line-height:1.7;}
  .bento-mini{padding:24px;}
  .bento-mini .icon{width:38px;height:38px;border-radius:10px;background:rgba(59,130,246,0.1);color:var(--cyan);
    display:flex;align-items:center;justify-content:center;margin-bottom:14px;}
  .bento-mini h3{font-size:17px;margin-bottom:8px;}
  .bento-mini p{font-size:13.5px;}
  .bento-deco{
    position:absolute;bottom:20px;right:20px;opacity:.06;
    font-family:var(--ff-mono);font-size:9px;line-height:1.6;color:var(--cyan);
    text-align:right;pointer-events:none;
  }

  /* segmentos como tags */
  .segments-tags{display:flex;flex-wrap:wrap;gap:10px;margin-bottom:24px;}
  .seg-tag{
    display:inline-flex;align-items:center;gap:8px;
    border:1px solid var(--border-strong);border-radius:999px;
    padding:9px 18px;font-size:14px;font-weight:500;color:var(--muted);
    transition:border-color .2s, color .2s;
  }
  .seg-tag:hover{border-color:rgba(59,130,246,0.4);color:var(--text);}
  .seg-tag svg{flex-shrink:0;}
  .sectors-line{font-size:15px;max-width:680px;color:var(--muted);}

  /* mini cases bento */
  .mini-cases-bento{display:grid;grid-template-columns:1.5fr 1fr;grid-template-rows:auto auto;gap:20px;}
  .mini-case-a{grid-row:span 1;}
  .mini-case-c{grid-column:span 2;}
  .mini-case .tag{margin-bottom:14px;}
  .mini-case h3{font-size:17px;margin-bottom:8px;}
  .mini-case .result{margin-top:16px;padding-top:14px;border-top:1px solid var(--border);font-family:var(--ff-mono);font-size:12.5px;color:var(--cyan);}
  .case-deco{position:absolute;bottom:16px;left:20px;font-family:var(--ff-mono);font-size:48px;font-weight:700;
    background:var(--gradient-data);-webkit-background-clip:text;background-clip:text;color:transparent;
    opacity:.05;line-height:1;pointer-events:none;}

  /* depoimento */
  .testimonial{
    max-width:720px;margin:0 auto;text-align:center;
    border:1px solid var(--border);border-radius:var(--radius-xl);padding:52px 48px;
    background:var(--surface);position:relative;overflow:hidden;
  }
  .testimonial .quote-deco{
    position:absolute;top:10px;left:32px;
    font-family:var(--ff-display);font-size:100px;font-weight:700;line-height:1;
    background:var(--gradient-data);-webkit-background-clip:text;background-clip:text;color:transparent;
    opacity:.1;pointer-events:none;user-select:none;
  }
  .testimonial p.quote{font-family:var(--ff-display);font-size:clamp(18px,2.3vw,24px);color:#fff;font-weight:500;line-height:1.45;margin-bottom:24px;position:relative;z-index:1;}
  .testimonial .author{font-size:13px;color:var(--muted);position:relative;z-index:1;}
  .testimonial .author strong{color:var(--text);}

  /* cta final */
  .final-cta{text-align:center;}
  .final-cta h2{margin-bottom:16px;}
  .final-cta p{max-width:520px;margin:0 auto 32px;font-size:16.5px;}
  .final-cta-bg{
    position:absolute;inset:0;pointer-events:none;
    background:radial-gradient(ellipse 70% 60% at 50% 50%, rgba(59,130,246,0.14), transparent);
  }

  @media (max-width:980px){
    .hero .container{grid-template-columns:1fr;}
    .hero-visual{order:-1;transform:none;}
    .dash{transform:none;}
    .float-card.fc-receita{left:-6%;top:5%;}
    .float-card.fc-ticket{right:-6%;bottom:8%;}
    .float-card.fc-churn{display:none;}
    .bento{grid-template-columns:1fr;}
    .bento-main{grid-row:span 1;}
    .mini-cases-bento{grid-template-columns:1fr;}
    .mini-case-c{grid-column:span 1;}
  }
  @media (max-width:680px){
    .hero{padding:140px 0 80px;}
    .float-card{display:none;}
    .mini-cases-bento{grid-template-columns:1fr;}
  }
</style>
```

**Markup do `<nav>` (substituir o nav existente):**
```html
<nav class="nav" id="nav">
  <div class="nav-pill">
    <a href="index.html" class="nav-logo">
      <svg class="q-mark" viewBox="0 0 100 100" fill="none"><circle cx="50" cy="42" r="32" stroke="#fff" stroke-width="11"/><path d="M50 42 L72 70 L50 70 Z" fill="#fff"/></svg>
      QUANTTIA
    </a>
    <ul class="nav-links" id="navLinks">
      <li><a href="servicos.html">Serviços</a></li>
      <li><a href="cases.html">Cases</a></li>
      <li><a href="sobre.html">Sobre</a></li>
      <li><a href="blog.html">Blog</a></li>
      <li><a href="contato.html" class="nav-cta">Agendar diagnóstico</a></li>
    </ul>
    <button class="nav-toggle" id="navToggle" aria-label="Abrir menu"><span></span></button>
  </div>
</nav>
```

- [ ] **Step 2: Substituir o `<header class="hero">` pelo novo hero**

```html
<header class="hero grid-bg">
  <div class="glow-spot" style="width:560px;height:560px;top:-180px;right:-100px;"></div>
  <div class="container">
    <div data-reveal>
      <div class="hero-badge"><span class="dot"></span>BI + IA PARA NEGÓCIOS</div>
      <h1>Dados que viram <span class="hl">decisões</span><br>para a sua empresa.</h1>
      <p class="lead">A Quanttia projeta dashboards, análises e treinamentos para equipes que querem clareza nos dados — e não mais achismo nas decisões.</p>
      <div class="hero-ctas">
        <a href="contato.html" class="btn-primary">
          Agendar diagnóstico gratuito
          <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
        </a>
        <a href="servicos.html" class="btn-ghost">
          Ver serviços
          <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
        </a>
      </div>
      <div class="hero-tags">
        <span>Power BI</span><span>SQL & ETL</span><span>Python</span><span>Analytics</span>
      </div>
    </div>

    <div class="hero-visual" data-reveal>
      <div class="float-card fc-receita">
        <div class="fc-label">Receita</div>
        <div class="fc-val">R$ 2.4M</div>
        <div class="fc-delta" style="color:var(--cyan);">↑ 18% este mês</div>
      </div>
      <div class="float-card fc-churn">
        <div class="fc-label">Churn</div>
        <div class="fc-val">2.1%</div>
        <div class="fc-delta" style="color:#f87171;">↓ 0.4pp</div>
      </div>
      <div class="float-card fc-ticket">
        <div class="fc-label">Ticket Médio</div>
        <div class="fc-val">R$ 1.230</div>
        <div class="fc-delta" style="color:var(--cyan);">↑ 6%</div>
      </div>
      <div class="dash">
        <div class="dash-bar"><span></span><span></span><span></span></div>
        <div class="dash-body">
          <div class="dash-kpis">
            <div class="dash-kpi"><div class="v">R$ 2.4M</div><div class="l">Receita</div><div class="up">↑ 18%</div></div>
            <div class="dash-kpi"><div class="v">R$ 1.230</div><div class="l">Ticket médio</div><div class="up">↑ 6%</div></div>
            <div class="dash-kpi"><div class="v">2.1%</div><div class="l">Churn</div><div class="up" style="color:#f87171;">↓ 0.4pp</div></div>
          </div>
          <div class="dash-chart">
            <svg viewBox="0 0 280 70" width="100%" height="70">
              <defs>
                <linearGradient id="lineFill" x1="0" y1="0" x2="0" y2="1">
                  <stop offset="0%" stop-color="#22D3EE" stop-opacity="0.3"/>
                  <stop offset="100%" stop-color="#22D3EE" stop-opacity="0"/>
                </linearGradient>
              </defs>
              <path d="M0,55 L40,40 L80,46 L120,24 L160,30 L200,12 L240,18 L280,4 L280,70 L0,70 Z" fill="url(#lineFill)"/>
              <path d="M0,55 L40,40 L80,46 L120,24 L160,30 L200,12 L240,18 L280,4" fill="none" stroke="#22D3EE" stroke-width="2.2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
          <div class="dash-bars">
            <div class="col" style="height:45%"></div>
            <div class="col" style="height:70%"></div>
            <div class="col" style="height:55%"></div>
            <div class="col" style="height:85%"></div>
            <div class="col" style="height:65%"></div>
            <div class="col" style="height:95%"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</header>
```

- [ ] **Step 3: Verificar hero no browser**

Abrir `index.html`. Verificar: badge com ponto pulsando, título grande com gradiente, floating metric cards sobrepostos ao dashboard, glow atrás do dashboard.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: homepage hero — badge animado, floating metric cards, split layout"
```

---

### Task 3: Homepage — Seções de conteúdo (bento, segments, métricas, cases, testimonial, CTA)

**Files:**
- Modify: `index.html` (todas as seções após o hero)

**Interfaces:**
- Consumes: classes da Task 1 + estilos inline da Task 2
- Produces: homepage completa

- [ ] **Step 1: Substituir seção "Para quem trabalhamos" (segmentos como tags)**

Substituir a `<section>` dos segmentos pelo seguinte:

```html
<section>
  <div class="container">
    <div class="section-head" data-reveal>
      <div class="eyebrow">Para quem trabalhamos</div>
      <h2>De autônomos a grandes operações.</h2>
    </div>
    <div class="segments-tags" data-reveal>
      <span class="seg-tag"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 21h18M5 21V9l7-5 7 5v12M9 21v-6h6v6"/></svg>Varejo</span>
      <span class="seg-tag"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="7" width="20" height="14" rx="2"/><path d="M16 7V5a2 2 0 0 0-2-2h-4a2 2 0 0 0-2 2v2"/><line x1="12" y1="12" x2="12" y2="16"/><line x1="10" y1="14" x2="14" y2="14"/></svg>Indústria</span>
      <span class="seg-tag"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 12h-4l-3 9L9 3l-3 9H2"/></svg>Saúde</span>
      <span class="seg-tag"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="8" r="4"/><path d="M4 21c0-4 4-6 8-6s8 2 8 6"/></svg>Serviços</span>
      <span class="seg-tag"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="1" y="3" width="15" height="13" rx="2"/><path d="M16 8h4l3 5v3h-7V8z"/><circle cx="5.5" cy="18.5" r="2.5"/><circle cx="18.5" cy="18.5" r="2.5"/></svg>Logística</span>
      <span class="seg-tag"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M22 10v6M2 10l10-5 10 5-10 5z"/><path d="M6 12v5c3 3 9 3 12 0v-5"/></svg>Educação</span>
    </div>
    <p class="sectors-line" data-reveal>Adaptamos BI e análise de dados à realidade de cada operação — do diagnóstico inicial à evolução contínua dos indicadores.</p>
  </div>
</section>
```

- [ ] **Step 2: Substituir seção "O que fazemos" pelo bento grid**

```html
<section class="grid-bg">
  <div class="container">
    <div class="section-head" data-reveal>
      <div class="eyebrow">O que fazemos</div>
      <h2>Soluções de dados, do diagnóstico à decisão.</h2>
    </div>
    <div class="bento">
      <div class="card bento-main" data-reveal>
        <div class="icon"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9" rx="1.5"/><rect x="14" y="3" width="7" height="5" rx="1.5"/><rect x="14" y="12" width="7" height="9" rx="1.5"/><rect x="3" y="16" width="7" height="5" rx="1.5"/></svg></div>
        <span class="badge-pill">Business Intelligence</span>
        <h3 style="margin-top:16px;">Dashboards & Indicadores</h3>
        <p style="margin-top:10px;">Organizamos seus dados e construímos dashboards sob medida, com KPIs visíveis em tempo real. Do Power BI ao Looker — escolhemos a ferramenta certa para a sua operação.</p>
        <div class="bento-deco">SELECT receita,<br>ticket_medio,<br>churn_rate<br>FROM analytics<br>WHERE periodo = 'atual'</div>
      </div>
      <div class="card bento-mini" data-reveal>
        <div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 3h18v18H3zM3 9h18M9 21V9"/></svg></div>
        <span class="badge-pill cyan">Análise de Dados</span>
        <h3 style="margin-top:14px;">Análise & Diagnóstico</h3>
        <p>Diagnósticos profundos, segmentações e previsões que revelam padrões ocultos na sua operação.</p>
      </div>
      <div class="card bento-mini" data-reveal>
        <div class="icon"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 14a4 4 0 1 0 0-8 4 4 0 0 0 0 8Z"/><path d="M5 21a7 7 0 0 1 14 0"/></svg></div>
        <span class="badge-pill">Consultoria</span>
        <h3 style="margin-top:14px;">Consultoria & Treinamentos</h3>
        <p>Acompanhamos sua equipe na adoção de dados e IA no dia a dia, com treinamento prático.</p>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Substituir seção de métricas**

```html
<section>
  <div class="container">
    <div class="kpi-grid" data-reveal>
      <div class="kpi"><div class="num"><span class="grad" data-count="120" data-suffix="+">0+</span></div><div class="label">Dashboards entregues</div></div>
      <div class="kpi"><div class="num"><span class="grad" data-count="40" data-suffix="+">0+</span></div><div class="label">Clientes atendidos</div></div>
      <div class="kpi"><div class="num"><span class="grad" data-count="98" data-suffix="%">0%</span></div><div class="label">Taxa de satisfação reportada</div></div>
      <div class="kpi"><div class="num"><span class="grad">10x</span></div><div class="label">Decisões mais rápidas com dado</div></div>
    </div>
  </div>
</section>
```

- [ ] **Step 4: Substituir seção "Mini Cases"**

```html
<section class="grid-bg">
  <div class="container">
    <div class="section-head" data-reveal>
      <div class="eyebrow">Resultados</div>
      <h2>Alguns casos que estamos construindo.</h2>
    </div>
    <div class="mini-cases-bento">
      <div class="card mini-case mini-case-a" data-reveal>
        <div class="case-deco">01</div>
        <span class="badge-pill">[Segmento]</span>
        <h3 style="margin-top:14px;">[Cliente / Segmento]</h3>
        <p>[Desafio resumido em 1 linha — o que travava a operação antes da Quanttia.]</p>
        <div class="result">[Resultado-chave, ex: -32% no tempo de geração de relatório]</div>
      </div>
      <div class="card mini-case" data-reveal>
        <div class="case-deco">02</div>
        <span class="badge-pill cyan">[Segmento]</span>
        <h3 style="margin-top:14px;">[Cliente / Segmento]</h3>
        <p>[Desafio resumido em 1 linha.]</p>
        <div class="result">[Resultado-chave, ex: +18% de leads qualificados]</div>
      </div>
      <div class="card mini-case mini-case-c" data-reveal>
        <div class="case-deco">03</div>
        <span class="badge-pill">[Segmento]</span>
        <h3 style="margin-top:14px;">[Cliente / Segmento]</h3>
        <p>[Desafio resumido em 1 linha — o que travava a operação antes da Quanttia.]</p>
        <div class="result">[Resultado-chave, ex: dashboard único substitui 6 planilhas]</div>
      </div>
    </div>
    <div style="text-align:center;margin-top:36px;" data-reveal>
      <a href="cases.html" class="btn-ghost">Ver todos os cases
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
      </a>
    </div>
  </div>
</section>
```

- [ ] **Step 5: Substituir seção de Depoimento**

```html
<section>
  <div class="container">
    <div class="testimonial" data-reveal>
      <div class="quote-deco">"</div>
      <p class="quote">"A Quanttia entregou em 6 semanas o que a gente tentou construir em 2 anos. Agora a liderança revisa o dashboard antes de toda reunião."</p>
      <div class="author"><strong>Diretor Comercial</strong> — Setor alimentício</div>
    </div>
  </div>
</section>
```

- [ ] **Step 6: Substituir CTA final**

```html
<section class="grid-bg">
  <div class="final-cta-bg"></div>
  <div class="glow-spot" style="width:500px;height:500px;bottom:-200px;left:calc(50% - 250px);"></div>
  <div class="container final-cta" data-reveal style="position:relative;z-index:1;">
    <div class="eyebrow" style="justify-content:center;">Próximo passo</div>
    <h2>Pronto para transformar dados em decisão?</h2>
    <p>Agende um diagnóstico gratuito e descubra onde BI e IA destravam resultado no seu negócio.</p>
    <a href="contato.html" class="btn-primary">
      Agendar diagnóstico gratuito
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.4"><path d="M5 12h14M13 6l6 6-6 6"/></svg>
    </a>
  </div>
</section>
```

- [ ] **Step 7: Verificar homepage completa no browser**

Abrir `index.html`. Verificar: bento grid assimétrico, tags de segmento em pills, KPI grid com bordas, mini cases com número decorativo, aspas grandes no depoimento, CTA com glow radial.

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "feat: homepage — bento grid, segments tags, kpi grid, mini cases, testimonial, cta"
```

---

### Task 4: Atualizar nav em todas as páginas internas + estilos de page-hero

**Files:**
- Modify: `servicos.html`, `sobre.html`, `cases.html`, `blog.html`, `contato.html`, `blog-dashboard-vs-planilha.html`, `blog-bi-mais-ia.html`

**Interfaces:**
- Consumes: `.nav`, `.nav-pill`, `.page-hero`, `.deco-line` (Task 1)
- Produces: nav pill consistente e page-hero padronizado em todas as páginas

- [ ] **Step 1: Em cada página, substituir o markup do `<nav>` pelo padrão pill**

Em `servicos.html`, `sobre.html`, `cases.html`, `blog.html`, `contato.html`, `blog-dashboard-vs-planilha.html` e `blog-bi-mais-ia.html`, substituir o `<nav class="nav" id="nav">` existente pelo markup pill, ajustando apenas o link `class="active"` para a página correta:

```html
<nav class="nav" id="nav">
  <div class="nav-pill">
    <a href="index.html" class="nav-logo">
      <svg class="q-mark" viewBox="0 0 100 100" fill="none"><circle cx="50" cy="42" r="32" stroke="#fff" stroke-width="11"/><path d="M50 42 L72 70 L50 70 Z" fill="#fff"/></svg>
      QUANTTIA
    </a>
    <ul class="nav-links" id="navLinks">
      <li><a href="servicos.html" [class="active" se for servicos]>Serviços</a></li>
      <li><a href="cases.html" [class="active" se for cases]>Cases</a></li>
      <li><a href="sobre.html" [class="active" se for sobre]>Sobre</a></li>
      <li><a href="blog.html" [class="active" se for blog]>Blog</a></li>
      <li><a href="contato.html" class="nav-cta">Agendar diagnóstico</a></li>
    </ul>
    <button class="nav-toggle" id="navToggle" aria-label="Abrir menu"><span></span></button>
  </div>
</nav>
```

- [ ] **Step 2: Atualizar `<style>` inline de servicos.html**

Substituir o bloco `<style>` inline de `servicos.html`:

```html
<style>
  .service-block{padding:56px 0;border-top:1px solid var(--border);}
  .service-block .container{display:grid;grid-template-columns:0.85fr 1.15fr;gap:56px;align-items:start;}
  .service-num{font-family:var(--ff-mono);font-size:12px;color:var(--muted-2);margin-bottom:10px;letter-spacing:.1em;text-transform:uppercase;}
  .service-block h2{font-size:clamp(24px,2.8vw,34px);margin-bottom:14px;}
  .service-block .lead{font-size:16px;margin-bottom:6px;}
  .service-block-left{position:relative;}
  .service-num-deco{
    position:absolute;top:-10px;left:-8px;
    font-family:var(--ff-display);font-size:120px;font-weight:700;line-height:1;
    background:var(--gradient-data);-webkit-background-clip:text;background-clip:text;color:transparent;
    opacity:.055;pointer-events:none;user-select:none;z-index:0;
  }
  .service-block-left > *{position:relative;z-index:1;}
  .detail-grid{display:grid;grid-template-columns:1fr 1fr;gap:18px;}
  .detail-card{background:var(--surface);border:1px solid var(--border);border-radius:14px;padding:20px;transition:border-color .2s;}
  .detail-card:hover{border-color:rgba(59,130,246,0.35);}
  .detail-card h4{font-size:11px;color:var(--cyan);font-family:var(--ff-mono);letter-spacing:.1em;margin-bottom:12px;text-transform:uppercase;}
  .detail-card ul li{font-size:13.5px;color:var(--text);margin-bottom:8px;padding-left:18px;position:relative;}
  .detail-card ul li::before{content:"—";position:absolute;left:0;color:var(--muted-2);}
  .stack-note{grid-column:1/-1;font-size:12px;color:var(--muted-2);font-style:italic;margin-top:4px;}
  @media (max-width:900px){
    .service-block .container{grid-template-columns:1fr;}
    .detail-grid{grid-template-columns:1fr;}
  }
</style>
```

- [ ] **Step 3: Adicionar `<div class="service-num-deco">` e `<div class="service-block-left">` em cada service-block de servicos.html**

Em cada `.service-block`, envolver a coluna de texto em `<div class="service-block-left">` e adicionar o número decorativo antes do conteúdo. Exemplo para o primeiro bloco (número "01"):

```html
<div class="service-block">
  <div class="container">
    <div class="service-block-left">
      <div class="service-num-deco">01</div>
      <div class="service-num">Serviço 01</div>
      <!-- restante do conteúdo existente -->
    </div>
    <div class="detail-grid">
      <!-- detail cards existentes -->
    </div>
  </div>
</div>
```
Repetir para os blocos 02 e 03.

- [ ] **Step 4: Atualizar `<style>` inline de sobre.html**

```html
<style>
  .story .container{display:grid;grid-template-columns:0.9fr 1.1fr;gap:56px;align-items:start;}
  .story p{font-size:16px;margin-bottom:14px;}

  /* hero sobre com stats */
  .sobre-hero-stats{display:flex;flex-direction:column;gap:24px;padding:20px 0;}
  .stat-item{display:flex;align-items:flex-start;gap:14px;padding:20px;background:var(--surface);border:1px solid var(--border);border-radius:16px;transition:border-color .2s;}
  .stat-item:hover{border-color:rgba(59,130,246,0.3);}
  .stat-num{font-family:var(--ff-display);font-size:38px;font-weight:700;background:var(--gradient-data);-webkit-background-clip:text;background-clip:text;color:transparent;line-height:1;}
  .stat-label{font-size:13.5px;color:var(--muted);margin-top:4px;}

  /* bento valores */
  .values-bento{display:grid;grid-template-columns:1fr 1fr;grid-template-rows:auto auto;gap:20px;}
  .value-main{grid-column:span 2;padding:32px;}
  .value-main .icon{width:48px;height:48px;border-radius:12px;background:rgba(59,130,246,0.12);color:var(--cyan);display:flex;align-items:center;justify-content:center;margin-bottom:18px;}
  .value-mini{padding:24px;}
  .value-mini .icon{width:38px;height:38px;border-radius:10px;background:rgba(59,130,246,0.1);color:var(--cyan);display:flex;align-items:center;justify-content:center;margin-bottom:14px;}

  .diff-list{display:grid;grid-template-columns:1fr 1fr;gap:18px;}
  .diff-item{display:flex;gap:14px;align-items:flex-start;}
  .diff-item .ic{flex:none;width:36px;height:36px;border-radius:10px;background:rgba(34,211,238,0.1);color:var(--cyan);display:flex;align-items:center;justify-content:center;}
  .diff-item p{font-size:14.5px;color:var(--text);font-weight:500;}

  @media (max-width:900px){
    .story .container{grid-template-columns:1fr;}
    .sobre-hero-stats{flex-direction:row;flex-wrap:wrap;}
    .stat-item{flex:1;min-width:140px;}
    .values-bento{grid-template-columns:1fr;}
    .value-main{grid-column:span 1;}
    .diff-list{grid-template-columns:1fr;}
  }
</style>
```

- [ ] **Step 5: Atualizar o `<header class="page-hero">` de sobre.html para split com stats**

Substituir o header existente:
```html
<header class="page-hero grid-bg">
  <div class="glow-spot" style="width:420px;height:420px;top:-120px;right:-80px;"></div>
  <div class="container">
    <div style="display:grid;grid-template-columns:1.1fr 0.9fr;gap:56px;align-items:center;" data-reveal>
      <div>
        <div class="eyebrow">Sobre a Quanttia</div>
        <h1>BI e IA com propósito de negócio.</h1>
        <div class="deco-line"></div>
        <p class="lead">Somos uma consultoria especializada em transformar dados dispersos em clareza estratégica — combinando Business Intelligence e Inteligência Artificial.</p>
      </div>
      <div class="sobre-hero-stats">
        <div class="stat-item">
          <div>
            <div class="stat-num">5+</div>
            <div class="stat-label">Anos de experiência</div>
          </div>
        </div>
        <div class="stat-item">
          <div>
            <div class="stat-num">40+</div>
            <div class="stat-label">Clientes atendidos</div>
          </div>
        </div>
        <div class="stat-item">
          <div>
            <div class="stat-num">120+</div>
            <div class="stat-label">Dashboards entregues</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</header>
```

- [ ] **Step 6: Verificar servicos.html e sobre.html no browser**

Abrir ambas as páginas. Verificar nav pill, números decorativos de fundo nos service-blocks, stats cards no hero de sobre.

- [ ] **Step 7: Commit**

```bash
git add servicos.html sobre.html cases.html blog.html contato.html blog-dashboard-vs-planilha.html blog-bi-mais-ia.html
git commit -m "feat: nav pill em todas as páginas + page-hero padronizado"
```

---

### Task 5: Cases, Blog e Blog Posts

**Files:**
- Modify: `cases.html`, `blog.html`, `blog-dashboard-vs-planilha.html`, `blog-bi-mais-ia.html`
- Modify: `script.js` (adicionar filtro de cases)

**Interfaces:**
- Consumes: `.card`, `.badge-pill`, `.page-hero`, `.deco-line`, `.eyebrow` (Task 1)
- Produces: cases com filtro funcional, blog com card destaque, posts com tipografia de leitura

- [ ] **Step 1: Atualizar `<style>` inline de cases.html**

```html
<style>
  /* filter tabs */
  .filter-tabs{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:40px;}
  .filter-tab{
    padding:8px 20px;border-radius:999px;font-size:13.5px;font-weight:600;cursor:pointer;
    border:1.5px solid var(--border-strong);color:var(--muted);
    background:none;transition:all .2s ease;
  }
  .filter-tab:hover{color:var(--text);border-color:rgba(59,130,246,0.4);}
  .filter-tab.active{background:var(--gradient-data);color:#04101F;border-color:transparent;box-shadow:0 4px 16px rgba(59,130,246,0.3);}

  /* cases grid */
  .cases-grid{display:grid;grid-template-columns:1fr 1fr;gap:24px;}
  .case-card{padding:32px;position:relative;overflow:hidden;}
  .case-card::before{content:"";position:absolute;top:0;left:0;right:0;height:3px;background:var(--gradient-data);}
  .case-card.hidden{display:none;}
  .case-card h3{font-size:19px;margin:14px 0 10px;}
  .case-card .result{margin-top:18px;padding-top:16px;border-top:1px solid var(--border);font-family:var(--ff-mono);font-size:12.5px;color:var(--cyan);}
  .case-card .challenge{font-size:14.5px;}

  @media (max-width:760px){
    .cases-grid{grid-template-columns:1fr;}
    .filter-tabs{gap:6px;}
    .filter-tab{font-size:12.5px;padding:7px 14px;}
  }
</style>
```

- [ ] **Step 2: Atualizar o HTML de cases.html — adicionar filter tabs e atualizar cards**

Após o `<header class="page-hero">`, substituir o conteúdo da seção principal:

```html
<section>
  <div class="container">
    <div class="filter-tabs" data-reveal>
      <button class="filter-tab active" data-filter="all">Todos</button>
      <button class="filter-tab" data-filter="varejo">Varejo</button>
      <button class="filter-tab" data-filter="industria">Indústria</button>
      <button class="filter-tab" data-filter="saude">Saúde</button>
      <button class="filter-tab" data-filter="servicos">Serviços</button>
      <button class="filter-tab" data-filter="logistica">Logística</button>
    </div>
    <div class="cases-grid" id="casesGrid">
      <div class="card case-card" data-segment="varejo" data-reveal>
        <span class="badge-pill">Varejo</span>
        <h3>[Cliente Varejo]</h3>
        <p class="challenge">[Desafio: o que travava a operação antes da Quanttia.]</p>
        <div class="result">[Resultado-chave, ex: -32% no tempo de geração de relatório]</div>
      </div>
      <div class="card case-card" data-segment="industria" data-reveal>
        <span class="badge-pill cyan">Indústria</span>
        <h3>[Cliente Indústria]</h3>
        <p class="challenge">[Desafio.]</p>
        <div class="result">[Resultado-chave]</div>
      </div>
      <div class="card case-card" data-segment="saude" data-reveal>
        <span class="badge-pill">Saúde</span>
        <h3>[Cliente Saúde]</h3>
        <p class="challenge">[Desafio.]</p>
        <div class="result">[Resultado-chave]</div>
      </div>
      <div class="card case-card" data-segment="servicos" data-reveal>
        <span class="badge-pill cyan">Serviços</span>
        <h3>[Cliente Serviços]</h3>
        <p class="challenge">[Desafio.]</p>
        <div class="result">[Resultado-chave]</div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Adicionar lógica de filtro ao script.js**

No final de `script.js`, adicionar:

```js
// Cases filter
const filterTabs = document.querySelectorAll('.filter-tab');
const caseCards = document.querySelectorAll('.case-card');

filterTabs.forEach(tab => {
  tab.addEventListener('click', () => {
    filterTabs.forEach(t => t.classList.remove('active'));
    tab.classList.add('active');
    const filter = tab.dataset.filter;
    caseCards.forEach(card => {
      if (filter === 'all' || card.dataset.segment === filter) {
        card.classList.remove('hidden');
      } else {
        card.classList.add('hidden');
      }
    });
  });
});
```

- [ ] **Step 4: Atualizar `<style>` inline de blog.html**

```html
<style>
  /* destaque post */
  .post-destaque{display:grid;grid-template-columns:1fr 1.2fr;gap:0;border-radius:var(--radius-lg);overflow:hidden;border:1px solid var(--border);margin-bottom:40px;transition:border-color .25s, box-shadow .25s;}
  .post-destaque:hover{border-color:rgba(59,130,246,0.35);box-shadow:0 24px 60px rgba(0,0,0,0.4);}
  .post-destaque-visual{background:var(--surface-2);display:flex;align-items:center;justify-content:center;min-height:280px;position:relative;overflow:hidden;}
  .post-destaque-visual svg{opacity:.15;}
  .post-destaque-visual .cat-label{position:absolute;top:20px;left:20px;}
  .post-destaque-body{padding:36px;background:var(--surface);display:flex;flex-direction:column;justify-content:space-between;}
  .post-destaque-body h2{font-size:clamp(20px,2.2vw,26px);margin-bottom:12px;line-height:1.3;}
  .post-destaque-body p{font-size:15px;}
  .post-meta{display:flex;align-items:center;gap:16px;margin-top:auto;padding-top:20px;font-family:var(--ff-mono);font-size:11.5px;color:var(--muted-2);}
  .post-meta a{color:var(--cyan);font-weight:600;}
  .post-meta a:hover{text-decoration:underline;}

  /* grid secundário */
  .posts-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:22px;}
  .post-card{padding:24px;}
  .post-card h3{font-size:16px;margin:12px 0 8px;line-height:1.35;}
  .post-card p{font-size:13.5px;}
  .post-card .post-meta{padding-top:14px;margin-top:14px;border-top:1px solid var(--border);}

  @media (max-width:900px){
    .post-destaque{grid-template-columns:1fr;}
    .post-destaque-visual{min-height:160px;}
    .posts-grid{grid-template-columns:1fr 1fr;}
  }
  @media (max-width:600px){
    .posts-grid{grid-template-columns:1fr;}
  }
</style>
```

- [ ] **Step 5: Atualizar HTML da seção principal de blog.html**

```html
<section>
  <div class="container">
    <div class="section-head" data-reveal>
      <div class="eyebrow">Conteúdo</div>
      <h2>Insights sobre dados e decisão.</h2>
    </div>

    <a href="blog-bi-mais-ia.html" class="post-destaque" data-reveal>
      <div class="post-destaque-visual">
        <span class="cat-label"><span class="badge-pill">BI + IA</span></span>
        <svg width="120" height="120" viewBox="0 0 24 24" fill="none" stroke="#22D3EE" stroke-width="1"><path d="M3 3h18v18H3zM3 9h18M9 21V9"/></svg>
      </div>
      <div class="post-destaque-body">
        <div>
          <h2>BI + IA: a combinação que transforma dados em vantagem competitiva</h2>
          <p>Entenda como unir Business Intelligence com Inteligência Artificial cria um sistema de decisão muito mais poderoso do que cada um separado.</p>
        </div>
        <div class="post-meta">
          <span>8 min de leitura</span>
          <span>·</span>
          <span>Quanttia</span>
          <a href="blog-bi-mais-ia.html" style="margin-left:auto;">Ler artigo →</a>
        </div>
      </div>
    </a>

    <div class="posts-grid">
      <a href="blog-dashboard-vs-planilha.html" class="card post-card" data-reveal>
        <span class="badge-pill">Business Intelligence</span>
        <h3>Dashboard vs Planilha: qual é o certo para o seu negócio?</h3>
        <p>Descubra quando cada ferramenta faz sentido e como evitar o erro de usar a errada na hora errada.</p>
        <div class="post-meta">
          <span>6 min de leitura</span>
          <span>·</span>
          <a href="blog-dashboard-vs-planilha.html">Ler →</a>
        </div>
      </a>
      <div class="card post-card" data-reveal style="opacity:.6;pointer-events:none;">
        <span class="badge-pill cyan">Em breve</span>
        <h3>Como identificar os KPIs certos para a sua operação</h3>
        <p>Um guia prático para mapear os indicadores que realmente movem o negócio.</p>
        <div class="post-meta"><span>Em breve</span></div>
      </div>
      <div class="card post-card" data-reveal style="opacity:.6;pointer-events:none;">
        <span class="badge-pill">Em breve</span>
        <h3>ETL sem dor: como organizar dados de múltiplas fontes</h3>
        <p>Estratégias práticas para consolidar dados de ERPs, CRMs e planilhas em um único lugar.</p>
        <div class="post-meta"><span>Em breve</span></div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 6: Atualizar `<style>` inline dos blog posts**

Em `blog-dashboard-vs-planilha.html` e `blog-bi-mais-ia.html`, substituir o bloco `<style>` inline:

```html
<style>
  .post-hero{padding:140px 0 56px;}
  .post-hero .eyebrow{margin-bottom:18px;}
  .post-hero h1{font-size:clamp(28px,4vw,46px);max-width:760px;margin-bottom:18px;}
  .post-hero .post-meta{display:flex;align-items:center;gap:16px;font-family:var(--ff-mono);font-size:12px;color:var(--muted-2);}
  .post-hero .deco-line{width:180px;height:2px;background:var(--gradient-data);border-radius:2px;margin:20px 0 0;}

  .post-body{max-width:720px;margin:0 auto;padding:0 24px;}
  .post-body h2{font-size:clamp(20px,2.4vw,26px);margin:42px 0 14px;}
  .post-body h3{font-size:18px;margin:32px 0 10px;}
  .post-body p{font-size:17px;line-height:1.8;margin-bottom:20px;}
  .post-body ul,.post-body ol{margin:0 0 20px 24px;}
  .post-body li{font-size:16px;line-height:1.75;color:var(--muted);margin-bottom:8px;}
  .post-body strong{color:var(--text);font-weight:600;}
  .post-body blockquote{border-left:3px solid var(--cyan);padding:16px 20px;margin:28px 0;background:rgba(34,211,238,0.06);border-radius:0 10px 10px 0;}
  .post-body blockquote p{font-size:16px;color:var(--text);margin:0;}

  .post-cta{margin-top:60px;padding:36px;background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-lg);text-align:center;}
  .post-cta h3{margin-bottom:10px;}
  .post-cta p{margin-bottom:22px;}
</style>
```

- [ ] **Step 7: Verificar cases.html e blog.html no browser**

Abrir `cases.html`. Clicar nas tabs de filtro — cards devem aparecer/desaparecer. Abrir `blog.html`. Verificar card destaque em layout split, grid de 3 posts abaixo.

- [ ] **Step 8: Commit**

```bash
git add cases.html blog.html blog-dashboard-vs-planilha.html blog-bi-mais-ia.html script.js
git commit -m "feat: cases com filtro por segmento, blog card destaque, posts com tipografia de leitura"
```

---

### Task 6: Página de Contato

**Files:**
- Modify: `contato.html`

**Interfaces:**
- Consumes: `.form-card`, `.field`, `.submit-btn`, `.card`, `.page-hero`, `.deco-line` (Task 1)
- Produces: layout split formulário/informações

- [ ] **Step 1: Substituir `<style>` inline de contato.html**

```html
<style>
  .contato-grid{display:grid;grid-template-columns:1fr 1fr;gap:48px;align-items:start;}
  .contato-info h2{font-size:clamp(24px,2.8vw,34px);margin-bottom:14px;}
  .contato-info p{font-size:16px;margin-bottom:28px;}

  .next-steps{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-lg);padding:28px;margin-top:28px;}
  .next-steps h4{font-family:var(--ff-mono);font-size:11px;letter-spacing:.1em;text-transform:uppercase;color:var(--muted-2);margin-bottom:18px;}
  .next-step-item{display:flex;gap:14px;align-items:flex-start;margin-bottom:18px;}
  .next-step-item:last-child{margin-bottom:0;}
  .step-num{
    flex-shrink:0;width:28px;height:28px;border-radius:50%;
    background:rgba(59,130,246,0.12);color:var(--cyan);
    font-family:var(--ff-mono);font-size:12px;font-weight:700;
    display:flex;align-items:center;justify-content:center;
  }
  .step-text{font-size:14px;color:var(--text);padding-top:4px;}

  .contato-links{display:flex;flex-direction:column;gap:12px;margin-top:24px;}
  .contato-link{display:flex;align-items:center;gap:10px;font-size:14.5px;color:var(--muted);transition:color .2s;}
  .contato-link:hover{color:var(--text);}
  .contato-link svg{flex-shrink:0;color:var(--cyan);}

  @media (max-width:860px){
    .contato-grid{grid-template-columns:1fr;}
  }
</style>
```

- [ ] **Step 2: Substituir a seção principal de contato.html**

Ler o HTML existente de contato.html para preservar o formulário, então envolver em layout split:

```html
<section>
  <div class="container">
    <div class="contato-grid">
      <!-- Coluna esquerda: formulário existente -->
      <div>
        <div class="form-card">
          <!-- manter o formulário existente aqui integralmente -->
        </div>
      </div>

      <!-- Coluna direita: informações -->
      <div class="contato-info" data-reveal>
        <div class="eyebrow">Fale conosco</div>
        <h2>Vamos conversar sobre seus dados.</h2>
        <p>Conte brevemente sobre seu negócio e o que está travando suas decisões. Respondemos em até 24 horas.</p>

        <div class="next-steps">
          <h4>O que esperar</h4>
          <div class="next-step-item">
            <div class="step-num">1</div>
            <div class="step-text">Você preenche o formulário com os dados do seu negócio.</div>
          </div>
          <div class="next-step-item">
            <div class="step-num">2</div>
            <div class="step-text">Em até 24h, entramos em contato para agendar o diagnóstico.</div>
          </div>
          <div class="next-step-item">
            <div class="step-num">3</div>
            <div class="step-text">Diagnóstico gratuito: mapeamos onde dados e IA podem destravar resultado.</div>
          </div>
          <div class="next-step-item">
            <div class="step-num">4</div>
            <div class="step-text">Proposta personalizada, sem compromisso.</div>
          </div>
        </div>

        <div class="contato-links">
          <a href="https://instagram.com/quanttia.bi" target="_blank" rel="noopener" class="contato-link">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="2" y="2" width="20" height="20" rx="5"/><path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"/><line x1="17.5" y1="6.5" x2="17.51" y2="6.5"/></svg>
            @quanttia.bi no Instagram
          </a>
        </div>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Verificar contato.html no browser**

Abrir `contato.html`. Verificar layout split, card "O que esperar" com steps numerados, formulário intacto.

- [ ] **Step 4: Commit**

```bash
git add contato.html
git commit -m "feat: contato — layout split formulário + informações com próximos passos"
```

---

### Task 7: Atualizar footer e verificação final responsiva

**Files:**
- Modify: `index.html`, `servicos.html`, `sobre.html`, `cases.html`, `blog.html`, `contato.html`, `blog-dashboard-vs-planilha.html`, `blog-bi-mais-ia.html`

**Interfaces:**
- Consumes: `.footer-*` (Task 1)
- Produces: footer atualizado em todas as páginas, site responsivo verificado

- [ ] **Step 1: Atualizar markup do footer em todas as páginas**

Em todas as páginas, substituir o footer existente pelo seguinte (apenas o HTML — os estilos já estão no styles.css da Task 1):

```html
<footer>
  <div class="container">
    <div class="footer-top">
      <div class="footer-brand-col">
        <div class="footer-brand">
          <svg class="q-mark" viewBox="0 0 100 100" fill="none"><circle cx="50" cy="42" r="32" stroke="#fff" stroke-width="11"/><path d="M50 42 L72 70 L50 70 Z" fill="#fff"/></svg>
          <strong>QUANTTIA</strong>
        </div>
        <p>Business Intelligence e Inteligência Artificial aplicados a decisões de negócio.</p>
      </div>
      <div class="footer-links-col">
        <h4>Navegação</h4>
        <a href="servicos.html">Serviços</a>
        <a href="cases.html">Cases</a>
        <a href="sobre.html">Sobre</a>
        <a href="blog.html">Blog</a>
      </div>
      <div class="footer-links-col">
        <h4>Contato</h4>
        <a href="contato.html">Falar com a Quanttia</a>
        <a href="https://instagram.com/quanttia.bi" target="_blank" rel="noopener">@quanttia.bi</a>
      </div>
    </div>
    <p class="footer-bottom">© <span id="year"></span> Quanttia — Análise de dados e inteligência.</p>
  </div>
</footer>
```

- [ ] **Step 2: Verificar responsividade em 375px (mobile)**

Abrir DevTools, simular iPhone SE (375px). Verificar em todas as páginas:
- Nav pill vira barra full-width com toggle hamburguer
- Hero vira 1 coluna
- Bento grids viram 1 coluna
- KPI grid vira 2×2
- Formulário vira 1 coluna

- [ ] **Step 3: Verificar responsividade em 768px (tablet)**

Simular iPad (768px). Verificar bento grids, hero, seções de casos.

- [ ] **Step 4: Commit final**

```bash
git add .
git commit -m "feat: footer atualizado em todas as páginas + verificação responsiva"
```
