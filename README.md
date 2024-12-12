# Analyse_de_ventes_du_restaurant_Balaji_Fast_Food_Portfolio

## Introduction
Ce projet consiste à analyser des données de ventes afin d'extraire des informations précieuses pour les parties prenantes de l'entreprise. Le jeu de données utilisé contient des informations sur les commandes, les transactions et les activités de vente. L'analyse est réalisée en utilisant SQL pour l'exploration des données, Python pour les traitements avancés, ainsi que Power BI pour la visualisation et la création de rapports.

---

## Jeu de données
Le jeu de données utilisé pour cette analyse est un fichier CSV disponible sur [Kaggle](https://www.kaggle.com/datasets/ahmedhalimo/balaji-fast-food-sales). 
Il contient les colonnes suivantes :

- **order_id** : Identifiant unique pour chaque commande.
- **date** : Date de la commande.
- **item_name** : Nom de l'article vendu.
- **item_type** : Catégorie ou type de l'article.
- **item_price** : Prix de l'article.
- **quantity** : Quantité de l'article vendu.
- **transaction_amount** : Montant total de la transaction.
- **transaction_type** : Mode de paiement (par exemple : espèces, en ligne).
- **received_by** : Nom du vendeur ayant réalisé la transaction.
- **time_of_sale** : Heure de la transaction.

---

## Objectifs
1. Réaliser une analyse exploratoire des données avec SQL.
2. Nettoyer et prétraiter les données pour gérer les valeurs manquantes ou incohérentes.
3. Visualiser les tendances de vente avec Power BI.
4. Fournir des informations exploitables aux parties prenantes basées sur l'analyse.

---

## Principales requêtes SQL
Quelques-unes des requêtes SQL utilisées pour l'exploration des données :

1. **Revenu total par article** :
   ```sql
   SELECT item_name, SUM(transaction_amount) AS total_revenue
   FROM sales
   GROUP BY item_name
   ORDER BY total_revenue DESC;
   ```

2. **Types d'articles les plus vendus** :
   ```sql
   SELECT item_type, SUM(quantity) AS total_quantity
   FROM sales
   GROUP BY item_type
   ORDER BY total_quantity DESC;
   ```

3. **Meilleurs vendeurs par heure de vente** :
   ```sql
   WITH cte AS (
       SELECT time_of_sale, received_by, SUM(transaction_amount) AS total_revenue,
              ROW_NUMBER() OVER (PARTITION BY time_of_sale ORDER BY SUM(transaction_amount) DESC) AS rank
       FROM sales
       GROUP BY time_of_sale, received_by
   )
   SELECT time_of_sale, received_by, total_revenue
   FROM cte
   WHERE rank = 1;
   ```

4. **Contribution des types d'articles au revenu total** :
   ```sql
   SELECT item_type, (SUM(transaction_amount) / (SELECT SUM(transaction_amount) FROM sales) * 100) AS contribution_percentage
   FROM sales
   GROUP BY item_type;
   ```

5. **Articles avec les revenus mensuels les plus élevés** :
   ```sql
   WITH cte AS (
       SELECT EXTRACT(YEAR FROM date) AS year, EXTRACT(MONTH FROM date) AS month, item_name, SUM(transaction_amount) AS total_revenue,
              ROW_NUMBER() OVER (PARTITION BY EXTRACT(YEAR FROM date), EXTRACT(MONTH FROM date) ORDER BY SUM(transaction_amount) DESC) AS rank
       FROM sales
       GROUP BY EXTRACT(YEAR FROM date), EXTRACT(MONTH FROM date), item_name
   )
   SELECT year, month, item_name, total_revenue
   FROM cte
   WHERE rank = 1;
   ```

---

## Visualisations Power BI
Visualisations clés créées :

1. **Total des transaction, Nombre total des commandes, Quantite total d'articles vendus** :
   - Affichage en carte de Total des transaction, Nombre total des commandes, Quantite total d'articles vendus .

2. **Total des transactions par Année et Mois** :
   - Graphique linéaire montrant les revenus totaux au fil du temps.

3. **Types d'articles les plus vendus** :
   - Diagramme en barres affichant les types d'articles avec les ventes les plus élevées.

4. **Répartition des modes de paiement** :
   - Diagramme circulaire montrant la distribution des transactions en espèces et en ligne.

5. **Total des transactions par temps de vente** :
   - Diagramme en barres affichant le total des transactions par temps de vente.


---

## Configuration du projet

### Prérequis
- **Python** : Pour le prétraitement des données et les imputations avancées.
- **SQL** : Pour l'analyse exploratoire des données.
- **Power BI** : Pour créer des tableaux de bord interactifs.

  
---

## Résultats et recommandations
1. **Articles les plus vendus** : Se concentrer sur la promotion des articles les plus vendus pour augmenter les revenus.
2. **Performance des vendeurs** : Inciter les meilleurs vendeurs à maintenir leur productivité.
3. **Préférences de paiement** : Assurer une expérience de paiement en ligne fluide car cela représente une part importante des revenus.
4. **Heures de pointe** : Optimiser le personnel et l'inventaire pendant les heures de pointe.

---

## Contributeurs
- [Younes ZIAT](https://github.com/younes9888) - Analyste de données

---
