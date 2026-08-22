# DJ Set

## Objectif

Application web mobile (fichier HTML unique) utilisée pendant un DJ set comme outil d'animation. Les joueurs écoutent un extrait musical et doivent deviner la réponse liée au thème (ex. thème Créature → deviner la créature associée au morceau).

## Versions

- **V1** : `../app DJ set.html` — version mono-thème (Créature), fonctionnelle, **ne pas modifier**
- **V2** : `app DJ set v2.html` — version multi-thèmes, design inspiré de djset.olemains.com

## V2 — Architecture (fichier unique)

### Écrans (show/hide, pas de routing)

1. **Menu** (`#screen-menu`) : grille de 10 tuiles thèmes + bouton "Connexion Spotify" (ouvre `accounts.spotify.com/login` — une fois connecté dans le navigateur, les embeds jouent les morceaux complets depuis le début, pour toute la session)
2. **Player** (`#screen-player`) : carrousel disques vinyle superposés, bandeau "Voir la réponse" pleine largeur, bouton play central avec anneau timer vert, bouton "← Menu", "Suivant →"

### Thèmes (34)

Créature, Animaux, Météo/Saisons, Chiffre/Nombre, Partie du corps, Chanteur, Réalisateur, Groupe 1-3, Film 1-2, Séries 1-4, Pays 1-2, Sport, Décennie, Hommes et Femmes 1-4, Couleur, Vêtement, Nom de famille, À ne pas faire 1-2, Il faut, Lieux 1-2, Jours.

Toute catégorie dont la liste fournie dépasse 7 morceaux → scindée en sous-catégories numérotées (Séries, Pays, Film, Groupe, Hommes et Femmes, À ne pas faire, Lieux). Pattern à réutiliser systématiquement.

Renseignés complets ✅ : Animaux, Météo/Saisons, Partie du corps, Hommes et Femmes 1-2, Réalisateur, Film 1, Séries 2, Pays 1, Groupe 1-3, Lieux 1.
Partiels : Créature (2/7), Chiffre/Nombre (6/7), Film 2 (6/7), Séries 1 (6/7), Séries 3 (5/7), Séries 4 (3/7), Pays 2 (4/7), Sport (6/7), Hommes et Femmes 3 (6/7), Hommes et Femmes 4 (5/7), Couleur (6/7), Vêtement (2 confirmés + 1 flag), Nom de famille (6/7, réponses déduites), À ne pas faire 1 (6/7 + 1 flag), À ne pas faire 2 (2/7), Il faut (2/7, réponses déduites), Lieux 2 (3/7), Jours (3/7).
Vides : Chanteur, Décennie.

⚠️ **Points à vérifier avec l'utilisateur** :
- **Séries 1 #7 "SIX FEET UNDER"** : `spotifyId` vide — l'ID fourni (`2eHj0klWkwRQuIrNlPpCPa`) était une collision avec I'm Every Woman.
- ~~Film 2 #5/#6~~ corrigé : réponses = titre du film (JURASSIC PARK, LE GRAND BLEU), pas le concept.
- **Ville** (nouveau thème, complet ✅) : réponses déduites du titre sauf "New York" (donnée). Kansas et Tennessee sont des états, pas des villes, à valider si voulu tel quel.
- **Jours de la semaine** (renommé depuis "Jours", complet ✅) : 7/7.
- **Biopic 1-3** (nouveau, 20 morceaux répartis 7/7/6) : réponse = titre du film biopic.
  - ⚠️ Biopic 1 #5 (Rocketman / Sacrifice — Elton John) : ID fourni trop court (20 caractères au lieu de 22), laissé vide, à redonner.
  - Biopic 3 #3 (8 Mile) et #4 (La Môme) : mêmes chansons déjà présentes ailleurs (Film 1, À ne pas faire 1) mais avec un ID Spotify différent de celui donné ici — intégré tel quel, à vérifier si voulu.
