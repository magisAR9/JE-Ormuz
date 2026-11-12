# Index des données

Ce dépôt regroupe les données attributaires sur les navires, les données AIS (positions dynamiques et métadonnées) pour trois flottes : pétroliers, porte-conteneurs et méthaniers ainsi que des données d'escales. 
Période couverte du 01/12/2025 au 31/05/2026 , fermeture du détroit 28 Février 2026.
Toutes les données sont hébergées sur figshare.

## Données attributaires sur les navires

La description des colonnes, des codes et des précautions d'usage se trouve dans [BDD_Navire.md](BDD_Navire.md).

| Jeu de données | DOI |
|---|---|
| Caractéristiques des navires | [10.6084/m9.figshare.34788261](https://doi.org/10.6084/m9.figshare.34788261) |

Source :
[IHS Maritime](https://maritime.ihs.com/)

## Données AIS (localisation dynamique et métadonnées)

La description des colonnes, des codes et des précautions d'usage se trouve dans [AIS_infos.md](AIS_infos.md).

| Zone | Flotte | DOI |
|---|---|---|
|Détroit  | Pétroliers (*TANKER*) | [10.6084/m9.figshare.34788252](https://doi.org/10.6084/m9.figshare.34788252) |
|Détroit  | Porte-conteneurs (*CONTAINER*) | [10.6084/m9.figshare.34788243](https://doi.org/10.6084/m9.figshare.34788243) |
|Détroit  | Méthaniers (*GNL*) | [10.6084/m9.figshare.34788225](https://doi.org/10.6084/m9.figshare.34788225) |


Source :
[AIS Hub](https://www.aishub.net/)

## Données Escales 

La description des colonnes, des codes et des précautions d'usage se trouve dans [Escales_infos.md](Escales_infos.md).

| Zone |Flotte | DOI |
|---|---|---|
|Détroit  |Toutes|[10.6084/m9.figshare.34992688](https://doi.org/10.6084/m9.figshare.34992688)|
|Monde  |GNL & TANKER|[10.6084/m9.figshare.34992688](https://doi.org/10.6084/m9.figshare.34992688)|

Source :
[IHS Maritime](https://maritime.ihs.com/)

## Téléchargement

Chaque jeu de données peut être récupéré via l'API figshare à partir de son identifiant d'article (le nombre final du DOI). Par exemple, en R :

```r
article_id <- 34788252  # pétroliers
meta <- jsonlite::fromJSON(sprintf("https://api.figshare.com/v2/articles/%s", article_id))
meta$files[, c("name", "size", "download_url")]
```

