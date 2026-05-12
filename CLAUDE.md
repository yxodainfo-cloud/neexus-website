# BRIEF CRÉATIF — NEEXUS SDS Agency Website

## CONTEXTE PROJET

Tu dois créer le site web de **NEEXUS — Smart Digital Solution Agency**.
C'est une agence digitale spécialisée en IA, automatisation et solutions digitales pour entreprises.
LLC : SDS Smart Digital Solution LLC.
Domaine : neexusds.com (hébergé sur Vercel, déployé via GitHub).

Le site est en **français**. C'est un site vitrine one-page avec une approche **marketing de conversion** (copywriting orienté résultats, CTAs stratégiques, preuve sociale).

⚠️ **NIVEAU D'EXIGENCE** : Ce site doit être au niveau des meilleurs sites d'agences créatives. Pense Apple.com, Stripe.com, ou les sites primés sur Awwwards. L'expérience de scroll doit être CINÉMATIQUE — l'utilisateur doit avoir l'impression de vivre une vidéo interactive, pas de scroller un site classique.

## IDENTITÉ DE MARQUE

- **Nom** : NEEXUS (avec le double "E" qui évoque une ligature infini/ADN = connexion, continuité)
- **Tagline** : Smart Digital Solution • Agency
- **Logo** : Le "EE" du logo a un traitement typographique spécial en forme de double lien/maillons
- **Palette** : Dark theme premium
  - Background principal : #0a0e1a (dark navy profond)
  - Background secondaire : #0f1424, #131830
  - Gradient principal : linear-gradient(135deg, #4d7cff 0%, #7c5cfc 40%, #d63384 100%) — bleu → violet → magenta
  - Gradient texte : linear-gradient(90deg, #4d7cff 0%, #9b6dff 50%, #e8548e 100%)
  - Texte principal : #e8eaf0
  - Texte secondaire : #8b90a5
  - Accents : blue #4d7cff, purple #7c5cfc, magenta #d63384, pink #e8548e
- **Typographie** : Syne (display/titres, 400-800) + DM Sans (body, 300-700) via Google Fonts
- **Ambiance** : Tech premium, sophistiqué mais accessible, pas froid — une agence qui inspire confiance

## LIBRAIRIES CDN À UTILISER

```html
<!-- GSAP + ScrollTrigger (animations scroll cinématiques) -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollToPlugin.min.js"></script>

<!-- Lenis (smooth scroll fluide comme du beurre) -->
<script src="https://unpkg.com/lenis@1.1.18/dist/lenis.min.js"></script>

<!-- SplitType (text splitting pour animations de texte) -->
<script src="https://unpkg.com/split-type@0.3.4/umd/index.min.js"></script>

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;0,700;1,400&display=swap" rel="stylesheet">
```

## TECHNIQUES D'ANIMATION IMMERSIVES (OBLIGATOIRES)

### 1. Smooth Scroll (Lenis)
```javascript
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  smoothWheel: true,
});
function raf(time) {
  lenis.raf(time);
  requestAnimationFrame(raf);
}
requestAnimationFrame(raf);
// Sync avec GSAP ScrollTrigger
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => { lenis.raf(time * 1000); });
gsap.ticker.lagSmoothing(0);
```

### 2. Text Splitting + Reveal (SplitType + GSAP)
Chaque titre principal est splitté en mots ou caractères qui s'animent individuellement au scroll :
```javascript
const splitText = new SplitType('.hero-title', { types: 'chars, words' });
gsap.from(splitText.chars, {
  opacity: 0,
  y: 100,
  rotateX: -90,
  stagger: 0.02,
  duration: 1,
  ease: 'back.out(1.7)',
  scrollTrigger: { trigger: '.hero-title', start: 'top 80%' }
});
```

### 3. Sections Pinnées (Pinned Sections)
Certaines sections restent fixes pendant que le contenu s'anime à l'intérieur. L'utilisateur a l'impression que la page "respire" :
```javascript
ScrollTrigger.create({
  trigger: '.services-section',
  start: 'top top',
  end: '+=300%',
  pin: true,
  scrub: 1,
});
```
Utilise le pinning pour :
- La section Hero (le titre se transforme pendant qu'on scroll)
- La section Services (les 3 piliers se révèlent un par un en restant pinné)
- La section Résultats (les chiffres comptent pendant que la section est pinnée)

### 4. Parallax Multi-couches
Les éléments se déplacent à des vitesses différentes pour créer de la profondeur :
```javascript
gsap.to('.glow-orb-1', {
  y: -200,
  scrollTrigger: { trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: 1 }
});
gsap.to('.glow-orb-2', {
  y: -400,
  scrollTrigger: { trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: 0.5 }
});
```

### 5. Horizontal Scroll Section
La section des services ou des cas clients défile HORIZONTALEMENT pendant que l'utilisateur scroll verticalement :
```javascript
let sections = gsap.utils.toArray('.horizontal-panel');
gsap.to(sections, {
  xPercent: -100 * (sections.length - 1),
  ease: 'none',
  scrollTrigger: {
    trigger: '.horizontal-container',
    pin: true,
    scrub: 1,
    end: () => '+=' + document.querySelector('.horizontal-container').offsetWidth
  }
});
```

### 6. Reveal Animations Progressives
Pas de fade-in basique ! Les éléments doivent :
- Venir de directions différentes (gauche, droite, bas)
- Avoir du stagger (délai entre chaque élément)
- Utiliser des easings expressifs : `power4.out`, `back.out(1.7)`, `elastic.out(1, 0.3)`
- Se transformer (scale, rotation subtile, clip-path reveal)

### 7. Clip-Path Reveals
Les images et sections se révèlent avec des clip-path animés :
```javascript
gsap.from('.service-visual', {
  clipPath: 'inset(100% 0% 0% 0%)',
  duration: 1.5,
  ease: 'power4.inOut',
  scrollTrigger: { trigger: '.service-visual', start: 'top 70%' }
});
```

### 8. Cursor Glow / Magnetic Effect
Un glow subtil qui suit le curseur sur la page + les boutons CTA ont un effet magnétique (ils bougent légèrement vers le curseur quand il s'approche).

### 9. Compteurs Animés avec Scrub
Les chiffres (stats) comptent de 0 à leur valeur PENDANT le scroll, pas juste au trigger :
```javascript
gsap.to('.counter', {
  textContent: 150,
  duration: 1,
  snap: { textContent: 1 },
  scrollTrigger: { trigger: '.stats-section', start: 'top 60%', end: 'center center', scrub: 1 }
});
```

### 10. Navbar Transform
La navbar se transforme au scroll : transparente en haut → fond blur + bordure subtile quand on scroll.

### 11. Page Load Sequence
Au chargement de la page, une séquence orchestrée :
1. Écran noir
2. Le logo NEEXUS apparaît au centre (scale from 0 + glow)
3. Il se déplace vers sa position dans la navbar
4. Le contenu hero se révèle avec stagger (badge → titre lettre par lettre → sous-titre → CTAs → stats)
5. Les glow orbs apparaissent en fondu

### 12. Scroll Progress Indicator
Une barre de progression fine en haut de la page (gradient bleu→magenta) qui indique la progression du scroll.

## STRUCTURE DU SITE (SECTIONS)

### 1. PRELOADER / INTRO ANIMATION
- Écran noir avec logo NEEXUS au centre
- Animation d'entrée (2-3 secondes)
- Transition fluide vers le hero

### 2. NAVIGATION
- Logo "NEEXUS" à gauche
- Liens : Services, Méthode, Résultats, Témoignages
- CTA : "Audit gratuit →"
- Menu hamburger mobile
- Sticky avec effet blur au scroll
- Scroll progress bar en haut

### 3. HERO (PINNED — reste fixe pendant ~200vh de scroll)
- Badge : "Smart Digital Solution Agency" avec dot animé pulsant
- Titre principal splitté en caractères, chaque lettre s'anime individuellement : "Vos concurrents automatisent." puis en gradient "Vous faites encore tout à la main ?"
- Sous-titre qui fade-in avec un léger delay
- 2 CTAs avec effet magnétique : "Réserver mon audit gratuit" (primary, gradient bg) + "Découvrir nos solutions ↓" (secondary/ghost avec border gradient)
- Stats animées en bas : "150+ automatisations déployées" / "98% de satisfaction client" / "10x ROI moyen"
- Pendant le scroll dans le hero pinné : les éléments bougent en parallax, le titre se décale, les glow orbs pulsent

### 4. PROBLÈMES (Pain Points) — REVEAL SECTION
- Titre : "Si vous reconnaissez ces situations, on peut vous aider." (text split animation)
- 4 cartes problèmes qui apparaissent en stagger avec clip-path reveal :
  - 🔄 Tâches répétitives — "Votre équipe passe 60% de son temps sur des tâches qui ne génèrent aucun chiffre d'affaires."
  - 📞 Clients perdus — "Un prospect qui attend 5 minutes sans réponse a 80% de chances de partir chez le concurrent."
  - 🔧 Outils déconnectés — "CRM, facturation, emails, planning — vos outils ne se parlent pas."
  - ⏰ Pas le temps — "Trop occupé à faire tourner le quotidien pour penser à demain."
- Les cartes ont un glow gradient au hover

### 5. SERVICES (3 piliers) — HORIZONTAL SCROLL ou PINNED SEQUENCE
- Titre : "Trois piliers pour digitaliser votre entreprise." (text split)
- Les 3 services apparaissent un par un soit en horizontal scroll, soit en séquence pinnée

**Pilier 01 — IA & Agents Intelligents**
- Chatbots intelligents multi-canaux (site, WhatsApp, Instagram)
- Secrétaire IA — gestion d'appels et prise de RDV automatique
- SAV automatisé avec escalade intelligente vers l'humain
- Répondeurs IA pour emails et messages entrants
- Visual animé : fenêtre de chat avec messages qui apparaissent un par un (typing indicator → message)

**Pilier 02 — Automatisation & Workflows**
- Automatisations Make / n8n sur mesure
- Applications internes métier
- Connexion de tous les outils (CRM, ERP, facturation, agenda)
- Workflows intelligents qui éliminent les tâches manuelles
- Visual animé : noeuds de workflow qui se connectent avec des lignes animées, les données "coulent" entre les noeuds

**Pilier 03 — Présence Digitale**
- Sites web modernes et performants
- Création de contenu (réseaux sociaux, blog, newsletters)
- SEO & stratégie de visibilité
- Visual animé : un site web qui se "construit" ligne par ligne avec un cursor qui tape

### 6. MÉTHODE / PROCESS (4 étapes) — PINNED PROGRESS
- Titre : "Notre méthode en 4 étapes."
- Section pinnée : une timeline/progress bar se remplit au scroll, et chaque étape se révèle quand la barre atteint son point
1. **Audit gratuit** — On analyse vos process et identifie les quick-wins
2. **Stratégie** — On conçoit la solution sur mesure
3. **Déploiement** — On construit et on intègre sans perturber votre activité
4. **Optimisation** — On mesure, on ajuste, on scale

### 7. RÉSULTATS (Chiffres / Social proof) — COMPTEURS SCRUB
- Section pinnée pendant que les compteurs montent
- Gros chiffres en gradient qui comptent au scroll :
  - "47h" économisées par mois en moyenne
  - "3x" plus de leads qualifiés
  - "98%" de satisfaction client
  - "< 24h" temps de réponse moyen
- Background : grille animée ou particules subtiles

### 8. TÉMOIGNAGES — CAROUSEL ou STAGGER
- 3 témoignages clients fictifs mais crédibles :
  - Marc Dubois, CEO — Restaurant Le Comptoir (Genève) : "NEEXUS a automatisé nos réservations et notre SAV. On a récupéré 20h par semaine."
  - Sophie Martin, Directrice — Cabinet Avocats Martin & Associés (Lyon) : "Leur chatbot gère 70% de nos demandes de premier contact. Mon équipe peut enfin se concentrer sur les dossiers."
  - Thomas Keller, Fondateur — TK Immobilier (Zürich) : "Le ROI a été visible dès le premier mois. Les automatisations Make ont changé notre façon de travailler."
- Cartes avec effet 3D tilt au hover (perspective transform)

### 9. CTA FINAL — GRAND IMPACT
- Titre énorme : "Prêt à automatiser votre croissance ?" (text split, apparition dramatique)
- Sous-titre : "On identifie ensemble les 3 quick-wins qui auront le plus d'impact sur votre business. Sans engagement."
- CTA principal gros et magnétique : "Réserver mon audit gratuit"
- CTA secondaire : "Voir un cas client →"
- Background : gradient glow intensifié, orbes qui pulsent

### 10. FOOTER
- Logo NEEXUS + description courte
- Liens : Services, Méthode, Résultats, Contact
- Email : contact@neexusds.com
- Réseaux sociaux (icônes SVG)
- © 2026 SDS Smart Digital Solution LLC — Tous droits réservés
- Le footer se révèle avec un effet "rideau" (le CTA section slide vers le haut pour révéler le footer en dessous)

## DIRECTIVES DE DESIGN AVANCÉES

### Glow Orbs
- 3 orbes minimum, positions fixes, blur intense (120px+)
- Couleurs : bleu, violet, magenta (reprenant le gradient)
- Bougent en parallax au scroll
- Pulsent doucement (animation CSS)

### Noise Texture
- Overlay SVG de bruit fin sur tout le body (opacity 0.03)
- Donne de la matière au fond sombre

### Grid Background
- Grille subtile dans le hero (lignes fines rgba blanc 0.03)
- S'estompe en bas avec un gradient mask

### Cartes et éléments
- Background : rgba(255,255,255,0.03) avec border rgba(255,255,255,0.06)
- Au hover : border gradient animé (pseudo-element avec conic-gradient qui tourne)
- Backdrop-filter: blur sur les cartes pour effet glassmorphism

### Responsive
- Desktop first, mais **entièrement responsive** (mobile, tablette, desktop)
- Sur mobile : les animations GSAP sont simplifiées (pas de pinning complexe, révélations plus simples)
- Navigation hamburger avec animation d'ouverture (menu fullscreen overlay)
- Grilles qui passent en colonnes sur mobile
- Tailles de typo clamp() pour une adaptation fluide

### Technique
- **Single file HTML** (tout dans un seul fichier : HTML + CSS + JS)
- Librairies via CDN (GSAP, Lenis, SplitType, Google Fonts)
- CSS custom properties (variables)
- Pas d'images externes — tout en CSS/SVG inline
- Performant : utiliser will-change et transform/opacity pour les animations (GPU accelerated)
- requestAnimationFrame pour les animations custom

## TONE OF VOICE (Copywriting)
- Direct, confiant, sans jargon inutile
- Vouvoiement des prospects
- Orienté résultats et ROI
- Légèrement provocateur dans les accroches ("Vos concurrents automatisent...")
- Concret et spécifique (pas de "solutions innovantes" vagues)
- Français soigné mais pas corporate

## WORKFLOW DE DÉPLOIEMENT
Le fichier principal doit s'appeler `index.html` à la racine du projet.
Après chaque modification :
```
git add .
git commit -m "description du changement"
git push
```
Vercel déploie automatiquement sur neexusds.com.

## RÉSUMÉ DES PRIORITÉS
1. **L'expérience de scroll doit être CINÉMATIQUE** — c'est le #1 critère
2. Le design doit être dark, premium, et cohérent avec la palette
3. Le copywriting doit donner envie de réserver un audit
4. Les animations doivent être fluides (60fps) et jamais saccadées
5. Le site doit être responsive et fonctionnel sur mobile
6. Tout dans un seul fichier index.html

---

## PHASE 2 — ENRICHISSEMENT (12 mai 2026)

Voir `BRIEF-ENRICHISSEMENT.md` pour le brief original. Cette section reflète **l'état final livré**.

### Ajouts de sections (3 nouvelles)

| ID HTML | Numéro affiché | Position dans le flux | Layout |
|---|---|---|---|
| `#profils` | **`02 Profils`** | Entre Problems et Services | 6 cartes — carrousel snap horizontal desktop, stack vertical mobile |
| `#stack` | **`06 Stack`** | Entre Résultats et Témoignages | 15 tuiles d'outils — zoom irrégulier désynchronisé, parallax souris desktop |
| `#faq` | **`08 Questions`** | Entre Témoignages et CTA Final | Accordéon 7 items, ouverture mutuellement exclusive |

### Renumérotation finale (ordre du scroll respecté)

| Section | Avant | Après |
|---|---|---|
| Hero | — | — (inchangé) |
| Problems | `01` | `01` (inchangé) |
| **Profils** | — | **`02`** (nouveau) |
| Services | `02` | **`03`** |
| Méthode | `03` | **`04`** |
| Résultats | `04` | **`05`** |
| **Stack** | — | **`06`** (nouveau) |
| Témoignages | `05` | **`07`** |
| **FAQ** | — | **`08`** (nouveau) |
| CTA Final | `06` | **`09`** |

⚠️ Le brief original (§6) suggérait de garder Services à `02` et numéroter Profils `03`, créant une discontinuité visuelle au scroll (`01 → 03 → 02 → ...`). Nous avons corrigé en ordre scroll naturel.

### Navbar

- **Ajout du lien `FAQ`** entre `Témoignages` et `Audit gratuit` (desktop + menu mobile)
- **Gap inter-liens responsive via `clamp()`** pour éviter une nav tassée sur laptops 13-14" :
  ```css
  .nav-links { gap: clamp(24px, calc(3.125vw - 8px), 32px); }
  ```
  → `24px` à ≤1024px, interpolation linéaire jusqu'à `32px` à ≥1280px, locked aux extrémités.

### Ajustements de ton (principe « augmentation > remplacement »)

| Section | Élément | Modification |
|---|---|---|
| Problems | H2 | « Si vous reconnaissez ces situations, **vos équipes méritent mieux**. » |
| Problems | Intro | Ajout : « *Et surtout, elles épuisent les équipes qui les subissent au quotidien.* » |
| Problems | P-01 | Ajout : « *autant d'heures qui devraient servir à autre chose.* » |
| Problems | P-04 | Ajout : « *Pendant ce temps, vos meilleurs éléments s'épuisent.* » |
| Services | Pilier 01 lead | Reformulé : « *prennent en charge le répétitif … passent la main à vos équipes pile au bon moment* » |
| Services | Pilier 02 lead | Reformulé : « *Vos équipes retrouvent du temps pour ce qui compte vraiment.* » |
| Résultats | Libellé chiffre 1 | « Heures rendues par mois aux équipes » (au lieu de « Économisées par mois ») |
| Résultats | Libellé chiffre 4 | « Temps de première réponse, tous canaux » (au lieu de « Temps de réponse moyen ») |
| Résultats | Footnote ajoutée | *« Chiffres mesurés sur l'ensemble des projets livrés. Grille de calcul disponible sur demande. »* |

### Notes techniques sur les 3 nouvelles sections

**Pour Qui (`#profils`)**
- `scroll-snap-type: x mandatory` sur `.profiles-carousel`, `scroll-snap-align: center` sur `.profile-card`
- Padding latéral `22vw` desktop (→ `10vw` à ≤1024px → `0` à ≤768px) pour centrer le snap
- Carte centrale : `transform: scale(1)`, latérales : `scale(.85) + filter: saturate(.55) brightness(.7)`
- Détection de la carte centrale via `IntersectionObserver` **+** recalcul sur événement `scroll` (raf-throttled) pour fiabilité
- `initProfilesCarousel()` est appelée **uniquement sur desktop** (snap horizontal). Sur mobile : `flex-direction: column`, `scroll-snap-type: none`, toutes les cartes en `scale(1)`.

**Stack (`#stack`)**
- 15 tuiles avec **monogrammes inline** (lettre/symbole stylisé + couleur de marque) — placeholders à remplacer plus tard par les vrais SVG officiels, tuile par tuile
- Animation phare : chaque tuile reçoit son **propre cycle de respiration** désynchronisé
  ```js
  const minScale = 0.92 + Math.random() * 0.02;
  const maxScale = 1.04 + Math.random() * 0.06;
  const duration = 3 + Math.random() * 4;
  const delay    = Math.random() * 2;
  gsap.fromTo(tile, { scale: minScale }, {
    scale: maxScale, duration, delay,
    repeat: -1, yoyo: true, ease: 'sine.inOut'
  });
  ```
- Entrée staggerée random : `stagger: { each: .05, from: 'random' }` + `ease: 'back.out(1.4)'`
- **Hover desktop** : `gsap.killTweensOf(tile)` puis `scale: 1.15`, tooltip apparaît. Au mouseleave, le cycle de respiration redémarre avec de nouvelles valeurs random.
- **Parallax souris desktop** : la grille suit le curseur en direction opposée (±8px max, lerp 0.08)
- **`prefers-reduced-motion: reduce`** : skip les loops infinies (entrée stagger reste, repos statique ensuite)
- **Mobile** : `backdrop-filter: none`, tooltips cachées, grille 3 cols

**FAQ (`#faq`)**
- `<button class="faq-question" type="button" aria-expanded="false">` (sémantique a11y correcte)
- Icône `+ → ×` : pseudo-éléments `::before` (barre horizontale) et `::after` (barre verticale via `rotate(90deg)`), parent `.faq-icon` qui passe à `transform: rotate(45deg)` quand `.is-open` → le `+` devient `×` par rotation
- Animation : `gsap.set(answer, { height: 'auto' }); gsap.from(answer, { height: 0, duration: .45, ease: 'power3.inOut' })` pour ouvrir
- **Ouverture mutuellement exclusive** : ouvrir un item ferme les autres ouverts (parcourt tous les `.faq-item.is-open` et anime `height: 0`)
- Hover sur item fermé : `background` passe à `var(--grad-soft)` via CSS transition

### Stack technique

Inchangée : HTML + GSAP 3.12.5 (+ ScrollTrigger + ScrollToPlugin) + Lenis 1.1.18 + SplitType 0.3.4, single-file `index.html`, déploiement Vercel auto via push sur `main`.

### Commits Phase 2

- `0d9ad08` — Phase 2 : Pour Qui + Stack + FAQ + ajustements copy + renumérotation
- `10ba4fe` — nav : clamp gap 24→32px interpolé entre 1024 et 1280px viewport

### Conventions à respecter pour toute Phase 3+

1. **Design tokens** : réutiliser ceux de `:root`, ne pas en ajouter sans raison forte
2. **Split desktop/mobile** : pattern `gsap.matchMedia()` avec
   ```js
   const QUERY_DESKTOP = '(min-width: 1025px)';
   const QUERY_MOBILE  = '(max-width: 1024px)';
   ```
3. **Animations universelles** dans `initSite()` directement (mêmes sur desktop et mobile), **polish desktop** dans le bloc `mm.add(QUERY_DESKTOP, …)` (Lenis, cursor, magnetic, tilt, orbs parallax, profiles carousel)
4. **Animations en boucle infinie** (style stack zoom) **doivent être désactivées** si `prefers-reduced-motion: reduce` — voir le guard `if (reducedMotion) return;` dans `initStack()`
5. **Sur mobile (≤ 768px)** :
   - Désactiver les `backdrop-filter` coûteux (`@media (max-width:768px) { ... backdrop-filter: none !important; }`)
   - Désactiver les `::before` avec `conic-gradient` animés
   - Pas de pin, pas de scrub, pas de Lenis, pas de cursor custom, pas de magnetic/tilt
6. **Numérotation affichée** : doit suivre l'ordre du scroll. Si une section est ajoutée au milieu, renuméroter celles d'après.
7. **Anchors HTML** (`#services`, `#methode`, etc.) : ne JAMAIS changer les IDs techniques, ils sont stables. Seuls les **numéros affichés** dans `.section-eyebrow .num` bougent lors d'une renumérotation.

---

## PHASE 3 — RÉINJECTION CINÉMATIQUE MOBILE-FIRST (12 mai 2026)

Voir `BRIEF-PHASE3-CINEMATIC.md`. **Renversement de la doctrine Phase 2** : 98% du trafic Neexus est mobile, donc **toutes les animations cinématiques (pin, scrub, reveals) tournent sur mobile ET desktop**. Seuls cursor / magnetic / 3D hover / parallax souris restent desktop-only (pas applicables au touch).

### Setup global

- **Lenis universel** avec `smoothTouch: false` → on garde le scroll natif des smartphones, Lenis ne pilote que roue souris / trackpad. Les `ScrollTrigger` fonctionnent quand même via `lenis.on('scroll', ScrollTrigger.update)`.
- **`min-height: 100svh`** sur `.hero` et `.hero-inner` (fallback `100vh`) → respecte la barre de nav des smartphones (Safari iOS, Chrome Android).
- **Guard `prefers-reduced-motion: reduce`** à la fin d'`initSite()` : kill tous les `ScrollTrigger` avec `pin` ou `scrub`, `clearProps: 'all'` sur les éléments animés, kill les boucles infinies.

### Modifications majeures par section

| Section | Phase 2 (avant) | Phase 3 (après) |
|---|---|---|
| Hero | Entry timeline seule | Entry timeline **+ pin scrubé 120% desktop / 80% mobile** : titre/sub/CTAs/stats parallaxent en directions opposées, orbs scale up |
| Services | Stack vertical fade-up | **Desktop** : horizontal pin scrubbed (track translate X, snap entre panels) **/ Mobile** : native swipe carousel CSS (`scroll-snap-type: x mandatory`). Internal reveal par panel : visual en `clip-path inset(0% 100% 0% 0%)`, texte en `back.out` stagger |
| Méthode | Progress bar fill au passage | **SVG line verticale tracée scrubed** (strokeDashoffset 0 → 0) + **glow scrubed par step** via CSS custom property `--glow-opacity` |
| Résultats | Compteurs one-shot `onEnter` | **Compteurs SCRUBÉS** (`ease: 'none'`, `scrub: 1`) — les chiffres montent en continu pendant que les cards traversent l'écran |
| Stack | Entry stagger random + zoom yoyo | **Entry `clip-path: circle(0% at 50% 50%)`** + scale + stagger random. Zoom yoyo conservé. Hover 3D `rotationY:8 rotationX:-4` **desktop only** |
| Témoignages | Fade-up stagger | **3D tilt scrubed** (`rotationY: -12 → 12, rotationX: 5 → -5`) tied au scroll vertical — universel (touch + mouse) |
| FAQ | Fade-up entry | **Entry `rotationX: -50` + `back.out(1.4)`** depuis `transformOrigin: top center` (effet "fold" 3D) |
| Orbs | Parallax desktop only | **Parallax universel** (était déjà universel après Phase 2.1) |

### Structures HTML modifiées

- **Services** : ajout d'un wrapper `<div class="services-rail" id="services-rail">` autour de `<div class="services-stack">` pour gérer l'overflow-x mobile sans casser le pin desktop
- **Méthode** : ajout d'un SVG `<svg class="methode-line" id="methode-line">` avec `<defs><linearGradient id="methode-line-grad">` et `<path id="methode-line-path">` injecté dans `.methode-steps`, calibré dynamiquement à la hauteur du conteneur
- **Container parents** : ajout de `perspective: 1000px` (stack) / `perspective: 1400px` (testi) / `perspective: 1200px` (faq) + `transform-style: preserve-3d` sur les enfants concernés

### Architecture JS — `initSite()` v3

```js
function initSite(){
  initNav();
  initScrollProgress();
  initLenis();              // universal Lenis avec smoothTouch:false
  initSplitTitles();
  initSplitCta();
  initFades();
  initServiceVisuals();
  initOrbsParallax();       // universal

  // Cinematic universal (use mm.add() internally if needs desktop/mobile variant)
  initHeroPinned();         // pin scrub 120%/80%
  initProblems();
  initProfilesCarousel();   // snap detection works on touch too
  initProfiles();
  initServices();           // dual: desktop pin / mobile swipe
  initMethode();            // SVG line traced + step glow scrub
  initResults();            // scrubed counters
  initStack();              // clip-path circle + 3D hover desktop only
  initTesti();              // 3D tilt scrub
  initFaq();                // rotateX entry

  // Desktop polish only
  gsap.matchMedia().add(QUERY_DESKTOP, () => {
    document.body.classList.add('has-cursor');
    initCursor();
    initMagnetic();
    initTilt();             // hover-driven tilt (different from scrub tilt of testi)
  });

  if (reducedMotion) { /* nuke pins+scrubs+loops */ }
  ScrollTrigger.refresh();
}
```

### Performance — points de vigilance

- **`will-change: transform`** ajouté sur `.hero`, `.services-stack`, `.stack-grid`, `.stack-tile`
- **`transform-style: preserve-3d`** sur les containers 3D (stack-tile, testi-card via stack-grid/testi-grid)
- Le `scrub: 1` partout : si Lighthouse Mobile descend sous 75, baisser à `0.5` voire `true`
- Le zoom yoyo des 15 stack-tiles + le 3D tilt scrubed sur 3 testi-cards = ~18 tweens actifs simultanément. Sur smartphone milieu de gamme, devrait passer.

### Commits Phase 3

- (à pusher) — Phase 3 : Réinjection cinématique mobile-first (pin/scrub/reveals universels)

---

## PHASE 4 — Refonte contenu + structure (12 sections finales)

**Date** : Mai 2026 — 6 micro-commits (Option C, safety mode)
**Trafic cible** : 95% mobile, 3% tablet, 2% PC — décisions architecturées smartphone-first.
**Préparation vidéo** : tous les cards utilisent `rgba(10,14,26,.55–.62)` + `backdrop-filter: blur(18px) saturate(140%)` pour rester lisibles sur une vidéo de fond future (à brancher quand la longueur du site sera figée — ping 🎥).

### Numérotation finale des sections

Le Hero n'a **pas d'eyebrow numéroté**. Les autres sections suivent la séquence narrative naturelle. Tous les `.section-eyebrow` sont **masqués via CSS** (`display:none !important`) — la numérotation reste éditoriale et reversible.

| Eyebrow | Section | ID | État |
|---|---|---|---|
| — | Hero | `#hero` | Refonte textes (H1, sous-titre, micro-preuve count-up) |
| **01** | Le Constat | `#constat` | **NOUVEAU** — 4 blocs en carousel horizontal pinné |
| **02** | Notre Mission | `#mission` | **NOUVEAU** — 3 cartes plaque tournante 3D |
| **03** | Nos Services | `#services` | 5 piliers (3 → 5) avec Voix IA + Marketing en plus |
| **04** | Pour Qui | `#profils` | 6 profils (inchangé visuellement) |
| **05** | Notre Méthode | `#methode` | Layout cascade diagonale (escalier décalé) |
| **06** | Les Résultats | `#resultats` | 4 grid → 3 carousel scroll-snap + counter fr-FR |
| **07** | Notre Stack | `#stack` | Inchangé (constellation 15 logos zoom désynchronisé) |
| **08** | Témoignages | `#temoignages` | 3 grid → 6 roue 3D 60° apart |
| **09** | FAQ | `#faq` | 8 questions (ajout Q8 "Et si ça ne marche pas ?") |
| **10** | CTA Final | `#cta` | Refonte boutons → **formulaire visuel + datepicker natif** |
| — | Footer | — | Refonte 4 colonnes (À propos · Services · Ressources · Contact) |

### Sections nouvelles ou refondues — détails techniques

#### 01 · Le Constat (`#constat`)
- **Comportement** : pin de la section, le scroll vertical pilote la translation horizontale de `.constat-track` (4 blocs).
- **Focus carte centrée** : à chaque `onUpdate`, la carte la plus proche du centre viewport reçoit `.is-focus` (scale 1.04, border magenta, glow). Les autres passent à `opacity:.5`.
- **État initial** : blocs 1-2 visibles, 3-4 en teaser à droite.
- **JS** : `initConstat()` — `ScrollTrigger.create({ trigger: '.constat-pin', pin:true, scrub:1, end: distance*1.3 + 0.5vh })`.
- **Mobile** : flex-basis `min(78vw, 320px)`, padding réduit.
- **Reduced-motion** : track passe en `flex-wrap:wrap` + toutes cartes en `.is-focus`.

#### 02 · Notre Mission (`#mission`)
- **Comportement** : plaque tournante 3D — 3 cartes positionnées à `rotateY(0/120/240deg) translateZ(--mission-r)`, toutes groupées sur le même axe Y.
- **`backface-visibility:hidden`** : masque naturellement les cartes du dos.
- **Rotation totale** : `0° → -240°` sur 2.4 viewports de scroll (pin actif). Termine face à la 3ᵉ carte (`Au service de vos équipes`).
- **JS** : `initMission()` — `ScrollTrigger.create({ trigger: '.mission-pin', pin:true, scrub:1, onUpdate: rot = -240 * progress })`.
- **`--mission-r`** : 260px desktop, 240px tablet, 215px mobile ≤640, 195px ≤380.
- **Reduced-motion** : platter passe en flex column, cartes empilées.

#### 06 · Les Résultats (`#resultats`)
- **Avant** : grid de 4 cards avec compteurs.
- **Après** : 3 cards (1 800+ heures, 70+ solutions, 96% satisfaction) dans `.results-track` avec `scroll-snap-type:x mandatory`.
- **Compteur 1 800** : `data-format="thousand"` → formaté en `toLocaleString('fr-FR')` puis normalisé en espace standard.
- **Dots magenta** : 3 `.rdot`, le centré reçoit `.is-active` via IntersectionObserver-lite sur le scroll du track.
- **Phrase finale** : "Et ce ne sont pas des chiffres de plaquette. La grille de calcul est disponible sur demande."

#### 08 · Témoignages (`#temoignages`)
- **Avant** : 3 cards en grid avec 3D tilt scrubé.
- **Après** : 6 cards sur une **roue 3D cylindrique**, chacune à `rotateY(i*60deg) translateZ(--testi-r)`.
- **Sens de rotation** : scroll bas = sens horaire vu d'en haut = `rotationY: -300deg` au total (s'arrête à la 6ᵉ carte). Les cartes de droite viennent au centre.
- **`backface-visibility:hidden`** masque les cartes au dos pendant la rotation.
- **6 témoins** : Sofia L. (Salon), Vincent T. (Logistique), Camille D. (SaaS), Léa K. (E-commerce), Jean-Marc V. (Cabinet), Mehdi B. (Industrie).
- **KPI gradient** en headline de chaque carte (ex: "+35% de RDV hors heures d'ouverture").
- **6 dots magenta** synchro via `onUpdate` (carte active = `Math.round(progress * 5)`).
- **JS** : `initTesti()` — pin sur 3.2 viewports, scrub 1.
- **Reduced-motion** : stage devient stack vertical, perspective désactivée.

#### 10 · CTA Final (`#cta`)
- **Avant** : 2 boutons (mailto + lien temoignages).
- **Après** : formulaire visuel complet — Nom, Email, Téléphone, **Datepicker natif HTML5** (`<input type="datetime-local">` avec `color-scheme:dark`), Message (textarea optionnel).
- **Datepicker** : valeur par défaut = demain 14h00 (locale browser), `min` = maintenant.
- **Submit** : `preventDefault()` + feedback visuel (bouton → "Envoyé ✓" pendant 3.5s puis reset form).
- **Pas de backend** : c'est un formulaire visuel uniquement (placeholder UX).
- **JS** : `initCTAForm()` — entry stagger fade-up + datepicker init + submit handler.

### Architecture JS — `initSite()` v4 (Phase 4)

```js
function initSite(){
  initNav();
  initScrollProgress();
  initLenis();
  initSplitTitles();
  initSplitCta();
  initFades();
  initServiceVisuals();
  initOrbsParallax();

  initHeroPinned();
  initConstat();              // Phase 4 NEW : pin + horizontal scrub (4 blocs)
  initMission();              // Phase 4 NEW : pin + rotation Y groupée (3 cartes)
  initProfilesCarousel();
  initProfiles();
  initServices();             // 5 piliers (Phase 4 : était 3)
  initMethode();              // cascade diagonale (Phase 4)
  initResults();              // 3 carousel + dots + counter fr-FR (Phase 4)
  initStack();
  initTesti();                // 6 cards roue 3D (Phase 4 : était 3)
  initFaq();
  initCTAForm();              // Phase 4 NEW : datepicker + visual submit
  // ...
}
```

### Glassmorphism — préparation vidéo de fond

Toutes les cartes des sections sont configurées pour rester lisibles si une vidéo de fond est ajoutée plus tard :

- `background: rgba(10,14,26,.55)` à `.62` selon la densité texte
- `backdrop-filter: blur(18px) saturate(140%)` + préfixe `-webkit-`
- `border: 1px solid var(--line)` pour contraste
- Z-index strict : `video(0) → overlay(1) → content(5) → nav(1000)` (à appliquer quand la vidéo arrivera)

Quand la longueur définitive du site sera figée, intégrer la vidéo via :
```html
<video class="bg-video" autoplay muted playsinline loop>
  <source src="bg.mp4" type="video/mp4">
</video>
<div class="video-overlay"></div>
```
Avec ré-encodage ffmpeg keyframe-par-frame pour scrub fluide (commande à fournir).

### Commits Phase 4

1. **Phase 4 commit 1/6** — Textes Hero + Pour Qui + FAQ Q8 + Footer 4 blocs + CSS hide section-eyebrow + count-up micro-preuve Hero.
2. **Phase 4 commit 2/6** — Services 3 → 5 piliers (ajout Voix IA + Marketing) + `.service-text .example` avec gradient bar.
3. **Phase 4 commit 3/6** — Méthode cascade diagonale (`--step-offset` 0/13/26/39% desktop, 0/8/14/0% tablet, 0 mobile) + arrows diagonales `::before`.
4. **Phase 4 commit 4/6** — Résultats 4 grid → 3 carousel scroll-snap + counter `data-format="thousand"` fr-FR + 3 dots magenta.
5. **Phase 4 commit 5/6** — Témoignages 3 grid → 6 roue 3D cylindrique 60° apart, pin + scrub -300°, 6 dots.
6. **Phase 4 commit 6/6** — **Nouvelles sections 01 Le Constat (carousel horizontal pinné) + 02 Notre Mission (plaque tournante 3D) + CTA Final → formulaire visuel avec datepicker natif + renumérotation finale + doc CLAUDE.md**.

---

## PHASE 4.5 / 4.6 — Polish massif post-Phase 4 (13 mai 2026)

**Contexte** : Phase 4 livrée mais nombreuses imperfections en usage réel (95% mobile). Cette phase couvre une vingtaine de micro-commits qui finalisent l'expérience : refontes de sections clés, isolation des couleurs pour éviter les bugs `bg-clip:text` + SplitType, polish des animations d'entrée. **À la fin de cette phase, le site est jugé prêt pour intégration d'une vidéo de fond** (voir §"Préparation vidéo" plus bas).

### Modifications majeures par section

#### Hero (`#hero`)
- **Word-aware SplitType** : `new SplitType(line, { types: 'words, chars' })` au lieu de `'chars'` seul → empêche le wrap au milieu d'un mot (bug constaté quand le viewport devenait trop étroit pour une ligne)
- H1 réduit : `clamp(...140px max)` → `clamp(...92px max)`, plus `overflow:visible` sur `.line` + padding-top breathing
- Mobile menu : overflow corrigé, sous-titre passé en blanc lisible, ligne italique en blanc
- Sous-CTA mobile (`.hero-micro-proof`) : passe en **grid 3 colonnes** au lieu de flex wrap, gap 14×10px, font 10.5px, text-shadow pour visibilité sur glow
- Preloader : `initSite()` désormais appelé à `2.4s` au lieu de `onComplete` (3.45s) → entry hero démarre EN MÊME TEMPS que le clipPath reveal du preloader, supprime ~1s de temps mort
- Parallax scrub du pin Hero adouci (sub `opacity: 0.75` au lieu de 0.25, CTA `y: 18` au lieu de 40) → contenu reste lisible en mi-scrub

#### Constat (`#constat`) + Mission (`#mission`)
- Focus progress du Constat basé sur `self.progress` (pas sur la position viewport-center) → garantit que ça démarre TOUJOURS sur carte 1 puis 2/3/4 dans l'ordre
- Sous-titre Constat : couleur passe en blanc, carte mobile non coupée
- Sous-titres Constat + Mission : **suppression conflit `data-fade` vs init internes** — les deux initialisaient le même `<p>` avec stagger contradictoire → ajout de `clearProps:'transform,opacity'` après les anim d'entry
- Mission : sous-titre raccourci, "euros" → "argent" pour ouvrir le ROI au-delà du financier, cards mobile dans le viewport (radius 215→195 à ≤380)

#### Pour Qui (`#profils`) — refonte complète
**Abandon du scroll-snap horizontal** (commit `c4363ce`) → **deck 3D avec envol** :
- 6 cartes empilées en Z négatif, front card a `.is-front` (halo conic)
- Pin durant 5 transitions (5 × 60% vh)
- Scroll fait s'envoler la front card vers la gauche+haut (rotation Z + slide off) + la suivante remonte
- `initProfilesDeck()` remplace `initProfilesCarousel()` dans `initSite()` — l'ancien est gardé en dead code pour ref
- `.profiles-progress` : barre verticale gauche qui se remplit selon `self.progress`

#### Services (`#services`) — refonte mobile critique
Le user signale "9 swipes pour passer 1 carte" → carousel horizontal mobile cassé sur son téléphone.
- **Abandon du carousel horizontal mobile** → stack vertical natif (commit `1bc3dd6`)
- Tentative 1 : tilted card drop 3D scrubed → rejetée (incohérent entre piliers)
- Tentative 2 : tilted drop one-shot → bof
- **Final** : zigzag slide horizontal alterné (commit `fe0b11c`). P1/P3/P5 entrent depuis la gauche (`x: -100vw`), P2/P4 depuis la droite (`x: +100vw`). One-shot `top 85%` once.
- Desktop reste en horizontal pin scrubbed (snap entre panels)
- Padding desktop 100px pour clear navbar, mobile ultra-compact gap 18px (était 80px → cause principale des panels énormes mobile), titres -30%, visuals responsive, description clamp 4 lignes
- Navbar réordonnée pour matcher l'ordre du scroll réel (commit `bda2bb6`) : `Constat → Mission → Pour Qui → Services → ...` (Pour Qui passe avant Services dans le menu, cohérent avec ce qu'on voit en scrollant)

#### Méthode (`#methode`) — SURPRISE notebook flip 3D
- Avant : cascade diagonale (Phase 4) + SVG line traced (Phase 3)
- Phase 4.5 : **notebook flip 3D** (commit `1183601`) — chaque step se déroule depuis sa tranche supérieure (`rotateX:-90deg`, `transformOrigin: '50% 0%'`, `transformPerspective: 1200`) vers son état plat. Effet "tourner une page de carnet vertical". Pulse magenta `drop-shadow` sur le `.step-num` à 0.7s du début (quand la carte est presque ouverte).
- **Phase 4.5 final** (commit `db1e640`) : entrée **adoucie** — `rotationX: -55` (au lieu de -90), ajout `y: 30` (slide-up), duration 1.5s (au lieu de 1.1), ease `power2.out` (au lieu de power3.out), trigger `top 90%` (au lieu de 82%). Pulse num adouci : scale 1.05 + 14px glow + sine.out + delay 1.0s. Les cards ne "tapent" plus l'écran.

#### Résultats (`#resultats`) — REFONTE COMPLÈTE en roue 3D
Commits `2ada70d` → `7032030`. Plusieurs itérations.

**État final** :
- **Roue 3D axe X** (Ferris wheel / "roue de voiture") — pas axe Y. La rotation est autour d'un axe HORIZONTAL traversant l'écran, les cartes basculent verticalement par-dessus.
- Pattern miroir de Mission mais avec axe X : `.results-wheel-stage` (perspective 1500, height `clamp(500px,68vh,620px)`, `display:grid;place-items:center`) → `.results-wheel` (relative, `clamp(320px,84vw,440px)` × `clamp(340px,46vh,400px)`, `transform-style:preserve-3d`) → 3 `.result-card` en `inset:0`, transform `rotateX(var(--i)) translateZ(var(--results-r))`.
- Radius responsive : 240 desktop / 220 ≤1024 / 190 ≤640 / 170 ≤380
- Pin durant 2.4 vh, rotation `rotationX: 0 → -240°` sur progress 0→1 (3 cartes × 120°, finit face à carte 3)
- 3 dots magenta synchro via `Math.round(self.progress * (N-1))`
- `backface-visibility:hidden` masque les cartes au dos

**Compteurs ONE-SHOT (pas scrubés)** — point clé livré après plusieurs tentatives :
```js
document.querySelectorAll('.big-counter').forEach((el, i) => {
  const target = +el.dataset.target;
  const obj = { v: 0 };
  gsap.to(obj, {
    v: target,
    duration: 1.6 + i * 0.15,
    ease: 'power2.out',
    scrollTrigger: { trigger: '.results', start: 'top 75%', once: true },
    onUpdate: () => renderCounter(el, target, obj.v / target)
  });
});
```
Les 3 compteurs animent UNE FOIS quand la section entre dans le viewport (avant que le pin engage). Quand chaque carte arrive face caméra durant la rotation, son chiffre est **déjà à son target complet** (1800 / 70 / 96) — sinon le 1800 n'atteignait jamais sa cible avant que sa carte tourne hors champ (bug remonté plusieurs fois).

**Isolation des couleurs par enfant** (commit `399c24d`) — fix architectural :
- `.result-card .num` (parent) : **plus de bg-clip:text + color:transparent** au niveau parent (cassait l'affichage du `.unit` qui héritait `color:transparent`)
- `.result-card .num .big-counter` : bg-clip:text + gradient **isolé sur le chiffre seul**
- `.result-card .num .unit` : **solid `var(--pink)`** + `flex-shrink:0` + `display:inline-block` + `-webkit-text-fill-color:var(--pink)` — le `%` / `+` sont TOUJOURS visibles, jamais coupés
- Font reduit : `clamp(2.8rem, 6.5vw, 6.5rem)` au lieu de `4-10rem` pour que "1 800+" tienne dans la carte wheel (largeur clippée par `overflow:hidden`) + `flex-wrap:nowrap` + `white-space:nowrap`

**`.italic-glow` (titre H2 "n'importe qui")** — itéré 3 fois :
- ❌ Tentative 1 : `filter:drop-shadow(...)` + bg-clip:text → **filter casse bg-clip:text sur Chrome/Edge** (bug rendering connu), texte totalement invisible
- ❌ Tentative 2 : `text-shadow:0 0 22px ... 0 0 44px` heavy → halo gigantesque qui **engloutit** le texte gradient (blob flou magenta/violet)
- ✅ **Final** : `color: var(--pink)` solid + `text-shadow:0 0 8px rgba(214,51,132,.55)` léger
- + `.italic-glow .word { padding-right: 0.35em; padding-left: 0.08em; }` — fix du "i" coupé : SplitType met inline `overflow:hidden` sur chaque `.word` pour clipper le slide-up vertical, mais en italique le glyphe final (point ascenseur du "i" + slant) + le text-shadow halo dépassent à droite et se font couper. Le padding scoped donne de l'air avant le clip.

#### Stack (`#stack`) — refonte (commits `2a56d69` → `c8da569`)
**Avant** : 15 tuiles dans des cards glass avec background, border, aspect-ratio, conic gradient hover border, grid auto-fit, breathing scale 0.92-1.10.

**Après** :
- **Plus de cards autour des logos** : `.stack-tile` perd `background`, `border`, `aspect-ratio`, `border-radius`, `overflow:hidden`. `::before` → `display:none`. Mobile : `background:none` aussi.
- Les `.mark` (carrés colorés avec lettres) restent intacts — c'est l'identité visuelle de chaque tool
- **Hover** : remplace le conic border par `filter: drop-shadow(0 0 14px rgba(124,92,252,.5))` sur le `.mark`. Scale hover passe de 1.18 → 1.5 (plus d'espace puisqu'il n'y a plus de card)
- **Zoom amplifié** : `minScale: 0.78 + random*.07` / `maxScale: 1.20 + random*.12` → amplitude ~50% (au lieu de ~18%). Cycle 2.5-6.5s. Breathing **vraiment visible**.
- **Scatter random** : `gsap.set` avant l'entry sur chaque tile : `x: ±12px, y: ±14px, rotation: ±7°`. Les tiles sortent légèrement de leur cellule grid sans se disperser → effet "cloud rapproché". Le breathing tween ne touche QUE scale, donc scatter persiste pendant les zooms. Idem pour le hover (touche scale + rotationX + rotationY, pas x/y/rotationZ).
- **Conflict entry/breathing résolu** : avant le breathing démarrait à l'init en parallèle de l'entry → les deux tweens se battaient sur scale. Maintenant breathing démarre dans le `onComplete` de l'entry.
- **Desktop 5/5/5** (commit `c8da569`) : `@media (min-width:1025px) { .stack-grid { grid-template-columns: repeat(5,1fr); max-width: 720px; } }` → 3 rangées exactes de 5 (au lieu de 6×2 + 3 orphelines à gauche), cluster centré.
- Grid resserré : minmax 120→100 desktop, gap 16→8 desktop / 10→6 mobile

#### Témoignages (`#temoignages`) — spacing reduit
Le titre apparaissait à 1/3 du viewport à cause de `.section{padding:140px 0}` + `.testi-pin{padding:80px 0; justify-content:center; min-height:100svh}` (centre vertical sur 100svh).
- `.testi` override : `padding-top: 60px !important; padding-bottom: 0 !important;` (au lieu des 140 du `.section`)
- `.testi-pin` : `justify-content: flex-start` (au lieu de center), `padding: 20px 0 0` (au lieu de 80px 0)
- `.testi-head margin-bottom: 32px` desktop (au lieu de 48)
- Mobile (≤640) : encore tighter — `padding-top: 30px !important`, `padding-top: 0` sur pin, `margin-bottom: 18px` sous titre

#### CTA Final (`#cta`)
- **"gagner" du H2 visible** : `.cta-final h2 .grad` était `bg-clip:text + color:transparent` → même bug que `.italic-glow`. Solution : `background:none; color:#fff; -webkit-text-fill-color:#fff` (solid white). Plus de gradient mais lisible.
- **Espace bouton ↔ texte rassurance** : `.cta-form-rassurance margin-top` passe de `4px` → `56px`. Cause : `.btn-primary` a `box-shadow: 0 12px 40px rgba(124,92,252,.35)` qui projette un halo violet ~50-60px sous le bouton — le texte était DANS le halo. Avec flex-gap 18px du `.cta-form` parent, le total est 74px → clear complet.

#### Footer
- **Suppression de `.footer-mega`** (le "NEEXUS" géant tout au fond, font-size `clamp(80px, 18vw, 260px)` en bg-clip gradient blanc transparent). HTML retiré + règles CSS retirées (3 endroits : base, responsive override 80px, commentaire de remplacement).

### ❌ CSS `zoom` DESKTOP — TENTÉ ET REVERTED — NE PAS RÉESSAYER

Commits `53c20e0` (apply) → `df31f2b` (revert immédiat).

**Contexte** : utilisateur trouvait le site trop grand sur son écran principal, regardait le navigateur à 80% zoom dans les paramètres → demande "fais ça par défaut pour les autres utilisateurs".

**Tentative** :
```css
@media (min-width: 1025px) {
  html { zoom: 0.8; }
}
```

**Bugs critiques constatés** :
1. **Coordonnées de souris décalées** — hover sur le menu nav "Pour Qui" allume "Résultats" (1 cran d'écart). Le navigateur ne recalcule pas les coordonnées de pointer events quand `zoom` est appliqué à `html`. Connu sur Chrome/Edge.
2. **Viewport décalé à gauche** — `zoom: 0.8` rend le contenu à 80% du viewport mais ne change pas le viewport lui-même → 20% de vide à droite (et en bas). Section Méthode notamment montre une énorme zone vide.

**Leçon** : ne JAMAIS utiliser CSS `zoom` global pour "shrinker" le site. C'est non-standard et a des interactions buggy.

**Options viables pour atteindre l'objectif "site plus compact sur desktop"** (à explorer si l'envie revient) :
- A) Réduire `--container` 1340 → 1100px + paddings `.section` 140 → 100px
- B) Refactor des `clamp(min, vw, max)` de fonts globalement : shrinker tous les `max` de 20%
- C) Combinaison A + B
- D) Wrapper transform: scale avec gestion manuelle des fixed elements (complexe, faisable)

