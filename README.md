# Guide d'installation et d'exécution du déploiement avec Docker

Ce document décrit les étapes nécessaires pour déployer notre site web à l'aide de Docker sur une machine Mac/Linux.

## Table des matières
1. [Introduction](#introduction)
2. [Prérequis](#prérequis)
3. [Téléchargement du projet](#téléchargement-du-projet)
4. [Docker Compose](#docker-compose)
5. [Vérification de l'installation](#vérification-de-linstallation)

## Introduction
Ce guide vous permettra de déployer le site web du Club Photo Nailloux en utilisant Docker. Le déploiement est conçu pour fonctionner sur des machines avec un processeur ARM64.

## Prérequis
Avant de commencer, assurez-vous d'avoir :
- Une machine virtuelle sous Debian avec une connexion Internet active.
- Un accès administrateur (sudo).
- Docker et Git installés sur la machine.

## Téléchargement du projet
1. Connectez-vous à votre machine virtuelle.
2. Créez un répertoire et placez-vous dedans :
   ```bash
   mkdir mon_projet
   cd mon_projet
   ```
3. Clonez le projet Git :
   ```bash
   git clone https://github.com/PabloHavane/docker-sae.git
   ```
4. Placez-vous dans le répertoire du projet :
   ```bash
   cd docker-sae
   ```

## Docker Compose
Avant de démarrer les conteneurs, vous pouvez modifier les identifiants de connexion à la base de données dans le fichier de configuration `.env` (par défaut `user/password`).

Exemple de configuration dans le fichier `.env` :
```env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=db_cpn_prod
DB_USERNAME=user
DB_PASSWORD=password
```

Lancez les conteneurs avec la commande suivante :
```bash
  docker compose up
```
Pour lancer les conteneurs en arrière-plan, ajoutez le flag `-d` :
```bash
  docker compose up -d
```

## Vérification de l'installation
1. Une fois le déploiement terminé, ouvrez un navigateur et accédez à : [http://localhost:8000](http://localhost:8000).
2. Si tout fonctionne correctement, le site sera accessible.
3. Pour vous connecter, créez un nouveau profil utilisateur en cliquant sur le bouton « S'inscrire » depuis la page d'accueil.

### Validation manuelle de l'utilisateur
1. Connectez-vous à phpMyAdmin à l'adresse suivante : [http://localhost:8080](http://localhost:8080).
2. Accédez à la base de données `db_cpn_prod`, puis à la table `users`.
3. Modifiez manuellement les colonnes `email_verified_at` et `statut` :
    - Sélectionnez une date antérieure à celle d'aujourd'hui pour `email_verified_at`.
    - Changez la valeur de `statut` de 0 à 1.
4. Pour devenir administrateur, changez le rôle de l'utilisateur en `admin`.

### Connexion au site
Revenez sur la fenêtre de connexion au site et connectez-vous avec les identifiants de l'utilisateur. Vous devriez maintenant avoir accès à toutes les fonctionnalités du site.

**Note :** La page « Événements » renvoie une page blanche en raison d'un bug non corrigé.

