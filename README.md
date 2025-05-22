# Pipeline ETL Complet - Extract, Transform & Load

> Ce projet Python propose un pipeline ETL complet incluant la transformation des données et leur chargement avec historisation. Il implémente toutes les étapes du processus ETL : nettoyage, transformation, validation, insertion SQL, UPSERT, et sauvegarde historique avec timestamps.

## 🛠 Fonctionnalités principales

### **Phase Transform** 📊
- **Analyse complète** : dimensions, valeurs manquantes, doublons, valeurs aberrantes
- **Nettoyage robuste** : traitement des valeurs manquantes, suppression d'anomalies, dédoublonnage
- **Transformations avancées** : colonnes calculées, catégories, normalisation Min-Max
- **Agrégation intelligente** : résumés statistiques par produit avec métriques de performance
- **Validation croisée** : contrôles qualité automatiques et cohérence des données
- **Tests unitaires** : validation sur données synthétiques avant traitement réel

### **Phase Load** 💾
- **Insertion SQL** : INSERT simple, UPSERT, TRUNCATE INSERT
- **Base de données** : SQLite intégrée avec création automatique des tables
- **Historisation** : sauvegarde automatique avec timestamps pour traçabilité
- **Backup préventif** : sauvegarde de la base avant modifications
- **Rapports de chargement** : statistiques détaillées post-insertion

## 📦 Dépendances

```bash
pip install pandas numpy sqlalchemy
```

**Librairies utilisées :**
- `pandas` - manipulation des DataFrames
- `numpy` - calculs numériques et normalisation
- `sqlalchemy` - ORM pour les opérations SQL
- `sqlite3` - base de données embarquée
- `logging`, `datetime`, `os`, `shutil` (bibliothèques standard)

## 🚀 Utilisation

### **Mode Transform uniquement**
```python
from etl_complet_simplifie import pipeline_etl_complet

# Pipeline de transformation
donnees_finales, agregats = pipeline_etl_complet("donnees_ventes.csv")
```

### **Mode ETL Complet (Transform + Load)**
```python
from pipeline_etl_load_complet import PipelineETLAvecLoad

# Pipeline complet avec base de données
pipeline = PipelineETLAvecLoad()
pipeline.pipeline_complet("donnees_ventes.csv", mode_chargement="upsert")
```

### **Modes de chargement disponibles**
- `"insert"` - Insertion simple des nouvelles données
- `"upsert"` - Mise à jour si existe, insertion sinon (recommandé)
- `"truncate_insert"` - Vider la table puis insérer (données fraîches)

## 📊 Architecture du Pipeline

### **🔄 Phase EXTRACT**
```
Fichier CSV → DataFrame Pandas → Validation initiale
```

### **🧹 Phase TRANSFORM**
1. **Analyse des données**
   - Dimensions et types
   - Valeurs manquantes par colonne
   - Détection de doublons et aberrants

2. **Nettoyage automatisé**
   - `Nom_produit` → "Produit_Inconnu" si manquant
   - `Quantite_vendue` → médiane si manquant
   - `Prix_unitaire` → moyenne si manquant
   - Suppression des valeurs aberrantes (IQR)
   - Déduplication intelligente

3. **Transformations enrichies**
   - `Chiffre_affaires` = Quantité × Prix
   - `Categorie_prix` : Bas/Moyen/Élevé/Premium
   - `Categorie_volume` : Faible/Moyen/Élevé/Très_élevé
   - Normalisation Min-Max : valeurs entre 0 et 1
   - `Timestamp_traitement` automatique

4. **Agrégation par produit**
   - Somme, moyenne, count des quantités
   - Min, max, moyenne des prix
   - Total et moyenne du chiffre d'affaires
   - Score de performance automatique

5. **Validation croisée**
   - Cohérence des calculs
   - Intégrité des transformations
   - Tests de qualité (8 critères)

### **💾 Phase LOAD**
1. **Préparation base de données**
   - Création automatique des tables (`ventes`, `agregats_produits`)
   - Sauvegarde préventive
   - Gestion des contraintes

2. **Chargement intelligent**
   - **INSERT** : Ajout simple
   - **UPSERT** : `INSERT ... ON DUPLICATE KEY UPDATE`
   - **TRUNCATE INSERT** : Remplacement complet

3. **Historisation complète**
   - Sauvegarde horodatée de chaque étape
   - Organisation en répertoires (`historique_donnees/`, `backup_donnees/`)
   - Traçabilité complète des traitements

## 📁 Structure de fichiers générée

