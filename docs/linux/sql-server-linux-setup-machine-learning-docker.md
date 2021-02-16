---
title: Installer sur Docker
titleSuffix: SQL Server Machine Learning Services
description: Découvrez comment installer SQL Server Machine Learning Services (Python et R) sur Docker.
author: dphansen
ms.author: davidph
manager: cgronlun
ms.date: 05/11/2020
ms.topic: how-to
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 1e6e658023419a9a676b861efb8769a5bbf6cadf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030781"
---
# <a name="install-sql-server-machine-learning-services-python-and-r-on-docker"></a>Installer SQL Server Machine Learning Services (Python et R) sur Docker

[!INCLUDE [SQL Server 2019 - Linux](../includes/applies-to-version/sqlserver2019-linux.md)]

Cet article explique comment installer SQL Server Machine Learning Services sur Docker. Vous pouvez utiliser Machine Learning Services pour exécuter des scripts Python et R en base de données. Nous ne fournissons pas de conteneurs prédéfinis avec Machine Learning Services. Vous pouvez en créer un à partir des conteneurs SQL Server à l’aide d’[un exemple de modèle disponible sur GitHub](https://github.com/Microsoft/mssql-docker/tree/master/linux/preview/examples/mssql-mlservices).

## <a name="prerequisites"></a>Prérequis

- Interface de ligne de commande GIT.

- Moteur Docker 1.8+ sur n’importe quelle distribution Linux prise en charge ou Docker pour Mac/Windows. Pour plus d’informations, consultez [Obtenir Docker](https://docs.docker.com/get-docker/).

- Consultez également la [configuration système requise pour SQL Server sur Linux](sql-server-linux-setup.md#system).

## <a name="clone-the-mssql-docker-repository"></a>Clonez le référentiel mssql-docker

La commande suivante clone le dépôt Git `mssql-docker` vers un répertoire local.

1. Ouvrez un terminal bash sur Linux ou Mac.

2. Créez un répertoire pour y stocker une copie locale du dépôt mssql-docker.

3. Exécuter la commande clone git pour cloner le référentiel mssql-docker :

    ```bash
    git clone https://github.com/microsoft/mssql-docker mssql-docker
    ```

## <a name="build-a-sql-server-linux-container-image"></a>Générer une image conteneur Linux de SQL Server

Pour générer l’image Docker, effectuez les étapes suivantes :

1. Modifier le répertoire par le répertoire mssql-mlservices :
    
    ```bash
    /mssql-docker/linux/preview/examples/mssql-mlservices
    ```

2. Dans le même répertoire, exécutez la commande suivante :

    ```bash
    docker build -t mssql-server-mlservices .
    ```

3. Exécutez la commande suivante :

    ```bash
    docker run -d -e MSSQL_PID=Developer -e ACCEPT_EULA=Y -e ACCEPT_EULA_ML=Y -e MSSQL_SA_PASSWORD=<password> -v <directory on the host OS>:/var/opt/mssql -p 1433:1433 mssql-server-mlservices
    ```
  
    > [!NOTE]
    > Chacune des valeurs suivantes peut être utilisés pour MSSQL_PID : Developer (gratuit), Express (gratuit), Enterprise (payant), Standard (payant). Si vous utilisez une édition payante, merci de vérifier que vous avez acheté une licence. Remplacez (password) par votre mot de passe réel. Le montage en volume à l’aide de-v est facultatif. Remplacez (répertoire sur le système d’exploitation hôte) par un répertoire réel dans lequel vous souhaitez monter les fichiers de données et les fichiers journaux de la base de données.
    

4. Confirmez en exécutant la commande suivante :

    ```bash
    docker ps -a
    ```

   > [!NOTE]
   > Pour générer l’image Docker, vous devez installer des packages d’une taille de plusieurs Go. Le script peut prendre plus ou moins de temps pour s’exécuter entièrement en fonction de la bande passante réseau disponible.

## <a name="run-the-sql-server-linux-container-image"></a>Exécuter l’image conteneur Linux de SQL Server

1. Définissez des variables d’environnement avant d’exécuter le conteneur. Définir la variable d’environnement PATH_TO_MSSQL sur un répertoire hôte :

   ```bash
   export MSSQL_PID='Developer'
   export ACCEPT_EULA='Y'
   export ACCEPT_EULA_ML='Y'
   export PATH_TO_MSSQL='/home/mssql/'
   ```
  
   > [!NOTE]
   > Le processus d’exécution des éditions SQL Server de production dans des conteneurs est légèrement différent. Pour plus d’informations, consultez [Configurer des images de conteneur SQL Server sur Docker](./sql-server-linux-docker-container-deployment.md). Si vous utilisez les mêmes noms de conteneur et ports, le reste de cette procédure pas à pas fonctionne toujours avec les conteneurs de production.

2. Pour afficher vos conteneurs Docker, exécutez la commande `docker ps` :

   ```bash
   sudo docker ps -a
   ```

3. Si la colonne **ÉTAT** affiche **En cours d’exécution**, SQL Server est en cours d’exécution dans le conteneur et il écoute sur le port spécifié dans la colonne **PORTS**. Si la colonne **ÉTAT** pour votre conteneur SQL Server affiche **Quitté**, consultez la [section Résolution des problèmes dans le guide de configuration](./sql-server-linux-docker-container-troubleshooting.md).

 
    Sortie :

    ```
    CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS              PORTS                    NAMES
    941e1bdf8e1d        mcr.microsoft.com/mssql/server/mssql-server-linux   "/bin/sh -c /opt/m..."   About an hour ago   Up About an hour     0.0.0.0:1401->1433/tcp   sql1
    ```

## <a name="enable-machine-learning-services"></a>Activer Machine Learning Services

Pour activer Machine Learning Services, connectez-vous à votre instance SQL Server et exécutez l’instruction T-SQL suivante :

```sql
EXEC sp_configure  'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE
```

## <a name="next-steps"></a>Étapes suivantes

Les développeurs Python peuvent apprendre à utiliser Python avec SQL Server en effectuant les didacticiels suivants :

+ [Tutoriel Python : Prédire la location de ski avec régression linéaire dans SQL Server Machine Learning Services](../machine-learning/tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Tutoriel Python : Catégoriser des clients à l’aide de k-moyennes avec SQL Server Machine Learning Services](../machine-learning/tutorials/python-clustering-model.md)

Les développeurs peuvent démarrer avec quelques exemples simples et découvrir les principes de base du fonctionnement de R avec SQL Server. Pour l’étape suivante, consultez les liens suivants :

+ [Démarrage rapide : Exécuter R dans T-SQL](../machine-learning/tutorials/quickstart-r-create-script.md)
+ [Tutoriel : Analytique dans la base de données pour les développeurs R](../machine-learning/tutorials/r-taxi-classification-introduction.md)