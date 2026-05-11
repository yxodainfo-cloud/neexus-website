# Brief d'enrichissement — Site Neexus (Phase 2)

**Pour Claude Code.** Ce document est le brief de la Phase 2 du site Neexus. La Phase 1 (site initial) est déjà déployée sur **neexusds.com**. Tu vas **enrichir** le fichier `index.html` existant sans le casser.

---

## 0 · TL;DR — Ce qu'on fait

1. **On garde tout l'existant** : la stack (HTML + GSAP + Lenis + SplitType), le design system, les sections déjà construites. Rien à reconstruire.
2. **On ajuste légèrement la copy** sur les sections existantes (ton "augmentation > remplacement", reformulations ciblées) — *changements minimes, listés en §2*.
3. **On ajoute 3 nouvelles sections** :
   - **Pour Qui** — 6 contextes clients (à insérer entre `#problems` et la section `#services`)
   - **Notre Stack** — 15 logos d'outils avec animation zoom in/out irrégulier (à insérer entre `#resultats` et `#temoignages`)
   - **FAQ** — 7 questions/réponses en accordéon (à insérer entre `#temoignages` et `#cta`)
4. **On met à jour `CLAUDE.md`** pour refléter les ajouts.

**Important :** suis les conventions de code déjà en place (variables CSS dans `:root`, sections CSS commentées avec les bannières `=====`, patterns GSAP `matchMedia` desktop/mobile, etc.). Ne reformate pas l'existant.

---

## 1 · État actuel inventorié

### Stack technique en place
- **Framework :** Aucun. Single-file HTML (`index.html` ≈ 2 000 lignes), CSS dans `<style>`, JS dans `<script>` à la fin du fichier.
- **Animations :** GSAP 3.12.5 + ScrollTrigger + ScrollToPlugin (via CDN cdnjs)
- **Smooth scroll :** Lenis 1.1.18 (via unpkg, desktop only)
- **Text splitting :** SplitType 0.3.4 (via unpkg)
- **Fonts :** Syne (display), DM Sans (body), JetBrains Mono (mono) — via Google Fonts
- **Déploiement :** GitHub → Vercel auto, hébergement Infomaniak/Vercel

### Design tokens en place (à NE PAS modifier — réutiliser tels quels)

```css
--bg-0:#070a14; --bg-1:#0a0e1a; --bg-2:#0f1424; --bg-3:#131830;
--line:rgba(255,255,255,.06); --line-strong:rgba(255,255,255,.12);
--text:#e8eaf0; --text-dim:#8b90a5; --text-mute:#5b607a;
--blue:#4d7cff; --purple:#7c5cfc; --magenta:#d63384; --pink:#e8548e;
--grad: linear-gradient(135deg,#4d7cff 0%,#7c5cfc 40%,#d63384 100%);
--grad-text: linear-gradient(90deg,#4d7cff 0%,#9b6dff 50%,#e8548e 100%);
--grad-soft: linear-gradient(135deg,rgba(77,124,255,.14) 0%,rgba(124,92,252,.10) 50%,rgba(214,51,132,.14) 100%);
--glass: rgba(255,255,255,.025); --glass-strong: rgba(255,255,255,.045);
--r-md:18px; --r-lg:28px; --container:1340px;
```

### Sections déjà en place (ordre actuel dans index.html)

| # | ID | Ligne approx. | Statut |
|---|-----|---------------|--------|
| 1 | `#hero` | 1046 | ✅ OK |
| 2 | `#problems` | 1097 | ✅ OK (ajustements mineurs §2.1) |
| 3 | `#services` (3 piliers horizontal scroll) | 1143 | ✅ OK (ajustements §2.2) |
| 4 | `#methode` | 1311 | ✅ OK |
| 5 | `#resultats` | 1389 | ✅ OK (ajustement chiffres §2.3) |
| 6 | `#temoignages` | 1430 | ✅ OK |
| 7 | `#cta` | 1485 | ✅ OK |
| 8 | `footer` | 1511 | ✅ OK |

