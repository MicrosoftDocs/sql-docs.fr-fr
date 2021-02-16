---
title: Exemple de table HumanResources.myTeam (SQL Server) | Microsoft Docs
description: Pour exécuter des exemples de code pour l’importation et l’exportation de données en bloc dans SQL Server, vous devez créer une table de test nommée myTeam dans le schéma HumanResources.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- myTeam sample table [SQL Server]
- bulk importing [SQL Server], examples
- bulk exporting [SQL Server], examples
ms.assetid: 27da45a0-c1f4-4bf4-ab24-6196e80d3834
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da6202e2a32fd0e553e014097cabddce686e5b0a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351366"
---
# <a name="humanresourcesmyteam-sample-table-sql-server"></a>Exemple de table HumanResources.myTeam (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Les exemples de code fournis au chapitre [Importation et exportation de données en bloc](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) font généralement appel à une table de test spéciale nommée **myTeam**. Pour pouvoir mettre à exécution les exemples, vous devez d’abord créer la table **myTeam** dans le schéma **HumanResources** de la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
> [!NOTE]  
>  [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] est l’un des exemples de bases de données fournis dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
 La table **myTeam** contient les colonnes suivantes.  
  
|Colonne|Type de données|Possibilité de valeurs nulles|Description|  
|------------|---------------|-----------------|-----------------|  
|**EmployeeID**|**smallint**|Non Null|Clé primaire des lignes. EmployeeID d'un membre de l'équipe.|  
|**Nom**|**nvarchar(50)**|Non Null|Name est un membre de l'équipe.|  
|**Titre**|**nvarchar(50)**|Nullable|Fonction occupée par l'employé au sein de l'équipe.|  
|**Contexte**|**nvarchar(50)**|Non Null|Date et heure de dernière mise à jour de la ligne (Par défaut)|  
  
**Pour créer HumanResources.myTeam**  
  
-   Utilisez les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
    ```sql
    --Create HumanResources.MyTeam:   
    USE AdventureWorks;  
    GO  
    CREATE TABLE HumanResources.myTeam   
    (EmployeeID smallint NOT NULL,  
    Name nvarchar(50) NOT NULL,  
    Title nvarchar(50) NULL,  
    Background nvarchar(50) NOT NULL DEFAULT ''  
    );  
    GO  
    ```  
  
**Pour remplir HumanResources.myTeam**  
  
-   Exécutez les instructions `INSERT` suivantes pour remplir la table de deux lignes :  
  
    ```sql
    USE AdventureWorks;  
    GO  
    INSERT INTO HumanResources.myTeam(EmployeeID,Name,Title,Background)  
       VALUES(77,'Mia Doppleganger','Administrative Assistant','Microsoft Office');  
    GO  
    INSERT INTO HumanResources.myTeam(EmployeeID,Name,Title,Background)  
       VALUES(49,'Hirum Mollicat','I.T. Specialist','Report Writing and Data Mining');  
    GO  
    ```  
  
    > [!NOTE]  
    >  Ces instructions ignorent la quatrième colonne, `Background`. Celle-ci a une valeur par défaut. En l'ignorant, l'instruction `INSERT` laisse cette colonne vide.  
  
## <a name="see-also"></a>Voir aussi  
 [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  
