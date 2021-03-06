---
title: Supprimer un instantané de base de données (Transact-SQL) | Microsoft Docs
description: Découvrez comment supprimer un instantané de base de données à l’aide de Transact-SQL, ce qui supprime l’instantané de SQL Server et les fichiers partiellement alloués utilisés par l’instantané.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- removing database snapshots
- deleting database snapshots
- database snapshots [SQL Server], deleting
ms.assetid: ad70ec97-d5fb-41aa-b72a-915e74b61b76
author: stevestein
ms.author: sstein
ms.openlocfilehash: 9ae40c8d9b4a93ff1f035953207caae5addb56b2
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85756159"
---
# <a name="drop-a-database-snapshot-transact-sql"></a>supprimer un instantané de base de données (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La suppression d'un instantané de base de données entraîne la suppression de l'instantané de base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ainsi que celle des fichiers partiellement alloués utilisés par l'instantané. Lorsque vous supprimez un instantané de base de données, toutes ses connexions utilisateur sont terminées.  
  
## <a name="security"></a>Sécurité  
  
###  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Tout utilisateur doté des autorisations DROP DATABASE peut supprimer un instantané de base de données.  
  
##  <a name="how-to-drop-a-database-snapshot-using-transact-sql"></a><a name="TsqlProcedure"></a> Procédure de suppresssion d'un instantané de base de données (à l'aide de Transact-SQL)  
 **Pour supprimer un instantané de base de données**  
  
1.  Identifiez l'instantané à supprimer. Vous pouvez afficher les instantanés d'une base de données dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Afficher un instantané de base de données &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md).  
  
2.  Exécutez l’instruction [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md) en définissant le nom de l’instantané de base de données à supprimer. La syntaxe est la suivante :  
  
     DROP DATABASE *nom_instantané_base_de_données* [ **,** ...*n* ]  
  
     où *nom_instantané_base_de_données* est le nom de l’instantané de base de données à supprimer.  
  
####  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 L'exemple suivant supprime un instantané de base de données nommé SalesSnapshot0600 sans affecter la base de données source.  
  
```  
DROP DATABASE SalesSnapshot0600 ;  
```  
  
 Les connexions utilisateur à SalesSnapshot0600 sont terminées, et tous les fichiers physiques partiellement alloués du système de fichiers NTFS utilisés par l'instantané sont supprimés.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation des fichiers partiellement alloués par les instantanés de base de données, consultez [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Créer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md)  
  
-   [Afficher un instantané de base de données &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [Rétablir une base de données dans l’état d’un instantané de base de données](../../relational-databases/databases/revert-a-database-to-a-database-snapshot.md)  
  
  
## <a name="see-also"></a>Voir aussi  
 [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md)   
 [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)  
  
  
