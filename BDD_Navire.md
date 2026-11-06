# Données attributaires sur les navires — dictionnaire des colonnes

DOI : [10.6084/m9.figshare.34788261](https://doi.org/10.6084/m9.figshare.34788261)

Ce jeu de données décrit les caractéristiques des navires suivis dans les [données AIS](AIS_DATA.md) : identité, construction, capacité, pavillon et armement. Il est fourni sous forme d'un classeur Excel qui contient **une feuille par type de navire** (pétroliers, porte-conteneurs, méthaniers). Toutes les feuilles ont la même structure de colonnes.

Le nom de la colonne `IMO/LR/IHS No.` indique que les données proviennent d'un registre de type IHS Markit / S&P Global (ex-Lloyd's Register Fairplay).

## Colonnes

| Colonne | Type | Unité / format | Description |
|---|---|---|---|
| `IMO/LR/IHS No.` | entier (7 chiffres) | — | Numéro OMI du navire, attribué à la coque pour toute sa durée de vie. C'est la **clé de jointure principale** avec la colonne `IMO` des données AIS. |
| `Name of Ship` | texte | — | Nom du navire à la date d'extraction. Il peut différer du `NAME` AIS (renommage, saisie à bord). |
| `Built` | entier | année | Année de construction (livraison). |
| `Callsign` | texte | — | Indicatif d'appel radio, qui change avec le pavillon. |
| `Deadweight` | réel | tonnes (tpl / DWT) | Port en lourd : masse maximale que le navire peut embarquer (cargaison, carburant, eau, équipage, provisions). Mesure de capacité de charge. |
| `Flag` | texte | — | État du pavillon (immatriculation). |
| `Operator` | texte | — | Société qui exploite commercialement le navire. |
| `Registered Owner` | texte | — | Propriétaire légal enregistré, souvent une société écran (*single-ship company*). |
| `Ship Type` | texte | — | Type détaillé selon la nomenclature du registre (ex. *Crude Oil Tanker*, *Container Ship (Fully Cellular)*, *LNG Tanker*). Plus fin que la feuille et que le code `TYPE` de l'AIS. |
| `Status` | texte | — | Statut du navire dans le registre (ex. *In Service/Commission*, *Laid-Up*, *Under Construction*, *Broken Up*). |
| `Cargo Tanks` | entier | nombre | Nombre de citernes à cargaison. Pertinent pour les pétroliers et méthaniers ; généralement vide ou nul pour les porte-conteneurs. |
| `MMSI` | entier (9 chiffres) | — | Identifiant de l'émetteur AIS, clé de jointure secondaire avec la colonne `MMSI` des données AIS. |
| `GT` | réel | sans unité | Jauge brute (*Gross Tonnage*) : indice du volume intérieur total du navire, calculé selon la convention de 1969. Ce n'est pas une masse. |

## Lecture en R

```r
library(readxl)
library(data.table)

f <- "chemin/vers/le_fichier.xlsx"
feuilles <- excel_sheets(f)

navires <- rbindlist(
  lapply(feuilles, function(s) as.data.table(read_excel(f, sheet = s))[, sheet := s]),
  fill = TRUE
)
setnames(navires, make.names(names(navires)))  # "IMO/LR/IHS No." -> "IMO.LR.IHS.No."
```

La colonne `sheet` garde le type de navire issu du nom de la feuille.

## Jointure avec les données AIS

- Joindre de préférence sur l'**IMO**, qui est stable. Le **MMSI** peut changer lors d'un changement de pavillon ou d'opérateur.


