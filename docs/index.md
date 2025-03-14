# Documentation

[https://github.com/allisterk2703/mlops-project](https://github.com/allisterk2703/mlops-project)

### Team Members

Allister KOHN
[allister.kohn@student-cs.fr](mailto:allister.kohn@student-cs.fr)

Elizaveta VASILEVA
[elizaveta.vasileva@student-cs.fr](mailto:elizaveta.vasileva@student-cs.fr)

Enzo PALOS
[enzo.palos@student-cs.fr](mailto:enzo.palos@student-cs.fr)

Jinsuh YOU
[jinsuh.you@student-cs.fr](mailto:jinsuh.you@student-cs.fr)

---

### Introduction

The project proposed to us as part of our MLOps course aims to design an MLOps workflow to industrialize the lifecycle of Machine Learning models. A classic lifecycle includes data ingestion, model training, evaluation, and deployment. The goal is to identify and implement key features to facilitate the management, scalability, and reproducibility of models in production. The project is divided into two parts:

1. Write a specification detailing the chosen workflows and features
2. Implement the corresponding code, to be hosted on GitHub.

The objective is to structure a modular, clear, and reusable system, adopting a pragmatic and robust approach.

To this end, the GitHub repository [dsba-platform](https://github.com/JoachimZ/dsba-platform) is a relevant starting point, as it offers a modular architecture that integrates the essential steps of an MLOps workflow. It also includes a CLI, an API (with FastAPI), and a Dockerfile, facilitating containerization, integration, and deployment to the cloud. Furthermore, its structure easily allows for adding advanced features that ensure a smooth and reproducible workflow.

The code in this repository is suited to **binary classification** tasks, like the famous **Titanic** dataset, which predicts passenger survival based on their characteristics. Consequently, we have chosen to focus on a tool intended exclusively for binary classification tasks in order to remain focused on a clear and well-controlled scope.

---

### Tutorial: forker et configurer le projet pour la première fois

[https://github.com/JoachimZ/dsba-platform](https://github.com/JoachimZ/dsba-platform)

- Cliquer sur `Fork` depuis la page GitHub, puis sur `Create fork`
- Récupérer le lien du dépôt forké dans son propre espace GitHub
- Dans un terminal, exécuter la commande : `cd Desktop && git clone <repo_url>`
- Toujours dans le terminal, exécuter les commandes (remplacer par le chemin où est situé le dossier `dsba-platform`) :
    
    `echo 'export PYTHONPATH="$PYTHONPATH:/Users/allisterkohn/Desktop/dsba-platform/src"' >> ~/.zshrc`
    
    `echo "export DSBA_MODELS_ROOT_PATH='/Users/allisterkohn/Desktop/dsba-platform/models'" >> ~/.zshrc`
    
- Ouvrir VS Code, aller dans `*Fichier > Ouvrir le dossier`* et ouvrir le dossier `dsba-platform`
- Taper `Control ^` + `Shift ⇧` + `<` pour ouvrir un terminal dans VS Code
- Créer un environnement virtuel *.venv* et installer les packages nécessaires en exécutant successivement les commandes suivantes :
    - `python -m venv .venv`
    - `echo ".venv/" >> .gitignore`
    - `source .venv/bin/activate`
    - `pip install hatch`
    - `hatch env create`
    - `pip install -e .`
- Vérifier que tout fonctionne en exécutant dans le terminal de VS Code la commande : `python src/cli/dsba_cli list`

---

## Setup du dépôt forké

- Ouvrir le projet dans VS Code, puis taper `Control ^` + `Shift ⇧` + `<` pour ouvrir un terminal dans VS Code
- Créer un environnement virtuel *.venv* et installer les packages nécessaires en exécutant successivement dans le terminal lancé précédemment les commandes suivantes :
    - `python -m venv .venv`
    - `source .venv/bin/activate`
    - `pip install hatch`
    - `hatch env create`
    - `pip install -e .`
- Créer un fichier `.env` à la racine du projet et y insérer les éléments suivants :
    
    https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Feu-west-3.console.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26region%3Deu-west-3%26state%3DhashArgsFromTB_eu-west-3_f832d75c15eb7d4a&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&forceMobileApp=0&code_challenge=bE_YlZZ6wzlaVIe1sWQhQrBRCIvTRRUlpOTwVqOJmFI&code_challenge_method=SHA-256
    
    ```bash
    AWS_ACCESS_KEY=<YOUR_AWS_ACCESS_KEY>
    AWS_SECRET_KEY=<YOUR_AWS_SECRET_KEY>
    AWS_REGION=<YOUR_AWS_REGION>
    S3_BUCKET_NAME=<YOUR_S3_BUCKET_NAME>
    ```
    

---

## Features

### Ajouter un dataset

Permet d’ajouter un dataset à l’outil.

- **Paramètres :**
    - `--name` (obligatoire) : Nom attribué au dataset lors de son ajout.
    - `--file` : Chemin vers un fichier local contenant le dataset.
    - `--url` : URL d’une source distante pour récupérer le dataset.
    - `--new` : Indique qu'il s'agit d'un **nouveau dataset** (à ne pas inclure pour une nouvelle version d’un dataset existant).
    
    Remarques :
    
    - L'un des paramètres `--file` ou `--url` doit être spécifié, mais pas les deux simultanément.
    - Le flag `--new` est nécessaire pour ajouter un dataset qui n’existe pas encore.
- **Exemples :**
    - Ajouter un dataset depuis un fichier local :
        
        ```bash
        python src/cli/dsba_cli save_dataset --name titanic --file /Users/allisterkohn/Desktop/titanic.csv
        ```
        
    - Ajouter un dataset depuis une URL :
        
        ```bash
        python src/cli/dsba_cli save_dataset --name titanic --url https://www.kaggle.com/api/v1/datasets/download/yasserh/titanic-dataset
        ```
        

### Afficher tous les datasets locaux

Affiche la liste de tous les datasets enregistrés localement sur la machine.

- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli list_local_datasets
    ```
    

### Afficher les versions d’un dataset local spécifique

Permet de lister toutes les versions d’un dataset spécifique stocké localement.

- **Paramètre :**
    - `--dataset` (obligatoire) : Nom du dataset.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli list_dataset_versions --dataset titanic
    ```
    

### Afficher tous les datasets S3

Affiche la liste des datasets stockés dans le bucket S3 `dsba-mlops-project-bucket`, disponibles pour téléchargement.

- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli list_s3_datasets
    ```
    

### Afficher les versions d’un dataset S3 spécifique

…

### Télécharger un dataset de S3

Permet de récupérer un dataset stocké dans le bucket S3 `dsba-mlops-project-bucket` et de le télécharger dans l’environnement local.

- **Paramètre :**
    - `--s3_filename` (obligatoire) : Nom du fichier tel qu’il est retourné par la commande `list_s3_datasets`.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli download_dataset_from_S3 --s3_filename titanic/titanic_v1_2025-03-06_22-29.csv
    ```
    

### Prétraiter un dataset

Effectue un prétraitement sur un dataset en remplissant les valeurs manquantes selon le mode spécifié et en supprimant les colonnes inutiles.

- **Paramètres :**
    - `--dataset` (obligatoire) : Chemin vers le dataset à prétraiter.
    - `--target` (obligatoire) : Nom de la variable cible.
    - `--mode` (obligatoire) : Mode de remplissage des valeurs manquantes (modes possibles : `mean`, `median`, `most_frequent`, `constant`)
    - `--useless` (optionnel) : Liste des colonnes à exclure du dataset (séparées par un espace).
    
    Remarque :
    
    - Si `--useless` n'est pas spécifié, aucune colonne n'est supprimée.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli preprocess --dataset titanic/titanic_v1_2025-03-01_14-29.csv --target Survived --mode mean --useless PassengerId Name Ticket
    ```
    

### Entraîner un modèle

Permet d’entraîner un modèle après prétraitement du dataset.

- **Paramètres :**
    - `--dataset` (obligatoire) : Chemin vers le dataset prétraité.
    - `--target` (obligatoire) : Nom de la variable cible.
    - `--model` (obligatoire) : Modèle d’apprentissage à utiliser (modèles possibles : `xgboost`, `random_forest`, `logistic_regression`, `svm`, `decision_tree`, `all`)
    - `--gridsearch` (optionnel, `False` par défaut) : Indique si un GridSearch doit être utilisé pour trouver les meilleurs hyperparamètres.
- **Exemple :**
    - Entraîner un modèle XGBoost avec GridSearch :
        
        ```bash
        python src/cli/dsba_cli train --dataset preprocessed_datasets/titanic/titanic_v1_2025-03-01_14-29 --target Survived --model xgboost --gridsearch
        ```
        
    - Entraîner tous les modèles disponibles sans GridSearch :
        
        ```bash
        python src/cli/dsba_cli train --dataset preprocessed_datasets/titanic/titanic_v1_2025-03-01_14-29 --target Survived --model all
        ```
        

### Prétraiter un dataset et entraîner un modèle

Effectue le prétraitement d’un dataset et entraîne un modèle en une seule commande.

- **Paramètres :**
    - `--dataset` (obligatoire) : Chemin vers le dataset à prétraiter.
    - `--target` (obligatoire) : Nom de la variable cible.
    - `--mode` (obligatoire) : Mode de remplissage des valeurs manquantes (modes possibles : `mean`, `median`, `most_frequent`, `constant`)
    - `--useless` (optionnel) : Liste des colonnes à exclure du dataset (séparées par un espace).
    - `--model` (obligatoire) : Modèle d’apprentissage à utiliser (modèles possibles : `xgboost`, `random_forest`, `logistic_regression`, `svm`, `decision_tree`, `all`)
    - `--gridsearch` (optionnel, `False` par défaut) : Indique si un GridSearch doit être utilisé pour trouver les meilleurs hyperparamètres.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli preprocess_and_train --dataset titanic/titanic_v1_2025-03-01_14-29.csv --target Survived --model xgboost --mode mean
    ```
    

### Afficher les modèles disponibles

Affiche la liste des modèles associés à un dataset spécifique.

- **Paramètre :**
    - `--dataset` (obligatoire) : Nom du dataset.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli list_models --dataset titanic
    ```
    

### Comparer les modèles

Affiche les métriques des modèles.

- **Paramètre :**
    - `--dataset` (obligatoire) : Nom du dataset.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli compare_models --dataset titanic
    ```
    

### Trouver le meilleur modèle pour un dataset

Teste plusieurs modèles et sélectionne celui offrant la meilleure performance selon la métrique spécifiée.

- **Paramètres :**
    - `--dataset` (obligatoire) : Nom du dataset.
    - `--metric` (optionnel, `f1_score` par défaut) : Métrique d’évaluation du modèle (métriques disponibles : `accuracy`, `precision`, `recall`, `f1_score`)
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli find_best_model --dataset titanic --metric f1_score
    ```
    
- **Remarque :**
    
    Le meilleur modèle est enregistré dans le fichier `best_model.txt` composé de 3 lignes :
    
    - nom de l’algorithme utilisé
    - GridSearch utilisé ou pas
    - dataset sur lequel a été entraîné le modèle

### **Faire une prédiction avec un modèle spécifique**

Prédit les résultats à partir d’un fichier de test et sauvegarde les prédictions dans un fichier de sortie.

- **Paramètres :**
    - `--input` (obligatoire) : Chemin vers le fichier de test.
    - `--output` (obligatoire) : Chemin du fichier de sortie des prédictions.
    - `--model` (obligatoire) : Modèle à utiliser pour la prédiction.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli predict --input titanic_test.csv --output predictions.csv --model titanic/titanic_v1_2025-03-01_1_random_forest
    ```
    

### **Faire une prédiction avec le meilleur modèle**

Utilise automatiquement le meilleur modèle disponible pour faire une prédiction.

- **Paramètres :**
    - `--input` (obligatoire) : Chemin vers le fichier de test.
    - `--output` (obligatoire) : Chemin du fichier de sortie des prédictions.
    - `--folder` (obligatoire) : Nom du dataset contenant le modèle à utiliser.
- **Exemple :**
    
    ```bash
    python src/cli/dsba_cli predict_with_best_model --input titanic_test.csv --output predictions.csv --folder titanic
    ```
    

### Build image

```bash
python src/cli/dsba_cli build_image
```

### Run container

```bash
python src/cli/dsba_cli run_container
```

---

## FastAPI

- Pré-requis : `pip install "fastapi[standard]"`
- Lancer l’API :
    
    ```bash
    fastapi dev src/api/api.py
    ```
    

### Afficher la liste des modèles disponibles pour un dataset spécifique :

Endpoint : `http://127.0.0.1:8000/models/?dataset=<nom_dataset>`

### Récupérer les données et leur type à envoyer pour faire une requête à un modèle

Endpoint : `http://127.0.0.1:8000/get_coltypes/?dataset=<nom_dataset>`

### Faire une prédiction en utilisant un modèle spécifique entraîné sur un dataset spécifique

Endpoint : `http://127.0.0.1:8000/predict/`

- Avec cURL :
    
    ```bash
    curl -X POST "http://127.0.0.1:8000/predict/" \
         -H "Content-Type: application/json" \
         -d '{
              "model_id": "titanic/titanic_v2_2025-04-01_14-29_random_forest",
              "query": {
                  "Pclass": 3,
                  "Sex": 1,
                  "Age": 22.0,
                  "SibSp": 1,
                  "Parch": 0,
                  "Fare": 7.25,
                  "Cabin": 3,
                  "Embarked": 2
              }
         }'
    ```
    

- Avec Python :
    
    ```python
    import requests
    
    url = "http://127.0.0.1:8000/predict/"
    data = {
    		"model_id": "titanic/titanic_v2_2025-04-01_14-29_random_forest",
        "query": {
            "Pclass": 3,
            "Sex": 1,
            "Age": 22.0,
            "SibSp": 1,
            "Parch": 0,
            "Fare": 7.25,
            "Cabin": 3,
            "Embarked": 2
        }
    }
    response = requests.post(url, json=data)
    print(response.json())
    ```
    

### Faire une prédiction en utilisant le meilleur modèle entraîné sur un dataset spécifique

Endpoint : `http://127.0.0.1:8000/predict_with_best_model/`

- Avec cURL :
    
    ```bash
    curl -X POST "http://127.0.0.1:8000/predict_with_best_model/" \
         -H "Content-Type: application/json" \
         -d '{
              "model_id": "titanic",
              "query": {
                  "Pclass": 3,
                  "Sex": 1,
                  "Age": 22.0,
                  "SibSp": 1,
                  "Parch": 0,
                  "Fare": 7.25,
                  "Cabin": 3,
                  "Embarked": 2
              }
         }'
    ```
    

- Avec Python :
    
    ```python
    import requests
    
    url = "http://127.0.0.1:8000/predict_with_best_model/"
    data = {
    		"model_id": "titanic",
        "query": {
            "Pclass": 3,
            "Sex": 1,
            "Age": 22.0,
            "SibSp": 1,
            "Parch": 0,
            "Fare": 7.25,
            "Cabin": 3,
            "Embarked": 2
        }
    }
    response = requests.post(url, json=data)
    print(response.json())
    ```
    

---

## Docker

`~/Desktop/DSBA/T2 - Data Sciences Electives/MLOps/dsba-platform`

- Build l’image :
    - Architecture ARM64 :
        
        ```bash
        docker build -t fastapi-app -f src/api/Dockerfile .
        ```
        
    - Architecture AMD64 :
        
        ```bash
        docker buildx build --platform linux/amd64 -t fastapi-app -f src/api/Dockerfile . --load
        ```
        
- Run le conteneur :
    
    ```bash
    docker run -d -p 8000:8000 --name fastapi-container -v "/Users/allisterkohn/Desktop/DSBA/T2 - Data Sciences Electives/MLOps/Project/dsba-platform/models:/app/models" -e DSBA_MODELS_ROOT_PATH="/app/models" fastapi-app
    ```
    
- Tagger l’image pour AWS ECR :
    
    ```bash
    docker tag fastapi-app 217831684037.dkr.ecr.eu-west-3.amazonaws.com/mlops-app-runner:latest
    ```
    
- S’authentifier auprès d’AWS :
    
    ```bash
    aws ecr get-login-password --region eu-west-3 | docker login --username AWS --password-stdin 217831684037.dkr.ecr.eu-west-3.amazonaws.com
    ```
    
- Créer un dépôt sur AWS ECR :
    
    ```bash
    aws ecr create-repository --repository-name mlops-app-runner --region eu-west-3
    ```
    
- Pousser l’image sur ce dépôt :
    
    ```bash
    docker push 217831684037.dkr.ecr.eu-west-3.amazonaws.com/mlops-app-runner:latest
    ```
    
- Créer un service AWS App Runner :
    
    ```bash
    aws apprunner create-service --service-name mlops-app-runner \
        --region eu-west-3 \
        --profile s3-user \
        --source-configuration '{
            "AuthenticationConfiguration": {
                "AccessRoleArn": "arn:aws:iam::217831684037:role/service-role/AppRunnerECRAccessRole"
            },
            "ImageRepository": {
                "ImageIdentifier": "217831684037.dkr.ecr.eu-west-3.amazonaws.com/mlops-app-runner:latest",
                "ImageRepositoryType": "ECR",
                "ImageConfiguration": {
                    "Port": "8000",
                    "RuntimeEnvironmentVariables": {
                        "DSBA_MODELS_ROOT_PATH": "/app/models"
                    }
                }
            },
            "AutoDeployments": true
        }'
    ```
    
- Récupérer le `ServiceURI` du service déployé :
    
    ```bash
    aws apprunner list-services --region eu-west-3 --profile s3-user
    ```
    

---

## AWS

- Installer le CLI AWS : `brew install awscli`
- Se connecter à n’importe quel utilisateur de votre compte AWS : `aws configure`
- Créer un nouvel utilisateur :
    
    ```bash
    aws iam create-user --user-name MyIAMUser
    ```
    
- Attacher des politiques à cet utilisateur :
    
    ```bash
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryFullAccess
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/AmazonEC2ReadOnlyAccess
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/AmazonECS_FullAccess
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/AWSAppRunnerFullAccess
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/service-role/AWSAppRunnerServicePolicyForECRAccess
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/IAMFullAccess
    aws iam attach-user-policy --user-name MyIAMUser --policy-arn arn:aws:iam::aws:policy/IAMReadOnlyAccess
    ```
    
- Créer une clé d’accès :
    
    ```bash
    aws iam create-access-key --user-name MyIAMUser
    ```
    
- Se connecter à l’utilisateur créé :
    
    ```bash
    aws configure
    ```
    

---