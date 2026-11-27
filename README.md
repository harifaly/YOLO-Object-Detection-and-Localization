# YOLOv8-VisDrone-Object-Detection

## Projet final - Réseaux de neurones : Vision par ordinateur

**Sujet :** Étude comparative de l'influence de la résolution d'entrée et de la taille du modèle YOLOv8 sur la détection d'objets dans des images aériennes.

---

## 1. Présentation du projet

Ce projet porte sur la détection automatique d'objets dans des images aériennes à l'aide des réseaux de neurones convolutifs et, plus particulièrement, de la famille de modèles YOLOv8.
L'architecture YOLO est beaucoup plus rapide et permet d'atteindre des vitesses de traitement allant jusqu'à 45 images par seconde. Ce modèle propose de prédire les "bounding box" et les probabilités d'appartenance à une classe directement grâce à un unique réseau de neurones.

L'étude utilise le dataset **VisDrone2019-DET**, qui contient des images acquises par des drones et présente plusieurs difficultés, notamment la petite taille des objets, leur densité et les variations des conditions de prise de vue.

L'objectif principal est d'étudier l'influence de deux paramètres sur les performances de détection :

- La résolution des images en entrée du modèle.
- La taille du modèle YOLOv8 (Nano et Small).

Trois expériences ont été réalisées afin de comparer les performances de détection, la précision et le coût de calcul.

## 2. Objectifs

Les objectifs de ce projet sont les suivants :

- Comprendre le fonctionnement de YOLOv8 pour la détection d'objets.
- Étudier l'influence de la résolution d'entrée sur les performances de détection.
- Comparer deux tailles de modèles : YOLOv8n et YOLOv8s.
- Évaluer les performances à l'aide des métriques Precision, Recall, mAP@50 et mAP@50-95.
- Analyser les résultats par classe et selon la taille des objets.
- Étudier le compromis entre les performances de détection et le temps de calcul.

## 3. Dataset utilisé

Le projet utilise le dataset **VisDrone2019-DET**, destiné à la détection d'objets dans des images aériennes capturées par drone.

Le jeu de données comporte 10 catégories d'objets :

| ID | Classe |
|---|---|
| 0 | Pedestrian |
| 1 | People |
| 2 | Bicycle |
| 3 | Car |
| 4 | Van |
| 5 | Truck |
| 6 | Tricycle |
| 7 | Awning-tricycle |
| 8 | Bus |
| 9 | Motor |

### Répartition des données

| Ensemble | Nombre d'images |
|---|---:|
| Entraînement initial | 6 471 |
| Sous-ensemble utilisé pour l'entraînement | 2 000 |
| Validation | 548 |
| Test | Non utilisé |

Pour garantir une comparaison équitable, le même sous-ensemble de 2 000 images d'entraînement et le même ensemble de validation ont été utilisés pour les trois expériences.

Le dataset n'est pas inclus dans ce dépôt en raison de sa taille. Il doit être téléchargé et préparé séparément.

**Source du dataset :**  
https://github.com/VisDrone/VisDrone-Dataset

## 4. Expériences réalisées

Trois configurations expérimentales ont été étudiées :

| Expérience | Modèle | Résolution | Époques |
|---|---|---:|---:|
| E0 | YOLOv8n | 640 × 640 | 30 |
| E1 | YOLOv8n | 960 × 960 | 30 |
| E2 | YOLOv8s | 640 × 640 | 30 |

### Paramètres expérimentaux

- Modèles pré-entraînés sur COCO.
- Nombre d'époques : 30.
- Batch size : 16.
- Seed : 0.
- Même sous-ensemble d'entraînement pour les trois expériences.
- Même ensemble de validation.
- Environnement d'entraînement : Google Colab avec GPU NVIDIA Tesla T4.

L'objectif est de comparer l'effet de la résolution d'entrée (E0 contre E1), puis l'effet de la taille du modèle (E0 contre E2).

## 5. Résultats et comparaison

### 5.1. Indicateurs globaux

