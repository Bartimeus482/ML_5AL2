# Pense-bête : les étapes d'un projet ML, de bout en bout

Checklist de référence pour ne rien oublier, du cadrage métier au suivi du
modèle en production. Deux parcours : **supervisé** (classification,
régression) et **non supervisé** (clustering, réduction de dimension). Les
deux partagent le même socle (cadrage, préparation, analyse) puis divergent
à partir de la modélisation.

Renvois utiles : `01_non_supervise/`, `02_supervise/` (TP), `slides/` decks
7 (évaluation), 7bis (explicabilité), 8 / 8bis (pipeline en pratique), 12
(MLOps).

---

## 0. Cadrage : avant d'ouvrir un notebook

Commun aux deux parcours — si cette étape est bâclée, tout le reste est fragile.

- [ ] Identifier le cas métier concret : qui a le problème, pourquoi, pour qui
- [ ] Formuler la question en une phrase ("prédire X pour décider Y")
- [ ] Décider du type de tâche : régression / classification / clustering /
      réduction de dimension
- [ ] Identifier si une cible `y` existe réellement (sinon : non supervisé)
- [ ] Lister les sources de données disponibles et leur fraîcheur
- [ ] Identifier les contraintes : réglementaires (RGPD, secteur régulé),
      délai, budget de calcul, explicabilité exigée ou non
- [ ] Définir le critère de succès côté métier (pas seulement une métrique ML)
- [ ] Identifier les parties prenantes qui utiliseront le résultat

---

## A. Parcours supervisé (classification / régression)

### A1. Collecte & préparation des données
- [ ] Rassembler les données (fichiers, base, API, entrepôt)
- [ ] Vérifier le grain des données (une ligne = une observation métier claire)
- [ ] Traiter les valeurs manquantes (imputer ou supprimer, avec parcimonie)
- [ ] Traiter les doublons
- [ ] Traiter les valeurs aberrantes (erreurs vs valeurs réelles extrêmes)
- [ ] Corriger les types et formats incohérents
- [ ] Encoder les variables catégorielles (one-hot, ordinal...)
- [ ] Cadrer explicitement `X` (les variables) et `y` (la cible à prédire)

### A2. Analyse exploratoire & visualisation
- [ ] Statistiques descriptives par variable (numériques et catégorielles)
- [ ] Distribution de la cible `y` (déséquilibre de classes ? valeurs extrêmes ?)
- [ ] Corrélations entre variables numériques (Pearson)
- [ ] Associations entre variables catégorielles (V de Cramér)
- [ ] Lien variable catégorielle / numérique (rapport de corrélation η²)
- [ ] Repérer les variables à fort risque de fuite de données (data leakage)
- [ ] Feature engineering : créer, transformer ou sélectionner des variables

### A3. Modélisation
- [ ] Séparer train / test (ex. 80/20), stratifié si classification
- [ ] Mettre en place une validation croisée pour le réglage des hyperparamètres
- [ ] Normaliser / standardiser si l'algorithme le requiert (KNN, régression
      linéaire régularisée, réseaux de neurones...)
- [ ] Entraîner une baseline simple (régression logistique / linéaire)
- [ ] Entraîner plusieurs familles d'algorithmes (arbre, forêt, boosting...)
- [ ] Régler les hyperparamètres clés (grid search / random search)
- [ ] Surveiller l'écart train/test à chaque étape (diagnostic overfitting)

### A4. Évaluation
- [ ] Choisir la métrique adaptée à la tâche et au coût d'erreur métier
      (classification : précision / rappel / F1 / ROC-AUC ; régression :
      MAE / RMSE / R²)
- [ ] Évaluer uniquement sur le jeu de test, jamais sur le train
- [ ] Matrice de confusion (classification) ou analyse des résidus (régression)
- [ ] Courbes de validation / d'apprentissage (biais vs variance)
- [ ] Comparer tous les modèles candidats dans un tableau récapitulatif

### A5. Explicabilité
- [ ] Identifier si le meilleur modèle est nativement interprétable
      (linéaire, arbre peu profond) ou boîte noire (forêt, boosting, réseau)
- [ ] Si boîte noire : calculer les valeurs de Shapley (SHAP)
- [ ] Lire l'explicabilité globale (summary plot / feature importance)
- [ ] Lire l'explicabilité locale sur quelques prédictions individuelles
      (waterfall / force plot)
- [ ] Vérifier que les variables influentes ont un sens métier (pas un
      artefact ou une fuite de données)

### A6. Choix du modèle final
- [ ] Arbitrer sur trois critères, pas un seul : erreur minimale, performance
      maximale, explicabilité suffisante
- [ ] Vérifier si le gain de performance d'un modèle boîte noire est net ou
      marginal face à un modèle interprétable
- [ ] Documenter la décision et sa justification (pas seulement le score)

### A7. Stockage & déploiement
- [ ] Sauvegarder le modèle final (`joblib.dump` pour scikit-learn,
      `torch.save(model.state_dict(), ...)` pour PyTorch)
- [ ] Sauvegarder séparément le `scaler` / préprocesseur si le modèle n'est
      pas encapsulé dans un `Pipeline`
