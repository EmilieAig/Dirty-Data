# HAX004X - Dirty Data

## Description générale
Ce dépôt regroupe les projets réalisés dans le cadre de l'UE **HAX004X - Dirty Data** du Master SSD à l'Université de Montpellier. Les projets portent sur le traitement et l'analyse de données biologiques et environnementales contaminées par du bruit de mesure et des valeurs aberrantes.

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
- **Source** : Capteurs autonomes dans un lac en région parisienne
- **Période** : 15 octobre 2022 - 31 août 2024
- **Fréquence d'échantillonnage** : 15 minutes
- **Variables mesurées** : date, concentration en chlorophylle (Chl), concentration en cyanobactéries (Cyano)

**Méthodologie** :
- Nettoyage des données (suppression des valeurs manquantes)
- Visualisation des séries temporelles brutes
- Agrégation par moyennes mobiles sur fenêtres de 75 minutes
- Analyse comparative avant/après lissage
- Détection des pics de concentration (blooms algaux)

### 2. Régression non linéaire sur le poids des brebis

Analyse et modélisation de l'évolution du poids de brebis à partir de mesures automatiques issues d'une balance connectée. Les données présentent de nombreuses valeurs aberrantes dues aux limitations du système de pesée (brebis partiellement sur la balance, plusieurs brebis simultanément, etc.).

**Objectifs** :
- Identifier et filtrer les mesures aberrantes (outliers)
- Estimer le poids réel de chaque brebis au cours du temps
- Modéliser l'évolution pondérale par une fonction logistique
- Prédire le poids futur des animaux

**Données** :
- **Source** : Balance connectée automatique (bergerie d'Arles)
- **Période** : 27 janvier 2021 - 29 avril 2021 (~3 mois)
- **Effectif** : 107 brebis, 32 361 mesures au total

**Méthodologie** :
1. **Nettoyage initial** : suppression des poids négatifs ou nuls (10,8% des données)
2. **Modèle de contamination de Huber** : 
   - Modélisation des données comme mélange de deux distributions gaussiennes
   - $\mathcal{P} = \epsilon \mathcal{P}_0 + (1-\epsilon)\mathcal{P}_1$
   - $\mathcal{P}_1$ : mesures correctes (poids réel + bruit)
   - $\mathcal{P}_0$ : outliers (pesées erronées)
3. **Algorithme EM** (Expectation-Maximization) :
   - Estimation itérative des paramètres du modèle
   - Calcul de la probabilité que chaque mesure soit un outlier
   - Classification binaire (seuil à τ = 0,5)
   - Résultat : 42,5% d'outliers détectés en moyenne
4. **Régression logistique** :
   - Agrégation quotidienne des mesures fiables (médiane pondérée)
   - Ajustement d'une courbe logistique : $f(t) = \frac{L}{1 + e^{-k(t-t_0)}} + b$
   - Modélisation de la croissance des brebis sur 3 mois

**Résultats** :
- Séparation efficace entre mesures fiables et outliers pour la majorité des brebis
- Courbes de croissance cohérentes capturant l'évolution pondérale
- Identification de brebis à croissance nette vs. brebis à données très bruitées
- Limitation : le modèle EM ne prend pas en compte l'évolution temporelle, entraînant une sur-détection d'outliers en fin de période

---

## Technologies utilisées

- **Langage** : [Julia](https://julialang.org/) 1.x
- **Packages principaux** :
  - `CSV` : Lecture et écriture de fichiers CSV
  - `DataFrames` : Manipulation de données tabulaires
  - `Dates` : Gestion des dates et horaires
  - `Statistics` : Calculs statistiques de base
  - `Plots` : Visualisation graphique
  - `RollingFunctions` : Lissage par moyennes mobiles (TP1)
  - `Distributions` : Distributions de probabilité (TP2)
  - `LsqFit` : Régression non linéaire (TP2)
- **Environnement** : Quarto pour la génération de rapports HTML

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
