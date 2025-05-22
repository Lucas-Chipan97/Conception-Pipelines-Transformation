# Projet ETL - Transformation et Qualité des Données

> Pipeline complet pour le traitement, l'enrichissement et la transformation robuste de données dans un contexte ETL.

## Objectif

Assurer un flux de traitement des données fiable, automatisé et évolutif, incluant toutes les étapes clés du cycle de vie d’un jeu de données en ETL : nettoyage, validation, transformation, enrichissement, normalisation, avec gestion d’erreurs intégrée.

## Étapes du Pipeline

### 🔹 1. Nettoyage des données
- Suppression des doublons (via `np.unique` ou `pandas.drop_duplicates`)
- Remplacement des valeurs manquantes (`np.nanmean`, `fillna`)
- Fusion des entités similaires (logique métier ou fuzzy matching)
- Normalisation initiale (min-max ou z-score)

### 🔹 2. Validation des données
- Conformité aux types et formats (dates, numériques, etc.)
- Contrôle de plages (valeurs réalistes uniquement)
- Vérification des clés primaires/étrangères
- Cohérence intercolonnes (ex. : âge ↔ date de naissance)

### 🔹 3. Transformation des données
- Calculs dérivés (TVA, revenus, délais, etc.)
- Agrégation (groupes par région, produit, etc.)
- Conversion de formats (dates, devises, unités)
- Ajout de colonnes métier ou horodatées
- Adaptation aux besoins spécifiques de l’entreprise

### 🔹 4. Enrichissement
- Intégration de données externes (démographie, segments)
- Ajout de colonnes issues de mappings ou de regroupements
- Personnalisation client / produit
- Préparation pour la segmentation, analyse ou visualisation

### 🔹 5. Normalisation
- Mise à l’échelle des variables (min-max / z-score)
- Réduction de redondance pour assurer la qualité
- Structuration des données pour l’entreposage
- Conformité aux bonnes pratiques de modélisation

### 🔹 6. Gestion des erreurs
- `try/except` avec messages explicites
- Journalisation automatique avec détails des erreurs
- Mise en quarantaine des lignes corrompues
- Reprise du traitement sans perte de données

## Exemple de pipeline (simplifié)

```python
from pipeline_etl import executer_pipeline

executer_pipeline("ventes_brutes.csv")
# Résultat: ventes_brutes_nettoyees.csv, logs.txt, rapport_qualite.txt
```

## Bonnes pratiques appliquées

- **Logging horodaté** et suivi des erreurs
- **Validation croisée** à chaque étape
- **Variables paramétrables** pour l’adaptabilité
- **Tests unitaires recommandés** avant intégration
- **Documentation incluse** dans les scripts

## Configurations possibles

```python
SEUIL_IQR = 1.5
FORMAT_DATE_ATTENDU = "%Y-%m-%d"
TAUX_TVA = 20
STRATEGIE_VALEURS_MANQUANTES = {
    "Prix": "moyenne",
    "Quantité": "médiane",
    "Date": "default"
}
```

## Résultats attendus

- ✅ Données sans doublons ni valeurs aberrantes
- ✅ Variables harmonisées et cohérentes
- ✅ Agrégats prêts pour reporting ou BI
- ✅ Logs clairs et auditables
- ✅ Traçabilité complète de chaque transformation

## Version actuelle

**v1.3.0 - Pipeline robuste avec enrichissement et normalisation**

- Enrichissement externe intégré
- Création dynamique de colonnes
- Gestion des exceptions améliorée
- Journalisation multi-niveaux
