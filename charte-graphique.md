# Charte graphique — Site Neexus (neexusds.com)

> Fichier de référence à donner à une IA (ou un dev) pour produire / modifier des composants **dans le style exact du site**. Ambiance : **dark, premium, tech**, dégradé bleu→violet, typographie serif élégante (Fraunces) + sans-serif moderne (Geist).

---

## 1. Polices (Google Fonts)

**Lien d'import à mettre dans le `<head>` :**

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,opsz,wght@0,9..144,300;0,9..144,400;0,9..144,500;1,9..144,300;1,9..144,400&family=Geist+Mono:wght@400;500&family=Geist:wght@300;400;500;600&display=swap" rel="stylesheet" />
```

| Police | Usage | Graisses |
|--------|-------|----------|
| **Fraunces** (serif) | Titres, gros chiffres, noms de produits | 300, 400, 500 + italique 300/400 |
| **Geist** (sans-serif) | Texte courant, paragraphes | 300, 400, 500, 600 |
| **Geist Mono** (monospace) | Petits libellés, numéros de section, tags | 400, 500 |

Familles avec secours :
- `--font-display: 'Fraunces', Georgia, serif;`
- `--font-body: 'Geist', -apple-system, BlinkMacSystemFont, sans-serif;`
- `--font-mono: 'Geist Mono', 'SF Mono', Menlo, monospace;`

---

## 2. Couleurs — variables CSS (copier-coller dans `:root`)

```css
:root {
  /* Fonds */
  --bg: #07080d;              /* fond principal (noir bleuté profond) */
  --bg-elevated: #0f1119;     /* éléments en relief */
  --bg-card: #0c0d14;         /* cartes */

  /* Textes */
  --ink: #ffffff;             /* texte principal */
  --ink-muted: #8b90a8;       /* texte secondaire (gris-bleu) */
  --ink-dim: #4a4d63;         /* texte estompé */

  /* Accents & dégradé signature */
  --accent: #6366f1;          /* bleu-indigo */
  --accent-bright: #818cf8;   /* indigo clair */
  --accent-violet: #a78bfa;   /* lavande */
  --accent-glow: rgba(99, 102, 241, 0.18);
  --accent-glow-soft: rgba(167, 139, 250, 0.12);
  --gradient: linear-gradient(120deg, #6366f1 0%, #8b5cf6 50%, #a78bfa 100%);

  /* Bordures */
  --border: rgba(255, 255, 255, 0.06);
  --border-strong: rgba(255, 255, 255, 0.14);

  /* Divers */
  --wa-green: #25D366;        /* vert WhatsApp (widget) */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --gutter: clamp(1.25rem, 3vw, 2.5rem);
}
```

**Dégradé signature** : bleu-indigo `#6366f1` → violet `#8b5cf6` → lavande `#a78bfa`.
Il colore les titres mis en avant, les boutons, et les chiffres clés.

---

## 3. Patterns de composants (à réutiliser pour rester on-brand)

### Titre de section (libellé mono + grand titre serif)

```html
<div class="section-header">
  <span class="section-num">06 · Études de cas</span>
  <h2 class="display">Un titre fort<br/>avec un <em>mot</em> en couleur.</h2>
</div>
```

```css
.section-num {
  font-family: var(--font-mono);
  font-size: 0.72rem; letter-spacing: 0.18em; text-transform: uppercase;
  color: var(--ink-muted);
}
.display {
  font-family: var(--font-display); font-weight: 300;
  font-size: clamp(2rem, 5vw, 4rem); line-height: 1.05; letter-spacing: -0.02em;
  color: var(--ink);
}
/* Le mot en <em> reçoit le dégradé */
.display em {
  font-style: italic;
  background: var(--gradient);
  -webkit-background-clip: text; background-clip: text;
  color: transparent; -webkit-text-fill-color: transparent;
}
```

### Bouton principal (dégradé)

```css
.btn-primary {
  display: inline-flex; align-items: center; gap: 0.5rem;
  padding: 0.85rem 1.5rem; border-radius: 100px;
  background: var(--gradient); color: #fff;
  font-family: var(--font-body); font-weight: 500;
  box-shadow: 0 10px 30px rgba(99, 102, 241, 0.3);
  transition: transform 0.3s var(--ease-out), box-shadow 0.3s var(--ease-out);
}
.btn-primary:hover { transform: translateY(-2px); box-shadow: 0 14px 36px rgba(99, 102, 241, 0.45); }
```

### Carte (glassmorphism léger)

```css
.card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 24px;
  padding: clamp(1.75rem, 4vw, 3rem);
  transition: border-color 0.4s var(--ease-out), transform 0.4s var(--ease-out);
}
.card:hover { border-color: var(--accent); transform: translateY(-4px); }
```

---

## 4. 🎠 CARROUSEL — pattern exact du site (scroll horizontal)

C'est le modèle utilisé sur le site : **défilement horizontal au doigt/souris**, aimantage (scroll-snap), dégradés de fondu sur les bords, et boutons flèches. **Mobile-first** (95 % du trafic Neexus est mobile).

### HTML

```html
<div class="carousel-wrap">
  <div class="carousel" id="carousel">
    <div class="carousel-card"><!-- contenu 1 --></div>
    <div class="carousel-card"><!-- contenu 2 --></div>
    <div class="carousel-card"><!-- contenu 3 --></div>
    <!-- ... -->
  </div>
  <div class="carousel-indicator">
    <div class="hint">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M5 12h14M13 5l7 7-7 7"/></svg>
      <span>Glissez pour voir plus</span>
    </div>
    <div class="carousel-nav">
      <button id="carPrev" aria-label="Précédent" disabled>
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M19 12H5M11 19l-7-7 7-7"/></svg>
      </button>
      <button id="carNext" aria-label="Suivant">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M5 12h14M13 5l7 7-7 7"/></svg>
      </button>
    </div>
  </div>
</div>
```

### CSS

```css
.carousel-wrap { position: relative; margin: 0 calc(var(--gutter) * -1); padding: 0.5rem 0; }

/* Fondus sur les bords gauche/droite */
.carousel-wrap::before, .carousel-wrap::after {
  content: ''; position: absolute; top: 0; bottom: 0;
  width: clamp(2rem, 6vw, 5rem); z-index: 2; pointer-events: none;
}
.carousel-wrap::before { left: 0; background: linear-gradient(to right, var(--bg), transparent); }
.carousel-wrap::after  { right: 0; background: linear-gradient(to left, var(--bg), transparent); }

.carousel {
  display: flex; gap: 1.25rem;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  scroll-padding-left: var(--gutter);
  padding: 0.5rem var(--gutter) 1.5rem;
  cursor: grab;
  scrollbar-width: none; -ms-overflow-style: none; -webkit-overflow-scrolling: touch;
}
.carousel::-webkit-scrollbar { display: none; }
.carousel.dragging { cursor: grabbing; scroll-snap-type: none; }

.carousel-card {
  flex: 0 0 clamp(280px, 32vw, 400px);   /* largeur des cartes */
  scroll-snap-align: start;
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 18px;
  padding: 2rem;
  transition: border-color 0.5s var(--ease-out), box-shadow 0.5s var(--ease-out);
  user-select: none;
}
.carousel-card:hover { border-color: var(--accent); box-shadow: 0 30px 60px -20px rgba(99, 102, 241, 0.25); }

/* Barre d'indication + flèches */
.carousel-indicator {
  display: flex; align-items: center; justify-content: space-between;
  padding: 0 var(--gutter); margin-top: 1rem;
  font-family: var(--font-mono); font-size: 0.6875rem;
  letter-spacing: 0.15em; text-transform: uppercase; color: var(--ink-muted);
}
.carousel-indicator .hint { display: flex; align-items: center; gap: 0.5rem; }
.carousel-indicator .hint svg { width: 14px; height: 14px; animation: hintSlide 2s ease-in-out infinite; }
@keyframes hintSlide { 0%,100% { transform: translateX(0); } 50% { transform: translateX(4px); } }
.carousel-nav { display: flex; gap: 0.5rem; }
.carousel-nav button {
  width: 36px; height: 36px; border-radius: 50%;
  border: 1px solid var(--border-strong); color: var(--ink-muted);
  display: flex; align-items: center; justify-content: center;
  transition: all 0.3s var(--ease-out);
}
.carousel-nav button:hover:not(:disabled) { border-color: var(--accent); color: var(--accent); background: var(--accent-glow); }
.carousel-nav button:disabled { opacity: 0.3; cursor: not-allowed; }
.carousel-nav button svg { width: 14px; height: 14px; }
```

### JS (glisser-déposer + flèches)

```js
const track = document.getElementById('carousel');
const prev = document.getElementById('carPrev');
const next = document.getElementById('carNext');

if (track) {
  let isDown = false, startX = 0, scrollLeft = 0;
  track.addEventListener('mousedown', e => { isDown = true; startX = e.pageX - track.offsetLeft; scrollLeft = track.scrollLeft; });
  track.addEventListener('mouseleave', () => { isDown = false; track.classList.remove('dragging'); });
  track.addEventListener('mouseup', () => { isDown = false; setTimeout(() => track.classList.remove('dragging'), 50); });
  track.addEventListener('mousemove', e => {
    if (!isDown) return; e.preventDefault();
    const walk = (e.pageX - track.offsetLeft - startX) * 1.5;
    if (Math.abs(walk) > 5) track.classList.add('dragging');
    track.scrollLeft = scrollLeft - walk;
  });

  const cardWidth = () => {
    const first = track.querySelector('.carousel-card');
    const gap = parseFloat(getComputedStyle(track).gap) || 20;
    return first ? first.offsetWidth + gap : 320;
  };
  prev?.addEventListener('click', () => track.scrollBy({ left: -cardWidth(), behavior: 'smooth' }));
  next?.addEventListener('click', () => track.scrollBy({ left: cardWidth(), behavior: 'smooth' }));

  const updateNav = () => {
    const max = track.scrollWidth - track.clientWidth;
    if (prev) prev.disabled = track.scrollLeft <= 5;
    if (next) next.disabled = track.scrollLeft >= max - 5;
  };
  track.addEventListener('scroll', updateNav, { passive: true });
  window.addEventListener('resize', updateNav);
  updateNav();
}
```

---

## 5. Règles à respecter (résumé pour l'IA)

1. **Dark only** : fond `--bg`, jamais de fond clair.
2. **Le dégradé `--gradient`** sert d'accent ponctuel (titres en `<em>`, boutons, chiffres) — pas en aplat partout.
3. **Titres** = Fraunces (serif). **Texte** = Geist. **Libellés/numéros** = Geist Mono majuscules avec `letter-spacing`.
4. **Coins arrondis généreux** (18–24px sur les cartes, 100px sur les boutons/pills).
5. **Mobile-first** : 95 % du trafic est mobile. Tester d'abord en étroit.
6. **Transitions douces** avec `--ease-out`, jamais brutales.
7. **Bordures subtiles** (`--border`), qui passent à `--accent` au survol.
```
