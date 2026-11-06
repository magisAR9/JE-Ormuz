# Données AIS — dictionnaire des colonnes

Ce jeu de données contient des messages **AIS** (*Automatic Identification System*). Les navires émettent ces messages par VHF pour signaler leur identité, leur position et leur route. Chaque ligne est un message horodaté émis par un navire.

Les colonnes appartiennent à deux familles :

- **Dynamiques** : position, vitesse, cap, statut. Elles viennent des messages de position (types AIS 1, 2, 3, ou 18/19 pour la classe B) et sont émises toutes les 2 secondes à 3 minutes selon la vitesse.
- **Statiques / de voyage** : identité, type, tirant d'eau, destination. Elles viennent des messages de type 5 (ou 24 pour la classe B), émis environ toutes les 6 minutes. Elles sont **saisies manuellement à bord** et peuvent donc être incomplètes ou erronées.


## Colonnes

| Colonne | Famille | Type | Unité / format | Description | Valeur « non disponible » |
|---|---|---|---|---|---|
| `MMSI` | identité | entier (9 chiffres) | — | *Maritime Mobile Service Identity* : identifiant de l'émetteur AIS, clé principale pour suivre un navire. Les 3 premiers chiffres (MID) indiquent le pavillon (ex. 226–228 = France). | — |
| `TSTAMP` | dynamique | date-heure | UTC (à vérifier) | Horodatage de réception du message. | — |
| `LATITUDE` | dynamique | réel | degrés décimaux, WGS84 | Latitude, de −90 à 90. | `91` |
| `LONGITUDE` | dynamique | réel | degrés décimaux, WGS84 | Longitude, de −180 à 180. | `181` |
| `COG` | dynamique | réel | degrés, 0–359.9 | *Course Over Ground* : route fond, c'est-à-dire la direction réelle du déplacement. | `360` |
| `SOG` | dynamique | réel | nœuds, 0–102.2 | *Speed Over Ground* : vitesse fond (1 nœud = 1,852 km/h). | `102.3` |
| `HEADING` | dynamique | entier | degrés, 0–359 | Cap vrai : direction de l'étrave. Il diffère du COG en cas de courant ou de vent. | `511` |
| `NAVSTAT` | dynamique | entier (0–15) | code | Statut de navigation déclaré (voir table ci-dessous). | `15` |
| `IMO` | statique | entier (7 chiffres) | — | Numéro OMI, attaché à la coque : il reste le même si le MMSI change. Absent pour beaucoup de petites unités. | `0` |
| `NAME` | statique | texte (≤ 20 car.) | — | Nom du navire. | vide / `@@@…` |
| `CALLSIGN` | statique | texte (≤ 7 car.) | — | Indicatif d'appel radio. | vide / `@@@…` |
| `TYPE` | statique | entier (0–99) | code | Type de navire et de cargaison (voir table ci-dessous). | `0` |
| `DRAUGHT` | voyage | réel | mètres, 0.1–25.5 | Tirant d'eau actuel déclaré. | `0` |
| `DEST` | voyage | texte (≤ 20 car.) | — | Destination déclarée, en texte libre et non normalisée (codes UN/LOCODE, noms de ports, fautes…). | vide |
| `ETA` | voyage | texte / date | `MMJJHHMM` UTC dans la norme | Heure d'arrivée estimée. La norme ne prévoit **pas d'année** : il faut la déduire de `TSTAMP`. | mois `00`, jour `00`, heure `24`, minute `60` |

## Codes `NAVSTAT`

| Code | Statut |
|---|---|
| 0 | En route au moteur |
| 1 | Au mouillage |
| 2 | Non maître de sa manœuvre |
| 3 | Capacité de manœuvre restreinte |
| 4 | Gêné par son tirant d'eau |
| 5 | Amarré |
| 6 | Échoué |
| 7 | En pêche |
| 8 | En route à la voile |
| 9–10 | Réservés (HSC, WIG) |
| 11 | Remorquage à l'arrière |
| 12 | Poussage / remorquage à couple |
| 13 | Réservé |
| 14 | AIS-SART, MOB, EPIRB actif |
| 15 | Non défini (défaut) |

## Codes `TYPE` (regroupements usuels)

| Code | Catégorie |
|---|---|
| 20–29 | Engin à effet de sol (WIG) |
| 30 | Pêche |
| 31–32 | Remorquage |
| 33 | Dragage / travaux sous-marins |
| 34 | Plongée |
| 35 | Militaire |
| 36 | Voilier |
| 37 | Plaisance |
| 40–49 | Navire à grande vitesse (HSC) |
| 50 | Pilote |
| 51 | Recherche et sauvetage |
| 52 | Remorqueur |
| 53 | Navette portuaire |
| 54 | Lutte antipollution |
| 55 | Police |
| 58 | Transport médical |
| 60–69 | Passagers |
| 70–79 | Cargo |
| 80–89 | Pétrolier / navire-citerne |
| 90–99 | Autres |

Dans les familles 40, 60, 70 et 80, le second chiffre code la catégorie de marchandise dangereuse (1 = X … 4 = Z selon MARPOL ; 9 = sans information complémentaire).


## Référence

ITU-R M.1371-5, *Technical characteristics for an automatic identification system using time division multiple access in the VHF maritime mobile band*.
