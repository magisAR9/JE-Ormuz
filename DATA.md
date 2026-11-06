# Index des données

Ce dépôt regroupe les données attributaires sur les navires et les données AIS (positions dynamiques et métadonnées) pour trois flottes : pétroliers, porte-conteneurs et méthaniers. Toutes les données sont hébergées sur figshare.

## Données attributaires sur les navires

| Jeu de données | DOI |
|---|---|
| Caractéristiques des navires | [10.6084/m9.figshare.34788261](https://doi.org/10.6084/m9.figshare.34788261) |

## Données AIS (localisation dynamique et métadonnées)

La description des colonnes, des codes et des précautions d'usage se trouve dans [AIS_infos.md](AIS_infos.md).

| Flotte | DOI |
|---|---|
| Pétroliers (*tankers*) | [10.6084/m9.figshare.34788252](https://doi.org/10.6084/m9.figshare.34788252) |
| Porte-conteneurs (*container ships*) | [10.6084/m9.figshare.34788243](https://doi.org/10.6084/m9.figshare.34788243) |
| Méthaniers (*LNG carriers*) | [10.6084/m9.figshare.34788225](https://doi.org/10.6084/m9.figshare.34788225) |

## Téléchargement

Chaque jeu de données peut être récupéré via l'API figshare à partir de son identifiant d'article (le nombre final du DOI). Par exemple, en R :

```r
article_id <- 34788252  # pétroliers
meta <- jsonlite::fromJSON(sprintf("https://api.figshare.com/v2/articles/%s", article_id))
meta$files[, c("name", "size", "download_url")]
```

