# HAX004X - Dirty Data

## Description générale
Ce dépôt regroupe les projets réalisés dans le cadre de l'UE **HAX004X - Dirty Data** du Master SSD à l'Université de Montpellier. Les projets portent sur le traitement et l'analyse de données biologiques et environnementales contaminées par des valeurs aberrantes et des données manquantes.

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
  
### 3. Prédiction de l'ozone

Analyse et prédiction du taux d'ozone à partir de données météorologiques. L'objectif est de développer un modèle de régression linéaire multiple permettant de prédire les concentrations d'ozone en fonction de variables météorologiques (température, nébulosité, vitesse du vent).

**Objectifs** :
- Traiter les valeurs manquantes (jusqu'à 37,5% pour certaines variables)
- Identifier les variables météorologiques significatives pour la prédiction
- Construire un modèle de régression linéaire multiple
- Évaluer la performance et la robustesse du modèle

**Données** :
- **Source** : Mesures météorologiques
- **Période** : 1er juin 2001 - 30 septembre 2001 (~4 mois)
- **Variables mesurées** : taux d'ozone, température (T9, T12, T15), nébulosité (Ne9, Ne12, Ne15), vitesse du vent (Vx9, Vx12, Vx15), direction du vent

**Méthodologie** :
1. **Traitement des valeurs manquantes** :
   - Suppression des lignes où la variable cible (maxO3) est manquante (14,29%)
   - Imputation par interpolation linéaire pour les variables explicatives
2. **Modélisation par régression linéaire multiple** :
   - 10 variables explicatives utilisées
   - R² = 0,75 (75% de variance expliquée)
   - Variables significatives : maxO3v (ozone de la veille) et Ne9 (nébulosité matinale)
3. **Validation du modèle** :
   - Diagnostics graphiques (résidus, Q-Q plot, homoscédasticité)
   - Validation croisée k-fold (k=5) : R² moyen = 0,67 ± 0,15
4. **Métriques de performance** :
   - RMSE = 14,29 (erreur ~15,7% de la moyenne)
   - MAPE = 12,95%

## Technologies utilisées

- **Langage** : [Julia](https://julialang.org/)
- **Packages principaux** :
  - `CSV` : Lecture et écriture de fichiers CSV
  - `DataFrames` : Manipulation de données tabulaires
  - `Dates` : Gestion des dates et horaires
  - `Statistics` : Calculs statistiques de base
  - `Plots` : Visualisation graphique
  - `RollingFunctions` : Lissage par moyennes mobiles (TP1)
  - `Distributions` : Distributions de probabilité (TP2)
  - `LsqFit` : Régression non linéaire (TP2)
  - `GLM` : Modèles linéaires généralisés (TP3)
  - `Interpolations` : Interpolation de données (TP3)
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
│   ├── SSD.png                         # Logo SSD
│   └── Univ_Mtp.png                    # Logo Université de Montpellier
│
├── TP3 - Prédiction de l'ozone/
│   ├── ozoneNA.csv                      # Données météorologiques
│   ├── AIGOIN-LABOURAIL-TP3.qmd        # Rapport Quarto
│   ├── AIGOIN-LABOURAIL-TP3.html       # Rapport HTML
│   ├── SSD.png                          # Logo SSD
│   └── Univ_Mtp.png                     # Logo Université de Montpellier
|
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
