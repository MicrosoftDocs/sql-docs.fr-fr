---
title: Exemples de bases de données AdventureWorks
description: Suivez ces instructions pour télécharger et installer des exemples de bases de données AdventureWorks dans SQL Server à l’aide de Transact-SQL (T-SQL), SQL Server Management Studio (SSMS) ou Azure Data Studio.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 06/16/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: ec38d7eeed89d03bab4ebd92446517542e2e51a0
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597002"
---
# <a name="adventureworks-sample-databases"></a>Exemples de bases de données AdventureWorks
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cet article fournit des liens directs pour télécharger des exemples de bases de données AdventureWorks, ainsi que des instructions pour les restaurer sur SQL Server et Azure SQL Database. 

Pour plus d’informations sur les exemples, consultez le [référentiel GitHub Samples](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases). 

## <a name="prerequisites"></a>Prérequis

- [SQL Server](https://www.microsoft.com/evalcenter/evaluate-sql-server-2019) ou [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) ou [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)


## <a name="download-backup-files"></a>Télécharger les fichiers de sauvegarde 

Utilisez ces liens pour télécharger l’exemple de base de données approprié pour votre scénario. 

- Les données **OLTP** sont pour la plupart des charges de travail de traitement de transactions en ligne courantes. 
- Les données de **Data Warehouse (DW)** sont destinées aux charges de travail d’entreposage de données. 
- Les données **légères (LT)** sont une version légère et réduite de l’exemple **OLTP** . 

Si vous n’êtes pas sûr de ce dont vous avez besoin, commencez par la version OLTP qui correspond à votre version de SQL Server. 

|**OLTP** |**Data Warehouse** |**Légèreté**|
|---------|---------|---------|
|[AdventureWorks2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak)|[AdventureWorksDW2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2019.bak)|[AdventureWorksLT2019. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2019.bak)|
|[AdventureWorks2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2017.bak)|[AdventureWorksDW2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2017.bak)|[AdventureWorksLT2017. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2017.bak)|
|[AdventureWorks2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016.bak)|[AdventureWorksDW2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016.bak)|[AdventureWorksLT2016. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2016.bak)|
|[AdventureWorks2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016_EXT.bak)|[AdventureWorksDW2016_EXT. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2016_EXT.bak)| N/A |
|[AdventureWorks2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2014.bak)|[AdventureWorksDW2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2014.bak)|[AdventureWorksLT2014. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2014.bak)|
|[AdventureWorks2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2012.bak)|[AdventureWorksDW2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW2012.bak)|[AdventureWorksLT2012. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksLT2012.bak)|
|[AdventureWorks2008R2. bak](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-oltp.bak)| [AdventureWorksDW2008R2. bak](https://github.com/microsoft/sql-server-samples/releases/download/adventureworks2008r2/adventure-works-2008r2-dw.bak) | N/A |

Des fichiers supplémentaires se trouvent directement sur GitHub : 

- [SQL Server 2014-2019](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)
- [SQL Server 2012](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2012)
- [SQL Server 2008 et 2008R2](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks2008r2)


## <a name="restore-to-sql-server"></a>Restaurer à SQL Server 

Vous pouvez utiliser le `.bak` fichier pour restaurer votre exemple de base de données sur votre instance de SQL Server. Vous pouvez le faire à l’aide de la commande [Restore (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) ou à l’aide de l’interface graphique (GUI) dans [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md) ou [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

# <a name="sql-server-management-studio-ssms"></a>[SQL Server Management Studio (SSMS)](#tab/ssms)

Si vous n’êtes pas familiarisé avec l’utilisation de SQL Server Management Studio (SSMS), vous pouvez voir [connexion & requête](../ssms/quickstarts/ssms-connect-query-sql-server.md) pour commencer. 

Pour restaurer votre base de données dans SQL Server Management Studio, procédez comme suit :

1. Téléchargez le `.bak` fichier approprié à partir de l’un des liens fournis dans la section [Télécharger les fichiers de sauvegarde](#download-backup-files) .
2. Déplacez le `.bak` fichier vers votre SQL Server emplacement de sauvegarde. Cela dépend de votre emplacement d’installation, du nom de l’instance et de la version de SQL Server. Par exemple, l’emplacement par défaut d’une instance par défaut de SQL Server 2019 est :

   `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`. 

3. Ouvrez SQL Server Management Studio (SSMS) et connectez-vous à votre SQL Server dans. 
4. Cliquez avec le bouton droit sur **bases de données** dans l' **Explorateur d’objets**  >  **restaurer la base de données...** pour lancer l’Assistant **restauration de base de** données. 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-ssms.png" alt-text="Capture d’écran montrant comment choisir de restaurer votre base de données en cliquant avec le bouton droit sur bases de données dans l’Explorateur d’objets, puis en sélectionnant restaurer la base de données.":::


1. Sélectionnez **appareil** , puis sélectionnez les points de suspension **(...)** pour choisir un appareil. 
1. Sélectionnez **Ajouter** , puis choisissez le `.bak` fichier que vous avez récemment déplacé vers cet emplacement. Si vous avez déplacé votre fichier à cet emplacement mais que vous ne le voyez pas dans l’Assistant, cela indique généralement un problème d’autorisation SQL Server ou l’utilisateur connecté à SQL Server n’a pas d’autorisation sur ce fichier dans ce dossier. 
1. Sélectionnez **OK** pour confirmer la sélection de la sauvegarde de la base de données et fermez la fenêtre **Sélectionner les unités de sauvegarde** . 
1. Sélectionnez l’onglet **fichiers** pour confirmer la **restauration en tant qu'** emplacement et les noms de fichiers correspondant à l’emplacement et aux noms de fichiers souhaités dans l’Assistant **restauration de base de données** . 
1. Sélectionnez **OK** pour restaurer votre base de données. 

   :::image type="content" source="media/adventureworks-install-configure/restore-db-wizard-ssms.png" alt-text="Capture d’écran montrant la fenêtre restaurer la base de données avec le jeu de sauvegarde à restaurer mis en surbrillance et l’option OK appelée out.":::

Pour plus d’informations sur la restauration d’une base de données SQL Server, consultez [restaurer une sauvegarde de base de données à l’aide de SSMS](../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md).

# <a name="transact-sql-t-sql"></a>[Transact-SQL (T-SQL)](#tab/tsql)

Vous pouvez restaurer votre exemple de base de données à l’aide de Transact-SQL (T-SQL). Un exemple de restauration de AdventureWorks2019 est fourni ci-dessous, mais le nom de la base de données et le chemin d’accès au fichier d’installation peuvent varier en fonction de votre environnement. 

Pour restaurer AdventureWorks2019 sur **Windows**, modifiez les valeurs en fonction de votre environnement, puis exécutez la commande Transact-SQL (T-SQL) suivante :


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019] 
FROM  DISK = N'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup\AdventureWorks2019.bak' 
WITH  FILE = 1,  NOUNLOAD,  STATS = 5
GO

```

Pour restaurer AdventureWorks2019 sur **Linux**, modifiez le chemin d’accès du système de fichiers Windows en Linux, puis exécutez la commande Transact-SQL (T-SQL) suivante : 


```sql
USE [master]
RESTORE DATABASE [AdventureWorks2019]
FROM DISK = '/var/opt/mssql/backup/AdventureWorks2019.bak'
WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf',
MOVE 'AdventureWorks2017_log' TO '/var/opt/mssql/data/AdventureWorks2019_log.ldf',
FILE = 1,  NOUNLOAD,  STATS = 5
GO
```

# <a name="azure-data-studio"></a>[Azure Data Studio](#tab/data-studio)

Si vous n’êtes pas familiarisé avec [Azure Data Studio Studio](../azure-data-studio/download-azure-data-studio.md), vous pouvez voir [connexion & requête](../azure-data-studio/quickstart-sql-server.md) pour commencer

Pour restaurer votre base de données dans Azure Data Studio, procédez comme suit :

1. Téléchargez le `.bak` fichier approprié à partir de l’un des liens fournis dans la section [Télécharger les fichiers de sauvegarde](#download-backup-files) .
1. Déplacez le `.bak` fichier vers votre SQL Server emplacement de sauvegarde. Cela dépend de votre emplacement d’installation, du nom de l’instance et de la version de SQL Server. Par exemple, l’emplacement par défaut d’une instance par défaut de SQL Server 2019 est :

    `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Backup`.

1. Ouvrez Azure Data Studio Studio et connectez-vous à votre instance SQL Server.
1. Cliquez avec le bouton droit sur votre serveur et sélectionnez **gérer**.

   :::image type="content" source="media/adventureworks-install-configure/ads-manage.png" alt-text="Capture d’écran montrant Azure Data Studio avec l’option gérer mise en surbrillance et appelée.":::

1. Sélectionner **restaurer**

   :::image type="content" source="media/adventureworks-install-configure/ads-restore-database.png" alt-text="Sélectionnez restaurer dans le menu supérieur pour restaurer votre base de données.":::

1. Sous l’onglet **général** , renseignez les valeurs listées sous **source**.
    1. Sous **restaurer à partir de**, sélectionnez *fichier de sauvegarde*.
    1. Sous **chemin d’accès du fichier de sauvegarde**, sélectionnez l’emplacement où vous avez stocké le fichier. bak. 
    
   :::image type="content" source="media/adventureworks-install-configure/ads-source.png" alt-text="Sélectionner le chemin d’accès de votre fichier de sauvegarde":::
    
    Cette option remplit automatiquement le reste des champs tels que **base de données**, **base de données cible** et **restaurer sur**. 

   :::image type="content" source="media/adventureworks-install-configure/ads-destination-restore-plan.png" alt-text="Une fois que vous avez choisi un chemin d’accès de fichier de sauvegarde, les autres champs se remplissent":::

1. Sélectionnez **restaurer** pour restaurer votre base de données. 

   :::image type="content" source="media/adventureworks-install-configure/ads-restore.png" alt-text="Une fois que vous êtes prêt, sélectionnez restaurer pour restaurer votre base de données.":::

---

## <a name="deploy-to-azure-sql-database"></a>Déployer sur Azure SQL Database

Deux options s’offrent à vous pour afficher des exemples de données de Azure SQL Database. Vous pouvez utiliser un exemple lorsque vous créez une nouvelle base de données, ou vous pouvez déployer une base de données à partir de SQL Server directement vers Azure à l’aide de SQL Server Management Studio (SSMS).

Pour obtenir des exemples de données pour Azure SQL Managed Instance à la place, consultez [restaurer des importateurs mondiaux dans sql Managed instance](/azure/azure-sql/managed-instance/restore-sample-database-quickstart). 

### <a name="deploy-new-sample-database"></a>Déployer un nouvel exemple de base de données

Lorsque vous créez une nouvelle base de données dans Azure SQL Database, vous avez la possibilité de créer une base de données vide ou un exemple de base de données. 

Procédez comme suit pour utiliser un exemple de base de données pour créer une nouvelle base de données : 

1. Connectez-vous à votre Portail Azure.
1. Sélectionnez **créer une ressource** dans le coin supérieur gauche du volet de navigation. 
1. Sélectionnez **bases de données** , puis **SQL Database**. 
1. Renseignez les informations demandées pour créer votre base de données. 
1. Sous l’onglet **paramètres supplémentaires** , choisissez **exemple** comme données existantes sous **source de données**: 

   :::image type="content" source="media/adventureworks-install-configure/deploy-sample-to-azure.png" alt-text="Choisissez exemple comme source de données sous l’onglet Paramètres supplémentaires dans le Portail Azure lors de la création de votre Azure SQL Database":::

1. Sélectionnez **créer** pour créer votre SQL Database, qui est la copie restaurée de la base de données AdventureWorksLT. 


### <a name="deploy-database-from-sql-server"></a>Déployer une base de données à partir d’SQL Server

SQL Server Management Studio offre la possibilité de déployer une base de données directement sur Azure SQL Database. Cette méthode ne fournit pas actuellement de validation des données. elle est destinée au développement et aux tests et ne doit pas être utilisée pour la production. 

Pour déployer un exemple de base de données à partir de SQL Server vers Azure SQL Database, procédez comme suit :

1. Connectez-vous à votre SQL Server dans SQL Server Management Studio. 
1. Si vous ne l’avez pas déjà fait, [restaurez l’exemple de base de données sur SQL Server](#restore-to-sql-server). 
1. Cliquez avec le bouton droit sur votre base de données restaurée dans tâches de l' **Explorateur d’objets**  >    >  **déployer la base de données sur Microsoft Azure SQL Database...**. 

   :::image type="content" source="media/adventureworks-install-configure/deploy-db-to-azure.png" alt-text="Choisissez de déployer votre base de données sur Microsoft Azure SQL Database en cliquant avec le bouton droit sur votre base de données et en sélectionnant tâches.":::

1. Suivez l’Assistant pour vous connecter à Azure SQL Database et déployer votre base de données. 


## <a name="creation-scripts"></a>Scripts de création

Au lieu de restaurer une base de données, vous pouvez également utiliser des scripts pour créer les bases de données AdventureWorks, quelle que soit la version. 

Les scripts ci-dessous peuvent être utilisés pour créer l’intégralité de la base de données AdventureWorks : 

- [Zip des scripts OLTP AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks-oltp-install-script.zip)
- [Zip des scripts AdventureWorks DW](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorksDW-data-warehouse-install-script.zip)

Vous trouverez des informations supplémentaires sur l’utilisation des scripts sur [GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks). 

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez restauré votre base de données exemple, à l’aide des didacticiels suivants pour commencer à utiliser SQL Server : 


[Didacticiels pour le moteur de base de données SQL Server](../relational-databases/database-engine-tutorials.md)   
[Se connecter et interroger avec SQL Server Management Studio (SSMS)](../ssms/quickstarts/ssms-connect-query-sql-server.md)   
[Se connecter et interroger avec Azure Data Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)