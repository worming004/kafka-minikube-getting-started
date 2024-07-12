# Getting started

Ce repository permet de setup rapidement un cluster kafka single node. Lisez et exécutez les commandes de bout en bout

## Prérequis

- Docker installé
- Minikube up and running
- Kubectl installé et configuré pour minikube

## Setup Strimzi

Strimzi est un opérateur kafka. Pour l'installer, il suffit de la commande suivante

```bash
kubectl create namespace kafka
kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka
```

Démarrez le cluster kafka avec `kubectl apply -f ./deployment/kafka.yml -n kafka`

## Écrire quelques messages

Une application golang est disponible dans /src. Vous pouvez le lancer dans le cluster avec

```bash
cd /src
docker build -t myapp:1.0.0 .
minikube image load myapp:1.0.0
cd ..
```

> Vérifiez la présence de l'image sur le cluster avec `minikube image ls | grep myapp`

Lancez le pod avec

```bash
kubectl create namespace myapp
kubectl apply -f ./deployment/myapp.yml -n myapp
```

## Observer le topic avec kafka ui

Installez kafka ui avec

```bash
kubectl create namespace kafka-ui
kubectl apply -f ./deployment/ui.yml -n kafka-ui
```

naviguez sur `http://localhost:8080`, intro

Indiquez que le serveur bootstrap est sur my-cluster-kafka-bootstrap.kafka.svc.cluster.local avec le port 9092.
Enregistrez, rafraichissez la page.

Naviguez sur Topics>my-topic>messages

Les 3 messages du pod golang s'y trouve.

# Voilà