Aucune solution one-liner type CSS zoom n'existe — c'est du vrai travail. Ne pas re-tenter sans plan.

### Rappel critique — préparation vidéo de fond

(Voir aussi §"Glassmorphism" Phase 4 plus haut.)

**Toutes les sections sont déjà configurées pour qu'une vidéo de fond puisse être ajoutée sans casser la lisibilité** :
- Cards avec `background: rgba(10,14,26, .55-.62)` + `backdrop-filter: blur(18px) saturate(140%)` + `border: 1px solid var(--line)`
- Aucune section n'utilise un solid black opaque pour son fond — toutes utilisent les vars `--bg-0` à `--bg-3` (semi-transparents possibles avec rgba)
- Le z-index est prêt à recevoir : `video(0) → overlay(1) → orbs(0-fixed) → content(2-5) → nav(1000) → preloader(top)`

**Quand la vidéo arrivera** :
```html
<video class="bg-video" autoplay muted playsinline loop preload="metadata">
  <source src="bg.mp4" type="video/mp4">
</video>
<div class="video-overlay"></div>
```
+ CSS :
```css
.bg-video{
  position:fixed; inset:0; width:100%; height:100%;
  object-fit:cover; z-index:0; pointer-events:none;
}
.video-overlay{
  position:fixed; inset:0; z-index:1; pointer-events:none;
  background:linear-gradient(180deg, rgba(7,10,20,.55), rgba(7,10,20,.75));
}
```
**Pour scroll-motion vidéo** (le user souhaite que la vidéo réagisse au scroll) :
- Option simple : `position: fixed` sur la vidéo, GSAP `ScrollTrigger` qui scrub `currentTime` selon `scroll progress` global
- Option performante : ré-encoder la vidéo en keyframe-par-frame (`ffmpeg -i in.mp4 -g 1 -keyint_min 1 -c:v libx264 -crf 22 out.mp4`) pour scrub fluide sur tous browsers
- Tester sur mobile : iOS Safari refuse souvent le scrub vidéo programmatique → fallback vidéo en boucle classique sur mobile, scrub uniquement sur desktop
- Code de référence (à insérer dans `initSite()` au moment voulu) :
  ```js
  const video = document.querySelector('.bg-video');
  if (video && !reducedMotion) {
    video.pause();
    ScrollTrigger.create({
      trigger: 'body', start: 'top top', end: 'bottom bottom', scrub: 1,
      onUpdate: self => {
        if (video.duration) video.currentTime = video.duration * self.progress;
      }
    });
  }
  ```