- [ ] Versionner le modèle (nom de fichier, tag, date, ou registre de modèles)
- [ ] Écrire un script d'inférence simple : charger, prétraiter, prédire
- [ ] Sauvegarder les prédictions (CSV, base, file de messages...)
- [ ] Documenter le format d'entrée / sortie attendu par le modèle

### A8. Cycle de vie en production
- [ ] Surveiller la dérive des données (data drift) : les nouvelles données
      ressemblent-elles encore aux données d'entraînement ?
- [ ] Surveiller la dérive du concept (concept drift) : la relation X → y
      a-t-elle changé ?
- [ ] Suivre la performance en production dans le temps (dashboard, alertes)
- [ ] Journaliser les prédictions pour audit et ré-entraînement futur
- [ ] Planifier un ré-entraînement périodique ou déclenché par seuil
- [ ] Prévoir un plan de rollback vers la version précédente du modèle
- [ ] Réévaluer périodiquement le critère de succès métier (l'objectif de
      départ est-il toujours le bon ?)

---

## B. Parcours non supervisé (clustering / réduction de dimension)

### B1. Collecte & préparation des données
- [ ] Rassembler les données
- [ ] Traiter les valeurs manquantes, doublons, types incohérents
- [ ] Traiter les valeurs aberrantes
- [ ] Encoder les variables catégorielles si l'algorithme le requiert
- [ ] Cadrer `X` uniquement : il n'y a pas de cible `y`
- [ ] Normaliser / standardiser les variables (encore plus critique qu'en
      supervisé : le clustering est sensible à l'échelle)

### B2. Analyse exploratoire & visualisation
- [ ] Statistiques descriptives et distributions par variable
- [ ] Corrélations entre variables (redondance à traiter avant clustering)
- [ ] Réduction de dimension (PCA) pour un premier aperçu visuel des données
- [ ] Repérer à l'œil nu d'éventuels groupes ou anomalies

### B3. Modélisation
- [ ] Choisir un ou plusieurs algorithmes adaptés (K-means, DBSCAN, CAH...)
- [ ] Pas de split train/test : l'algorithme s'entraîne sur tout le jeu de
      données
- [ ] Choisir le nombre de clusters `k` (méthode du coude, score de
      silhouette) ou les hyperparamètres de densité (DBSCAN : eps,
      min_samples)
- [ ] Tester plusieurs algorithmes : ils peuvent donner des segmentations
      différentes, toutes potentiellement valides
- [ ] Détecter les anomalies / points de bruit si pertinent (Isolation
      Forest, DBSCAN)

### B4. Évaluation
- [ ] Score de silhouette (cohésion et séparation des clusters)
- [ ] Inertie intra-classe (à minimiser) et inter-classe (à maximiser)
- [ ] Si un label caché existe (a posteriori) : comparer avec ARI / NMI
- [ ] Garder à l'esprit que sans étiquette, l'interprétation métier compte
      autant que le score numérique

### B5. Visualisation des clusters
- [ ] Projection 2D (PCA, t-SNE) colorée par cluster
- [ ] Vérifier visuellement que les clusters ont du sens et sont bien séparés
- [ ] Comparer les profils moyens de chaque cluster (boxplots par groupe)
- [ ] Identifier les variables les plus discriminantes entre clusters

### B6. Intégration
- [ ] Rattacher un identifiant de cluster à chaque observation
- [ ] Injecter ce `cluster_id` dans le SI, le CRM ou l'entrepôt de données
- [ ] Rendre le résultat exploitable par d'autres équipes (pas seulement
      dans un notebook)

### B7. Qualification
- [ ] Analyser le profil de chaque groupe (moyennes, variables
      discriminantes, effectifs)
- [ ] Donner un nom métier explicite à chaque cluster
- [ ] Vérifier que chaque groupe est bien distinct et actionnable
- [ ] Faire valider les segments par les experts métier

### B8. Prise de décision
- [ ] Traduire chaque segment en action concrète (offre, message, priorité,
      seuil de traitement)
- [ ] Partager les segments qualifiés avec les parties prenantes
- [ ] Mesurer l'impact des actions issues de la segmentation
- [ ] Documenter la décision et les hypothèses associées à chaque segment

### B9. Cycle de vie en production
- [ ] Surveiller la dérive des données : la population change-t-elle avec
      le temps (nouveaux clients, nouveaux comportements) ?
- [ ] Réévaluer périodiquement si le nombre de clusters `k` est toujours
      pertinent
- [ ] Planifier un ré-entraînement / une re-segmentation périodique
- [ ] Suivre la stabilité des segments dans le temps (un client change-t-il
      souvent de cluster ?)
- [ ] Réévaluer périodiquement le critère de succès métier

---

## Résumé visuel : ce qui diffère

| Étape | Supervisé | Non supervisé |
|---|---|---|
| Cible `y` | Oui, à prédire | Non, aucune étiquette |
| Split train/test | Oui (80/20 typique) | Non, tout le dataset sert à l'entraînement |
| Choix du modèle | Erreur + performance + explicabilité | Silhouette + inertie + sens métier |
| Résultat brut | Une prédiction (valeur ou classe) | Un identifiant de cluster |
| Étape propre à ce parcours | Explicabilité (SHAP), déploiement/inférence | Intégration, qualification, prise de décision |
| Suivi en production | Dérive des données et du concept, réentraînement | Dérive de la population, stabilité des segments, re-segmentation |