Les performances obtenues sur l'ensemble de validation sont présentées dans le tableau suivant :

| Métrique | E0 – YOLOv8n 640 | E1 – YOLOv8n 960 | E2 – YOLOv8s 640 |
|---|---:|---:|---:|
| Precision | 0.367 | 0.458 | 0.449 |
| Recall | 0.285 | 0.363 | 0.337 |
| mAP@50 | 0.245 | 0.345 | 0.318 |
| mAP@50-95 | 0.134 | 0.199 | 0.179 |
| Temps d'entraînement | 24.0 min | 38.8 min | 26.8 min |

### 5.2. Interprétation des résultats

Les résultats montrent que l'expérience E1, utilisant YOLOv8n avec une résolution de 960 × 960, obtient les meilleures performances sur les quatre métriques évaluées.

Par rapport à E0, l'augmentation de la résolution améliore :

- La précision de détection.
- Le rappel des objets détectés.
- La qualité globale de détection mesurée par les deux indicateurs mAP.

L'expérience E2, utilisant YOLOv8s à 640 × 640, améliore également les performances par rapport à E0. Cependant, elle reste inférieure à E1 sur les quatre métriques.

Ces résultats suggèrent que, dans les conditions expérimentales étudiées, l'augmentation de la résolution d'entrée apporte un gain plus important que l'augmentation de la taille du modèle.

### 5.3. Analyse par classe

Une comparaison des performances mAP@50 a également été réalisée pour les dix catégories d'objets.

Les résultats montrent que les classes telles que Car et Bus sont globalement mieux détectées que les classes Bicycle et Awning-tricycle.

L'expérience E1 présente les meilleures performances pour chacune des dix catégories par rapport aux deux autres expériences.

### 5.4. Analyse selon la taille des objets

Une analyse complémentaire a été réalisée pour étudier l'influence de la résolution sur la détection des objets de petite, moyenne et grande taille.

| Taille des objets | E0 – 640 | E1 – 960 |
|---|---:|---:|
| Small | 23.09 % | 34.36 % |
| Medium | 64.77 % | 73.65 % |
| Large | 84.08 % | 87.64 % |

Cette analyse montre que l'augmentation de la résolution améliore particulièrement le rappel des petits objets, qui représentent la majorité des objets présents dans l'ensemble de validation.

**Remarque :** ces valeurs correspondent à une analyse personnalisée du rappel selon la taille des objets, et non à la métrique officielle mAP par taille. Cette analyse concerne uniquement E0 et E1.

## 6. Coût de calcul

Les trois expériences ont également été comparées en fonction de leur coût de calcul.

| Indicateur | E0 – YOLOv8n 640 | E1 – YOLOv8n 960 | E2 – YOLOv8s 640 |
|---|---:|---:|---:|
| Paramètres | 3.13 M | 3.13 M | 11.13 M |
| GFLOPs à 640 | 8.6 | 8.6 | 28.5 |
| Temps d'entraînement | 24.0 min | 38.8 min | 26.8 min |
| Inférence GPU (ms/image) | 3.5 | 5.4 | 4.8 |

L'augmentation de la résolution améliore les performances de détection, mais entraîne également une augmentation du temps de calcul.

Les mesures d'inférence GPU sont indicatives et dépendent des conditions d'exécution. Les GFLOPs indiqués correspondent à la résolution 640 ; ils ne représentent pas une mesure à 960.

## 7. Conclusion

Cette étude comparative montre que la résolution d'entrée joue un rôle important dans les performances de détection d'objets dans les images aériennes.

Parmi les trois configurations étudiées, YOLOv8n avec une résolution de 960 × 960 obtient les meilleurs résultats sur les métriques Precision, Recall, mAP@50 et mAP@50-95.

L'analyse selon la taille des objets montre également une amélioration particulièrement importante pour les petits objets.

Toutefois, cette amélioration nécessite un temps d'entraînement et d'inférence plus élevé. Le choix de la configuration dépend donc du compromis recherché entre précision et coût de calcul.

