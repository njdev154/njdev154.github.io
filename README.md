# MINYCAN EXPERIENCE

Un petit univers pour Minycan : un accueil, quelques mots doux, une exposition de **huit portfolios**,
un choix à faire, et une dernière phrase.

- **Site central :** https://njdev154.github.io/
- **Portfolios :** https://njdev154.github.io/portfolio-01/ … https://njdev154.github.io/portfolio-08/

## Le parcours

| Scène | Ce qui se passe |
| --- | --- |
| 1. Accueil | « Bienvenue, ma princesse. » monte comme un générique. Le bouton « Entrer dans mon petit univers » ouvre un iris rouge et lance le son (une seule fois). |
| 2. Petites attentions | Les messages s'écrivent un par un quand elle descend (points de saisie, puis les mots). Les petits noms sont espacés dans le parcours au lieu d'être enchaînés dans ces messages. « Emmmmmmmmmmanuella ! » s'étire lettre par lettre (une seule fois). « Owwwpapiiiii ! » danse ; on peut le toucher pour le refaire danser. |
| 3. Exposition | Huit œuvres accrochées différemment (petite, large, de biais, plein écran, téléphone…), chacune avec un cartel : numéro, nom, technologies, description, lien. « Nini » l'accompagne ici pendant sa découverte. |
| 4. Explorer | Chaque portfolio s'ouvre à sa vraie adresse dans un nouvel onglet. En fermant l'onglet, elle revient exactement au même endroit, avec un petit mot. Les visités sont marqués « déjà visité ». |
| 5. Le choix | Un seul choix possible, modifiable avant validation. « Mon bébé » apparaît ici, puis « Mi amor » à la confirmation. Le message WhatsApp est déjà écrit, mais elle l'envoie elle-même. |
| 6. Fin | « Je t'aime de tout mon cœur, ma vie. » s'allume mot après mot, la lumière bat doucement. Junior signe pour « ma bailouse ». |

## Arborescence

```
MINYCAN-EXPERIENCE/
├── index.html                  le site central (les 6 scènes)
├── 404.html                    page introuvable
├── .nojekyll                   GitHub Pages sert les fichiers tels quels
├── assets/
│   ├── css/experience.css      tout le design (couleurs et tailles en haut du fichier)
│   ├── js/
│   │   ├── main.js             point d'entrée
│   │   ├── data/portfolios.js  ★ les 8 portfolios (noms, descriptions, captures, URL)
│   │   ├── data/messages.js    ★ les messages, Emmanuella, Owwwpapii, les murmures
│   │   ├── data/config.js      ★ numéro WhatsApp, fichiers audio
│   │   ├── scenes/             une scène = un fichier (welcome, notes, gallery, choice, finale)
│   │   ├── audio.js            lecture unique + bouton couper/remettre le son
│   │   ├── store.js            mémoire locale (visites, choix)
│   │   └── utils.js
│   ├── fonts/                  Cormorant Garamond + Proza Libre (auto-hébergées, licence OFL)
│   └── media/
│       ├── audio/              owwwpapii-calee.m4a (copie exacte) + .ogg (secours)
│       ├── captures/           vraies captures des 8 portfolios (ordinateur + mobile, WebP)
│       ├── og-image.jpg        aperçu quand le lien est partagé sur WhatsApp
│       └── favicon.svg
├── portfolio-01/ … portfolio-08/   les 8 portfolios, indépendants
├── reference-audio/fichier-original.mp4   la vidéo d'origine du son
└── scripts/
    ├── captures.mjs            refaire les captures (Playwright)
    └── parcours-test.mjs       test du parcours complet, iPhone + ordinateur
```

## Lancer en local

Aucune installation, aucune dépendance : c'est du HTML, du CSS et du JavaScript natifs.

```
cd MINYCAN-EXPERIENCE
python -m http.server 8080
```

Puis ouvrir http://localhost:8080. (Ouvrir `index.html` en double-cliquant ne marche pas : les modules JavaScript ont besoin d'un petit serveur.)

## Modifier

