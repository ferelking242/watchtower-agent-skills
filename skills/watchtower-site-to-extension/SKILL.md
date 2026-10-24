---
name: watchtower-site-to-extension
description: "Cartographie un site public puis crée ou met à jour une source JavaScript Watchtower et son layout JSON. À utiliser lorsqu’on demande d’ajouter un site comme source Watchtower ou de refaire son accueil avec des composants variés."
---

# Transformer un site public en source Watchtower

Suis ce processus lorsqu’on te demande d’analyser un site de référence et d’en
faire une extension Watchtower. Le résultat attendu est une source fonctionnelle,
son layout d’accueil, son enregistrement dans l’index et un compte rendu des
pages et tests vérifiés.

## 1. Vérifier les dépôts et l’existant

Travaille dans les clones désignés par l’utilisateur. Pour le dépôt officiel,
les projets à examiner sont :

- `https://github.com/ferelking242/watchtower`
- `https://github.com/ferelking242/watchtower-extensions`

Avant toute modification :

1. Vérifie la branche, les changements locaux et les remotes. Ne remplace pas
   des modifications de l’utilisateur.
2. Lis les `AGENTS.md`, `README.md`, `ARCHITECTURE.md` et les instructions de
   développement applicables dans les deux dépôts.
3. Cherche le domaine, le nom et les alias du site dans `src/`, `index/` et
   `ui-layouts/`. Si la source existe, mets-la à jour au lieu d’en créer une
   deuxième. Vérifie aussi l’URL de base actuelle : un ancien domaine enregistré
   peut rediriger ou ne plus fonctionner.
4. Pour les composants d’interface, consulte la galerie réelle de Watchtower :
   `lib/modules/dev/component_gallery_screen.dart`,
   `lib/models/gallery_component_catalog.dart` et
   `lib/models/layout_component_registry.dart`. Lis aussi le validateur et les
   exemples actuels du dépôt d’extensions.

## 2. Cartographier le site sans crawler sans limite

Commence par l’URL fournie et son accueil. Utilise le HTML rendu pour les textes,
les liens et les états de pagination ; utilise une capture d’écran pour juger
la hiérarchie visuelle et les proportions des cartes.

Établis un inventaire des modèles de pages publics à partir de la navigation,
des liens internes et, si accessibles, de `robots.txt` et des sitemaps. Vérifie
au minimum les modèles qui existent réellement parmi :

- accueil et ses sections ;
- nouveautés, tendances et classements ;
- catalogue, catégories, genres, tags ou studios ;
- recherche et filtres ;
- fiche d’un élément ;
- pages de créateurs ou de catégories ;
- pagination, chargement supplémentaire et état sans résultat.

Pour chaque modèle, inspecte les liens de navigation et au moins un exemple
représentatif. Vérifie la page suivante quand il existe une pagination. Ne
confonds pas « couvrir toutes les routes et tous les modèles » avec visiter
chaque fiche de contenu : n’effectue pas un crawl illimité de centaines de
milliers d’éléments. Si l’utilisateur exige réellement chaque URL individuelle,
explique l’échelle et demande un périmètre avant un crawl massif.

Tiens une matrice courte avec : type de page, URL, données utiles, sélecteurs
stables, pagination, état observé et éventuel blocage. Note les pages
inaccessibles au lieu de deviner leur contenu. N’essaie pas de contourner une
connexion, un paywall, un CAPTCHA, une restriction d’âge, une limite de débit ou
une protection anti-robot.

N’enregistre pas de vidéos, d’images ou de médias du site dans l’extension.
Utilise seulement les métadonnées et les URL nécessaires à une intégration
autorisée ; respecte les conditions du site et la législation applicable.

## 3. Créer ou mettre à jour l’extension JavaScript

Adapte-toi à l’API actuellement documentée dans le dépôt : ne copie pas un
exemple ancien sans vérifier qu’il correspond toujours au runtime.

- Utilise le chemin `src/<catégorie>/<langue>/<slug>.js` et les conventions
  NSFW du dépôt si elles s’appliquent.
- Déclare les métadonnées `watchtowerSources` et la classe
  `DefaultExtension extends MProvider` selon les sources voisines actuelles.
