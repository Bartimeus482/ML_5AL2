# TP à trous : exercices

Versions "à compléter" des notebooks de chaque TP. Même structure, même
explication de cours : seules les cellules marquées `# TODO` ont leur code retiré
(remplacé par un `...` et un indice en commentaire). Tout le reste (imports,
chargement des données, affichages, graphiques) est déjà fourni et fonctionne
tel quel.

Pour les TP 01, 02, 04 et 05, les données ont été remplacées par des jeux de
données différents et plus conséquents que dans les corrigés (voir tableau
ci-dessous) : même technique enseignée, données différentes, pour éviter le
copier-coller depuis le corrigé et forcer une vraie compréhension. Les TP 03
(renforcement) et 06 (prompt engineering) sont inchangés (pas de dataset
tabulaire à remplacer).

## Installation

Un seul environnement pour tous les exercices. Depuis `cours_ml/todo/` :

```bash
python3.13 -m venv .venv
source .venv/bin/activate        # Windows : .venv\Scripts\activate
pip install -r requirements.txt
python -m ipykernel install --user --name=tp-exercices --display-name "TP Exercices"
```

Puis ouvre les notebooks avec le kernel **TP Exercices** (sélectionnable dans
Jupyter/VSCode). Chaque notebook rappelle en première cellule le TP dont il
s'inspire.

Pour `06_prompt_engineering/tp_prompt_engineering.ipynb` : crée un fichier
`.env` dans ce même dossier avec `ANTHROPIC_API_KEY=...` pour utiliser l'API
Claude, sinon le notebook bascule automatiquement sur un modèle local
(Qwen2.5-1.5B-Instruct, téléchargé au premier lancement).

## Correspondance avec les corrigés

| Dossier | Notebook(s) d'exercice | Corrigé complet | Dataset utilisé ici |
|---|---|---|---|
| `01_non_supervise/` | `tp_non_supervise.ipynb` | `cours_ml/01_non_supervise/` | Wine Quality (rouge) — 1599 vins, 11 variables physico-chimiques (`fetch_openml`) |
| `02_supervise/` | `tp_classification.ipynb` | `cours_ml/02_supervise/` | Spambase — 4601 emails, 57 variables, spam/non-spam (`fetch_openml`) |
| — | `tp_regression.ipynb` | — | California Housing — 20640 logements, 8 variables (`sklearn.datasets`) |
| `03_renforcement/` | `tp_reinforcement.ipynb` | `cours_ml/03_renforcement/` | Environnements Gymnasium (inchangé) |
| `04_deep_learning/` | `tp_deep_learning.ipynb` | `cours_ml/04_deep_learning/` | MLP : MagicTelescope — 19020 lignes, 10 variables (`fetch_openml`) |
| — | — | — | CNN : Fashion-MNIST — 70000 images 28×28, 10 classes de vêtements |
| `05_preparation_donnees/` | `tp_dataprep_1_propre.ipynb`, `tp_dataprep_2_sale.ipynb`, `tp_dataprep_3_analyse_visualisation.ipynb` | `cours_ml/05_preparation_donnees/` | Adult Census Income — 3000 individus (`adults_clean.csv`/`adults_dirty.csv`) |
| `06_prompt_engineering/` | `tp_prompt_engineering.ipynb` | `cours_ml/06_prompt_engineering/` | Pas de dataset (inchangé) |

Pour les TP 01, 02 et 04, les données se téléchargent automatiquement à
l'exécution (via `fetch_openml` ou `sklearn.datasets`, aucune authentification
requise). `05_preparation_donnees/` embarque ses deux CSV sources
(`adults_clean.csv`, `adults_dirty.csv`, dérivés du dataset Adult Census
Income) : c'est le seul TP qui charge des données locales plutôt que depuis
sklearn/OpenML. Le TP2 (`tp_dataprep_2_sale.ipynb`) produit en sortie
`adults_cleaned.csv`, repris ensuite par le TP3.

## Ce qui a été laissé à compléter

Les cellules d'installation, de chargement des données et la plupart des
graphiques sont fournis tels quels : l'objectif n'est pas de retaper du code
matplotlib, mais de pratiquer la technique ML elle-même (standardisation,
entraînement d'un modèle, calcul d'une métrique, recherche d'hyperparamètres,
implémentation d'une formule...). Chaque `# TODO` indique ce qu'il faut produire
et donne un indice sur la fonction/classe à utiliser.

`02_supervise/` (classification et régression) et la partie MLP de
`04_deep_learning/` vont plus loin que la comparaison de modèles : ils ajoutent
une section **explicabilité, choix du modèle final, stockage et inférence**
(après la section 9/1.5) qui couvre :

- l'explicabilité d'un modèle boîte noire avec les **valeurs de Shapley**
  (`shap.TreeExplainer` pour les modèles à base d'arbres, `shap.Explainer` +
  fonction `predict` enveloppée pour le MLP PyTorch) ;
- le choix du modèle final en arbitrant **erreur**, **performance** et
  **explicabilité**, pas seulement la métrique la plus haute ;
- la sauvegarde du modèle retenu (`joblib.dump` pour scikit-learn,
  `torch.save(model.state_dict(), ...)` pour PyTorch) ;
- une inférence simple, sans API, sur de nouvelles données, avec sauvegarde
  des prédictions dans un CSV.

`06_prompt_engineering` fait exception : le TP original contient déjà 5
exercices guidés (`# TODO`) intercalés avec des démonstrations, sa structure
n'a donc pas été modifiée.

## Notebooks non exécutés

Ces notebooks sont fournis sans sorties (pas encore exécutés) : lance-les
cellule par cellule après avoir complété chaque `# TODO`.

## Session à rendre

Chaque notebook se termine par une section **"Session à rendre"** : une série de
questions à répondre directement dans le notebook (une cellule markdown *Réponse*
vide sous chaque question), une fois les `# TODO` complétés et le notebook exécuté.
Contrairement aux `# TODO`, il n'y a pas de réponse universelle : les questions
portent sur les résultats propres à ton exécution (hyperparamètres retenus, scores
obtenus, lecture d'un graphique...), pour vérifier que le TP a bien été réalisé et
compris, pas seulement recopié depuis le corrigé.
