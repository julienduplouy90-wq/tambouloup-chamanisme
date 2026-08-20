# Tambouloup — site one-page

Site vitrine d'une page pour les **ateliers d'initiation aux pratiques chamaniques**
d'Alexandre Godgenger (Au Mélilot, chemin des Humas, 65200 Gerde).

En ligne : **https://khaki-spoonbill-538350.hostingersite.com/** (Hostinger, domaine temporaire)

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

## Direction artistique

Nuit de braise, avec les codes du papier imprimé empruntés à l'affiche.

- **Nuit** `#12100E` / `#1C1815`, **parchemin** `#F6EDDD`, **ocre** `#C9702B`,
  **braise** `#E0873A`, **or** `#E9BC63`, touche de **sauge** `#7E8E6C`
- Titres en **Cormorant Garamond**, textes en **Outfit** ; le mot « TAMBOULOUP »
  n'est jamais retypographié — c'est le logo lui-même.
- **Le loup passe devant le décor animé.** Les rayons (deux `repeating-conic-gradient`
  en rotation lente) et le halo qui respire sont **centrés sur le logo** et masqués
  au centre : l'anneau tourne autour du dessin, jamais au travers. Un médaillon
  sombre décolle le loup du fond, et l'encre noire est inversée en ivoire chaud.
- **Filets et fleurons `◆◆◆`** en ouverture de chaque section, bloc *Infos pratiques*
  présenté comme une affiche encadrée : les codes du placard imprimé, sur fond nuit.
- Les cartes de l'atelier sont nommées (« Premier geste »…) plutôt que numérotées :
  ce sont des gestes qui se suivent, pas une liste décorative.
- Grain de papier animé, apparitions en cascade, `prefers-reduced-motion` respecté.

## Le logo

`assets/logo.*` est généré à partir de la photo du dessin original : redimensionné à
760 px, **détouré** (la valeur de gris devient le canal alpha) et recoloré en encre.
Il se pose donc sur n'importe quel fond, sans le carré blanc d'origine, et le CSS
l'inverse en ivoire pour la version nuit.
Le poids (58 Ko) tient presque entièrement au canal alpha : baisser la qualité WebP
ne change rien, c'est déjà le minimum pour ce niveau de détail.

Pour le régénérer depuis une source plus propre (un vrai fichier vectoriel ou un PNG
haute définition), le principe est : `greyscale → negate → linear(1.45, -28)` comme
masque alpha, appliqué sur un aplat couleur encre.

## Parti pris performance

- **1 seule requête pour la page** : CSS et JS sont intégrés dans `index.html`
  (≈ 33 Ko non compressé, ≈ 9 Ko une fois servi en gzip/brotli). Sur un fichier
  de cette taille, supprimer deux allers-retours réseau rapporte bien plus que
  minifier — le code reste donc lisible et modifiable.
- **Polices auto-hébergées et préchargées** (`rel="preload"`), sous-ensemble latin
  uniquement, `font-display:swap` : plus de DNS + TLS vers `fonts.googleapis.com`
  et `fonts.gstatic.com`, plus de CSS tierce bloquant le rendu. 3 fichiers, 79 Ko.
  Outfit est une police variable : un seul fichier couvre les graisses 300 → 600.
- **Logo préchargé en WebP** via `<picture>`, dimensions et `aspect-ratio` réservés :
  aucun décalage de mise en page pendant le chargement.
- **Zéro écouteur de scroll.** L'en-tête collant et les apparitions au défilement
  passent par `IntersectionObserver` (callbacks hors du chemin critique).
- **Décors en CSS pur**, aucun SVG généré en JS, animations composées par le GPU et
  **mises en pause dès que le hero quitte l'écran**.
- Sans JavaScript, tout le contenu reste visible.

## Contenu

La page présente **l'ensemble de l'offre**, comme demandé par Alexandre : des ateliers
d'initiation aux pratiques chamaniques, dont l'atelier de base est le premier — il
conditionne tous les suivants. La section *Les ateliers* annonce les ateliers à venir
(plutôt orientés développement personnel) sans en promettre les dates : elles sont
communiquées directement, par téléphone ou par mail, aux personnes ayant suivi
l'atelier de base. Tous les appels à l'action pointent donc vers l'atelier de base.

À compléter :

- Les **dates** ne sont pas fixées (« à définir ultérieurement »).
- Le **déroulé en quatre passages** est une mise en récit de l'objectif indiqué sur
  l'affiche : à valider ou ajuster avec Alexandre.
- Possible ajout : photos du lieu, formulaire de contact, page mentions légales.

## Mise en ligne

Site 100 % statique hébergé chez **Hostinger** (plan Unlimited). Le site
`khaki-spoonbill-538350.hostingersite.com` a été créé le 20/08/2026 avec un
**domaine temporaire**, et le contenu de `public_html` déposé à la main (archive
zip extraite depuis le gestionnaire de fichiers), faute de compte FTP.

**GitHub Pages a été supprimé** : une seule adresse vivante.

Pour régénérer l'archive à déposer :

```bash
git archive --format=zip -o site.zip HEAD index.html 404.html robots.txt .htaccess assets
```

Pour passer au déploiement automatique, créer le compte FTP dans hPanel puis
déclarer `FTP_SERVEUR`, `FTP_UTILISATEUR` et `FTP_MOTDEPASSE` dans les secrets du
dépôt : le workflow s'active alors tout seul à chaque push sur `main`. Tant que
les secrets manquent, il s'arrête proprement sans faire échouer le build.

**À savoir sur le domaine temporaire** : Hostinger sert son propre `robots.txt`
sur les adresses en `.hostingersite.com` et ignore le nôtre. L'en-tête
`X-Robots-Tag: noindex` posé par le `.htaccess` et la balise meta continuent de
s'appliquer, donc le site reste hors des moteurs.