```
projet_etl/
├── etl_complet_simplifie.py          # Pipeline Transform uniquement
├── pipeline_etl_load_complet.py      # Pipeline ETL complet
├── data_warehouse.db                 # Base de données SQLite
├── historique_donnees/               # Historique horodaté
│   ├── donnees_brutes_20241222_143022.csv
│   ├── donnees_nettoyees_20241222_143022.csv
│   ├── agregats_20241222_143022.csv
│   └── rapport_load_20241222_143022.csv
├── backup_donnees/                   # Sauvegardes DB
│   └── backup_db_20241222_143022.db
├── jeu_donnees_etl_5000_lignes_nettoye.csv    # Résultats Transform
├── jeu_donnees_etl_5000_lignes_agregats.csv   # Agrégats
└── README.md
```

## 🎯 Exemples d'utilisation

### **Test rapide avec données synthétiques**
```python
# Mode interactif avec données de test
python etl_complet_simplifie.py
# Choisir 'o' pour tester avec données synthétiques

python pipeline_etl_load_complet.py
# Choisir le mode de chargement souhaité
```

### **Traitement de données réelles**
```python
# Transform seul
from etl_complet_simplifie import pipeline_etl_complet
donnees, agregats = pipeline_etl_complet("mes_donnees.csv")

# ETL complet avec Load
from pipeline_etl_load_complet import PipelineETLAvecLoad
pipeline = PipelineETLAvecLoad()
pipeline.pipeline_complet("mes_donnees.csv", "upsert")
```

## 📋 Validation et contrôles qualité

### **Tests automatiques (Transform)**
- ✅ Valeurs manquantes traitées
- ✅ Doublons supprimés  
- ✅ Valeurs aberrantes éliminées
- ✅ Colonnes calculées ajoutées
- ✅ Normalisation appliquée
- ✅ Agrégats créés
- ✅ Cohérence des calculs
- ✅ Validation croisée

### **Contrôles de chargement (Load)**
- 🗄️ Tables créées automatiquement
- 💾 Sauvegarde préventive effectuée
- 🔄 Mode UPSERT/INSERT exécuté
- 📊 Rapport de chargement généré
- 🗂️ Historisation complète
- ✅ Intégrité des données vérifiée

## 🧪 Gestion d'erreurs et logging

```python
# Logging automatique
2024-12-22 14:30:22 - INFO - Données chargées: jeu_donnees_etl_5000_lignes.csv
2024-12-22 14:30:23 - INFO - Historisation: donnees_brutes_20241222_143022.csv
2024-12-22 14:30:24 - INFO - Base de données sauvegardée: backup_db_20241222_143022.db
```

**Gestion robuste :**
- Try/catch à tous les niveaux
- Logging détaillé des opérations
- Validation étape par étape
- Sauvegarde préventive
- Recovery automatique

## 📊 Données d'entrée attendues

**Format CSV avec colonnes :**
- `ID_produit` (int) - Identifiant unique
- `Nom_produit` (str) - Nom du produit
- `Quantite_vendue` (float) - Quantité vendue
- `Prix_unitaire` (float) - Prix unitaire
- `Date_vente` (str) - Date de vente (YYYY-MM-DD)

**Le pipeline gère automatiquement :**
- Valeurs manquantes dans toutes les colonnes
- Doublons sur ID_produit ou lignes complètes
- Valeurs aberrantes (négatives, zéros, extrêmes)
- Formats de dates diverses
- Incohérences de données

## 🔧 Configuration avancée

```python
# Personnalisation du pipeline Load
pipeline = PipelineETLAvecLoad(db_path="ma_base.db")
pipeline.historique_dir = "mon_historique"
pipeline.backup_dir = "mes_sauvegardes"
```

## ✅ Résultats garantis

**Transform :**
- ✨ Données 100% nettoyées et validées
- 📈 Enrichissement avec colonnes calculées
- 🎯 Normalisation et catégorisation
- 📊 Agrégats statistiques par produit
- 🧪 Tests de qualité automatiques

**Load :**
- 🗄️ Base de données SQLite structurée
- 📚 Historique complet avec timestamps  
- 💾 Sauvegardes automatiques
- 📋 Rapports de chargement détaillés
- 🔄 Modes de chargement flexibles

---

## 🚀 Quick Start

```bash
# 1. Installer les dépendances
pip install pandas numpy sqlalchemy

# 2. Test rapide Transform
python etl_complet_simplifie.py

# 3. Test complet Transform + Load  
python pipeline_etl_load_complet.py

# 4. Vérifier les résultats
ls -la *.csv historique_donnees/ backup_donnees/
```

**🎉 Votre pipeline ETL est prêt !**

---

© Projet pédagogique ETL - Extract, Transform & Load avec Python • Données fictives • Pipeline production-ready
