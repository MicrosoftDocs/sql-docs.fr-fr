---
title: Évaluer la couche d’accès aux données d’une application avec Assistant Migration de données
description: Découvrez comment utiliser Assistant Migration de données pour évaluer la couche d’accès aux données d’une application.
ms.date: 01/23/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: f56976c9140cb4ac18c048d7d57d6949fc4ae74d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065158"
---
# <a name="assess-an-apps-data-access-layer-with-data-migration-assistant"></a>Évaluer la couche d’accès aux données d’une application avec Assistant Migration de données

En général, les applications se connectent et rendent les données persistantes dans une base de données. La couche d’accès aux données de l’application fournit un accès simplifié à ces données. Assistant Migration de données (DMA) a permis aux utilisateurs d’évaluer leurs bases de données et leurs objets connexes. La dernière version de DMA (v 5.0) introduit la prise en charge de l’analyse de la connectivité de base de données et des requêtes SQL incorporées dans le code de l’application.

Prenons le segment de code C# ci-dessous :

![Exemple de segment de code C#](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-segment.png)

Dans ce cas, vous pouvez voir que l’application utilise une requête SQL pour obtenir le nom d’un employé.

![Exemple de détail de segment de code C#](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-detail.png)

En tant que propriétaire de l’application, je dois être en mesure d’identifier les différentes bases de données auxquelles l’application peut se connecter et les requêtes incorporées dans la couche d’accès aux données de l’application. En outre, j’ai besoin d’identifier les modifications nécessaires pour moderniser l’application dans Azure Data Services.

## <a name="assess-an-app-with-data-access-migration-toolkit"></a>Évaluer une application à l’aide de la boîte à outils de migration d’accès aux données

Pour activer cette évaluation, nous avons récemment introduit le DAMT (Data Access Migration Toolkit), une extension de Visual Studio Code. La version la plus récente de cette extension (v 0,2) ajoute la prise en charge des applications .net et du dialecte T-SQL.

1. Téléchargez et installez VS Code à partir d' [ici](https://code.visualstudio.com/download).
2. Activez l’extension de la boîte à outils de migration de l' [accès aux données](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) à partir du Marketplace des extensions.

   ![Page d’extension de la boîte à outils de migration d’accès aux données](../dma/media/dma-assess-app-data-layer/dma-damt-extension-page.png)

3. Ouvrez le projet d’application dans Visual Studio Code.

   ![Projet d’application dans Visual Studio Code](../dma/media/dma-assess-app-data-layer/dma-app-project-in-vscode.png)

4. Démarrez la console d’extension (Ctrl-SHFT-P), puis exécutez la commande **accès aux données : analyser l’espace de travail** .

   ![Console d’extension dans Visual Studio Code](../dma/media/dma-assess-app-data-layer/dma-vscode-extension-console.png)

5. Sélectionnez le dialecte SQL Server.

   ![Sélection du dialecte SQL Server](../dma/media/dma-assess-app-data-layer/dma-sql-server-dialect.png)

   À la fin de l’analyse, la commande génère un rapport sur les requêtes et les commandes de connectivité SQL.

   ![Rapport d’accès aux données](../dma/media/dma-assess-app-data-layer/dma-data-access-report.png)

6. Examinez le rapport sur les composants de connectivité de données et les requêtes SQL incorporées dans le code de l’application, qui apparaissent en surbrillance.

   ![Requêtes SQL dans le code de l’application](../dma/media/dma-assess-app-data-layer/dma-sql-queries-in-app-code.png)

   Ces requêtes peuvent être analysées à l’aide de DMA pour la compatibilité et les problèmes de parité de fonctionnalités basés sur la plateforme SQL cible.

7. Pour évaluer la couche de données de l’application, exportez le rapport en tant que fichier JSON.

   ![exportation de fichier. JSON](../dma/media/dma-assess-app-data-layer/dma-json-file-export.png)

   Dans ce cas, le fichier généré est le suivant :

   ![Afficher le contenu du fichier JSON](../dma/media/dma-assess-app-data-layer/dma-json-file-contents.png)

   Assistant Migration de données permet d’évaluer les requêtes identifiées dans l’application dans le contexte de la modernisation de la base de données sur la plateforme de données Azure.

8. Démarrez Assistant Migration de données, puis créez un projet d’évaluation.

   ![Assistant Migration de données un nouveau projet d’évaluation](../dma/media/dma-assess-app-data-layer/dma-new-assessment-project.png)

9. Sélectionnez l’instance de SQL Server source.

   ![Assistant Migration de données sélectionner une source de SQL Server](../dma/media/dma-assess-app-data-layer/dma-select-sql-source.png)

10. Sélectionnez la base de données à laquelle l’application se connecte.

    ![Migration de l’accès aux données, sélectionner une base de données d’application](../dma/media/dma-assess-app-data-layer/dma-select-app-database.png)

    Pour faciliter l’évaluation de l’accès aux données, DMA introduit la possibilité d’inclure des fichiers JSON avec des requêtes d’application. Ensuite, nous inclurons le fichier JSON que nous avons créé précédemment avec les requêtes de l’application.

11. Sélectionnez la base de données et accédez au fichier JSON exporté à partir de la boîte à outils de migration de l’accès aux données pour inclure les requêtes de l’application pour l’évaluation.

    ![Assistant Migration de données ouvrir le fichier JSON DMAT](../dma/media/dma-assess-app-data-layer/dma-open-damt-json-file.png)

12. Démarrez l’évaluation.

    ![Assistant Migration de données l’évaluation de démarrage](../dma/media/dma-assess-app-data-layer/dma-start-assessment.png)

13. Passez en revue le rapport d’évaluation. Le rapport généré inclut tous les problèmes de compatibilité ou de parité de fonctionnalité détectés dans les requêtes d’application, comme indiqué ci-dessous.

    ![Rapport d’évaluation de Assistant Migration de données](../dma/media/dma-assess-app-data-layer/dma-assessment-report.png)

Désormais, en plus du point de vue de la migration, les utilisateurs ont également une vue du point de vue de l’application.

## <a name="see-also"></a>Voir également

* [Vue d’ensemble de l’Assistant Migration de données](../dma/dma-overview.md)
* [Assistant Migration de données : paramètres de configuration](../dma/dma-configurationsettings.md)
* [Assistant Migration de données : meilleures pratiques](../dma/dma-bestpractices.md)
* [Boîte à outils de migration de l’accès aux données](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)