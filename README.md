# Tambouloup — site one-page

Site vitrine d'une page pour l'atelier **Initiation aux pratiques chamaniques**
(Alexandre Godgenger — Au Mélilot, chemin des Humas, 65200 Gerde).

En ligne : https://julienduplouy90-wq.github.io/tambouloup-chamanisme/

> **Indexation désactivée pour le moment.** Le site est en ligne mais volontairement
> `noindex` (balise meta dans `index.html` + `robots.txt`) tant qu'Alexandre n'a pas
> validé le déroulé de l'atelier et les dates. Pour l'ouvrir aux moteurs :
> supprimer la balise `<meta name="robots">` et vider le `Disallow:` de `robots.txt`.

## Structure

```
index.html                    la page entière : HTML + CSS + JS intégrés
robots.txt                    blocage d'indexation temporaire
assets/logo.webp              logo détouré, servi en priorité (58 Ko)
assets/logo.png               même logo en PNG : repli navigateurs sans WebP,
                              og:image et favicon (90 Ko)
assets/fonts/*.woff2          polices auto-hébergées (sous-ensemble latin)
```

Aucun build, aucune dépendance, aucun appel à un domaine tiers.

## Direction artistique — « encre & papier »

La direction part des supports réels du client : un **dessin à l'encre noire** (le logo)
et une **affiche sur papier ancien**. Le site est donc du papier, pas une nuit étoilée.

- **Papiers** : `#EFE6D6` (fond), `#F7F1E7` (encarts), `#E2D5BE` (ombre de page)
- **Encres** : `#181411`, `#4A403A`, `#8A7C6E`
- **Accents tirés de l'affiche** : ocre `#A75B22`, or `#D9A441`
- Titres en **Cormorant Garamond** (le serif de l'affiche), textes en **Outfit** ;
  le mot « TAMBOULOUP » n'est jamais retypographié — c'est le logo lui-même.
- **Le logo est le hero** : présenté grand, sans filtre ni fond, avec un halo doré discret.
- **Filets doubles et fleurons** (`◆◆◆`) en séparateurs, comme un placard imprimé.
- **Une seule section à l'encre** : *Le déroulé*. L'inversion n'est pas décorative —
  c'est la partie « voyage intérieur » du week-end. La section contact reprend cette
  encre pour fermer la page.
- Grain de papier animé, apparitions en cascade, `prefers-reduced-motion` respecté.

## Le logo

`assets/logo.*` est généré à partir de la photo du dessin original : redimensionné à
760 px, **détouré** (la valeur de gris devient le canal alpha) et recoloré en encre
`#181411`. Il se pose donc sur n'importe quel fond, sans le carré blanc d'origine.
Le poids (58 Ko) tient presque entièrement au canal alpha : baisser la qualité WebP
ne change rien, c'est déjà le minimum pour ce niveau de détail.

Pour le régénérer depuis une source plus propre (un vrai fichier vectoriel ou un PNG
haute définition), le principe est : `greyscale → negate → linear(1.45, -28)` comme
masque alpha, appliqué sur un aplat couleur encre.

## Parti pris performance

- **1 seule requête pour la page** : CSS et JS sont intégrés dans `index.html`
  (≈ 29 Ko non compressé, ≈ 9 Ko une fois servi en gzip/brotli). Sur un fichier
  de cette taille, supprimer deux allers-retours réseau rapporte bien plus que
  minifier — le code reste donc lisible et modifiable.
- **Polices auto-hébergées et préchargées** (`rel="preload"`), sous-ensemble latin
  uniquement, `font-display:swap` : plus de DNS + TLS vers `fonts.googleapis.com`
  et `fonts.gstatic.com`, plus de CSS tierce bloquant le rendu. 3 fichiers, 79 Ko.
  Outfit est une police variable : un seul fichier couvre les graisses 300 → 600.
- **Logo préchargé en WebP** via `<picture>`, avec dimensions et `aspect-ratio`
  réservés : aucun décalage de mise en page pendant le chargement.
- **Zéro écouteur de scroll.** L'en-tête collant et les apparitions au défilement
  passent par `IntersectionObserver` (callbacks hors du chemin critique).
- **Décors en CSS pur**, aucun SVG généré en JS, `backdrop-filter` réservé à la
  barre de navigation.
- Sans JavaScript, tout le contenu reste visible.

Mesuré en local : 5 requêtes au total (page + 3 polices + logo), `DOMContentLoaded` ≈ 165 ms.

## Contenu à compléter

- Les **dates** ne sont pas fixées (« à définir ultérieurement ») — à remplacer dans la section *Infos pratiques* et dans le déroulé dès qu'elles le sont.
- Le **déroulé en quatre passages** est une mise en récit de l'objectif indiqué sur l'affiche : à valider ou ajuster avec Alexandre.
- Possible ajout : photos du lieu, formulaire de contact, page mentions légales.

## Mise en ligne

Site 100 % statique servi par GitHub Pages depuis la branche `main`.
