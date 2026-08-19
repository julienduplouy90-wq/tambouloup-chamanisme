# Tambouloup — site one-page

Site vitrine d'une page pour l'atelier **Initiation aux pratiques chamaniques**
(Alexandre Godgenger — Au Mélilot, chemin des Humas, 65200 Gerde).

En ligne : https://julienduplouy90-wq.github.io/tambouloup-chamanisme/

## Structure

```
index.html                    la page entière : HTML + CSS + JS intégrés
assets/fonts/*.woff2          polices auto-hébergées (sous-ensemble latin)
assets/logo.png               ← à déposer (le loup au tambourin)
```

Aucun build, aucune dépendance, aucun appel à un domaine tiers.

## Parti pris performance

- **1 seule requête pour la page** : CSS et JS sont intégrés dans `index.html`
  (≈ 28 Ko non compressé, ≈ 7 Ko une fois servi en gzip/brotli). Sur un fichier
  de cette taille, supprimer deux allers-retours réseau rapporte bien plus que
  minifier — le code reste donc lisible et modifiable.
- **Polices auto-hébergées et préchargées** (`rel="preload"`), sous-ensemble latin
  uniquement, `font-display:swap` : plus de DNS + TLS vers `fonts.googleapis.com`
  et `fonts.gstatic.com`, plus de CSS tierce bloquant le rendu. 3 fichiers, 79 Ko.
  Outfit est une police variable : un seul fichier couvre les graisses 300 → 600.
- **Zéro écouteur de scroll.** L'en-tête collant et les apparitions au défilement
  passent par `IntersectionObserver` (callbacks hors du chemin critique).
- **Rayons du hero en `repeating-conic-gradient`** au lieu de 60 balises SVG
  générées en JS : rien à construire au chargement, animation composée par le GPU.
- **Peintures allégées** : plus de `filter:blur()` sur le halo (le dégradé est déjà
  doux), `backdrop-filter` conservé uniquement sur la barre de navigation,
  `contain:layout paint` sur les cartes.
- **Décors mis en pause** dès que le hero sort de l'écran (`animation-play-state`).
- **Pas de décalage de mise en page** : le logo a des dimensions et un
  `aspect-ratio` réservés, même quand l'image est absente.
- `prefers-reduced-motion` respecté ; sans JavaScript, tout le contenu reste visible.

Mesuré en local : 4 requêtes au total, `DOMContentLoaded` ≈ 55 ms, chargement complet ≈ 470 ms.

## Le logo

Déposez le fichier du logo dans `assets/logo.png` : il s'affiche automatiquement
dans le hero (l'encre noire est inversée en clair pour le fond nuit).
Tant que le fichier est absent, un emblème SVG de secours (lune + loup stylisé)
prend sa place — aucune image cassée n'apparaît.

## Direction artistique

- **Nuit de braise** `#12100E` + **ocre** `#C9702B` + **or** `#E9BC63` + **parchemin** `#F6EDDD`, touche de **sauge** `#7E8E6C`
- Titres en Cormorant Garamond, textes en Outfit
- Grain de papier animé, halo qui respire, rayons du soleil en rotation lente
- Angles très arrondis, ombres douces, apparitions en cascade au défilement

## Contenu à compléter

- Les **dates** ne sont pas fixées (« à définir ultérieurement ») — à remplacer dans la section *Infos pratiques* et dans le déroulé dès qu'elles le sont.
- Le **déroulé en quatre passages** est une mise en récit de l'objectif indiqué sur l'affiche : à valider ou ajuster avec Alexandre.
- Possible ajout : photos du lieu, formulaire de contact, page mentions légales.

## Mise en ligne

Site 100 % statique servi par GitHub Pages depuis la branche `main`.
N'importe quel autre hébergement fonctionne également (Netlify, OVH…).