- **Les textes des messages** : `assets/js/data/messages.js`. `name` = le petit nom mis en rouge, il doit apparaître tel quel dans la phrase.
- **Les textes d'accueil, de choix et de fin** : directement dans `index.html` (chaque scène est commentée).
- **Un portfolio** (nom, sous-titre, technologies, description, URL) : `assets/js/data/portfolios.js`. Rien d'autre à toucher, l'exposition et le choix se mettent à jour seuls. Pour ajouter ou retirer un portfolio, ajoute ou retire une entrée ; `layout` choisit son accrochage (`small`, `wide`, `tilt`, `full`, `phone`, `large`, `offset`, `finale`).
- **Les captures** : remplace les fichiers dans `assets/media/captures/` en gardant les mêmes noms (`portfolio-NN-desktop-1440.webp`, `-desktop-760.webp`, `-mobile-600.webp`). Pour les refaire automatiquement : `npm i -D playwright && npx playwright install chromium`, lance le serveur local, puis `node scripts/captures.mjs http://localhost:8080 captures-brutes` et convertis en WebP.
- **Les URL** : champ `url` de chaque portfolio dans `portfolios.js` (la constante `BASE` en haut du fichier change le domaine de tous d'un coup).
- **Les couleurs** : bloc `:root` en haut de `assets/css/experience.css`.
- **Le numéro WhatsApp** qui reçoit le choix : `whatsappNumber` dans `assets/js/data/config.js` (actuellement `2250161506296`, repris de tes portfolios 03 et 06 : vérifie que c'est bien le tien).

## Le son

- Fichier : `assets/media/audio/owwwpapii-calee.m4a`, la **piste audio complète** de la vidéo fournie (14,5 s), copiée **sans réencodage** (`ffmpeg -i fichier-original.mp4 -vn -c:a copy`). La voix n'est pas modifiée. Les rires et l'exclamation présents dans l'enregistrement sont conservés tels quels.
- `owwwpapii-calee.ogg` est une conversion de secours, utilisée seulement si le navigateur ne lit pas l'AAC.
- Lecture : uniquement après le clic sur « Entrer dans mon petit univers », une seule fois, jamais en boucle. Si le son échoue, l'expérience continue sans lui. Le bouton « Couper le son » apparaît pendant la lecture puis disparaît à la fin.
- Changer de son : remplace les deux fichiers en gardant leurs noms (ou modifie `audio.sources` dans `config.js`).
- **Droits** : c'est un extrait de « Calée » de Daphné, enregistré depuis Spotify. Le site est en `noindex`, mais il reste public : pour une diffusion publique, l'accord des ayants droit est en principe nécessaire. À toi de juger, ou de retirer le son.

## Le choix de Minycan : comment il te parvient

GitHub Pages ne fait que servir des fichiers : il ne peut ni enregistrer ni t'envoyer quoi que ce soit.
Solution retenue (gratuite, honnête, sans clé ni compte) :

1. Elle choisit et valide : le choix est mémorisé **sur son téléphone** (localStorage).
2. Le bouton « Envoyer mon choix sur WhatsApp » ouvre WhatsApp avec un message déjà écrit. **Il ne part que si elle appuie sur Envoyer.** Le site ne prétend jamais l'avoir envoyé.

Si tu veux un envoi automatique plus tard :

| Option | Avantages | Limites | Coût |
| --- | --- | --- | --- |
| Formspree (déjà utilisé dans tes portfolios 04 et 07) | 10 minutes à brancher, tu reçois un e-mail | dépend d'un service externe, compte nécessaire | gratuit jusqu'à 50 envois/mois |
| Supabase (table + clé publique « anon » + règle RLS « insert seulement ») | vraie base de données, historique | configuration RLS obligatoire, compte nécessaire | gratuit (offre Free) |
| Fonction serveur (Cloudflare Workers, Netlify Functions) | contrôle total, peut t'envoyer un message | plus technique à maintenir | gratuit aux petits volumes |

Aucune de ces options n'est branchée : il faut ton accord et ton compte.

## Les 8 portfolios

| N° | Dossier | Nom | Techno | Remarques |
| --- | --- | --- | --- | --- |
| 1 | portfolio-01 | Mon portfolio | HTML, CSS | lien « Pigier » corrigé vers https://njdev154.github.io/pigier-connect/ (l'ancienne adresse n'existait pas) |
| 2 | portfolio-02 | NJ Dev | HTML, CSS, JS | 4 pages, CV PDF |
| 3 | portfolio-03 | Motion Edition | HTML, CSS, JS | FR/EN, études de cas |
| 4 | portfolio-04 | Ligne NJ | HTML, CSS, JS, canvas | formulaire Formspree (service externe déjà prévu dans le code) |
| 5 | portfolio-05 | Papier & cuivre | HTML, CSS, JS (compilé dans index.html) | nom repris du concept décrit dans son README |
| 6 | portfolio-06 | L'affiche risographie | HTML, CSS, JS | nom repris du concept décrit dans son README |
| 7 | portfolio-07 | Table de montage | HTML, CSS, JS | formulaire Formspree |
| 8 | portfolio-08 | njdev | HTML, CSS, JS | |

Tous sont **statiques** : ils fonctionnent sur GitHub Pages, sans PHP ni base de données.
Modifications faites, et seulement celles-ci (le design d'origine n'est pas touché) :

- adresses canoniques et images de partage pointées vers leur nouveau dossier (`/portfolio-NN/`) ;
- chemins absolus des pages 404 rendus relatifs ;
- clés de mémoire du navigateur préfixées (`p04-`, `p07-`…) : les 8 sites partagent le même domaine, sans ça l'intro du 07 se serait sautée si elle avait vu le 04 avant ;
- dossiers `.git` et `.vscode` retirés.

## Redéployer

Le dépôt GitHub est `njdev154/njdev154.github.io` (dépôt « site utilisateur » : GitHub Pages le publie à la racine du domaine).

Le workflow `.github/workflows/pages.yml` extrait `MINYCAN-EXPERIENCE-v3.zip` (à la racine du dépôt) et publie le site avec GitHub Pages après chaque commit sur `main`. Pour mettre à jour le site, remplace le ZIP dans le dépôt et valide le changement. Dans *Settings → Pages*, sélectionne *GitHub Actions* comme source de publication. Le premier déploiement peut prendre quelques minutes.

Pour travailler avec les fichiers plutôt qu'avec l'archive, tu peux aussi cloner le dépôt, copier les fichiers du projet à la racine, puis `git add -A`, `git commit -m "Mise à jour"` et `git push`.

## Tests

`scripts/parcours-test.mjs` rejoue tout le parcours en iPhone (390 × 844) et ordinateur (1440 × 900) : accueil, entrée, son et bouton du son, messages, Emmanuella, exposition, 8 captures, ouverture d'un portfolio, retour, choix, confirmation, mémoire après rechargement, fin, erreurs console, débordement horizontal.

```
npm i -D playwright && npx playwright install chromium
python -m http.server 8080 &
node scripts/parcours-test.mjs http://localhost:8080/ tests
```

## Accessibilité et performance

- Tout se fait au clavier et au doigt ; rien d'important ne dépend du survol.
- `prefers-reduced-motion` : toutes les animations s'arrêtent, tout reste lisible et utilisable.
- Aucune bibliothèque, aucune requête vers un autre site (sauf les liens WhatsApp et les portfolios). Captures en WebP, chargées au fur et à mesure.
