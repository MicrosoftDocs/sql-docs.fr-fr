---
description: Déplacer une base de données à l’aide de la méthode de détachement et d’attachement (Transact-SQL)
title: Déplacer une base de données à l’aide de la méthode de détachement et d’attachement (Transact-SQL)
ms.date: 06/03/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- database attaching [SQL Server]
- moving databases [SQL Server]
- database detaching [SQL Server]
- relocating databases [SQL Server]
- detaching databases [SQL Server]
- attaching databases [SQL Server]
ms.assetid: 6732a431-cdef-4f1e-9262-4ac3b77c275e
author: stevestein
ms.author: sstein
ms.custom: seo-dt-2019
ms.openlocfilehash: 99abbddfa51a62ef1075755126712515b3b5c9ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348011"
---
# <a name="move-a-database-using-detach-and-attach-transact-sql"></a>Déplacer une base de données à l’aide de la méthode de détachement et d’attachement (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment déplacer une base de données détachée vers un autre emplacement puis la rattacher à la même instance de serveur ou à une instance différente dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Toutefois, nous vous recommandons de déplacer les bases de données à l'aide de la procédure de déplacement planifié ALTER DATABASE, plutôt qu'à l'aide de la méthode de détachement et d'attachement. Pour plus d’informations, consultez [Déplacer des bases de données utilisateur](../../relational-databases/databases/move-user-databases.md).  
  
> [!IMPORTANT]  
>  Nous vous recommandons de ne pas attacher ni restaurer de bases de données provenant de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez également le code, notamment les procédures stockées ou le code défini par l’utilisateur, de la base de données.  
  
## <a name="procedure"></a>Procédure  
  
#### <a name="to-move-a-database-by-using-detach-and-attach"></a>Pour déplacer une base de données à l'aide des opérations de détachement et d'attachement  
  
1.  Détachez la base de données. Pour plus d’informations, consultez [Détacher une base de données](../../relational-databases/databases/detach-a-database.md).  
  
2.  Dans une fenêtre de l'Explorateur Windows ou de l'invite de commandes Windows, déplacez les fichiers journaux et les fichiers de base de données détachés à l'emplacement de votre choix.  
  
     Vous devez déplacer les fichiers journaux, même si vous envisagez d'en créer de nouveaux. Dans certains cas, le rattachement d'une base de données nécessite ses fichiers journaux existants. Par conséquent, conservez toujours tous les fichiers journaux détachés jusqu'à ce que la base de données ait été attachée avec succès sans eux.  
  
    > [!NOTE]  
    >  Si vous tentez d'attacher la base de données sans spécifier le fichier journal, l'opération attach recherche le fichier journal à son emplacement d'origine. Si une copie du journal existe toujours à l'emplacement d'origine, elle est attachée. Pour éviter d'utiliser le fichier journal d'origine, spécifiez le chemin d'accès au nouveau fichier journal ou supprimez la copie d'origine du fichier journal (après l'avoir copiée au nouvel emplacement).  
  
3.  Attachez les fichiers copiés. Pour plus d’informations, consultez [Attach a Database](../../relational-databases/databases/attach-a-database.md).  
  
## <a name="example"></a> Exemple  
 L'exemple suivant crée une copie de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] nommée `MyAdventureWorks`. Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sont exécutées dans une fenêtre d'éditeur de requêtes connectée à l'instance de serveur concernée par l'attachement.  
  
1.  Détachez la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en exécutant les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
    ```sql
    USE master;  
    GO  
    EXEC sp_detach_db @dbname = N'AdventureWorks2012';  
    GO  
    ```  
  
2.  À l'aide de la méthode de votre choix, copiez les fichiers de base de données dventureWorks208R2_Data.mdf et AdventureWorks208R2_log) vers : C:\MySQLServer\AdventureWorks208R2_Data.mdf et C:\MySQLServer\AdventureWorks208R2_Log.ldf, respectivement.  
  
    > [!IMPORTANT]  
    >  Dans le cas d'une base de données de production, placez la base de données et le journal des transactions sur des disques distincts.  
  
     Pour copier des fichiers via le réseau sur le disque d'un ordinateur distant, utilisez le nom UNC (Universal Naming Convention) de l'emplacement distant. Un nom UNC se présente sous la forme **\\\\** _Servername_ **\\** _Sharename_ **\\** _Path_ **\\** _Filename_. Comme lors de l'écriture de fichiers sur le disque dur local, les autorisations appropriées nécessaires à la lecture et à l'écriture d'un fichier sur le disque distant doivent être accordées au compte d'utilisateur utilisé par l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Attachez la base de données déplacée et si vous le souhaitez, son journal, en exécutant les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes :  
  
    ```sql
    USE master;  
    GO  
    CREATE DATABASE MyAdventureWorks   
        ON (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Data.mdf'),  
        (FILENAME = 'C:\MySQLServer\AdventureWorks2012_Log.ldf')  
        FOR ATTACH;  
    GO  
    ```  
  
     Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], une base de données nouvellement attachée n'est pas immédiatement visible dans l'Explorateur d'objets. Pour visualiser la base de données, dans l'Explorateur d'objets, cliquez sur **Affichage** puis sur **Actualiser**. Si le nœud **Bases de données** est développé dans l'Explorateur d'objets, la base de données récemment attachée apparaît dans la liste des bases de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
  
  