### Patterns GSAP déjà en place (à RÉUTILISER pour les nouvelles sections)

- `gsap.matchMedia()` avec `QUERY_DESKTOP` pour gérer les variantes desktop/mobile
- `ScrollTrigger` pour les déclencheurs scroll
- `SplitType` sur les titres principaux avec `gsap.from(splitChars, { y, opacity, stagger })`
- Fades génériques `.fade-up` avec `gsap.from(els, { y: 40, opacity: 0, stagger: .08, scrollTrigger })`
- Boutons magnétiques (curseur s'approche du bouton)
- 3D tilt sur cartes (desktop only)
- Orbs parallax desktop only

---

## 2 · Ajustements à faire sur l'existant

### 2.1 · Section `#problems` (4 pain points)

**Objectif :** ajuster le ton pour qu'il insiste sur le poids sur les équipes plutôt que sur la techno qui remplacerait. Garder la structure HTML, juste mettre à jour les textes des 4 cartes.

**Trouver et remplacer :**

| Trouve | Remplace par |
|--------|--------------|
| « Si vous reconnaissez ces situations, on peut vous aider. » | **« Si vous reconnaissez ces situations, vos équipes méritent mieux. »** |
| « Ces frictions coûtent en moyenne **28h par semaine** à une PME. » (intro) | **« Ces frictions coûtent en moyenne 28h par semaine à une PME. Et surtout, elles épuisent les équipes qui les subissent au quotidien. »** |

**Pain Point P-01 (Tâches répétitives) — description, remplacer par :**
> Votre équipe passe **60% de son temps** sur des tâches qui ne génèrent aucun chiffre d'affaires. Saisie manuelle, copier-coller entre outils, relances, devis — autant d'heures qui devraient servir à autre chose.

**Pain Point P-04 (Pas le temps) — description, remplacer par :**
> Trop occupé à faire tourner le quotidien pour penser à demain. La transformation digitale reste un projet « qu'on lancera bientôt »… depuis trois ans. Pendant ce temps, vos meilleurs éléments s'épuisent.

(P-02 et P-03 sont OK, ne pas toucher.)

### 2.2 · Section `#services` — 3 piliers (ajustements de copy)

**On garde les 3 piliers** (IA & Agents / Automatisation & Workflows / Présence Digitale) — c'est la structure mémorisable du site, on n'y touche pas.

**Petits ajustements de description pour aligner sur le principe directeur "augmentation > remplacement" :**

**Pilier 01 — IA & Agents Intelligents — sous-titre, remplacer par :**
> Des agents conversationnels qui prennent en charge le répétitif, qualifient, et passent la main à vos équipes pile au bon moment — 24h/24, dans toutes les langues, sans pause café.

**Pilier 02 — Automatisation & Workflows — sous-titre, remplacer par :**
> On connecte vos outils existants pour qu'ils travaillent ensemble. Plus de double saisie, plus d'oublis. Vos équipes retrouvent du temps pour ce qui compte vraiment.

**Pilier 03 — Présence Digitale — sous-titre :** OK tel quel.

### 2.3 · Section `#resultats` — chiffres

Garder les compteurs existants, mais aligner les libellés sur le nouveau cadrage chiffré :

| Chiffre | Libellé actuel | Nouveau libellé |
|---------|---------------|-----------------|
| Économisées | « Économisées par mois » | **« Heures rendues par mois aux équipes »** *(reformulation pour insister sur le gain humain plutôt que purement comptable)* |
| Leads qualifiés | OK tel quel | — |
| Satisfaction client | OK tel quel | — |
| Temps de réponse | « Temps de réponse moyen » | **« Temps de première réponse, tous canaux »** |

**Ajouter en bas du bloc chiffres une mention :**
> *Chiffres mesurés sur l'ensemble des projets livrés. Grille de calcul disponible sur demande.*

### 2.4 · FAQ existante dans l'`index.html` ?

Il n'y en a pas. → voir §5 (nouvelle section FAQ à ajouter).

---

## 3 · NOUVELLE SECTION — "Pour Qui"

### Position dans le flux

Insérer **entre `#problems` (fin ligne ~1141) et `#services` (début ligne ~1143)**. Logique : on agite la douleur (problems) → on cible le profil (pour qui) → on présente la solution (services).

### Wireframe / structure

**Layout desktop :** carrousel horizontal snap, **carte centrale plus grande** (les 2 latérales scaled à 0.85 et désaturées à 60%).

**Layout mobile :** stack vertical classique des cartes l'une sous l'autre (pas de carrousel — trop lourd).

### Copy à insérer (mot pour mot)

**Bandeau numéro de section (en haut, mono) :**
`07 Profils`

**H2 :**
**Là où la différence se voit le plus.**

**Sous-titre (paragraphe d'intro) :**
Voici quelques contextes que nous connaissons bien — ceux où nos clients ont retrouvé du temps, de la sérénité, et souvent du chiffre. Si le vôtre n'y figure pas exactement, c'est sans doute parce qu'il mérite une étude qui lui ressemble.

### Les 6 cartes (à structurer en `<article class="profile-card">` ou similaire)

Pour chaque carte, structure : *Étiquette secteur · Titre · Le quotidien · Ce qu'on déploie*

---

**Carte 1**
**Étiquette :** Commerces & proximité
**Titre :** Restaurants, salons, cliniques
**Le quotidien :** Vous ratez des appels pendant que vous êtes en service. Donc des réservations. Donc du chiffre.
**Ce qu'on déploie :** Un agent vocal IA qui prend le relais quand personne ne peut décrocher. Il enregistre les réservations, gère les annulations, envoie les rappels. Branché à votre agenda. Votre équipe ne lève plus le nez du client en face d'elle.

---

**Carte 2**
**Étiquette :** PME & grandes entreprises
**Titre :** Scaler sans s'épuiser
**Le quotidien :** Vos équipes plafonnent. Embaucher coûte cher, prend du temps, et ne résout pas le fond du sujet.
**Ce qu'on déploie :** Des automatisations sur vos workflows internes (RH, comptabilité, achats, reporting). Vos équipes redeviennent disponibles pour la stratégie, la décision, la relation. La croissance ne se paie plus en burnout.

---

**Carte 3**
**Étiquette :** Support client
**Titre :** Services SAV débordés
**Le quotidien :** 80% de vos tickets sont les mêmes 10 questions. Vos agents s'essoufflent à répéter, et les vraies urgences attendent leur tour.
**Ce qu'on déploie :** Un assistant IA multicanal qui absorbe le volume répétitif et alimente votre base de connaissance en continu. Vos agents reprennent la main sur les cas qui ont besoin d'eux — avec tout le contexte déjà préparé.

---

**Carte 4**
**Étiquette :** E-commerce
**Titre :** Monétiser l'audience qu'on a déjà
**Le quotidien :** Coût d'acquisition qui grimpe, panier moyen qui stagne, paniers abandonnés qui ne reviennent jamais.
**Ce qu'on déploie :** Automatisation des relances, recommandations personnalisées, assistant pré-achat, segmentation comportementale. Vous monétisez intelligemment l'audience que vous avez déjà conquise.

---

**Carte 5**
**Étiquette :** B2B & fidélisation
**Titre :** Fidéliser plutôt que courir après
**Le quotidien :** Vous courez après les nouveaux clients pendant que les actuels glissent doucement vers la concurrence.
**Ce qu'on déploie :** Programmes de fidélisation automatisés, détection prédictive des signaux de churn, parcours de réactivation personnalisés. Vous augmentez le CA sans dépenser un euro en acquisition.

---

**Carte 6**
**Étiquette :** Opérations
**Titre :** Équipes débordées
**Le quotidien :** Vos meilleurs éléments passent leur journée sur des tâches qu'ils détestent. Et c'est souvent pour ça qu'ils partent.
**Ce qu'on déploie :** Audit de la journée type, identification des 30% de tâches automatisables, déploiement progressif. Ils retrouvent leur temps. Leur motivation. Et la capacité de faire ce pour quoi vous les avez recrutés.

---

### Phrase de bascule (sous le carrousel, centrée, sobre)

> **Aucun de ces contextes ne ressemble exactement au vôtre ?**
> C'est souvent un bon signe — ça veut dire qu'il y a probablement des leviers spécifiques à votre métier qu'on n'a pas encore explorés ensemble. **C'est exactement pour ça que l'audit existe.**

### Animations GSAP pour cette section

**Entrée (au scroll trigger) :**
```js
// Titre + intro
gsap.from('.profiles .section-num, .profiles h2 .char', { ... }); // pattern SplitType habituel
gsap.from('.profiles .intro', { y: 30, opacity: 0, duration: .8, scrollTrigger: { trigger: '.profiles', start: 'top 70%' } });

// Cartes
gsap.from('.profile-card', {
  y: 60, opacity: 0, scale: .96,
  stagger: .08,
  duration: .9, ease: 'power3.out',
  scrollTrigger: { trigger: '.profiles-carousel', start: 'top 75%' }
});
```

**Carrousel snap horizontal (desktop) :**
Implémenter avec `scroll-snap-type: x mandatory` sur le conteneur + `scroll-snap-align: center` sur chaque carte. La carte centrale est détectée via `IntersectionObserver` et reçoit la classe `.is-active` (scale 1, full color), les autres ont scale 0.85 et `filter: saturate(.6) brightness(.7)`.

**Mobile :** désactiver le snap horizontal, passer les cartes en `flex-direction: column`.

**Hover sur une carte latérale (desktop) :**
```js
gsap.to(card, { scale: .92, filter: 'saturate(.85) brightness(.85)', duration: .35 });
```

---

## 4 · NOUVELLE SECTION — "Notre Stack"

### Position dans le flux

Insérer **entre `#resultats` (fin ligne ~1426) et `#temoignages` (début ligne ~1430)**. Logique : on a montré les résultats, on crédibilise la tech derrière, puis on enchaîne sur les preuves humaines (témoignages).

### Wireframe / structure

**Layout :** grille fluide de 15 tuiles (logos d'outils), en `grid-template-columns: repeat(auto-fit, minmax(120px, 1fr))`. Sur desktop, environ 5-6 colonnes ; sur mobile, 3 colonnes.

Chaque tuile : un carré arrondi (`rounded-3xl` ≈ var(--r-lg)), fond très léger `var(--glass)`, bordure subtile `var(--line)`, padding pour centrer le logo dedans.

### Copy à insérer

**Bandeau numéro de section :**
`08 Stack`

**H2 :**
**Le meilleur de l'IA. Au service de votre business.**

**Sous-titre :**
On ne vous vend pas un outil. On choisit le bon pour chaque mission, parmi les meilleurs du marché. Voici ceux qu'on déploie le plus souvent.

### Les 15 outils à intégrer

Pour chaque outil, prévoir le logo SVG (récupérer depuis le site officiel ou brandfetch.com) + nom affiché en `data-tooltip` ou en libellé sous le logo (texte mono, taille petite, `var(--text-dim)`).

| # | Nom affiché | Description tooltip |
|---|-------------|---------------------|
| 1 | Claude | IA conversationnelle de raisonnement avancé (Anthropic) |
| 2 | ChatGPT | Génération de contenu et assistants (OpenAI) |
| 3 | Gemini | IA multimodale texte/image/vidéo (Google) |
| 4 | n8n | Plateforme d'automatisation open-source |
| 5 | Make | Automatisation no-code de bout en bout |
| 6 | Zapier | Intégrations entre 6 000+ applications |
| 7 | ElevenLabs | Voix IA ultra-réalistes en 30+ langues |
| 8 | Vapi | Agents vocaux IA en temps réel |
| 9 | Retell AI | Conversations vocales naturelles |
| 10 | Voiceflow | Conception de chatbots et agents vocaux |
| 11 | Cursor | Développement assisté par IA |
| 12 | Midjourney | Génération d'images qualité studio |
| 13 | HeyGen | Vidéos avec avatars IA |
| 14 | Ionos | Hébergement et infrastructure cloud |
| 15 | Notion AI | Productivité et knowledge base augmentée |

### Animation phare — Zoom in/out lent et irrégulier (LE point clé client)

Chaque logo doit "**respirer**" à son propre rythme, **désynchronisé** des autres. Effet "constellation vivante".

**Implémentation GSAP :**
```js
// Pour chaque .stack-tile, génère son propre cycle
gsap.utils.toArray('.stack-tile').forEach((tile, i) => {
  const minScale = 0.92 + Math.random() * 0.02;     // 0.92 → 0.94
  const maxScale = 1.04 + Math.random() * 0.06;     // 1.04 → 1.10
  const duration = 3 + Math.random() * 4;            // entre 3s et 7s
  const delay = Math.random() * 2;                   // décalage au démarrage 0-2s

  gsap.to(tile, {
    scale: maxScale,
    duration,
    delay,
    repeat: -1,
    yoyo: true,
    ease: 'sine.inOut'
  });
});
```

**Entrée au scroll :**
```js
gsap.from('.stack-tile', {
  scale: 0.6, opacity: 0,
  stagger: { each: 0.05, from: 'random' },  // apparition désynchronisée
  duration: .8, ease: 'back.out(1.4)',
  scrollTrigger: { trigger: '.stack-grid', start: 'top 75%' }
});
```

**Parallax souris (desktop bonus) :**
Léger décalage des tuiles dans la direction opposée au curseur (max 8px de déplacement), via un mousemove listener sur le conteneur.

**Hover (desktop) :**
La tuile **s'arrête de pulser** (`gsap.killTweensOf(tile); gsap.to(tile, { scale: 1.15, duration: .35 })`), bordure devient gradient, tooltip avec nom + description apparaît au-dessus.

**Mobile :** zoom in/out conservé (essentiel pour le rendu). Pas de parallax souris. Tap = tooltip brief.

---

## 5 · NOUVELLE SECTION — "FAQ"

### Position dans le flux

Insérer **entre `#temoignages` (fin ligne ~1481) et `#cta` (début ligne ~1485)**. Logique : après les preuves humaines, on lève les dernières objections avant le CTA final.

### Wireframe / structure

**Layout :** accordéon vertical classique. Container en `max-width: 880px; margin: 0 auto;`. Chaque item est une ligne :
- Question à gauche (`Syne 500`, taille `clamp(1.05rem, 1.5vw, 1.3rem)`)
- Icône `+` à droite (qui devient `×` à l'ouverture, via rotation CSS)
- Réponse en dessous, repliée par défaut (`height: 0; overflow: hidden`)
- Séparateur fin entre items (`border-bottom: 1px solid var(--line)`)

### Copy à insérer

**Bandeau numéro de section :**
`09 Questions`

**H2 :**
**Les questions qu'on nous pose toutes les semaines.**

**Sous-titre (court) :**
Si la vôtre n'y est pas, écrivez-nous — on répond dans la journée.

### Les 7 questions/réponses

---

**Q1 — Combien ça coûte ?**
> Ça dépend de ce qu'on déploie. Un agent vocal IA pour un commerce démarre à quelques centaines d'euros par mois. Une refonte complète de workflow B2B, c'est un projet. Dans tous les cas, on ne lance rien tant que le ROI n'est pas chiffré et accepté par vous.

---

**Q2 — Combien de temps avant de voir des résultats ?**
> Les premières automatisations sont en production en 2 à 4 semaines. Les premiers gains chiffrables, en 30 à 60 jours. Vous ne payez pas pour attendre.

---

**Q3 — Est-ce que je dois changer mes outils ?**
> Non. On s'intègre à ce qui existe : votre CRM, votre ERP, votre site, vos messageries. Si un outil est vraiment limitant, on vous le dit. Mais c'est rare.

---

**Q4 — Vos solutions vont-elles remplacer mes équipes ?**
> Non, et c'est même tout le contraire. Notre rôle est de retirer aux équipes ce qui les épuise sans valeur ajoutée. Vos collaborateurs gardent leur poste, gagnent du temps, et remontent en compétence sur ce qui compte vraiment.

---

**Q5 — Mes équipes vont devoir apprendre quoi ?**
> Le minimum. Notre objectif, c'est que vos équipes utilisent moins d'outils, pas plus. On forme les bons interlocuteurs, on documente tout, on reste disponibles.

---

**Q6 — Et la confidentialité de mes données ?**
> On travaille en RGPD strict, avec des solutions hébergées en Europe quand le client le demande, et des accords de confidentialité dès le premier échange. Vos données ne nourrissent jamais un modèle public.

---

**Q7 — Vous travaillez avec quelle taille d'entreprise ?**
> De l'indépendant qui rate trop d'appels au groupe avec 500 collaborateurs. La méthode s'adapte. Le ROI doit être là dans tous les cas.

---

### Animations GSAP pour la FAQ

**Entrée (au scroll trigger) :**
```js
gsap.from('.faq h2 .char', { ... }); // split habituel
gsap.from('.faq-item', {
  y: 24, opacity: 0,
  stagger: .06,
  duration: .6,
  scrollTrigger: { trigger: '.faq-list', start: 'top 80%' }
});
```

**Ouverture/fermeture d'un item (vanilla JS + GSAP) :**
```js
document.querySelectorAll('.faq-item').forEach(item => {
  const trigger = item.querySelector('.faq-question');
  const answer = item.querySelector('.faq-answer');
  const icon = item.querySelector('.faq-icon');

  trigger.addEventListener('click', () => {
    const isOpen = item.classList.toggle('is-open');
    gsap.to(answer, {
      height: isOpen ? 'auto' : 0,
      duration: .45,
      ease: 'power3.inOut'
    });
    gsap.to(icon, {
      rotate: isOpen ? 45 : 0,
      duration: .35,
      ease: 'power2.out'
    });
  });
});
```

**Hover sur une question fermée :**
Fond passe de transparent à `var(--grad-soft)` via CSS `transition: background .35s`.

---

## 6 · Renuméroter les sections existantes

Avec l'ajout de 3 sections, les **numéros affichés en haut de chaque section** doivent être ajustés. Plan :

| Section | Numéro actuel | Nouveau numéro |
|---------|--------------|----------------|
| Hero | `000` (logo "N° 001 — SDS LLC" / pas vraiment numéroté) | **inchangé** |
| Problems | `01 Pain Points` | **`01 Pain Points`** (inchangé) |
| Services | `02 Services` | **`02 Services`** (inchangé) |
| **Pour Qui** | — | **NOUVEAU : `03 Profils`** |
| Méthode | `03 Méthode` | **`04 Méthode`** |
| Résultats | `04 Résultats` | **`05 Résultats`** |
| **Stack** | — | **NOUVEAU : `06 Stack`** |
| Témoignages | `05 Témoignages` | **`07 Témoignages`** |
| **FAQ** | — | **NOUVEAU : `08 Questions`** |
| CTA Final | `06 Et maintenant ?` | **`09 Et maintenant ?`** |

**Attention :** mettre à jour aussi les liens d'ancres dans la navbar et le footer si besoin.

---

## 7 · Navbar — ajouter les nouvelles ancres

La navbar actuelle contient : Services · Méthode · Résultats · Témoignages · Audit gratuit.

**Décision :** on **n'ajoute pas** Pour Qui / Stack / FAQ dans la nav principale (trop chargé). On les laisse découvrables au scroll. Mais si tu veux les rendre accessibles, **un seul ajout** est judicieux : **« FAQ »** entre **Témoignages** et **Audit gratuit**.

À ta discrétion — si tu juges la navbar trop chargée avec FAQ en plus, laisse comme c'est.

---

## 8 · Mettre à jour CLAUDE.md

À la fin du fichier `CLAUDE.md` existant, ajouter une section :

```markdown
---

## PHASE 2 — ENRICHISSEMENT (Mai 2026)

Ce site est désormais en Phase 2. Voir `BRIEF-ENRICHISSEMENT.md` pour le détail.

**Ajouts de sections :**
- Section "Pour Qui" — 6 contextes clients (entre Problems et Services)
- Section "Notre Stack" — 15 logos d'outils avec animation zoom in/out désynchronisé (entre Résultats et Témoignages)
- Section "FAQ" — accordéon 7 questions (entre Témoignages et CTA Final)

**Ajustements de ton :**
- Renforcement systématique du principe directeur : *la techno augmente les équipes, elle ne les remplace pas*.
- Voir BRIEF-ENRICHISSEMENT.md §2 pour la liste précise des modifications de copy sur les sections existantes.

**Stack technique :** inchangée (HTML + GSAP + Lenis + SplitType, single-file, déploiement Vercel auto).
```

---

## 9 · Workflow de déploiement

Identique à la Phase 1 :

```bash
git add .
git commit -m "Phase 2 — Ajout sections Pour Qui, Stack, FAQ + ajustements copy"
git push
```

Vercel redéploie automatiquement neexusds.com.

**Avant de pusher,** teste en local :
1. Ouvrir `index.html` dans Chrome → vérifier que tout fonctionne (animations, responsive desktop + mobile via DevTools)
2. Vérifier qu'il n'y a **aucune régression** sur l'existant
3. Vérifier les 3 nouvelles sections (Pour Qui carrousel, Stack zoom, FAQ accordéon)
4. Vérifier qu'aucune erreur n'apparaît dans la console
5. Lighthouse ≥ 90 sur Performance, 100 sur Accessibility

---

## 10 · Checklist de livraison

- [ ] Section `#problems` : 2 textes ajustés (P-01, P-04, intro)
- [ ] Section `#services` : sous-titres Pilier 01 et 02 ajustés
- [ ] Section `#resultats` : libellés 1 et 4 ajustés + mention en bas
- [ ] Section `#profils` **AJOUTÉE** entre Problems et Services (6 cartes + carrousel snap + animations GSAP)
- [ ] Section `#stack` **AJOUTÉE** entre Résultats et Témoignages (15 logos + zoom irrégulier + entrée stagger random)
- [ ] Section `#faq` **AJOUTÉE** entre Témoignages et CTA Final (accordéon 7 items)
- [ ] Numéros de section affichés mis à jour (cf. §6)
- [ ] Navbar : décision prise sur ajout éventuel "FAQ"
- [ ] `CLAUDE.md` mis à jour avec la section Phase 2
- [ ] 15 logos d'outils récupérés et placés dans un dossier (ex : `/assets/tools/`)
- [ ] Test local sans régression
- [ ] Commit + push + déploiement Vercel auto

---

## 11 · Notes importantes

1. **Ne touche pas** au preloader, au curseur custom, à la scroll progress bar, aux glow orbs, au grain — tout ça est déjà parfaitement en place.
2. **Ne réécris pas** les sections existantes — fais uniquement les modifications listées en §2.
3. **Réutilise** les classes et patterns CSS/JS existants au maximum. Si tu crées des classes nouvelles (`.profile-card`, `.stack-tile`, `.faq-item`), nomme-les en cohérence avec les conventions du fichier.
4. **`prefers-reduced-motion`** : assure-toi que les animations en boucle (zoom irrégulier de la stack notamment) se coupent quand cette préférence est active.
5. **Mobile** : si une animation est trop lourde (zoom irrégulier sur 15 logos peut consommer), réduis sa fréquence ou son amplitude, mais ne la supprime pas.
6. **Demande confirmation** avant de pusher si tu as des doutes sur un choix d'implémentation.

---

**Bonne phase 2.** Le site existant est déjà excellent — l'objectif ici est de **l'enrichir sans le casser**, et d'ajouter les éléments qui manquent pour qu'il soit complet.
