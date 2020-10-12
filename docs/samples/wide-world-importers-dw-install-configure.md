---
title: Installer & configurer l’exemple de base de données DW WideWorldImporters
description: Suivez ces instructions pour télécharger, installer et configurer l’exemple de base de données WideWorldImportersDW avec SQL Server Management Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=sqlallproducts-allversions||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 18d4e9c18c4848a0857c1afb146b0d0405f418ce
ms.sourcegitcommit: afb02c275b7c79fbd90fac4bfcfd92b00a399019
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91956560"
---
# <a name="wideworldimportersdw-installation-and-configuration"></a>Installation et configuration de WideWorldImportersDW
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
Instructions d’installation et de configuration pour la base de données WideWorldImportersDW.

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (ou version ultérieure) ou [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Pour utiliser la version complète de l’exemple, utilisez l’édition Évaluation SQL Server/Developer/Enterprise.
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Pour obtenir les meilleurs résultats, utilisez la version du 2016 juin ou une version ultérieure.

## <a name="download"></a>Téléchargement

La dernière version de l’exemple :

[larges-World-importateurs-version](https://go.microsoft.com/fwlink/?LinkID=800630)

Téléchargez l’exemple de sauvegarde de base de données WideWorldImportersDW/BacPac qui correspond à votre édition de SQL Server ou Azure SQL Database.

Le code source permettant de recréer l’exemple de base de données est disponible à partir de l’emplacement suivant. Notez que le remplissage des données est basé sur ETL à partir de la base de données OLTP (WideWorldImporters) :

[larges-World-importeurs-source](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

## <a name="install"></a>Installer


### <a name="sql-server"></a>SQL Server

Pour restaurer une sauvegarde sur une instance de SQL Server, vous pouvez utiliser Management Studio.

1. Ouvrez SQL Server Management Studio et connectez-vous à l’instance de SQL Server cible.
2. Cliquez avec le bouton droit sur le nœud **bases de données** , puis sélectionnez **restaurer la base de données**.
3. Sélectionnez **périphérique** , puis cliquez sur le bouton **...**
4. Dans la boîte de dialogue **Sélectionner les unités de sauvegarde**, cliquez sur **Ajouter**, accédez à la sauvegarde de la base de données dans le système de fichiers du serveur, puis sélectionnez la sauvegarde. Cliquez sur **OK**.
5. Si nécessaire, modifiez l’emplacement cible pour les fichiers de données et les fichiers journaux dans le volet **fichiers** . Notez qu’il est recommandé de placer les fichiers de données et les fichiers journaux sur des lecteurs différents.
6. Cliquez sur **OK**. La restauration de la base de données est lancée. Une fois l’opération terminée, la base de données WideWorldImporters est installée sur votre instance SQL Server.

### <a name="azure-sql-database"></a>Azure SQL Database

Pour importer un BacPac dans un nouveau SQL Database, vous pouvez utiliser Management Studio.

1. facultatif Si vous n’avez pas encore de SQL Server dans Azure, accédez au [portail Azure](https://portal.azure.com/) et créez un SQL Database. Dans le processus de création d’une base de données, vous allez créer un serveur. Notez le serveur.
   - Consultez [ce didacticiel](/azure/azure-sql/database/single-database-create-quickstart) pour créer une base de données en quelques minutes
2. Ouvrez SQL Server Management Studio et connectez-vous à votre serveur dans Azure.
3. Cliquez avec le bouton droit sur le nœud **bases de données** , puis sélectionnez **Importer Data-Tier application**.
4. Dans **Importer les paramètres** , sélectionnez **Importer à partir du disque local** et sélectionnez le BacPac de l’exemple de base de données à partir de votre système de fichiers.
5. Sous **paramètres de la base de données** , modifiez le nom de la base de données en *WideWorldImportersDW* et sélectionnez l’édition cible et l’objectif de service à utiliser.
6. Cliquez sur **suivant** et sur **Terminer** pour lancer le déploiement. L’exécution de cette opération nécessite quelques minutes. Lorsque vous spécifiez un objectif de service inférieur à S2, il peut prendre plus de temps.

## <a name="configuration"></a>Configuration

[S’applique à SQL Server 2016 (et versions ultérieures) Developer/Evaluation/Enterprise Edition]

L’exemple de base de données peut utiliser Polybase pour interroger des fichiers dans Hadoop ou le stockage d’objets BLOB Azure. Toutefois, cette fonctionnalité n’est pas installée par défaut avec SQL Server, vous devez la sélectionner lors de l’installation de SQL Server. Par conséquent, une étape consécutive à l’installation est requise.

1. Dans SQL Server Management Studio, connectez-vous à la base de données WideWorldImportersDW et ouvrez une nouvelle fenêtre de requête.
2. Exécutez la commande T-SQL suivante pour activer l’utilisation de Polybase dans la base de données :

   Exécutez [application]. [Configuration_ApplyPolyBase]