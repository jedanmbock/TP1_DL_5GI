# Projet de Classification d'Images MNIST

Ce projet est une application de machine learning pour la classification de chiffres manuscrits de la base de données MNIST. Il inclut un script pour entraîner un modèle Keras, une application Flask pour servir le modèle via une API, le suivi des expérimentations avec MLflow et une configuration Docker pour un déploiement facile.

## Prérequis

- Python 3.9+
- pip
- Docker (optionnel)

## Installation

1.  Clonez ce dépôt de code sur votre machine locale.
2.  Naviguez dans le répertoire du projet.
3.  Installez les dépendances Python nécessaires en utilisant le fichier `requirements.txt` :

    ```bash
    pip install -r requirements.txt
    ```

## Entraînement du modèle

Pour entraîner le modèle de classification sur le jeu de données MNIST, exécutez le script `train_model.py` :

```bash
python train_model.py
```

Ce script effectuera les actions suivantes :
- Chargement et prétraitement des données MNIST.
- Construction et compilation d'un modèle de réseau de neurones avec Keras.
- Entraînement du modèle sur les données.
- Sauvegarde du modèle entraîné dans le fichier `mnist_model.h5`.
- Utilisation de MLflow pour enregistrer les paramètres (époques, taille du lot), les métriques (précision) et l'artefact du modèle.

Pour visualiser les expérimentations et les résultats enregistrés par MLflow, lancez l'interface utilisateur de MLflow :
```bash
mlflow ui
```
Accédez ensuite à [http://localhost:5000](http://localhost:5000) dans votre navigateur.

## Utilisation de l'API de prédiction

L'application Flask (`app.py`) expose une API RESTful pour obtenir des prédictions à partir du modèle entraîné.

1.  Pour démarrer le serveur web Flask :

    ```bash
    python app.py
    ```
    Le serveur sera accessible à l'adresse `http://127.0.0.1:5000`.

2.  Vous pouvez ensuite envoyer des requêtes POST à l'endpoint `/predict`. Le corps de la requête doit être un JSON contenant la clé `image`, avec pour valeur un vecteur de 784 pixels (une image MNIST de 28x28 aplatie).

    Voici un exemple de requête avec `curl` :

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -d "{"image": [<votre_vecteur_image_de_784_pixels>]}" \
         http://localhost:5000/predict
    ```

    La réponse sera un objet JSON contenant le chiffre prédit et les probabilités associées à chaque classe :

    ```json
    {
      "prediction": 7,
      "probabilities": [
        [9.99e-01, 1.23e-04, ..., 8.90e-03]
      ]
    }
    ```

## Déploiement avec Docker

Le projet peut être facilement conteneurisé avec Docker.

1.  Assurez-vous que le service Docker est en cours d'exécution.
2.  Construisez l'image Docker à partir du `Dockerfile` :

    ```bash
    docker build -t mnist-classifier-app .
    ```

3.  Exécutez un conteneur à partir de l'image nouvellement créée :

    ```bash
    docker run -p 5000:5000 mnist-classifier-app
    ```

L'API de prédiction sera alors disponible sur `http://localhost:5000/predict`.
