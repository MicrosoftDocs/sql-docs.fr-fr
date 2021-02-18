---
title: Vue d’ensemble des outils SQL
description: Outils de gestion et de requête SQL pour SQL Server, Azure SQL (Azure SQL Database, Azure SQL Managed Instance, machines virtuelles SQL) et Azure Synapse Analytics.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ''
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 3595e84b4da525894c353c8b5d702b69b19f354d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338670"
---
# <a name="sql-tools-overview"></a>Vue d’ensemble des outils SQL

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Pour gérer votre base de données, vous avez besoin d’un outil. Que vos bases de données soient exécutées dans le cloud, sur Windows, sur macOS ou sur [Linux](../linux/sql-server-linux-overview.md), votre outil n’a pas besoin de s’exécuter sur la même plateforme que la base de données.

Vous pouvez afficher les liens vers les différents outils SQL dans les tableaux suivants.

> [!Note]
> Pour télécharger SQL Server, consultez [Installer SQL Server](../database-engine/install-windows/install-sql-server.md).

## <a name="recommended-tools"></a>Outils recommandés

Les outils suivants fournissent une interface utilisateur graphique (GUI).

| Outil | Description | Système d’exploitation |
|:--|:--|:--|
| [ **![Image ADS](../tools/media/overview-sql-tools/azure-data-studio.svg)</br></br>Azure Data Studio**](../azure-data-studio/download.md) | Éditeur léger capable d’exécuter des requêtes SQL à la demande, d’afficher et d’enregistrer les résultats au format texte, JSON ou Excel. Modifiez les données, organisez vos connexions de base de données favorites et parcourez les objets de base de données dans une expérience de navigation d’objets familière. | **Windows</br>macOS</br>Linux** |
| [ **![Image SSMS](../tools/media/overview-sql-tools/ssms.svg)</br></br>SQL Server Management Studio (SSMS)**](../ssms/download-sql-server-management-studio-ssms.md) | Gérez une instance ou une base de données SQL Server avec prise en charge complète de l’interface utilisateur graphique. Accédez à, configurez, gérez, administrez et développez tous les composants de SQL Server, Azure SQL Database et Azure Synapse Analytics. Fournit un utilitaire unique et complet qui associe un vaste ensemble d’outils graphiques à différents éditeurs de script performants, pour permettre aux développeurs et aux administrateurs de base de données de tous niveaux d’avoir accès à SQL. | **Windows** |
| [ **![Image SSDT](../tools/media/overview-sql-tools/ssdt.svg)</br>SQL Server Data Tools (SSDT)**](../ssdt/download-sql-server-data-tools-ssdt.md) | Outil de développement moderne permettant de générer des bases de données relationnelles SQL Server, des bases de données SQL Azure, des modèles de données AS (Analysis Services), des packages IS (Integration Services) et des rapports RS (Reporting Services). Avec SSDT, vous pouvez concevoir et déployer tout type de contenu SQL Server avec la même facilité que lorsque vous développez une application dans **[Visual Studio](https://visualstudio.microsoft.com/downloads/)** . | **Windows** |
| [ **![Image VS Code](../tools/media/overview-sql-tools/visual-studio-code.svg)</br></br>Visual Studio Code**](https://code.visualstudio.com/) | L’extension **[mssql](https://marketplace.visualstudio.com/items?itemName=ms-mssql.mssql)** pour Visual Studio Code est l’extension SQL Server officielle qui prend en charge les connexions à SQL Server et offre une expérience d’édition riche pour T-SQL dans Visual Studio Code. Écrivez des scripts T-SQL dans un éditeur léger. | **Windows</br>macOS</br>Linux** |

## <a name="command-line-tools"></a>Outils de ligne de commande

Les outils ci-dessous sont les principaux outils en ligne de commande.

| Outil | Description | Système d’exploitation |
|:--|:--|:--|
|[**bcp**](bcp-utility.md)|L’utilitaire **b** ulk **c** opy **p** rogram (**bcp**) copie en bloc des données entre une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et un fichier de données dans un format spécifié par l’utilisateur.| **Windows</br>macOS</br>Linux** |
|[**mssql-cli (préversion)**](mssql-cli.md)|**mssql-cli** est un outil de requête de ligne de commande interactif pour interroger SQL Server. Interrogez, en outre, SQL Server avec un outil en ligne de commande qui intègre IntelliSense, la mise en surbrillance de la syntaxe et bien plus encore. | **Windows</br>macOS</br>Linux** |
|[**mssql-conf**](../linux/sql-server-linux-configure-mssql-conf.md) | **mssql-conf** configure SQL Server s’exécutant sur Linux. | **Linux** |
|[**mssql-scripter (préversion)**](https://github.com/Microsoft/mssql-scripter) | **mssql-scripter** est une expérience en ligne de commande multiplateforme pour l’écriture de scripts de bases de données SQL Server. | **Windows</br>macOS</br>Linux** |
| [**sqlcmd**](sqlcmd-utility.md) |L’utilitaire **sqlcmd** vous permet d’entrer des instructions Transact-SQL, des procédures système et des fichiers de script à l’invite de commandes. | **Windows</br>macOS</br>Linux** |
| [**sqlpackage**](sqlpackage/sqlpackage.md) |**sqlpackage** est un utilitaire en ligne de commande qui automatise plusieurs tâches de développement de base de données. |**Windows</br>macOS</br>Linux** |
|[**SQL Server PowerShell**](../powershell/sql-server-powershell.md)| **SQL Server PowerShell** fournit des cmdlets pour l’utilisation de SQL. | **Windows</br>macOS</br>Linux** |

## <a name="migration-and-other-tools"></a>Migration et autres outils

Ces outils sont utilisés pour migrer, configurer et fournir d’autres fonctionnalités pour les bases de données SQL.

| Outil | Description |
|:--|:--|
| **[Gestionnaire de configuration](../tools/configuration-manager/sql-server-configuration-manager-help.md)** | Utilisez le Gestionnaire de configuration SQL Server pour configurer les services SQL Server et la connectivité réseau. Le Gestionnaire de configuration s’exécute sur Windows|
| **[Assistant Expérimentation de base de données](../dea/database-experimentation-assistant-overview.md)** | Utilisez Assistant Expérimentation de base de données pour évaluer une version ciblée de SQL pour une charge de travail donnée. |
| **[Assistant Migration des données](../dma/dma-overview.md)** | L’Assistant Migration de données vous aide à effectuer une mise à niveau vers une plateforme de données moderne en détectant les problèmes de compatibilité pouvant impacter les fonctionnalités des bases de données dans votre nouvelle version de SQL Server ou d’Azure SQL Database. |
| **[Distributed Replay](../tools/distributed-replay/install-distributed-replay-overview.md)** | Utilisez la fonctionnalité Distributed Replay pour vous aider à évaluer l’impact de futures mises à niveau de SQL Server. Vous pouvez également utiliser Distributed Replay pour évaluer l’impact des mises à niveau du matériel et du système d’exploitation, ainsi que des paramétrages de SQL Server. |
| **[ssbdiagnose](../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)** | L’utilitaire ssbdiagnose signale des problèmes dans des conversations Service Broker ou dans la configuration des services Service Broker. |
| **[Assistant Migration SQL Server](../ssma/sql-server-migration-assistant.md)** | Utilisez l’Assistant Migration SQL Server pour automatiser la migration de bases de données vers SQL Server à partir de Microsoft Access, DB2, MySQL, Oracle et Sybase.|

Si vous recherchez des outils supplémentaires qui ne sont pas mentionnés sur cette page, consultez [Utilitaires d’invite de commandes SQL](command-prompt-utility-reference-database-engine.md) et [Télécharger les fonctionnalités et les outils étendus de SQL Server](download-sql-feature-packs.md).
