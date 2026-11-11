# AutoInspect AI

Agent intelligent de contrôle qualité visuel pour le secteur automobile.

## Équipe

- ELASSALI Abdelkader
- Fatima Zahra Attoubi
- Aymen Sellag

Projet réalisé à l'EMSI.

## Contexte

L'inspection visuelle des pièces de carrosserie (portières, capots, ailes) est une étape critique du contrôle qualité automobile. Elle repose encore beaucoup sur l'œil humain, ce qui pose plusieurs problèmes : fatigue des opérateurs, subjectivité du diagnostic d'un opérateur à l'autre, et difficulté à mesurer précisément la surface des défauts.

## Problématique

Comment concevoir un agent intelligent de vision par ordinateur capable de détecter, segmenter et classifier les défauts de carrosserie, tout en expliquant visuellement ses décisions et en évaluant automatiquement la conformité de la pièce ?

## Objectifs

1. Constituer un dataset annoté à partir de plusieurs sources.
2. Entraîner un modèle de segmentation d'instances (YOLO-seg) pour segmenter chaque défaut au pixel près.
3. Ajouter une détection d'anomalies non supervisée (PatchCore) pour les défauts inconnus.
4. Intégrer un module d'IA explicable (Grad-CAM / LayerCAM).
5. Construire un moteur de décision : Conforme, Non conforme, Inspection manuelle.
6. Développer une API REST (FastAPI) et un dashboard (React).

## Fonctionnalités prévues

- Prétraitement des images (normalisation, redimensionnement 640x640, réduction de bruit)
- Détection et segmentation des défauts (bounding boxes + masques)
- Classification du type de défaut
- Cartes de chaleur d'explicabilité
- Décision automatique de conformité
- Rapports d'inspection en PDF et historique des analyses

## Datasets

| Dataset | Rôle |
|---|---|
| CarDD (Car Damage Detection) | Principal : 4 000 images, plus de 9 000 instances annotées |
| Car Parts and Car Damages | Secondaire : écaillage et corrosion |
| MVTec AD | Calibration du module PatchCore |

## Classes de défauts

- Dent (bosse)
- Scratch (rayure)
- Crack (fissure)
- Paint Flaking (écaillage)
- Corrosion (rouille)

## Architecture prévue

- **IA** : Ultralytics YOLO-seg (versions n et s comparées), PatchCore, Grad-CAM / LayerCAM, PyTorch
- **Backend** : FastAPI
- **Frontend** : React
- **Base de données** : MySQL

### Endpoints API prévus

| Méthode | Endpoint | Rôle |
|---|---|---|
| POST | `/api/v1/inspect` | Pipeline complet (segmentation, XAI, décision) |
| POST | `/api/v1/segment` | Masques et bounding boxes uniquement |
| POST | `/api/v1/explain` | Carte d'activation Grad-CAM |
| GET | `/api/v1/inspections` | Historique des inspections |
| GET | `/api/v1/statistics` | Statistiques globales |

## Moteur de décision

- **Conforme** : aucun défaut, ou surface totale des défauts sous le seuil toléré
- **Non conforme** : au moins un défaut critique, ou surface au-dessus de la tolérance
- **Inspection manuelle** : score de confiance ambigu ou alerte du module PatchCore

## Évaluation

- Détection : Precision, Recall, F1-score, mAP@50, mAP@50-95
- Segmentation : IoU et Dice
- Performance : temps d'inférence par image (cible < 200 ms sur GPU, < 800 ms sur CPU)

## Plan d'exécution

- [ ] Phase 1 : préparation des données (CarDD, conversion au format YOLO, contrôle des masques)
- [ ] Phase 2 : entraînement baseline et optimisation des hyperparamètres
- [ ] Phase 3 : explicabilité et moteur de décision
- [ ] Phase 4 : API backend et dashboard React
- [ ] Phase 5 : validation, calcul du mAP, documentation

## Structure du dépôt (prévue)

```
data/
notebooks/
src/
backend/
frontend/
docs/
README.md
```

