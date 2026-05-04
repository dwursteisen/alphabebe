# AlphaBébé

Une petite application web pour apprendre les lettres et les mots aux jeunes enfants (3-4 ans), en français.

L'enfant tape sur une lettre pour l'entendre, sur l'image pour entendre le mot entier, et passe au mot suivant d'un seul geste. Les parents gèrent la liste des mots depuis un menu.

## Fonctionnalités

- Cartes de lettres animées, chacune dans une couleur distincte
- Synthèse vocale française (lettre par lettre ou mot complet)
- Image associée à chaque mot (optionnelle, fournie par le parent)
- Liste de mots personnalisable (ajout, suppression, retour aux valeurs par défaut)
- Persistance locale — vos mots et images restent disponibles après fermeture du navigateur
- Raccourcis clavier sur ordinateur ; pensé d'abord pour le tactile

## Utilisation

L'application tient dans un seul fichier. Pour la lancer, ouvrez `index.html` dans un navigateur :

```bash
open index.html
```

Aucune installation, aucune dépendance, aucun serveur. Une connexion internet est nécessaire la première fois pour charger la police Lexend depuis Google Fonts.

### Sur mobile

Touchez une lettre pour l'entendre, touchez l'image pour entendre le mot, appuyez sur « Suivant » pour passer au mot suivant. Le menu en haut à droite ouvre la gestion des mots.

### Sur ordinateur

| Touche | Action |
|--------|--------|
| `Espace` | Mot suivant |
| `←` / `→` | Lettre précédente / suivante |
| `↑` / `↓` | Mot précédent / suivant |
| `L` | Lire la lettre courante |
| `M` | Lire le mot complet |
| `I` | Afficher / cacher l'image |

## Personnaliser la liste des mots

Le bouton menu (☰) en haut à droite ouvre la liste. Vous pouvez :

- saisir un nouveau mot et, en option, joindre une image depuis votre appareil ;
- toucher un mot pour l'afficher immédiatement ;
- supprimer un mot avec la croix rouge ;
- réinitialiser la liste avec le bouton « Remettre les mots par défaut ».

Pour trouver des images libres, [flaticon.com](https://www.flaticon.com) est un bon point de départ : téléchargez l'image, puis joignez-la depuis le formulaire d'ajout.

Les images sont encodées en base64 et stockées avec les mots dans le `localStorage` du navigateur. Le quota est d'environ 5 Mo par site — préférez de petites images si vous en ajoutez beaucoup.

## Structure du projet

```
alphabebe/
├── index.html                            ← l'application (HTML + CSS + JS)
├── CLAUDE.md                             ← notes pour Claude Code
└── stitch_cran_d_apprentissage_principal/
    ├── DESIGN.md                         ← langage visuel « Digital Playroom »
    ├── code.html                         ← maquette générée par Stitch
    └── screen.png
```

Le projet est volontairement gardé en un seul fichier : pas de build, pas de framework, pas de dépendances locales. Il suffit de copier `index.html` n'importe où pour le déployer.

## Compatibilité

Testé sur Chrome, Safari et Firefox récents (ordinateur et mobile). La synthèse vocale repose sur l'API `SpeechSynthesis` du navigateur ; sa qualité dépend des voix françaises installées sur l'appareil.
