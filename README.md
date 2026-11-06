# ClairMail — Export pour GitHub Pages

Le site exporté est celui de https://clairmail.p11vvv.chatgpt.site au commit indiqué dans PROVENANCE.json.
Le site actuellement publié n’a pas été modifié. Ce ZIP ne crée pas de dépôt et ne publie rien automatiquement.

## Ce que contient le ZIP

| Dossier | Contenu |
| --- | --- |
| `docs/` | Les cinq pages HTML adaptées à GitHub Pages, les images et la configuration publique. |
| `backend/` | Un serveur API Cloudflare Workers séparé pour la recherche Service Public. |
| `sources-originales/` | Les sources originales suivies par Git, avant adaptation, avec le serveur, les scripts et les images. |
| `PROVENANCE.json` | Référence de la version exportée. |

## Publier sur GitHub Pages

1. Décompressez ce ZIP.
2. Créez ou ouvrez votre dépôt GitHub.
3. Ajoutez **le contenu** du dossier `clairmail-github-pages` à la racine du dépôt : vous devez voir `docs/`, `backend/` et ce README directement dans le dépôt.
4. Enregistrez les fichiers sur la branche `main`.
5. Ouvrez **Settings → Pages → Build and deployment**.
6. Sélectionnez **Deploy from a branch**, la branche **main** et le dossier **/docs**, puis **Save**.
7. Quand GitHub indique que le site est publié, ouvrez l’adresse indiquée par GitHub.

Le fichier `docs/.nojekyll` doit être conservé. Il peut être masqué dans votre explorateur de fichiers.
Aucune compilation n’est nécessaire pour les fichiers de `docs/`.
Les liens relatifs fonctionnent à la racine d’un domaine et dans un dépôt de projet comme `https://compte.github.io/clairmail/`.
Les pages utilisent des adresses explicites en `.html` ; les anciennes adresses sans extension du site Sites ne sont pas automatiquement transférées.

## Pages disponibles

- `docs/index.html` : accueil, solution, équipe, soutien et arborescence.
- `docs/conseils-serenite.html` : article « 4 règles pour retrouver votre sérénité ».
- `docs/quiz.html` : quiz et résultats indicatifs.
- `docs/demarches-administratives.html` : article sur les démarches.
- `docs/espace-demarches.html` : interface de recherche et orientation.

## Ce qui fonctionne directement

Les pages, la navigation, les images, la maquette d’interface, le quiz et la vidéo intégrée sont statiques.
La vidéo YouTube et les liens externes demandent une connexion Internet et restent soumis aux conditions de leurs fournisseurs.
Le suivi du projet reste un bouton permettant de copier le lien, comme sur le site source. Il n’y a pas de formulaire d’inscription ni de collecte de dons.

## Recherche Service Public : serveur séparé nécessaire

**GitHub Pages héberge des fichiers statiques ; il n’exécute pas le serveur API du site actuel.**
Par défaut, `docs/config.js` laisse l’adresse de l’API vide. Les pages restent consultables et l’espace démarches explique que la recherche connectée n’est pas encore activée. Les liens vers les sites officiels restent accessibles. Aucune réponse de secours n’est inventée.

Pour activer les recherches, il faut héberger le serveur du dossier `backend/`, puis renseigner son adresse.
Le serveur reprend les trois routes existantes :

- `GET /api/demarches/search?q=...` : recherche de fiches ;
- `GET /api/demarches/fiche?id=...` : lecture d’une fiche ;
- `GET /api/demarches/local?commune=...&type=...` : organismes locaux.

### Exemple de déploiement sur Cloudflare Workers

Un compte Cloudflare et Node.js sont nécessaires. Les éventuelles limites d’usage dépendent du compte et du serveur tiers.

1. Dans `backend/wrangler.toml`, remplacez `https://VOTRE-COMPTE.github.io` par l’origine réelle du site Pages. Pour `https://pauline.github.io/clairmail/`, l’origine serait `https://pauline.github.io`, **sans le nom du dépôt**.
2. Depuis un terminal, ouvrez le dossier `backend/` et lancez :

```sh
npx wrangler login
npx wrangler deploy
```

3. Copiez l’adresse HTTPS effectivement retournée par Cloudflare.
4. Modifiez `docs/config.js` pour renseigner cette adresse dans `apiBaseUrl`, sans slash final. Cette adresse est publique ; n’y placez aucune clé ni aucun secret.
5. Enregistrez la modification dans GitHub, attendez la publication et testez « passeport », puis une recherche de mairie par commune.

CORS : le serveur accepte seulement les origines listées dans `ALLOWED_ORIGINS` pour les requêtes du navigateur. Pour plusieurs origines, séparez-les par des virgules. Cette règle n’est pas un mécanisme d’authentification ; les routes consultent des données publiques et restent appelables par des clients hors navigateur.
La disponibilité et la fraîcheur des données restent celles du serveur tiers Service Public de OneNicolas. Le serveur est une connexion de consultation : il ne dépose pas de dossier, ne connecte aucun compte administratif et ne collecte aucun justificatif.

## Tester en local

Depuis le dossier `docs/` :

```sh
python3 -m http.server 8000
```

Ouvrez `http://localhost:8000/`. Pour tester aussi l’API depuis cette adresse, ajoutez temporairement `http://localhost:8000` aux origines autorisées du serveur, et retirez-la ensuite si elle n’est plus utile.

## Sources originales

Les fichiers dans `sources-originales/` sont conservés sans transformation. Leur configuration `.openai/hosting.json` décrit l’ancien hébergement Sites ; ne l’utilisez pas comme configuration GitHub Pages.
Les fichiers de cette copie ne contiennent pas l’historique Git, les jetons de connexion, ni des variables d’environnement secrètes.
Pour continuer la migration, travaillez sur `docs/` et `backend/`, pas sur cette copie de référence.

## Références

- GitHub Pages : https://docs.github.com/en/pages/getting-started-with-github-pages/what-is-github-pages
- Source de publication : https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- Cloudflare Workers / Wrangler : https://developers.cloudflare.com/workers/wrangler/
- Serveur MCP tiers : https://github.com/OneNicolas/mcp-service-public