### Apprentissages CSS à retenir pour Phase 5+

1. **`bg-clip:text` + `color:transparent` est HÉRITÉ par les enfants** — si parent a ces props, les enfants deviennent transparents et le gradient parent est censé couvrir leur zone, mais ce mécanisme casse souvent (SplitType nested word spans, mélange de font-sizes, navigateurs). **Préférer isoler le bg-clip:text au plus petit élément possible** (sur la `<span>` directement, pas sur son parent flex container). Donner aux enfants secondaires une `color` solide explicite via `color: ...; -webkit-text-fill-color: ...`.
2. **`filter: drop-shadow()` casse `background-clip:text`** sur Chrome/Edge. Le rendu compositing détruit le gradient text. → Utiliser `text-shadow` pour les halos sur du gradient text.
3. **`text-shadow` heavy halos (>15px blur) sur petit texte** engloutissent visuellement le texte lui-même. Garder les blurs ≤10px pour rester subtils.
4. **SplitType met `overflow:hidden` inline sur chaque `.word`** (pour le slide-up). Les italiques (slant + dots ascenseurs) et les text-shadow halos dépassent à droite et se font clipper. → Pour tout span en italique inside `[data-split]`, prévoir `padding-right: 0.3-0.4em` scoped sur ses `.word`.
5. **CSS `zoom` est non-standard** et a des bugs critiques (coordonnées souris, viewport offset). Ne pas l'utiliser pour shrinker globalement.
6. **`gap` flex est additif avec margin-top/bottom des items**. Espace réel = gap + margin (sur le côté concerné). Penser au `box-shadow` étendu des boutons (`.btn-primary` projette ~50-60px sous lui).
7. **Pattern 3D wheel** : pour qu'une rotation autour de X (Ferris wheel) fonctionne visuellement, le stage doit être TALL (clamp 500-620px). Pour Y (Lazy Susan), il doit être WIDE. Cards en `inset:0` du wheel (pas `translate(-50%,-50%)`), wheel `transform-style: preserve-3d`, stage `perspective: 1400-1500px`.

### Commits Phase 4.5 / 4.6

État final pushé : `df31f2b` (revert du zoom).

Approximativement 25 commits, dont les majeurs :
- Hero : `c6d4732`, `e26f29f`, `a00978e`, `7b6375e`, `892dc91`
- Constat/Mission : `8f514a3`, `fcc8328`, `1e7450d`
- Pour Qui deck 3D : `c4363ce`
- Navbar reorder : `bda2bb6`
- Services mobile : `1bc3dd6`, `0809c13`, `8fe6eb6`, `fe0b11c`, `9fe4468`, `d573f85`, `636d3d0`
- Méthode notebook flip : `1183601`
- Résultats roue 3D itérations : `2ada70d`, `d0bc58d`, `304d2d2`, `399c24d`, `7032030`
- Stack refonte : `2a56d69`, `c8da569`
- 4 fixes finaux (témoignages + gagner + rassurance + footer-mega) : `db1e640`, `dd02cfe`
- ❌ Zoom desktop tenté + reverted : `53c20e0` + `df31f2b`
