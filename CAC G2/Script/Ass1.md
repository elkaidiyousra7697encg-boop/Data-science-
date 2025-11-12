# El kaidi yousra
# 25007958
# Le dataset "Heart Disease" de l'UCI Machine Learning Repository est une base de données médicale emblématique contenant des informations sur 303 patients et visant à prédire la présence de maladies cardiaques. Elle comporte 13 attributs cliniques incluant des données démographiques (âge, sexe), des mesures physiologiques (pression artérielle, cholestérol, fréquence cardiaque), des symptômes (type de douleur thoracique) et des résultats d'examens médicaux (ECG, test d'effort). La variable cible quantifie la présence et la sévérité de la maladie cardiaque sur une échelle de 0 à 4.

Cette base de données représente un outil précieux pour la recherche en intelligence artificielle médicale, permettant de développer des modèles prédictifs pour le diagnostic précoce des pathologies cardiaques. Sa taille modeste mais bien équilibrée en fait un dataset idéal pour l'expérimentation pédagogique tout en conservant une réelle pertinence clinique. Les défis principaux incluent le traitement des valeurs manquantes potentielles, l'encodage des variables catégorielles et la gestion d'éventuels déséquilibres entre les classes.

Utilisée depuis 1988 dans de nombreuses études, cette collection de données continue de servir de référence pour l'évaluation d'algorithmes de classification dans le domaine médical, offrant un terrain d'étude pour l'analyse des facteurs de risque cardiovasculaires et le développement d'outils d'aide au diagnostic.
# from ucimlrepo import fetch_ucirepo 
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

# Load dataset
heart_disease = fetch_ucirepo(id=45)
X, y = heart_disease.data.features, heart_disease.data.targets

# Basic preprocessing (example)
X.fillna(X.mean(), inplace=True)  # Handle missing values
y_binary = (y > 0).astype(int)    # Binarize target

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y_binary, test_size=0.2, random_state=42)

# Train model
model = RandomForestClassifier()
model.fit(X_train, y_train)

# Evaluate
predictions = model.predict(X_test)
print(classification_report(y_test, predictions))
