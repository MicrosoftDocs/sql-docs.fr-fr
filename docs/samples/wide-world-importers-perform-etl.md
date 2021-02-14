---
title: Flux de travail WideWorldImportersDW-ETL | Microsoft Docs
description: Utilisez le package ETL avec SQL Server Integration Services (SSIS) pour migrer périodiquement les données de la base de données WideWorldImporters vers WideWorldImportersDW.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: ce9cd8ee5a4153cb1d744d505a26b400497b460b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354052"
---
# <a name="wideworldimportersdw-etl-workflow"></a>Flux de travail ETL WideWorldImportersDW
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Utilisez le package ETL *WWI_Integration* pour migrer des données de la base de données wideworldimporters vers la base de données WideWorldImportersDW à mesure que les données changent. Le package est exécuté périodiquement (généralement quotidiennement).

Le package garantit des performances élevées en utilisant SQL Server Integration Services pour orchestrer les opérations T-SQL en bloc (plutôt que les transformations distinctes dans Integration Services).

Les dimensions sont chargées en premier, puis les tables de faits sont chargées. Vous pouvez réexécuter le package à tout moment après une défaillance.

Le flux de travail se présente comme suit :

 ![Flux de travail ETL WideWorldImporters](media/wide-world-importers/wideworldimporters-etl-workflow.png)

Le flux de travail commence par une tâche d’expression qui détermine le temps de coupure approprié. L’heure limite est l’heure actuelle moins quelques minutes. (Cette approche est plus robuste que la demande de données directement à l’heure actuelle.) Les millisecondes sont tronquées à partir du moment.

Le traitement principal commence par le remplissage de la table de dimension Date. Le traitement garantit que toutes les dates de l’année en cours ont été renseignées dans la table.

Ensuite, une série de tâches de workflow charge chaque dimension. Ils chargent ensuite chaque fait.

## <a name="prerequisites"></a>Prérequis

- SQL Server 2016 (ou version ultérieure), avec les bases de données WideWorldImporters et WideWorldImportersDW (dans le même ou dans des instances différentes de SQL Server)
- SQL Server Management Studio
- SQL Server 2016 Integration Services
  - Veillez à créer un catalogue de Integration Services. Pour créer un catalogue de Integration Services, dans SQL Server Management Studio l’Explorateur d’objets, cliquez avec le bouton droit sur **Integration Services**, puis sélectionnez **Ajouter un catalogue**. Laissez les options par défaut. Vous êtes invité à activer SQLCLR et à fournir un mot de passe.


## <a name="download"></a>Télécharger

Pour obtenir la version la plus récente de l’exemple, consultez [larges-World-](https://go.microsoft.com/fwlink/?LinkID=800630)Importers-Release. Téléchargez le fichier de package *ETL. ispac Integration Services quotidien* .

Pour obtenir le code source permettant de recréer l’exemple de base de données, consultez [larges-World-](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/wwi-ssis)Importers.

## <a name="install"></a>Installer

1. Déployez le package Integration Services :
   1. Dans l’Explorateur Windows, ouvrez le package *ETL. ISPAC quotidien* . Cela lance l’Assistant Déploiement de SQL Server Integration Services.
   2. Sous **Sélectionner une source**, suivez les valeurs par défaut pour le déploiement de projet, avec le chemin d’accès pointant vers le package *ETL. ISPAC quotidien* .
   3. Sous **Sélectionner la destination**, entrez le nom du serveur qui héberge le catalogue de Integration Services.
   4. Sélectionnez un chemin d’accès sous le catalogue Integration Services, par exemple, dans un nouveau dossier nommé *wideworldimporters*.
   5. Sélectionnez **déployer** pour terminer l’Assistant.

2. Créez un travail de SQL Server Agent pour le processus ETL :
   1. Dans Management Studio, cliquez avec le bouton droit sur **SQL Server Agent**, puis sélectionnez **nouveau**  >  **travail**.
   2. Entrez un nom, par exemple, *WIDEWORLDIMPORTERS ETL*.
   3. Ajoutez une **étape de travail** du type **SQL Server Integration Services package**.
   4. Sélectionnez le serveur qui contient le catalogue Integration Services, puis sélectionnez le package *ETL quotidien* .
   5. Sous   >  **gestionnaires de connexions** de configuration, assurez-vous que les connexions à la source et à la cible sont correctement configurées. La valeur par défaut consiste à se connecter à l’instance locale.
   6. Sélectionnez **OK** pour créer le travail.

3. Exécutez ou planifiez le travail.