### Rapport

Le rapport complet est dans [`report/Object_detection_project.pdf`](report/Object_detection_project.pdf) (source LaTeX : `report/main.tex`). Il est organisé en deux grandes parties :

1. **Rappel théorique** : réseaux de neurones convolutifs, R-CNN, principe de YOLO et ses évolutions, métriques d'évaluation, puis présentation de YOLOv8.
2. **Partie expérimentale** : matériel et méthodes, résultats des trois expériences (E0, E1, E2), discussion, limites et conclusion.

## 8. Organisation du dépôt

```text
YOLOv8-VisDrone-Object-Detection/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   ├── 01_E0_E1_YOLOv8n_640_vs_960.ipynb
│   └── 02_E2_YOLOv8s_640_et_comparaison.ipynb
│
├── experiments/
│   ├──  e0_640/
│   ├──  e1_960-2/
│   └──  e2_s640/
│
├── results/
│   ├── comparison_graphs/
│   ├── validation/
│   ├── size_analysis/
│   └── tables/
│
├── configuration/
│   └── data.yaml
│
└── report/
    ├── main.tex
    ├── figures/
    └── Object_detection_project.pdf
```

### Description des dossiers

- **notebooks/** : notebooks utilisés pour l'entraînement, l'évaluation et la comparaison des modèles.
- **experiments/** : résultats et fichiers générés lors des trois expériences.
- **results/** : graphiques comparatifs, résultats de validation, analyses par taille et tableaux.
- **configuration/** : fichier YAML de configuration du dataset.
- **report/** : rapport scientifique final au format LaTeX et PDF.

## 9. Installation et utilisation

Le projet peut être exécuté dans deux environnements : un ordinateur local (Windows/Linux) ou Google Colab.

### 9.1. Prérequis

- Python 3.10 ou version compatible avec les bibliothèques utilisées.
- Git.
- Jupyter Notebook ou Visual Studio Code.
- Une connexion Internet pour télécharger les dépendances et le dataset.
- Un GPU NVIDIA est recommandé pour accélérer l'entraînement.

### 9.2. Cloner le dépôt GitHub

Ouvrir un terminal et exécuter :

```bash
git clone https://github.com/harifaly/YOLO-Object-Detection-and-Localization.git

cd YOLO-Object-Detection-and-Localization
```


### 9.3. Créer un environnement virtuel

**Sous Windows :**

```bash
python -m venv venv
venv\Scripts\activate
```

**Sous Linux :**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 9.4. Installer les dépendances

```bash
pip install -r requirements.txt
```

Pour une utilisation avec un GPU NVIDIA, installer une version de PyTorch compatible avec la version de CUDA et le matériel disponible. Consulter la documentation officielle :

https://pytorch.org/get-started/locally/

### 9.5. Télécharger et préparer le dataset

Télécharger VisDrone2019-DET depuis :

https://github.com/VisDrone/VisDrone-Dataset

Préparer ensuite les images et les annotations au format YOLO, avec les classes et les partitions correspondant à l'expérience.

Le dataset doit être organisé de manière à correspondre aux chemins définis dans le fichier `configuration/data.yaml`.

**Important :** les annotations VisDrone d'origine ne sont pas directement au format YOLO. Une conversion et une préparation des données sont nécessaires avant l'entraînement.

### 9.6. Configurer le fichier data.yaml

Modifier les chemins du fichier `configuration/data.yaml` selon l'emplacement du dataset sur l'ordinateur.

Exemple sous Windows :

```yaml
path: C:/Users/Utilisateur/Datasets/VisDrone
train: images/train
val: images/val
test: images/test

names:
  0: pedestrian
  1: people
  2: bicycle
  3: car
  4: van
  5: truck
  6: tricycle
  7: awning-tricycle
  8: bus
  9: motor
```

Sous Linux, remplacer le chemin par le chemin réel du dataset, par exemple :

```yaml
path: /home/utilisateur/Datasets/VisDrone
```

Les chemins utilisés directement dans les notebooks doivent également être adaptés à l'environnement local.

### 9.7. Exécuter les notebooks

Ouvrir le projet avec Jupyter Notebook ou Visual Studio Code.

Exécuter les notebooks dans l'ordre :

1. `01_E0_E1_YOLOv8n_640_vs_960.ipynb`
2. `02_E2_YOLOv8s_640_et_comparaison.ipynb`

Les résultats et les fichiers générés peuvent être enregistrés dans les dossiers correspondants.

### 9.8. Utilisation avec Google Colab

Les notebooks peuvent également être exécutés sur Google Colab.

Pour cela :

1. Ouvrir Google Colab.
2. Importer les notebooks depuis le dépôt GitHub ou les télécharger.
3. Préparer le dataset dans l'environnement Colab.
4. Installer les dépendances nécessaires.
5. Adapter les chemins des fichiers et du dataset.
6. Exécuter les cellules dans l'ordre.

Pour les expériences originales, l'entraînement a été réalisé sur Google Colab avec un GPU NVIDIA Tesla T4.

## Environnement de développement et d'exécution (REMARQUES)

Ce projet a été développé et exécuté principalement sur **Google Colab**, en utilisant un environnement Python avec accélération GPU (NVIDIA Tesla T4) pour l'entraînement et l'évaluation des modèles YOLOv8.

Les notebooks disponibles dans le dossier `notebooks/` permettent de consulter les différentes étapes de préparation des données, d'entraînement, d'évaluation et de comparaison des trois expériences.

**Remarque :** les expériences présentées dans ce dépôt ont été réalisées sur Google Colab. Leur reproduction nécessite de disposer du dataset VisDrone2019-DET préparé au format YOLO, ainsi que des fichiers nécessaires à chaque expérience. Les chemins d'accès aux données et aux modèles doivent être adaptés à l'environnement d'exécution utilisé.

Les notebooks peuvent également être adaptés pour une exécution dans un environnement Python local, sous Windows ou Linux, à condition de configurer correctement les dépendances, les chemins des données et les ressources matérielles.

## 10. Reproductibilité et limites

Les résultats présentés dans ce dépôt correspondent aux expériences effectivement réalisées.

Quelques limites doivent être prises en compte :

- Les entraînements ont été réalisés sur un sous-ensemble de 2 000 images.
- Les modèles ont été entraînés pendant 30 époques.
- Une seule valeur de seed a été utilisée.
- L'ensemble de test n'a pas été utilisé pour cette étude.
- L'analyse personnalisée du rappel selon la taille des objets concerne uniquement E0 et E1.
- Les temps de calcul peuvent varier selon le matériel et l'environnement d'exécution.

Les résultats obtenus lors d'une nouvelle exécution peuvent donc présenter de légères différences.

## 11. Technologies utilisées

- Python
- PyTorch
- Ultralytics YOLOv8
- OpenCV
- NumPy
- Pandas
- Matplotlib
- Jupyter Notebook
- Google Colab

## 12. Références

1. Redmon, J., Divvala, S., Girshick, R., & Farhadi, A. (2016). *You Only Look Once: Unified, Real-Time Object Detection.*
2. Zhu, P., et al. (2020). *Detection and Tracking Meet Drones Challenge.*
3. Ultralytics Documentation: https://docs.ultralytics.com/
4. VisDrone Dataset: https://github.com/VisDrone/VisDrone-Dataset

## 13. Auteurs

**Projet réalisé dans le cadre du cours : Réseaux de neurones - Vision par ordinateur.**

- Étudiant 1 : ANDRIAMIRINDRAMANANA Harifaly Lunard
- Étudiant 2 : RAKOTOARIMANANA Joy Sandra Hasina

**Année universitaire :2025-2026**

**Parcours MAFI & Calcul Numérique**

---

*Ce dépôt présente les expériences, les résultats et les analyses réalisés dans le cadre de notre projet final sur la détection d'objets avec YOLOv8 et le dataset VisDrone2019-DET.*
