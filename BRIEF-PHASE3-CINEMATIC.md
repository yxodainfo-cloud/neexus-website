# Brief Phase 3 — Réinjection cinématique (Mobile-first)

**Pour Claude Code.** Cette phase remplace l'ancien brief Phase 3. La règle a changé : **toutes les animations cinématiques tournent sur mobile ET desktop**. Plus de "desktop only" pour les pin/scrub/reveals.

---

## 0 · Contexte critique

**98% du trafic Neexus est mobile.** Désactiver les animations en mobile = priver 98% des visiteurs de l'expérience signature du site. C'est inacceptable.

La règle inverse s'applique : **mobile = priorité 1**, desktop = polish supplémentaire (curseur, magnétisme, parallax souris). Tout le reste — pin, scrub, horizontal scroll, clip-path reveals, compteurs scrubés, parallax orbs — **doit fonctionner identiquement sur mobile**.

Les smartphones 2026 (iPhone 14+, Galaxy S22+, Pixel 7+, et même milieu de gamme) tournent GSAP + ScrollTrigger + Lenis sans broncher. Stripe, Apple, Linear, Vercel ont des sites mobiles plus animés que la moyenne des sites desktop. Aucune raison de se brider.

**Les seules animations à conserver desktop-only :**
- Curseur custom et son halo (pas applicable au touch)
- Magnetic hover (pas de souris en mobile)
- Parallax souris (pas de mouvement de souris)
- 3D tilt au hover (pas de hover en touch — remplacer par tilt au scroll)

**Tout le reste tourne partout.**

---

## 1 · Setup global

Vérifier dans le JS :

```js
gsap.registerPlugin(ScrollTrigger, ScrollToPlugin);

const QUERY_DESKTOP = '(min-width: 1025px)';
const QUERY_MOBILE  = '(max-width: 1024px)';
const REDUCED = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

const mm = gsap.matchMedia();
```

