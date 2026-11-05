# DermaSuivi : suivi photo de la cicatrisation

Application installable sur téléphone (Android, iPhone) et ordinateur, qui remplace le dossier
« un album par patient » de la galerie photo :

- **un espace par médecin** : au lancement, chacun choisit son espace (« Dr Martin »…) et ne voit que
  ses patients ; un code personnel peut le protéger (verrouillage automatique après 5 min hors de l'app) ;
- **photo guidée** : la caméra s'ouvre avec la photo précédente en transparence (et une grille) pour reprendre
  le même cadrage, la même distance et le même angle ; les comparaisons deviennent fiables ;
- **métadonnées** lues dans chaque photo, importée ou prise : date et heure de prise de vue (EXIF), appareil,
  objectif, réglages, résolution, fichier, GPS ; visibles dans « Informations de la photo » ;
- **annotations et mesures** : flèches, cercles, dessin, texte ; étalonnage sur une règle visible dans la photo,
  puis mesure de longueurs (mm) et de surfaces (cm²) ; courbe d'évolution de la surface pour chaque motif ;
- **outils d'examen** : loupe (molette, double-clic, pincement), luminosité, contraste, saturation, type de photo
  (clinique, dermoscopie, vue d'ensemble) ;
- **contrôles et consentement** : date du prochain contrôle par motif, liste « À revoir » à l'accueil ; consentement
  du patient (photos, analyse IA, enseignement) enregistré avec sa date ;
- **analyse IA (facultative)** : description structurée d'une photo (lit de la plaie, ABCDE, dermoscopie…),
  hypothèses à discuter, signes d'alerte, et analyse de l'évolution entre plusieurs photos, avec l'IA Claude
  d'Anthropic ; voir « Analyse IA » ci-dessous ;
- **plusieurs motifs de consultation par patient** : s'il revient pour autre chose (ex. un eczéma après
  un ulcère), on crée un nouveau motif dans le même dossier ; chaque motif a ses photos, son J0, ses notes et
  son statut (en cours / guéri) ;
- **un dossier par patient** (nom, n° de dossier, notes), recherche, tri récent / A→Z ;
- **prendre une photo** directement ou **importer** depuis la galerie (plusieurs à la fois) ;
  la date de prise de vue est lue automatiquement dans la photo et reste modifiable ;
- **zones / lésions** (ex. « jambe droite ») pour suivre plusieurs lésions chez un même patient ;
- **frise chronologique** avec le jour de suivi : J0, J+14, J+49… ;
- **comparaison côte à côte** de 2 à 4 photos, bouton « première / dernière photo »,
  et **curseur avant / après** qui superpose deux photos ;
- **image de comparaison** (photos + dates) à partager ou enregistrer, par exemple pour le patient ;
- **fonctionne hors ligne** ; **sauvegarde / restauration** dans un fichier pour changer d'appareil.

## Confidentialité

Les photos restent **uniquement sur l'appareil** (IndexedDB du navigateur) : rien n'est envoyé sur
internet. Les fichiers de sauvegarde contiennent des données de santé : à conserver en lieu sûr.
Sur iPhone, installer l'app sur l'écran d'accueil évite que Safari efface les données d'un site
inutilisé.

Le code d'un espace empêche un autre médecin utilisant le même appareil de l'ouvrir ; ce n'est pas un
chiffrement des photos. Chaque médecin peut aussi simplement installer l'app sur son propre téléphone.

Police : EB Garamond (licence libre SIL OFL), incluse dans `fonts/` pour fonctionner hors ligne.

## Analyse IA

Désactivée par défaut. Chaque médecin saisit sa propre clé API Anthropic dans Réglages → Analyse IA
(elle reste sur l'appareil). Modèle utilisé : Claude Opus 5.5, réponse en JSON structuré ; si le modèle
décline une demande, l'API la relance automatiquement sur le modèle de repli recommandé (`fallbacks: "default"`).

- Seuls l'image et le contexte clinique (motif, zone, type de photo, jour de suivi, mesures) sont envoyés :
  jamais le nom, le n° de dossier ni les notes générales.
- L'application demande le consentement du patient et l'enregistre dans son dossier.
- La photo quitte l'appareil pour les serveurs d'Anthropic (États-Unis), qui ne sont pas certifiés HDS :
  à valider avec votre DPO avant un usage courant.
- Le résultat est une aide à l'analyse, jamais un diagnostic.

Le SDK officiel (`@anthropic-ai/sdk` 1.1.0) est intégré dans `vendor/` (voir `vendor/README.md`).

## Installer

Adresse : `https://actionplantracker23-ship-it.github.io/DERMA-SUIVI/`

Chaque push sur `main` republie l'application (`.github/workflows/pages.yml`).
Première fois : **Settings → Pages → Source : GitHub Actions**.

- **iPhone / iPad** : ouvrir l'adresse dans Safari → Partager → « Sur l'écran d'accueil ».
- **Android** : Chrome → menu ⋮ → « Installer l'application ».
- **Ordinateur** : Chrome ou Edge → icône d'installation dans la barre d'adresse.

## Développement

Aucune dépendance ni compilation : HTML, CSS et JavaScript.

```sh
python3 -m http.server 8000   # puis http://localhost:8000
```

`index.html` (page), `app.js` (logique, base IndexedDB), `meta.js` (métadonnées EXIF), `annot.js`
(annotations et mesures), `camera.js` (photo guidée), `ai.js` (analyse IA), `style.css`, `sw.js` (hors ligne),
`manifest.webmanifest` et `icons/` (installation).
