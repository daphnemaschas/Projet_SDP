# Projet SDP : Explication de classements par structures de compensation (Trade-offs)

**Auteurs** : Adham Noureldin, Aymane Chalh, Daphné Maschas  
**Institution** : CentraleSupélec, Mention Intelligence Artificielle  
**Date** : 19 Janvier 2026

## 1. Introduction et Problématique
Ce projet s'inscrit dans le champ de l'IA explicable (XAI). L'objectif est de proposer une alternative à l'explication par score pondéré, souvent jugée trop abstraite pour l'utilisateur final. 

Nous développons ici un modèle formel permettant de justifier la supériorité d'une alternative $x$ sur une alternative $y$ à travers des structures de "trade-offs". La validation repose sur la capacité à partitionner l'ensemble des critères en groupes de compensation cohérents, où les avantages de $x$ couvrent de manière exhaustive et non redondante ses faiblesses par rapport à $y$.

## 2. Modélisation Mathématique
La résolution s'appuie sur la Programmation Linéaire en Nombres Entiers (MIP). Quatre modèles ont été implémentés à l'aide du solveur Gurobi :

* **Couplage simple (1-1)** : Mise en correspondance biunivoque entre critères favorables et défavorables.
* **Expansion (1-m)** : Un critère dominant compense un sous-ensemble de critères défavorables.
* **Agrégation (m-1)** : Une coalition de critères favorables compense un critère défavorable majeur.
* **Modèle Hybride** : Optimisation d'une partition mixte combinant les structures (1-m) et (m-1) pour maximiser la couverture explicative.

## 3. Architecture du Dépôt
Le répertoire est structuré afin de séparer le développement théorique des applications expérimentales :

* `livrable.ipynb` : Document principal synthétisant la démarche, les démonstrations de domination et les analyses de performance sur données réelles.
* `question1.ipynb` à `question4.ipynb` : Notebooks de travaux dirigés correspondant aux étapes de construction des modèles MIP.
* `data/` : Répertoire contenant les ressources de données :
    * `breastcancer_processed.csv` : Données oncologiques pour l'analyse de fidélité du modèle.
    * `ratp_cleaned_data.csv` : Jeu de données dédié à l'ordonnancement des priorités de rénovation.
    * `data27crit.xlsx` : Instance à haute dimensionnalité pour tester la robustesse algorithmique.
* `.gitignore` : Configuration pour l'exclusion des fichiers de logs et des artefacts de compilation.

## 4. Résultats et Analyse
Le cadre applicatif a permis de dégager les conclusions suivantes :

* **Interprétabilité médicale** : Sur le jeu de données "Breast Cancer", le modèle hybride parvient à expliquer 99,20 % des prédictions d'une régression logistique, identifiant précisément les cas marginaux où la décision repose sur une somme globale non structurelle.
* **Validation de la domination (RATP)** : Le modèle a permis d'établir une preuve de domination transitive pour la station Odéon (1-Best), justifiant son rang par rapport à l'intégralité du corpus de stations.
* **Efficacité algorithmique** : Les tests sur 27 critères confirment que l'utilisation d'un solveur de type Branch-and-Cut permet de surmonter la complexité combinatoire inhérente à l'espace des solutions possibles.

## 5. Dépendances et Installation
Pour reproduire les résultats, l'installation des bibliothèques suivantes est requise :
```bash
pip install gurobipy pandas numpy scikit-learn openpyxl