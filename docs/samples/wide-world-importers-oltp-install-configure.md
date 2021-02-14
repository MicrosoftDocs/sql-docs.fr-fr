---
title: Installer et configurer l’exemple de base de données WideWorldImporters
description: Suivez ces instructions pour télécharger, installer et configurer l’exemple de base de données WideWorldImporters avec SQL Server Management Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 04/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 588597398c11b9b374bdecd272162fe82053cd8f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354084"
---
# <a name="installation-and-configuration"></a>Installation et configuration
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Instructions d’installation et de configuration de la base de données OLTP des importateurs larges World.

## <a name="prerequisites"></a>Prérequis

- [SQL Server 2016](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) (ou version ultérieure) ou [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Pour obtenir la version complète de l’exemple, utilisez l’édition Évaluation SQL Server/Developer/Enterprise.
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md). Pour obtenir les meilleurs résultats, utilisez la version du 2016 juin ou une version ultérieure.

## <a name="download"></a>Télécharger

La dernière version de l’exemple :

[larges-World-importateurs-version](https://go.microsoft.com/fwlink/?LinkID=800630)

Téléchargez l’exemple de sauvegarde de base de données WideWorldImporters/BacPac qui correspond à votre édition de SQL Server ou Azure SQL Database.

Le code source permettant de recréer l’exemple de base de données est disponible à partir de l’emplacement suivant. Notez que la recréation de l’exemple entraînera de légères différences dans les données, car il existe un facteur aléatoire dans la génération de données :

[importateurs larges-World](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/wide-world-importers/sample-scripts)

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
5. Sous **paramètres de la base de données** , modifiez le nom de la base de données en *wideworldimporters* et sélectionnez l’édition cible et l’objectif de service à utiliser.
6. Cliquez sur **suivant** et sur **Terminer** pour lancer le déploiement. Quelques minutes sont à effectuer sur P1. Si vous souhaitez un niveau tarifaire inférieur, il est recommandé d’importer dans une nouvelle base de données P1, puis de modifier le niveau tarifaire au niveau souhaité.

## <a name="configuration"></a>Configuration

### <a name="full-text-indexing"></a>Indexation de texte intégral

L’exemple de base de données peut utiliser l’indexation de Full-Text. Toutefois, cette fonctionnalité n’est pas installée par défaut avec SQL Server, vous devez la sélectionner lors de l’installation de SQL Server (elle est activée par défaut dans Azure SQL Database). Par conséquent, une étape consécutive à l’installation est requise.

1. Dans SQL Server Management Studio, connectez-vous à la base de données WideWorldImporters et ouvrez une nouvelle fenêtre de requête.
2. Exécutez la commande T-SQL suivante pour activer l’utilisation de Full-Text indexation dans la base de données :  `EXECUTE Application.Configuration_ApplyFullTextIndexing`


### <a name="sql-server-audit"></a>SQL Server Audit

S’applique à : SQL Server

L’activation de l’audit dans SQL Server requiert la configuration du serveur. Pour activer l’audit de SQL Server pour l’exemple WideWorldImporters, exécutez l’instruction suivante dans la base de données :

```sql
EXECUTE [Application].[Configuration_ApplyAuditing]
```

Dans Azure SQL Database, l’audit est configuré par le biais du [portail Azure](https://portal.azure.com/).

### <a name="row-level-security"></a>Sécurité au niveau des lignes

S’applique à : Azure SQL Database

Row-Level la sécurité n’est pas activée par défaut dans le téléchargement BacPac de WideWorldImporters. Pour activer Row-Level la sécurité dans la base de données, exécutez la procédure stockée suivante :

```sql
EXECUTE [Application].[Configuration_ApplyRowLevelSecurity]
```