**Brancher Lenis aussi en mobile** (il marche très bien en mobile, l'inertie touch est même améliorée) :

```js
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true,
  smoothTouch: false,    // garder false pour respecter le scroll natif du touch
  touchMultiplier: 2,
});
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add(t => lenis.raf(t * 1000));
gsap.ticker.lagSmoothing(0);
```

`smoothTouch: false` est important : on ne veut pas que Lenis intercepte le scroll natif du smartphone (ça crée des bugs d'inertie). On garde Lenis pour la roue souris et le trackpad. Le smartphone garde son scroll natif (qui est déjà très fluide), mais les ScrollTrigger fonctionnent quand même.

---

## 2 · HERO — Pin scrubé sur mobile ET desktop

### Comportement universel (mobile + desktop)

Le hero **se pin** pendant ~120vh de scroll. Pendant ce pin, le titre se décale lentement vers le haut, les CTAs descendent, les stats fadent. Les orbs s'intensifient.

```js
const heroTl = gsap.timeline({
  scrollTrigger: {
    trigger: '.hero',
    start: 'top top',
    end: '+=120%',
    pin: true,
    scrub: 1,
    anticipatePin: 1,
  }
});

heroTl
  .to('.hero-title', { y: -80, opacity: 0.4, ease: 'none' }, 0)
  .to('.hero-sub',   { y: -50, opacity: 0.3, ease: 'none' }, 0)
  .to('.hero-ctas',  { y: 40,  opacity: 0.7, ease: 'none' }, 0)
  .to('.hero-stats', { y: -30, opacity: 0,   ease: 'none' }, 0.2)
  .to('.orb-1',      { scale: 1.4, opacity: 0.85, ease: 'none' }, 0)
  .to('.orb-2',      { scale: 1.6, opacity: 0.75, ease: 'none' }, 0);
```

**Important** : `.hero` doit avoir `min-height: 100svh` (svh = small viewport height, prend en compte la barre de navigation des smartphones). Pas `100vh` qui crée des sauts en mobile.

### Variante mobile (responsive uniquement)
Sur mobile, vu la hauteur d'écran plus petite, **réduire le pin à 80%** au lieu de 120% pour ne pas saouler :

```js
mm.add(QUERY_MOBILE, () => {
  ScrollTrigger.getById('hero-pin')?.scroll(80); // ajuster end
});
```

Ou plus propre : créer deux timelines distinctes via `mm.add(QUERY_DESKTOP, () => {...})` et `mm.add(QUERY_MOBILE, () => {...})` avec leurs propres valeurs `end`.

---

## 3 · SERVICES — Horizontal scroll desktop / Snap carousel swipeable mobile

### Desktop (≥ 1025px) — Horizontal scroll pinné classique

```js
mm.add(QUERY_DESKTOP, () => {
  const panels = gsap.utils.toArray('.service-panel');
  const track  = document.querySelector('.services-track');
  if (!panels.length || !track) return;

  gsap.to(panels, {
    xPercent: -100 * (panels.length - 1),
    ease: 'none',
    scrollTrigger: {
      trigger: '.services-section',
      pin: true,
      scrub: 1,
      snap: 1 / (panels.length - 1),
      end: () => '+=' + track.offsetWidth
    }
  });
});
```

### Mobile (≤ 1024px) — Snap horizontal carousel + animations internes

Le mobile NE FAIT PAS d'horizontal pinned scroll (ça consomme et ça casse le scroll naturel). À la place : **carousel horizontal swipeable** avec scroll-snap CSS + animations d'entrée GSAP par panel quand il devient visible.

```js
mm.add(QUERY_MOBILE, () => {
  const panels = gsap.utils.toArray('.service-panel');
  panels.forEach((panel, i) => {
    gsap.from(panel.querySelector('.service-visual'), {
      clipPath: 'inset(0% 100% 0% 0%)',
      duration: 1.2,
      ease: 'power4.inOut',
      scrollTrigger: { trigger: panel, start: 'top 70%' }
    });
    gsap.from(panel.querySelectorAll('.service-text > *'), {
      y: 40, opacity: 0,
      stagger: 0.08,
      duration: 0.8,
      ease: 'back.out(1.2)',
      scrollTrigger: { trigger: panel, start: 'top 75%' }
    });
  });
});
```

Et CSS mobile (carousel swipe) :
```css
@media (max-width: 1024px) {
  .services-track {
    display: flex;
    overflow-x: auto;
    scroll-snap-type: x mandatory;
    scroll-behavior: smooth;
    -webkit-overflow-scrolling: touch;
    gap: 16px;
    padding: 0 16px;
  }
  .service-panel {
    flex: 0 0 88%;
    scroll-snap-align: center;
  }
}
```

Résultat : sur mobile, l'utilisateur **swipe latéralement** entre les 3 piliers, chaque panel a une animation interne quand il entre dans la vue. Pas de pin (qui empêcherait le scroll naturel de la page), mais une vraie sensation cinématique.

---

## 4 · PROFILS — Snap carousel cinématique partout

### Desktop ET mobile — même comportement
Le carrousel des 6 cartes profils doit être **swipeable au touch** et **scrollable à la souris/trackpad**, avec snap.

```js
const profileCards = gsap.utils.toArray('.profile-card');

// Animation d'entrée des cartes
gsap.from(profileCards, {
  y: 80, opacity: 0, scale: 0.92,
  stagger: 0.1,
  duration: 1,
  ease: 'power4.out',
  scrollTrigger: {
    trigger: '.profiles-section',
    start: 'top 75%'
  }
});

// Détection de la carte centrale via IntersectionObserver
const trackEl = document.querySelector('.profiles-track');
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    entry.target.classList.toggle('is-active', entry.intersectionRatio > 0.7);
  });
}, { root: trackEl, threshold: [0, 0.7, 1] });
profileCards.forEach(c => observer.observe(c));
```

CSS unifié desktop + mobile :
```css
.profiles-track {
  display: flex;
  overflow-x: auto;
  scroll-snap-type: x mandatory;
  -webkit-overflow-scrolling: touch;
  scroll-behavior: smooth;
  gap: 24px;
  padding: 40px 12vw;  /* desktop : large padding pour aérer */
}
@media (max-width: 1024px) {
  .profiles-track { padding: 40px 6vw; gap: 16px; }
}
.profile-card {
  flex: 0 0 320px;
  scroll-snap-align: center;
  transform: scale(.88);
  filter: saturate(.65) brightness(.75);
  opacity: .7;
  transition: transform .5s cubic-bezier(.2,.8,.2,1), filter .5s, opacity .5s;
}
.profile-card.is-active {
  transform: scale(1);
  filter: saturate(1) brightness(1);
  opacity: 1;
}
```

---

## 5 · MÉTHODE — Pin scrubé sur mobile ET desktop

### Comportement universel

Section pinnée pendant ~150vh, **une ligne SVG se trace** progressivement, les 4 étapes s'allument une à une au passage de la ligne.

```js
const line = document.querySelector('.methode-line path');
if (line) {
  const len = line.getTotalLength();
  gsap.set(line, { strokeDasharray: len, strokeDashoffset: len });

  gsap.to(line, {
    strokeDashoffset: 0,
    ease: 'none',
    scrollTrigger: {
      trigger: '.methode-section',
      pin: true,
      scrub: 1,
      start: 'top top',
      end: '+=150%'
    }
  });
}

// Glow des étapes
gsap.utils.toArray('.methode-step').forEach((step, i) => {
  gsap.fromTo(step,
    { '--glow-opacity': 0 },
    {
      '--glow-opacity': 1,
      ease: 'none',
      scrollTrigger: {
        trigger: '.methode-section',
        start: () => `top+=${i * 35}% top`,
        end:   () => `top+=${(i + 1) * 35}% top`,
        scrub: true
      }
    }
  );
});
```

### Adaptation mobile
La ligne SVG passe en **trajectoire verticale** (les 4 étapes en colonne) au lieu de zigzag horizontal :

```html
<!-- Desktop -->
<svg class="methode-line desktop-only" viewBox="0 0 1200 600">
  <path d="M 100 80 Q 600 80 600 300 Q 600 520 1100 520" .../>
</svg>

<!-- Mobile -->
<svg class="methode-line mobile-only" viewBox="0 0 60 800">
  <path d="M 30 40 L 30 760" stroke-width="2" .../>
</svg>
```

Avec CSS :
```css
.desktop-only { display: block; }
.mobile-only  { display: none; }
@media (max-width: 1024px) {
  .desktop-only { display: none; }
  .mobile-only  { display: block; }
}
```

---

## 6 · RÉSULTATS — Compteurs scrubés sur mobile ET desktop

Le scrub sur compteurs fonctionne parfaitement sur smartphone. **Garder identique des deux côtés**.

```js
gsap.utils.toArray('.counter[data-target]').forEach(el => {
  const target = parseFloat(el.dataset.target);
  const obj = { val: 0 };

  gsap.to(obj, {
    val: target,
    ease: 'none',
    scrollTrigger: {
      trigger: '.results-section',
      start: 'top 80%',
      end: 'top 20%',
      scrub: 1,
      onUpdate: () => {
        if (target >= 100) el.textContent = Math.round(obj.val);
        else el.textContent = obj.val.toFixed(1);
      }
    }
  });
});
```

Le pin de la section est optionnel mais agréable. À garder court (`end: '+=80%'`) pour ne pas saouler.

---

## 7 · STACK — Renforcer l'entrée mobile + desktop

Le zoom in/out désynchronisé tourne **déjà** des deux côtés (Phase 2). On renforce juste **l'entrée**.

```js
gsap.from('.stack-tile', {
  clipPath: 'circle(0% at 50% 50%)',
  scale: 0.6,
  opacity: 0,
  stagger: { each: 0.04, from: 'random' },
  duration: 0.9,
  ease: 'back.out(1.6)',
  scrollTrigger: {
    trigger: '.stack-grid',
    start: 'top 80%'
  }
});
```

### Bonus desktop only — hover 3D
À ajouter dans le hover handler (pas applicable au touch) :
```js
mm.add(QUERY_DESKTOP, () => {
  document.querySelectorAll('.stack-tile').forEach(tile => {
    tile.addEventListener('mouseenter', () => {
      gsap.killTweensOf(tile, 'scale,rotation');
      gsap.to(tile, { scale: 1.18, rotationY: 8, rotationX: -4, duration: 0.4, ease: 'power3.out' });
    });
    tile.addEventListener('mouseleave', () => {
      gsap.to(tile, { rotationY: 0, rotationX: 0, duration: 0.5 });
      restartYoyoForTile(tile);  // helper Phase 2
    });
  });
});
```

CSS conteneur (s'applique partout) :
```css
.stack-grid { perspective: 1000px; }
.stack-tile { transform-style: preserve-3d; }
```

---

## 8 · TÉMOIGNAGES — 3D tilt scrubé sur mobile ET desktop

Le 3D tilt scrubé fonctionne très bien sur touch (en suivant le scroll vertical). À déployer des deux côtés.

```js
gsap.utils.toArray('.testi-card').forEach(card => {
  gsap.fromTo(card,
    { rotationY: -12, rotationX: 5 },
    {
      rotationY: 12, rotationX: -5,
      ease: 'none',
      scrollTrigger: {
        trigger: card,
        start: 'top bottom',
        end: 'bottom top',
        scrub: 1.2
      }
    }
  );
});
```

CSS parent :
```css
.testi-grid { perspective: 1400px; }
.testi-card { transform-style: preserve-3d; }
```

### Adaptation mobile
Si le 3D tilt + scroll vertical donne le mal de mer sur certains smartphones, **réduire l'amplitude** mobile (rotationY: -8 → 8 au lieu de -12 → 12). À tester sur device réel.

---

## 9 · CLIP-PATH REVEALS partout

Sur **toutes** les images / mockups / cartes importantes, remplacer le `fade-up` simple par un `clip-path` reveal. Fonctionne identique mobile + desktop.

```js
gsap.utils.toArray('.service-visual, .profile-card .visual, .testi-card').forEach(el => {
  gsap.from(el, {
    clipPath: 'inset(100% 0% 0% 0%)',
    duration: 1.3,
    ease: 'power4.inOut',
    scrollTrigger: {
      trigger: el,
      start: 'top 78%',
      toggleActions: 'play none none reverse'
    }
  });
});
```

---

## 10 · FAQ — Entrée renforcée mobile + desktop

```js
gsap.from('.faq-item', {
  y: 40,
  opacity: 0,
  rotationX: -50,
  transformOrigin: 'top center',
  stagger: 0.08,
  duration: 0.85,
  ease: 'back.out(1.4)',
  scrollTrigger: {
    trigger: '.faq-list',
    start: 'top 75%'
  }
});
```

CSS parent : `.faq-list { perspective: 1200px; }`

---

## 11 · GLOW ORBS — Parallax sur mobile ET desktop

Actuellement (Phase 1) les orbs ont du parallax desktop only. **À étendre au mobile**, mais en plus subtil pour économiser.

```js
gsap.to('.orb-1', { y: -150, scrollTrigger: { trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: 1 } });
gsap.to('.orb-2', { y: -300, scrollTrigger: { trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: 0.5 } });
gsap.to('.orb-3', { y: -200, x: 100, scrollTrigger: { trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: 1.2 } });
```

Pas de `mm.add(QUERY_DESKTOP)` ici. Universal.

---

## 12 · TEXT SPLIT REVEALS — Sur tous les H1/H2

Pour chaque grand titre, **SplitType + stagger char/word**. Universal mobile + desktop.

```js
gsap.utils.toArray('h1, h2.section-title').forEach(el => {
  const split = new SplitType(el, { types: 'chars, words' });
  gsap.from(split.chars, {
    y: 100,
    opacity: 0,
    rotateX: -90,
    stagger: 0.02,
    duration: 1,
    ease: 'back.out(1.7)',
    scrollTrigger: {
      trigger: el,
      start: 'top 80%'
    }
  });
});
```

---

## 13 · ANIMATIONS EN BOUCLE — Garder en mobile

Les zooms in/out de la section Stack et les pulses des glow orbs **doivent tourner sur mobile**. Pas de désactivation. Seules les animations en boucle qui drainent vraiment (effet aurora boréale en blur lourd) peuvent être réduites en mobile via `@media (max-width: 1024px) { animation-duration: 8s; }` au lieu de 4s.

**Garder absolument** :
- Zoom yoyo des stack-tiles (essentiel)
- Pulse des orbs (signature visuelle)
- Glow pulse des CTAs
- Cursor à pulse sur le hero ("Scroll" indicator)

---

## 14 · `prefers-reduced-motion`

Si l'utilisateur l'a activé (option d'accessibilité OS), **toutes les animations en boucle ET les pin/scrub se désactivent**. Les éléments apparaissent en simple fade-up court.

```js
if (REDUCED) {
  // Désactiver tous les pins
  ScrollTrigger.getAll().forEach(st => st.kill());
  // Reset des éléments à leur état final
  gsap.set('.hero-title, .hero-sub, .hero-ctas, .hero-stats', { clearProps: 'all' });
  gsap.set('.orb-1, .orb-2, .orb-3', { clearProps: 'all' });
  // Killer les boucles infinies
  gsap.globalTimeline.getChildren().forEach(t => {
    if (t.repeat() < 0) t.kill();
  });
}
```

---

## 15 · CHECKLIST de validation MOBILE + DESKTOP

À tester avec hard refresh et DevTools fermé :

### Mobile (smartphone réel ou DevTools en mode device, iPhone 12 Pro / Galaxy S20)
- [ ] **Hero** : se pin pendant 80-120vh, titre se décale, orbs s'intensifient
- [ ] **Pain Points** : cards reveal avec back.out + stagger
- [ ] **Profils** : carrousel swipeable, carte centrale en focus visible
- [ ] **Services** : carrousel swipeable horizontal, animations internes par panel
- [ ] **Méthode** : section pinnée, ligne verticale qui se trace, étapes qui s'allument
- [ ] **Résultats** : compteurs scrubés (chiffres montent au scroll)
- [ ] **Stack** : tuiles entrent en clip-path circle, zoom yoyo continue
- [ ] **Témoignages** : cartes tiltent en 3D au scroll
- [ ] **FAQ** : entrée en accordéon 3D rotateX
- [ ] **Orbs** : parallax au scroll (pas figés)
- [ ] **Titres** : split chars avec stagger reveal
- [ ] **Scroll** : fluide, pas de saccades, pas de double-scroll Lenis

### Desktop (≥ 1280px, fenêtre maximisée, DevTools fermé)
Tout ce qui précède, PLUS :
- [ ] **Curseur custom** avec halo visible et qui change sur les liens
- [ ] **Magnetic hover** sur les CTAs
- [ ] **Services** en horizontal scroll pinné classique (pas swipe carousel)
- [ ] **3D hover** sur les stack-tiles
- [ ] **Parallax souris** sur la section Stack

---

## 16 · Performance — À monitorer

Une fois pushé, **lancer Lighthouse Mobile** (priorité 1) sur neexusds.com :

- Performance : **viser ≥ 80** (la cinématique a un coût, c'est OK de descendre un peu)
- Accessibility : **100** (intouchable)
- Best Practices : **100**
- SEO : **100**

Si Performance Mobile descend sous 75 :
- Réduire les `scrub: 1` à `scrub: 0.5` (moins de frames calculées)
- Limiter le zoom yoyo à 8 tuiles au lieu de 15 sur mobile (les autres restent statiques avec un léger glow)
- Réduire l'amplitude du parallax orbs en mobile

---

## 17 · Notes critiques

1. **Mobile-first dans cette phase** : si tu hésites entre une implémentation desktop-only et une universelle, choisis universelle. Le client a 98% de trafic mobile.
2. **Ne pas casser** le preloader, le footer, la navbar — ils marchent.
3. **Tester sur device réel** au moins une fois avant push. DevTools ≠ device.
4. **Lenis avec `smoothTouch: false`** : on garde le scroll natif des smartphones. Lenis ne sert que pour la roue et le trackpad.
5. **Si une animation rend mal sur mobile** (mal de mer, saccade), **réduire l'amplitude** plutôt que désactiver. La règle absolue : aucune section ne doit être complètement statique en mobile.
6. **`will-change: transform`** sur les éléments pinnés et scrubés pour forcer l'accélération GPU.
7. **Ce n'est PAS un nouveau projet** : on enrichit l'index.html existant.

---

## 18 · Workflow

```bash
git add .
git commit -m "Phase 3 - Réinjection cinématique mobile-first (pin/scrub/reveals universels)"
git push origin main
```

Puis test impératif sur **smartphone réel** (pas DevTools) ET desktop maximisé.

---

**Objectif final** : qu'un utilisateur qui ouvre neexusds.com sur son iPhone ait **exactement la même impression de "vidéo interactive"** qu'un utilisateur qui l'ouvre sur un MacBook Pro 16". Pas de citoyen de seconde zone. La cinématique pour tout le monde.
