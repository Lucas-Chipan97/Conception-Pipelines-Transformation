# Pipeline ETL - Nettoyage, Transformation et Validation de Données

> Ce projet Python propose un pipeline ETL complet et dynamique permettant de traiter, enrichir et valider automatiquement un jeu de données. Il inclut une gestion robuste des erreurs, une journalisation, et une validation croisée pour assurer la qualité des données.

## 🛠 Fonctionnalités principales

- Analyse initiale : dimensions, valeurs manquantes, doublons, valeurs aberrantes
- Nettoyage automatisé : traitement des valeurs manquantes, suppression d’anomalies, dédoublonnage
- Transformation enrichie : colonnes calculées, catégories, normalisation
- Agrégation : résumés statistiques par produit
- Validation finale : contrôles qualité automatiques, validation croisée
- Logging intégré et gestion des erreurs (try/except)
- Sauvegarde des fichiers nettoyés et agrégés

## 📦 Dépendances

- `pandas`
- `numpy`
- `logging` (standard)
- `datetime` (standard)

## 🚀 Exemple d’utilisation

```python
from pipeline_etl import pipeline_etl_complet

# Exécution du pipeline
pipeline_etl_complet("donnees_ventes.csv")
```

## 📊 Étapes du Pipeline

### 1. Chargement
Lecture des données via `pandas.read_csv()`

### 2. Analyse des données
- Affichage des dimensions
- Comptage des valeurs manquantes par colonne
- Détection de doublons (généraux et sur les IDs)
- Identification des valeurs aberrantes (négatifs et zéros)

### 3. Nettoyage
- Remplacement des valeurs manquantes :
  - `Nom_produit` → "Produit_Inconnu"
  - `Quantite_vendue` → médiane
  - `Prix_unitaire` → moyenne
- Suppression des valeurs aberrantes (négatives, prix à 0)
- Plafonnement des extrêmes par méthode IQR
- Suppression des doublons (ligne complète et par ID)

### 4. Transformations
- Colonnes calculées : `Chiffre_affaires`, `Timestamp_traitement`
- Catégorisation : `Categorie_prix`, `Categorie_volume`
- Normalisation min-max : `Quantite_vendue`, `Prix_unitaire`, `Chiffre_affaires`

### 5. Agrégation
- Groupement par `Nom_produit`
- Statistiques : somme, moyenne, min/max, nombre de ventes
- Création d’une colonne `Performance` basée sur le chiffre d’affaires total

### 6. Validation
- Contrôle des valeurs manquantes et doublons
- Unicité des IDs
- Présence des colonnes transformées
- Validation croisée du chiffre d'affaires

### 7. Sauvegarde
- Fichier nettoyé : `*_nettoye.csv`
- Fichier agrégats : `*_agregats.csv`

## 🔍 Exemple de test intégré

Le script contient une fonction `test_pipeline()` qui permet de tester automatiquement le pipeline avec des données synthétiques. Ce test inclut des cas de valeurs manquantes, doublons et valeurs aberrantes pour valider le bon fonctionnement de bout en bout.

## 📁 Structure attendue

```
.
├── pipeline_etl.py
├── jeu_donnees_etl_5000_lignes.csv
├── jeu_donnees_etl_5000_lignes_nettoye.csv
├── jeu_donnees_etl_5000_lignes_agregats.csv
└── README.md
```

## ✅ Résultats attendus

- Données nettoyées, transformées et enrichies
- Agrégats synthétiques par produit
- Journaux d'exécution clairs
- Validation automatisée des données
- Exécution robuste même en cas d’erreurs

## 🧪 Test automatique

Le pipeline est automatiquement testable via une entrée console pour choisir le jeu de données de test synthétique ou réel.

---

© Projet pédagogique de traitement de données pour l’analyse ETL avec Python. Toutes les données sont fictives.
