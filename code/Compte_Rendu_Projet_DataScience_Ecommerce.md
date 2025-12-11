# GRAND GUIDE : Anatomie d'un Projet Data Science (E‑Commerce)

## 1. Contexte Métier et Mission

### 🎯 Problème (Business Case)

Dans le e‑commerce, un client insatisfait représente : - un risque de
**perte de revenu (churn)**\
- un impact négatif sur l'image de marque

Les causes principales d'insatisfaction sont généralement liées : - à la
**logistique** (retards, erreurs de livraison), - à la **qualité du
produit**.

**Objectif du projet :**\
Prédire la probabilité qu'un client laisse une *mauvaise note* (score \<
4) à partir des caractéristiques de sa commande, afin d'anticiper et
déclencher une action proactive du support avant la soumission du
commentaire.

### 🚨 L'Enjeu Critique : Le Coût du Churn

Le plus important n'est **pas l'accuracy**, mais la capacité à détecter
les clients à risque.

  ------------------------------------------------------------------------
  Type            Description                        Impact
  --------------- ---------------------------------- ---------------------
  **Faux Positif  Le modèle prédit "insatisfait"     Coût marketing
  (FP)**          alors que le client est satisfait  inutile (acceptable)

  **Faux Négatif  Le modèle prédit "satisfait" alors Perte de revenu,
  (FN)**          que le client sera insatisfait     mauvaise publicité
                                                     (très coûteux)
  ------------------------------------------------------------------------

👉 **Métrique prioritaire : le Recall sur la classe 0 (insatisfait).**

------------------------------------------------------------------------

## 2. Les Données (Input)

**Source :** `database_p4.csv`\
**X (Features) :** caractéristiques de commande, paiement, produit,
géolocalisation, et variables temporelles construites.\
**y (Target) :**\
- 0 = insatisfait (score ≤ 3)\
- 1 = satisfait (score ≥ 4)

------------------------------------------------------------------------

## 3. Code Python : Un Pipeline Professionnel

Le code utilise un **Pipeline Scikit‑learn** + **ColumnTransformer**
pour garantir : - robustesse, - nettoyage cohérent, - absence de **data
leakage**, - reproductibilité.

Toutes les étapes (imputation, normalisation, encodage, modèle) sont
regroupées proprement.

------------------------------------------------------------------------

## 4. Préparation & Ingénierie des Caractéristiques

### 🔧 Types de données présents :

-   **Numériques** : `payment_value`, `price`\
-   **Catégorielles** : `customer_state`,
    `product_category_name_english`\
-   **Temporelles** : dates de commande, expédition, livraison

### 🧠 Ingénierie de caractéristiques clés

Les dates brutes ne sont pas exploitables par la Régression Logistique.\
Nous les transformons en variables prédictives :

-   `delivery_time_days` : durée réelle de livraison\
-   `delay_vs_estimated_days` : retard ou avance par rapport à la date
    estimée → **variable clé**

### 🔄 ColumnTransformer

  ---------------------------------------------------------------------------
  Type               Transformation                           Rôle
  ------------------ ---------------------------------------- ---------------
  **Numérique**      Imputation → StandardScaler              Gestion NaN +
                                                              mise à
                                                              l'échelle

  **Catégorielle**   Imputation → OneHotEncoder               Conversion en
                                                              variables
                                                              binaires
  ---------------------------------------------------------------------------

------------------------------------------------------------------------

## 5. Exploration des Données (EDA)

### 📌 Distribution cible (déséquilibre)

  Classe   Signification   Pourcentage
  -------- --------------- -------------
  1        Satisfait       ≈ 80%
  0        Insatisfait     ≈ 20%

➡️ **L'accuracy est trompeuse**, un modèle naïf atteindrait déjà 80%.

### 💡 Insight clé issu de l'EDA

Les clients **insatisfaits** ont généralement : - un **retard plus
élevé**,\
- une **écart livraison réelle vs estimée** plus important.

👉 Le **délai de livraison** est un puissant prédicteur
d'insatisfaction.

------------------------------------------------------------------------

## 6. Méthodologie de Split

Utilisation de **train_test_split** avec :

-   `random_state=42` → reproductibilité\
-   `stratify=y` → maintenir le ratio 20/80 dans train et test

➡️ Garantit une évaluation fiable et non biaisée.

------------------------------------------------------------------------

## 7. Focus Théorique : Régression Logistique

### Pourquoi ce modèle ?

-   Simple, rapide, efficace pour débuter\
-   Modèle probabiliste → sortie interprétable (logits, probabilités)\
-   Coefficients lisibles :
    -   -   coefficient → augmente la probabilité d'être satisfait\

    -   -- coefficient → augmente la probabilité d'être insatisfait\
        (ex : `delay_vs_estimated_days` sera fortement négatif)

### Limitation :

Modèle **linéaire**, ne capture pas naturellement des interactions
complexes.

------------------------------------------------------------------------

## 📘 Conclusion Générale

Ce projet démontre comment structurer un pipeline complet de Data
Science appliqué à l'e‑commerce :

1.  Traduire un problème business en objectif ML clair\
2.  Comprendre l'impact critique des faux négatifs (insatisfaction +
    churn)\
3.  Concevoir une ingénierie de caractéristiques orientée métier\
4.  Construire un pipeline robuste et éviter le data leakage\
5.  Prioriser **le Recall** plutôt que l'accuracy\
6.  Démarrer avec un modèle interprétable avant d'aller vers des modèles
    plus complexes

Ce guide constitue une base solide pour tout projet de satisfaction
client dans le e‑commerce.
