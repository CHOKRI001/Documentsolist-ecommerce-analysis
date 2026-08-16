# Analyse E-commerce & Segmentation Clients — Olist (Brésil)

## Contexte

Projet d'analyse de données réalisé sur le dataset public **Olist Brazilian E-Commerce**
(~100k commandes, 2016-2018). Objectif : comprendre les moteurs de revenu, diagnostiquer
un problème logistique et sa relation avec la satisfaction client, puis segmenter la base
clients pour des actions marketing ciblées.

## Objectifs

- Identifier les tendances de vente et les catégories de produits les plus rentables
- Quantifier l'impact des délais de livraison sur la satisfaction client
- Segmenter les clients selon leur comportement d'achat (RFM)
- Construire un modèle prédictif du risque de retard de livraison

## Données

Source : [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
(Kaggle) — 8 tables reliées (commandes, clients, produits, paiements, avis, vendeurs).

## Méthodologie

| Étape | Fichier | Description |
|---|---|---|
| 1 | `01_chargement_jointures.py` | Chargement des 8 tables sources, nettoyage, agrégations et jointures en un dataframe analytique unique |
| 2 | `02_eda_kpi.py` | Exploration : CA mensuel, top catégories, délai de livraison vs note client, taux de retard |
| 3a | `03a_rfm_clustering.py` | Segmentation clients RFM (Récence/Fréquence/Montant) + clustering KMeans |
| 3b | `03b_prediction_retard.py` | Modèle de régression logistique pour prédire le risque de retard de livraison |

**Stack technique :** Python, pandas, matplotlib, seaborn, scikit-learn (KMeans, régression logistique)

## Résultats clés

- **Chiffre d'affaires** : croissance quasi continue d'octobre 2016 (quasi nul) à novembre 2017 (pic ~1M R$/mois), puis CA mensuel globalement stable autour de 0,8-1M R$ jusqu'à l'été 2018
- **Catégories** : `health_beauty` et `watches_gifts` sont les deux catégories générant le plus de CA, suivies de `bed_bath_table` et `sports_leisure`
- **Logistique** : seulement **6,8%** des commandes sont livrées en retard — mais le boxplot délai/note montre une relation claire : le délai de livraison diminue avec la note (délai médian nettement plus élevé pour les notes 1 que pour les notes 5), confirmant que la logistique impacte directement la satisfaction
- **Segmentation clients** : 4 segments identifiés — 97% des clients (clusters 0 et 1, ~88 400 clients) sont des **acheteurs uniques** (fréquence = 1 commande), 2 189 clients "premium" à panier élevé (~1142 R$), et seulement 2 774 clients récurrents (fréquence > 1)
- **Prédiction de retard** : le modèle (régression logistique) atteint **58% de rappel** sur la classe "en retard" (9% de précision) ; la variable la plus explicative est `frais_port_total`, suivie de `delai_estime_jours`

## Recommandations business

- **Fidélisation** : la quasi-totalité de la base clients (97%) n'a acheté qu'une seule fois — le principal levier de croissance n'est pas la rétention actuelle (quasi inexistante) mais la mise en place d'un programme de fidélisation ciblant les clusters 0/1 pour les convertir en acheteurs récurrents
- **Logistique** : bien que le taux de retard global soit faible (6,8%), son impact sur la satisfaction est net — prioriser la fiabilité des délais sur les catégories à fort volume (`health_beauty`, `watches_gifts`)
- **Prévention des retards** : les frais de port élevés et les délais annoncés courts sont les signaux les plus prédictifs d'un retard — ces commandes pourraient être surveillées en priorité ou bénéficier d'une estimation de livraison plus prudente
- **Limite du modèle prédictif** : la précision de 9% sur la classe "en retard" reste faible (beaucoup de fausses alertes) ; une piste d'amélioration serait de tester un modèle plus robuste au déséquilibre des classes (Random Forest, rééquilibrage SMOTE)

## Structure du repo

```
├── data/                          # CSV sources (non versionnés, à télécharger depuis Kaggle)
├── 01_chargement_jointures.py
├── 02_eda_kpi.py
├── 03a_rfm_clustering.py
├── 03b_prediction_retard.py
└── README.md
```

## Comment reproduire

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```
1. Télécharger le dataset Kaggle et placer les CSV dans `./data`
2. Exécuter les scripts dans l'ordre (01 → 02 → 03a / 03b)
