---
title: Extension de déploiement d’application
titleSuffix: SQL Server big data clusters
description: Déployez un script Python ou R en tant qu’application sur des clusters Big Data SQL Server.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d373b6739f30c0cae6d9534c26da2114dd0f718b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039869"
---
# <a name="how-to-use-visual-studio-code-to-deploy-applications-to-big-data-clusters-2019"></a>Comment utiliser Visual Studio Code pour déployer des applications sur [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article explique comment déployer des applications sur un cluster Big Data SQL Server en utilisant Microsoft Visual Studio Code avec l’extension de déploiement d’application.

## <a name="prerequisites"></a>Conditions préalables requises

- [Visual Studio Code](https://code.visualstudio.com/)
- [Cluster Big Data SQL Server](big-data-cluster-overview.md)

## <a name="capabilities"></a>Fonctionnalités

Cette extension prend en charge les tâches suivantes dans Visual Studio Code :

- Authentifier auprès du cluster Big Data SQL Server.
- Récupérer un modèle d’application à partir du dépôt GitHub pour le déploiement de runtimes pris en charge.
- Gérer les modèles d’application actuellement ouverts dans l’espace de travail de l’utilisateur.
- Déployer une application via une spécification au format YAML.
- Gérer les applications déployées dans un cluster Big Data SQL Server.
- Voir toutes les applications que vous avez déployées dans la barre latérale avec des informations supplémentaires.
- Générer une spécification d’exécution pour consommer l’application ou la supprimer du cluster.
- Consommer des applications déployées via une spécification d’exécution YAML.

Les sections suivantes vous guident tout au long du processus d’installation et fournissent une vue d’ensemble du fonctionnement de l’extension. 

### <a name="install"></a>Installer

Installez d’abord l’extension de déploiement d’application dans Visual Studio Code :

1. Téléchargez l’[extension de déploiement d’application](https://aka.ms/app-deploy-vscode) pour installer l’extension comme composant de Visual Studio Code.

1. Lancez Visual Studio Code et accédez à la barre latérale Extensions.

1. Cliquez sur le `…`menu contextuel en haut de la barre latérale et sélectionnez `Install from vsix`.

   ![Installer VSIX](media/vs-extension/install_vsix.png)

1. Recherchez le fichier `sqlservbdc-app-deploy.vsix` que vous avez téléchargé et choisissez-le pour l’installer.

Une fois que l’extension de déploiement d’application du cluster Big Data SQL Server a été installée, elle vous invite à recharger Visual Studio Code. Vous devez maintenant voir l’Explorateur d’applications du cluster Big Data SQL Server dans la barre latérale de Visual Studio Code.

### <a name="app-explorer"></a>Explorateur d’applications

Cliquez sur l’extension dans la barre latérale pour charger un panneau latéral montrant l’Explorateur d’applications. L’exemple de capture d’écran suivant de l’Explorateur d’applications ne montre aucune application ou spécification d’application disponible :

![Explorateur d’applications](media/vs-extension/app_explorer.png)

#### <a name="connect-to-cluster"></a>Se connecter au cluster

Pour vous connecter au point de terminaison du cluster, utilisez une des méthodes suivantes :

- Cliquez sur la barre d’état dans le bas, indiquant `SQL Server BDC Disconnected`.
- Vous pouvez aussi cliquer sur le bouton `Connect to Cluster` en haut avec la flèche pointant vers une entrée de porte.

Visual Studio Code demande le point de terminaison, le nom d’utilisateur et le mot de passe appropriés.

Le point de terminaison auquel se connecter est le point de terminaison de `Cluster Management Service` avec le port 30080.

Vous pouvez également trouver ce point de terminaison à partir de la ligne de commande via 

```
azdata bdc endpoint list
```

Une des autres méthodes permettant d’obtenir ces informations est de cliquer avec le bouton droit sur **Gérer** sur le serveur dans Azure Data Studio où se trouvent les points de terminaison des services listés.

![Point de terminaison Azure Data Studio](media/vs-extension/ads_end_point.png)

Une fois que vous avez trouvé le point de terminaison à utiliser, connectez-vous au cluster.

![Nouvelle connexion](media/vs-extension/connect_to_cluster.png)

 Si vous avez spécifié les informations d’identification et le point de terminaison d’application corrects, Visual Studio Code vous indique que vous êtes connecté au cluster : vous voyez alors les applications déployées dans la barre latérale. Si vous avez réussi à vous connecter, votre point de terminaison et votre nom d’utilisateur sont enregistrés dans `./sqldbc` dans le cadre de votre profil utilisateur. Aucun mot de passe ou jeton ne sera jamais enregistré. Quand vous vous reconnectez, l’invite est préremplie avec votre hôte et votre nom d’utilisateur enregistrés, mais vous devez toujours entrer un mot de passe. Si vous voulez vous connecter à un autre point de terminaison du cluster, recliquez simplement sur `New Connection`. La connexion se ferme automatiquement si vous fermez Visual Studio Code ou si vous ouvrez un autre espace de travail : vous devrez alors vous reconnecter.

### <a name="app-template"></a>Modèle d’application

Vous devez *ouvrir l’espace de travail* dans Visual Studio Code là où vous allez enregistrer les artefacts de l’application.

Pour déployer une nouvelle application à partir d’un de nos modèles, cliquez sur le bouton `New App Template` dans le volet `App Specifications`, où vous serez invité à entrer le nom, le runtime et l’emplacement où vous voulez placer la nouvelle application sur votre machine locale. Le nom et la version que vous spécifiez doivent être une étiquette DNS-1035 et contenir des caractères alphanumériques minuscules ou « - », commencer par un caractère alphabétique et se terminer par un caractère alphanumérique.

Il est recommandé de les placer dans votre espace de travail Visual Studio Code actuel pour pouvoir utiliser toutes les fonctionnalités de l’extension, mais vous pouvez les placer n’importe où dans votre système de fichiers local.

![Modèle de nouvelle application](media/vs-extension/new_app_template.png)

Une fois l’opération terminée, un modèle de nouvelle application est structuré pour vous à l’emplacement que vous avez spécifié, et le fichier `spec.yaml` de déploiement s’ouvre dans votre espace de travail. Si le répertoire que vous avez sélectionné se trouve dans votre espace de travail, vous devez également le voir listé sous le volet `App Specifications` :

![Modèle d’application chargé](media/vs-extension/loading_app_template.png)

Le modèle est une application `helloworld` simple que se présente comme suit dans le volet Spécifications de l’application :

- **spec.yaml**
   - Indique au cluster comment déployer votre application
- **run-spec.yaml**
   - Indique au cluster comment vous voulez appeler votre application

Le code source de l’application se trouve dans le dossier de l’espace de travail.

- **Nom du fichier source**
   - Il s’agit de votre fichier de code source tel que spécifié par `src` dans `spec.yaml`
   - Il a une seule fonction appelée `handler`, qui est considérée comme `entrypoint` de l’application, comme montré dans `spec.yaml`. Elle prend une entrée de chaîne appelée `msg` et retourne une sortie de chaîne appelée `out`. Celles-ci sont spécifiées dans `inputs` et `outputs` du fichier `spec.yaml`.

Si vous ne voulez pas d’un modèle structuré et que vous préférez simplement un fichier `spec.yaml` pour le déploiement d’une application que vous avez déjà générée, cliquez sur le bouton `New Deploy Spec` en regard du bouton `New App Template` et suivez le même processus : vous recevrez alors simplement le fichier `spec.yaml`, que vous pouvez modifier comme vous le souhaitez.

### <a name="deploy-app"></a>Déployer une application

Vous pouvez déployer instantanément cette application via le CodeLens `Deploy App` dans le fichier `spec.yaml` ou en appuyant sur le bouton du dossier avec un éclair en regard du fichier `spec.yaml` dans le menu Spécifications de l’application. L’extension va décompresser tous les fichiers dans le répertoire où se trouve `spec.yaml`, puis déployer votre application sur le cluster. 

>[!NOTE]
>Vérifiez que tous les fichiers de votre application se trouvent dans le même répertoire que votre fichier `spec.yaml`. Le fichier `spec.yaml` doit se trouver au niveau racine du répertoire du code source de votre application. 

![Bouton Déployer une application](media/vs-extension/deploy_app_lightning.png)

![CodeLens Déployer l’application](media/vs-extension/deploy_app_codelens.png)

Vous serez averti une fois l’application prête à être consommée en fonction de l’état de l’application dans la barre latérale :

![Application déployée](media/vs-extension/app_deploy.png)

![Barre latérale Application prête](media/vs-extension/app_ready_side_bar.png)

![Notification Application prête](media/vs-extension/app_ready_notification.png)

Dans le volet latéral, vous pouvez voir les éléments suivants disponibles :

Vous pouvez voir dans la barre latérale toutes les applications que vous avez déployées, avec les informations suivantes :

- state
- version
- paramètres d'entrée
- paramètres de sortie
- liens
  - fichier Swagger
  - details

Si vous cliquez sur `Links`, vous voyez que vous pouvez accéder au fichier `swagger.json` de votre application déployée, ce qui vous permet d’écrire vos propres clients qui appellent votre application :

![Capture d’écran de l’interface utilisateur VS Code montrant le fichier swagger.json.](media/vs-extension/swagger.png)

Pour plus d’informations, consultez [Utiliser des applications sur des clusters Big Data](app-consume.md).

### <a name="app-run"></a>Exécution de l’application

Une fois l’application prête, appelez-la avec le fichier `run-spec.yaml` fourni dans le cadre du modèle d’application :

![Spécification de l’exécution](media/vs-extension/run_spec.png)

Spécifiez une chaîne que vous voulez à la place de `hello`, puis réexécutez-la via le lien CodeLens ou le bouton avec un éclair dans la barre latérale, en regard de `run-spec.yaml`. Si vous n’avez pas de spécification d’exécution pour une raison quelconque, générez-en une à partir de l’application déployée dans le cluster :

![Obtenir une spécification d’exécution](media/vs-extension/get_run_spec.png)

Une fois que vous en avez une et que vous l’avez modifiée à votre convenance, exécutez-la. Visual Studio Code retourne le feedback approprié quand l’exécution de l’application est terminée :

![Sortie de l’application](media/vs-extension/app_output.png)

Comme vous pouvez le constater ci-dessus, la sortie est placée dans un fichier `.json` temporaire de votre espace de travail. Si vous voulez conserver cette sortie, vous pouvez l’enregistrer ; sinon, elle sera supprimée à la fermeture. Si votre application n’a pas de sortie à écrire dans un fichier, vous recevez seulement la notification `Successful App Run` dans le bas de la fenêtre. Si l’exécution n’a pas réussi, vous recevez un message d’erreur approprié qui vous aide à déterminer ce qui est incorrect.

Lors de l’exécution d’une application, vous disposez de plusieurs façons de passer des paramètres :

Vous pouvez spécifier toutes les entrées nécessaires via un fichier `.json`, c’est-à-dire :

- `inputs: ./example.json`

Lors de l’appel d’une application déployée, si des paramètres d’entrée sont intégrés à l’application ou à l’utilisateur spécifié, et que ce paramètre d’entrée est autre chose qu’un type primitif, comme un tableau, un vecteur, un dataframe, un JSON complexe, etc., spécifiez le type du paramètre directement dans la ligne lors de l’appel de l’application, c’est-à-dire :

- Vecteur
    - `inputs:`
        - `x: [1, 2, 3]`
- Matrix
    - `inputs:`
        - `x: [[A,B,C],[1,2,3]]`
- Object
    - `inputs:`
        - `x: {A: 1, B: 2, C: 3}`

Vous pouvez aussi fournir une chaîne en tant que chemin de fichier relatif ou absolu vers un fichier `.txt`, `.json` ou `.csv` qui donne l’entrée nécessaire au format attendu par votre application. L’analyse du fichier est basée sur `Node.js Path library`, où un chemin de fichier est défini en tant que `string that contains a / or \ character`.

Si le paramètre d’entrée n’est pas fourni comme attendu, un message d’erreur approprié est affiché avec le chemin du fichier incorrect si un chemin de fichier de chaîne a été fourni ou avec ce paramètre s’il n’était pas valide. Le créateur de l’application a la responsabilité de veiller à bien comprendre les paramètres qu’il définit.

Pour supprimer une application, cliquez simplement sur le bouton Corbeille en regard de l’application dans le volet latéral `Deployed Apps`.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, découvrez comment intégrer des applications déployées sur des [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans vos propres applications en consultant [Consommer des applications sur des clusters Big Data](app-consume.md). Vous pouvez également vous reporter aux exemples supplémentaires de [Exemples de déploiement d’applications](https://aka.ms/sql-app-deploy) pour essayer l’extension.

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md).


Notre objectif est de rendre cette extension pratique à utiliser : nous apprécions donc votre feedback. Envoyez-le à l’[[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]équipe](https://aka.ms/sqlfeedback).
