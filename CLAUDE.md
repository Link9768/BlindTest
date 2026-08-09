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

### Thèmes (10)

Créature, Animaux, Météo/Saisons, Chiffre/Nombre, Partie du corps, Chanteur, Réalisateur, Groupe, Film, Série, Pays, Sport, Décennie, Hommes et Femmes 1-4 (17).
Renseignés : **Créature** (partiel), **Animaux** ✅, **Météo/Saisons** ✅, **Chiffre/Nombre** (6/7), **Partie du corps** ✅. Les autres ont 7 slots placeholder (`reponse: '?'`, `spotifyId: ''`) à remplir dans le tableau `themes` du JS.

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

### Widget Spotify

- iframe embed dans `#spotify-window` (fenêtre circulaire 64px, `overflow: hidden`) calée pour ne montrer que le bouton play du widget
- Sync play/pause via `postMessage` (`playback_update`)
- Morceau sans `spotifyId` → icône 🚫 à la place du play

### Lecture depuis le début

L'embed sans connexion joue un extrait 30s choisi par Spotify (souvent le refrain). Connecté à un compte Premium dans le même navigateur → morceau complet depuis le début. Aucun paramètre d'URL ne force ça.

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
