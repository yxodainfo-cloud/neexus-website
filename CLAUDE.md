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

## PHASE 2 — ENRICHISSEMENT (Mai 2026)

Ce site est désormais en Phase 2. Voir `BRIEF-ENRICHISSEMENT.md` pour le détail.

**Ajouts de sections :**
- Section "Pour Qui" (`#profils`, `03 Profils`) — 6 contextes clients avec carrousel snap horizontal sur desktop, stack vertical sur mobile (entre Problems et Services)
- Section "Notre Stack" (`#stack`, `06 Stack`) — 15 logos d'outils avec animation zoom in/out désynchronisé via `gsap.fromTo(...repeat:-1, yoyo:true)` par tuile, durées 3-7s aléatoires (entre Résultats et Témoignages)
- Section "FAQ" (`#faq`, `08 Questions`) — accordéon 7 questions, ouverture mutuellement exclusive (entre Témoignages et CTA Final)

**Renumérotation des sections existantes :**
- Méthode : 03 → 04
- Résultats : 04 → 05
- Témoignages : 05 → 07
- CTA Final : 06 → 09

**Navbar :** ajout du lien "FAQ" entre "Témoignages" et "Audit gratuit" (desktop + menu mobile).

**Ajustements de ton :**
- Renforcement systématique du principe directeur : *la techno augmente les équipes, elle ne les remplace pas*.
- Voir BRIEF-ENRICHISSEMENT.md §2 pour la liste précise des modifications de copy sur les sections existantes (Problems intro + P-01 + P-04, sous-titres Pilier 01 et 02 des Services, libellés Résultats 1 et 4 + footnote).

**Stack technique :** inchangée (HTML + GSAP + Lenis + SplitType, single-file, déploiement Vercel auto).

**Conventions à respecter pour toute Phase 3+ :**
- Réutiliser les design tokens existants dans `:root` (ne pas en ajouter d'autres sans raison)
- Suivre le pattern `gsap.matchMedia()` pour le split desktop (`QUERY_DESKTOP = '(min-width: 1025px)'`) vs mobile (`QUERY_MOBILE = '(max-width: 1024px)'`)
- Animations universelles dans `initSite()` directement, polish desktop dans le bloc `mm.add(QUERY_DESKTOP, …)`
- Toute animation en boucle infinie (style stack zoom) doit être désactivée si `prefers-reduced-motion: reduce`
- Sur mobile (≤ 768px), désactiver les `backdrop-filter` coûteux et les `::before` avec `conic-gradient` animés