- Implémente uniquement les méthodes requises pour le type de contenu. Pour une
  source vidéo, vérifie les contrats actuels de `getPopular`,
  `getLatestUpdates`, `search`, `getDetail`, `getVideoList`,
  `getFilterList` et `getSourcePreferences`. Pour un type différent, suis son
  contrat spécifique. Conserve `getSourcePreferences()` même sans réglage.
- Transforme les cartes du site en résultats cohérents : titre, URL absolue,
  miniature si elle est publiquement exposée, et métadonnées réellement
  présentes. Résous correctement les URL relatives et signale les champs
  absents ; n’invente ni données ni pagination.
- Fais correspondre les sections retournées par l’extension aux identifiants
  réellement déclarés dans le layout. N’ajoute pas une section décorative si la
  source ne fournit aucune donnée pour la remplir.
- N’implémente pas de contournement de DRM, d’authentification, de géoblocage ou
  de protection anti-robot. N’essaie pas de transformer une URL de lecture en
  fichier téléchargé.

Si tu modifies une source existante, incrémente sa version selon les consignes
du dépôt. Pour une nouvelle source, choisis un ID libre dans la plage et le bon
index du type de contenu ; vérifie l’absence de collision. Mets à jour l’index
par la procédure prévue, puis synchronise les versions avec les outils du
dépôt.

## 4. Construire un layout varié et compatible

Dans le dépôt d’extensions actuel, le nom canonique est
`ui-layouts/<slug>.json` — pas un fichier arbitraire à la racine. Vérifie la
convention du dépôt et la propriété `uiLayout` de l’entrée d’index avant de
choisir le chemin.

Lis la galerie des cartes **et** des sections. Les identifiants de la galerie
ne sont pas tous interchangeables :

- `home.sections[].component` désigne un rendu de section et doit être autorisé
  dans le contexte « home » par le registre **et** accepté par le validateur
  actuel du dépôt d’extensions.
- `cardComponent`, lorsqu’il est accepté par le schéma courant, choisit une
  carte dans le contexte correspondant. Ne place pas un identifiant de carte
  dans `component` juste parce qu’il apparaît dans la galerie.
- Préfère les composants sélectionnables et réellement supportés. Évite les
  alias de compatibilité, les doublons marqués non sélectionnables et les
  composants absents du validateur.

Construis une vraie composition à partir des données observées : par exemple
un hero/spotlight, un classement, une section de nouveautés, une découverte de
catégories ou de créateurs et un catalogue paginé — uniquement si le site
fournit ces contenus. Varie les rendus de section, les cartes autorisées, leur
ordre, les accents et les paramètres. Ne réutilise pas simplement la même
succession de cartes et de sections d’un layout voisin. Ne duplique pas les
mêmes résultats sous plusieurs titres pour simuler de la variété.

Vérifie le format accepté sur des layouts actuels :

- `schemaVersion` entier supérieur ou égal à `1` ;
- `home.sections` sous forme de liste, chaque section ayant un `id` unique et
  un composant reconnu ;
- les sections catalogue, recherche, détail et lecteur décrivent uniquement
  des comportements effectivement fournis par Watchtower et par l’extension.

Si la galerie et le validateur du dépôt d’extensions ne sont pas alignés,
respecte le validateur pour livrer un layout installable. Signale clairement
les cartes de galerie qui ne peuvent pas être utilisées comme section ou
comme `cardComponent` dans la version courante ; ne modifie pas l’application
Watchtower pour contourner cette incompatibilité sans demande explicite.

## 5. Vérifier avant de livrer

Depuis la racine du dépôt d’extensions :

1. Vérifie la syntaxe de l’extension avec `node --check <chemin-de-la-source>`.
2. Vérifie le JSON du layout et l’entrée de l’index.
3. Lance `node tools/validate-extensions.mjs` (ou `npm run validate`) et
   corrige les erreurs causées par tes changements.
4. Lance le test live documenté par `AGENTS.md` uniquement si le site est
   publiquement accessible et que ce test respecte ses règles d’accès. Ne
   contourne aucun blocage pour faire réussir le test. Sinon, indique le test
   impossible et ce qui a été vérifié localement.
5. Vérifie que l’index référence la bonne source, la bonne version et le bon
   `uiLayout`, sans doublon d’ID.

Termine par un compte rendu bref : modèles de pages examinés, fichiers ajoutés
ou modifiés, choix de composition du layout, validations réussies et limites
rencontrées.
