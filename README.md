# HAX004X - Dirty Data - Analyse des Cyanobactéries et Chlorophylle

## Description

Ce projet s'inscrit dans le cadre de l'UE **HAX004X - Dirty Data** et porte sur l'analyse de données de capteurs mesurant la concentration en **chlorophylle** et en **cyanobactéries** dans un lac en région parisienne.

L'objectif principal est de traiter des données brutes échantillonnées toutes les 15 minutes, de les lisser et de les agréger en blocs de **1h15** afin d'obtenir une vision plus claire des tendances biologiques et de réduire le bruit de mesure.

## Objectifs du projet

- Visualiser les données brutes de concentration en chlorophylle (µg/L) et cyanobactéries (µg/L)
- Comprendre la variabilité temporelle et identifier les pics de concentration (blooms)
- Agréger les mesures par blocs de 75 minutes (5 mesures consécutives)
- Réduire le bruit de mesure et l'autocorrélation des données
- Comparer les données brutes et agrégées pour évaluer l'effet du lissage

## Données

- **Source** : Capteurs dans un lac en région parisienne
- **Période** : 15 octobre 2022 - 31 août 2024
- **Fréquence d'échantillonnage** : 15 minutes
- **Variables mesurées** :
  - `date` : Date et heure de la mesure
  - `Chl` : Concentration en chlorophylle (µg/L)
  - `Cyano` : Concentration en cyanobactéries (µg/L)

## Technologies utilisées

- **Langage** : [Julia](https://julialang.org/)
- **Packages** :
  - `CSV` : Lecture des données
  - `DataFrames` : Manipulation des données tabulaires
  - `Dates` : Gestion des dates et horaires
  - `Statistics` : Calculs statistiques
  - `Plots` : Visualisation graphique
  - `RollingFunctions` : Lissage par moyennes mobiles

## Structure du projet

```
.
├── data_chl_cyano.csv          # Données brutes des capteurs
├── AIGOIN-LABOURAIL-TP1.qmd    # Rapport Quarto (code + analyses)
├── AIGOIN-LABOURAIL-TP1.html   # Rapport html (code + analyses)
├── SSD.png                     # Logo SSD
├── Univ_Mtp.png                # Logo Université de Montpellier
└── README.md                   # Ce fichier
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
git clone https://github.com/votre-username/nom-du-repo.git
cd nom-du-repo
```

2. Lancez Julia et exécutez le rapport :

```julia
include("rapport.qmd")
```

Ou générez le rapport HTML avec Quarto :

```bash
quarto render rapport.qmd
```

## Auteurs

- **AIGOIN Emilie**
- **LABOURAIL Célia**

*Université de Montpellier - Master SSD*
