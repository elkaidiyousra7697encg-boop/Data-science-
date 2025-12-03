# El kaidi Yousra

<img src="2.jpg" style="height:300px;margin-right:300px; float:left; border-radius:10px;"/>

Numéro d'étudiant : 25007958

Classe : CAC2

Compte rendu

Analyse Prédictive de Régression des Tendances Mode Hivernale

Date : 03/12/2025

Table des Matières

Introduction et Contexte

Analyse Exploratoire des Données (Data Analysis)

Chargement et Structure du Dataset

Distribution de la Variable Cible

Préparation pour la Régression

Analyse Statistique et Visuelle

Méthodologie de Régression

Séparation des Données (Data Split)

Modèles de Régression Testés

Résultats et Comparaison des Performances

Performances Individuelles

Tableau Comparatif

Analyse des Coefficients

Conclusion

1. Introduction et Contexte
   
Ce rapport présente une analyse prédictive de régression sur un jeu de données réel des tendances mode hivernale Instagram, importé depuis Kaggle, réalisée dans le cadre d'une analyse de données. En suivant le cycle de vie des données, une exploration (EDA), un prétraitement et plusieurs modélisations de régression ont été menées.​

L'objectif est de prédire le prix (PriceUSD) des articles de mode en utilisant diverses techniques de régression linéaire, polynomiale, régularisée et non linéaire, et d'évaluer leurs performances respectives sur ce dataset.​

2. Analyse Exploratoire des Données (Data Analysis)

2.1 Chargement et Structure du Dataset

Le jeu de données WinterFashionTrendsDataset.csv contient des caractéristiques des articles de mode hivernale avec leurs prix en USD.​

Nombre d'échantillons ($N$) : 150 observations.
```python
import pandas as pd
df = pd.read_csv("WinterFashionTrendsDataset.csv")
print("Aperçu des premières lignes")
df.head()
df.info()

Nombre de variables ($d$) : 12 colonnes initiales (11 features + 1 target), étendu à 46 après encodage One-Hot.

Variables d'entrée ($X$) : Brand, Category, Color, Material, Style, Gender, Season, TrendStatus, PopularityScore, CustomerRating.
Variable de sortie ($Y$) : Prix (PriceUSD).
```
2.2 Distribution de la Variable Cible

Aucune valeur manquante n'a été détectée dans le dataset, rendant les imputations inutiles. Les distributions des variables numériques comme PriceUSD, PopularityScore et CustomerRating ont été visualisées par histogrammes, révélant des formes potentiellement asymétriques.​

2.3 Préparation pour la Régression

Les variables catégorielles ont été encodées via One-Hot, excluant l'ID, pour obtenir un DataFrame prêt pour la modélisation.
python
df_encoded = pd.get_dummies(df, columns=categorical_cols, drop_first=True)
y = df_encoded['PriceUSD']
X = df_encoded.drop(['ID', 'PriceUSD'], axis=1)

2.4 Analyse Statistique et Visuelle

Les fréquences des catégories (Brand, Category, etc.) ont été visualisées par diagrammes en barres. Les statistiques descriptives ont montré des échelles variées, mais aucune normalisation explicite n'a été appliquée dans cette analyse initiale
```python
plt.figure(figsize=(20, 20))
sns.countplot(data=df, y='Category', order=df['Category'].value_counts().index)
```
3. Méthodologie de Régression
   
3.1 Séparation des Données (Data Split)

Les données ont été divisées en ensembles d'entraînement (80%) et de test (20%) avec stratification pour assurer la reproductibilité
```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```
3.2 Modèles de Régression Testés

Plusieurs algorithmes ont été implémentés : régression linéaire simple/multiple, polynomiale (degré 2), Ridge, Lasso, Arbre de décision, Forêt Aléatoire, SVR. Les performances ont été évaluées via MAE, MSE et R² sur l'ensemble de test.​

4. Résultats et Comparaison des Performances
   
4.1 Performances Individuelles

Tous les modèles ont affiché des R² négatifs, indiquant une performance inférieure à la prédiction par la moyenne. La Forêt Aléatoire a obtenu le meilleur R² (-0.17) avec MAE 177.39 et MSE 44584.47.
4.2 Tableau Comparatif

   
| Modèle            | R²    | MAE    | MSE      |
| ----------------- | ----- | ------ | -------- |
| Linéaire Simple   | -0.26 | -      | -        |
| Linéaire Multiple | -0.44 | -      | -        |
| Polynomiale       | -1.09 | -      | -        |
| Ridge             | -0.41 | -      | -        |
| Lasso             | -0.38 | -      | -        |
| Arbre de Décision | -0.85 | -      | -        |
| Forêt Aléatoire   | -0.17 | 177.39 | 44584.47 |
| SVR               | -0.29 | -      | -        |

4.3 Analyse des Coefficients

Ridge réduit les coefficients sans annulation, tandis que Lasso effectue une sélection en mettant certains à zéro. Les 10 coefficients les plus importants ont été examinés pour chaque modèle.​

5. Conclusion

Cette analyse a révélé des performances globalement faibles (R² négatifs), soulignant la petitesse du dataset (150 échantillons) et la faible corrélation des features avec PriceUSD. La Forêt Aléatoire s'avère relativement supérieure, mais des améliorations via ingénierie de features, normalisation et optimisation d'hyperparamètres (Grid Search, validation croisée) sont nécessaires. Une exploration approfondie des corrélations et un dataset plus large permettraient de meilleures prédictions de prix.