- **Vêtement #2 "Laisse béton" (Renaud)** : aucune réponse fournie ni déductible du titre (pas de référence vêtement identifiée) — `reponse: '?'`.
- **À ne pas faire 1 #4 "Don't Look Back in Anger" (Oasis)** : réponse non fournie — `reponse: '?'`.
- **Nom de famille** et **Il faut** : réponses déduites du titre de la chanson (pas explicitement données par l'utilisateur), à faire valider.
- **Bob Marley — No Woman No Cry, réponse "pieds"** : non intégré. Réponse incohérente avec la chanson (aucune référence aux pieds) et ne correspond à aucun thème existant (Partie du corps est déjà plein 7/7). À clarifier avant intégration.
- **Réserves non intégrées** (mentionnées explicitement "pour usage futur") : Groupe 4 (Louise Attaque, Journey, Trust, Bon Jovi, Metallica), Hommes et Femmes 5 (Neil Young, Lynyrd Skynyrd — Old Man / Simple Man).

### Bouton "+ d'infos"

**Obligatoire pour tout nouveau morceau ajouté** : chaque entrée `morceaux[]` doit avoir un champ `artiste` (et `titre` si `reponse` n'est pas déjà le titre de la chanson). Le bouton "+ d'infos" apparaît automatiquement sous le bandeau réponse dès que `artiste` est renseigné ; au clic, affiche titre + artiste dans une bulle. Si le titre/artiste n'est pas donné par l'utilisateur, le récupérer via WebFetch sur `open.spotify.com/track/{id}` (page publique, meta og:title fiable) plutôt que de l'inventer.

Tous les thèmes remplis (Créature, Animaux, Météo/Saisons, Chiffre/Nombre, Partie du corps, Hommes et Femmes 1-2) ont ce champ à jour.

### Thème Animaux — morceaux (complet ✅)

HOMARD, CHIEN, OISEAU, TIGRE, COLOMBE, REQUIN, CROCODILE (IDs dans le JS).

### Thème Météo/Saisons — morceaux (complet ✅)

SOLEIL (Here Comes the Sun), PLUIE (Purple Rain), ORAGE (Riders on the Storm), NEIGE (Snow Hey Oh), VENT (Le Vent nous portera), TONNERRE (Thunderstruck), SOLEIL (Walking on Sunshine).

### Thème Chiffre/Nombre — morceaux (6/7)

SEPT (Seven Nation Army), 99 (99 Luftballons), UN (One — U2), TROIS (3 nuits par semaine), CINQ (Mambo No. 5), DEUX (Song 2) — slot 7 vide.

### Thème Partie du corps — morceaux (complet ✅)

CŒUR (Heart of Glass), HANCHES (Hips Don't Lie), VISAGE (Ma Gueule), CŒUR (My Heart Will Go On), YEUX (Eyes Without a Face), YEUX (Les Yeux revolver), CŒUR (Comme des enfants).

### Thème Créature — morceaux

| # | Réponse | Spotify ID |
|---|---------|-----------|
| 1 | ZOMBIE | `49wOjOkS4pBK3PQnPnNYjb` ✅ |
| 2 | DRAGON (générique Game of Thrones) | `0M9QjRGZhvqJATvgQS4Hob` ✅ |
| 3 | CRÉATURE | *(vide)* |
| 4 | MONSTRE | *(vide)* |
| 5 | VAMPIRE | *(vide)* |
| 6 | LOUP-GAROU | *(vide)* |
| 7 | ALIEN | *(vide)* |

### Design (palette olemains récupérée de leur CSS)

- Fond : radial bleu `#022d5a` → `#001f46` + rainures concentriques
- Disques : `#3982a3`, `#61c3d3`, `#f39fa3`, `#e8375f`, `#f59923`, `#fecd4f`, `#77ba4e` — label central blanc, sillons blancs haut/bas (mask)
- Timer : petit segment vert `#77ba4e` qui tourne autour de l'anneau en 30s (sens horaire, via `stroke-dashoffset` négatif), passe rouge `#e8375f` à 0
- Bandeau réponse : `rgba(0,80,137,0.51)`

### Widget Spotify — SDK officiel IFrame API

Remplace l'ancien bricolage (iframe brute rechargée en changeant `.src`, puis recréation du nœud DOM) : on utilise maintenant le vrai [SDK Spotify IFrame API](https://developer.spotify.com/documentation/embeds/references/iframe-api) (`https://open.spotify.com/embed/iframe-api/v1`, chargé en fin de `<body>`, **après** le script principal pour garantir que `window.onSpotifyIframeApiReady` existe déjà avant que le SDK async ne s'exécute).

- `IFrameAPI.createController(el, {width, height, uri}, callback)` crée un unique `EmbedController` réutilisé pour toute la session (pas recréé à chaque morceau)
- `loadTrack(uri)` → `embedController.loadUri(uri)` puis `seek(0)` après 300ms : **force la position à 0 même si Spotify se souvient de la dernière position écoutée** (bug découvert : compte connecté = Spotify reprend où on s'était arrêté au lieu de redémarrer)
- `stopTrack()` → `embedController.pause()`
- Sync play/pause via `embedController.addListener('playback_update', ...)` (remplace l'ancien `window.addEventListener('message', ...)` sur postMessage brut)
- `#spotify-embed` est un `<div>` (pas une iframe) dans lequel le SDK injecte sa propre iframe — CSS ciblant `#spotify-embed iframe` pour le zoom/recadrage sur le bouton play
- Morceau sans `spotifyId` → icône 🚫 à la place du play (inchangé)

### Lecture depuis le début

L'embed sans connexion joue un extrait 30s choisi par Spotify (souvent le refrain) — limite plateforme, pas de fix code possible. Avec un compte connecté, Spotify peut reprendre à la dernière position écoutée au lieu de redémarrer : réglé par le `seek(0)` forcé ci-dessus.

⚠️ **Connexion Spotify sur mobile (Samsung Internet/Chrome)** : la session `accounts.spotify.com` ne se propage généralement pas à l'iframe `open.spotify.com` (cookies tiers bloqués par défaut) → limite structurelle, décision utilisateur = rester sur extraits 30s, ne pas retenter de fix cosmétique sans repartir d'un vrai besoin validé.

## Ce qui reste à faire

- Remplir les `spotifyId` du thème Créature (3-7)
- Remplir morceaux + réponses des 9 autres thèmes (structure prête, juste compléter `themes[]`)
- Vérifier le calage de `#spotify-window` sur le bouton play réel du widget (peut varier selon la largeur du widget)

## Fichiers

```
DJ set/
├── CLAUDE.md              ← ce fichier
├── app DJ set v2.html     ← V2 multi-thèmes (actif)
../app DJ set.html         ← V1 (figée, fonctionnelle)
```
