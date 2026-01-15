# HAX004X - Dirty Data

## Description générale

Ce dépôt regroupe les projets réalisés dans le cadre de l'UE **HAX004X - Dirty Data** du Master SSD à l'Université de Montpellier. Les projets portent sur le traitement et l'analyse de données biologiques et environnementales.

## Projets

### 1. Analyse des Cyanobactéries et Chlorophylle

Analyse de données de capteurs mesurant la concentration en **chlorophylle** et en **cyanobactéries** dans un lac en région parisienne. L'objectif principal est de traiter des données brutes échantillonnées toutes les 15 minutes, de les lisser et de les agréger en blocs de **1h15** afin d'obtenir une vision plus claire des tendances biologiques et de réduire le bruit de mesure.

**Objectifs** :
- Visualiser les données brutes de concentration en chlorophylle (µg/L) et cyanobactéries (µg/L)
- Comprendre la variabilité temporelle et identifier les pics de concentration (blooms)
- Agréger les mesures par blocs de 75 minutes (5 mesures consécutives)
- Réduire le bruit de mesure et l'autocorrélation des données
- Comparer les données brutes et agrégées pour évaluer l'effet du lissage

**Données** :
- **Source** : Capteurs dans un lac en région parisienne
- **Période** : 15 octobre 2022 - 31 août 2024
- **Fréquence d'échantillonnage** : 15 minutes
- **Variables mesurées** : date, concentration en chlorophylle (Chl), concentration en cyanobactéries (Cyano)

### 2. Régression Non Linéaire - Poids des Brebis

Analyse du poids des brebis par régression non linéaire.

## Technologies utilisées

- **Langage** : [Julia](https://julialang.org/)
- **Packages principaux** :
  - `CSV` : Lecture des données
  - `DataFrames` : Manipulation des données tabulaires
  - `Dates` : Gestion des dates et horaires
  - `Statistics` : Calculs statistiques
  - `Plots` : Visualisation graphique
  - `RollingFunctions` : Lissage par moyennes mobiles

## Structure du projet

```
.
├── TP1 - Cyanobacteries et Chlorophylle/
│   ├── data_chl_cyano.csv              # Données brutes des capteurs
│   ├── AIGOIN-LABOURAIL-TP1.qmd        # Rapport Quarto
│   ├── AIGOIN-LABOURAIL-TP1.html       # Rapport HTML
│   ├── SSD.png                          # Logo SSD
│   └── Univ_Mtp.png                     # Logo Université de Montpellier
│
├── TP2 - Poids des brebis/
│   ├── data_arles2021.csv              # Données brutes des balances
│   ├── AIGOIN-LABOURAIL-TP2.qmd        # Rapport Quarto
│   ├── AIGOIN-LABOURAIL-TP2.html       # Rapport HTML
│   ├── SSD.png                          # Logo SSD
│   └── Univ_Mtp.png                     # Logo Université de Montpellier
│
└── README.md                            # Ce fichier
```

## Installation et utilisation

### Prérequis

- Julia 1.x installé sur votre machine
- Les packages nécessaires (voir section Technologies)

### Installation des packages

```julia
using Pkg
Pkg.add(["CSV", "DataFrames", "Dates", "Statistics", "Plots", "RollingFunctions"])
```

### Exécution

1. Clonez ce dépôt :
```bash
git clone https://github.com/votre-username/HAX004X-dirty-data.git
cd HAX004X-dirty-data
```

2. Naviguez vers le projet souhaité :
```bash
cd cyanobacteries/  # ou cd brebis/
```

3. Lancez Julia et exécutez le rapport :
```julia
include("AIGOIN-LABOURAIL-TP1.qmd")
```

Ou générez le rapport HTML avec Quarto :
```bash
quarto render AIGOIN-LABOURAIL-TP1.qmd
```

## Auteurs

- **AIGOIN Emilie**
- **LABOURAIL Célia**

*Université de Montpellier - Master SSD*

## Licence

Ce projet est réalisé dans un cadre académique.
