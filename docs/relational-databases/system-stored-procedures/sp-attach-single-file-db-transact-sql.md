---
description: sp_attach_single_file_db (Transact-SQL)
title: sp_attach_single_file_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_attach_single_file_db
- sp_attach_single_file_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_single_file_db
ms.assetid: 13bd1044-9497-4293-8390-1f12e6b8e952
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e8fd050ff5706cc2406fcb4502c5d93a8253bc4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197709"
---
# <a name="sp_attach_single_file_db-transact-sql"></a>sp_attach_single_file_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Associe au serveur actif une base de données qui ne possède qu'un seul fichier de données. **sp_attach_single_file_db** ne peut pas être utilisé avec plusieurs fichiers de données.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Nous vous recommandons d’utiliser CREATe DATABASE *database_name* pour Attach à la place. Pour plus d’informations, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md). N'utilisez pas cette procédure sur une base de données répliquée.  
  
> [!IMPORTANT]  
>  Nous vous recommandons de ne pas attacher ni restaurer de bases de données provenant de sources inconnues ou non approuvées. Ces bases de données peuvent contenir du code malveillant susceptible d'exécuter du code [!INCLUDE[tsql](../../includes/tsql-md.md)] indésirable ou de provoquer des erreurs en modifiant le schéma ou la structure physique des bases de données. Avant d’utiliser une base de données issue d’une source inconnue ou non approuvée, exécutez [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) sur la base de données sur un serveur autre qu’un serveur de production et examinez également le code, notamment les procédures stockées ou le code défini par l’utilisateur, de la base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_attach_single_file_db [ @dbname= ] 'dbname'  
    , [ @physname= ] 'physical_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbname = ] 'dbname'` Nom de la base de données à attacher au serveur. Le nom doit être unique. *dbname* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @physname = ] 'physical_name'` Nom physique, y compris le chemin d’accès, du fichier de base de données. *physical_name* est de type **nvarchar (260)**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  Cet argument crée un mappage vers le paramètre FILENAME de l'instruction CREATE DATABASE. Pour plus d’informations, consultez [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
 Quand vous attachez une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] qui contient des fichiers catalogue de texte intégral à une instance de serveur [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , les fichiers catalogue sont attachés à partir de leur emplacement précédent avec les autres fichiers de base de données, les mêmes que dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations, consultez [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md).  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Utilisez **sp_attach_single_file_db** uniquement sur les bases de données qui ont été précédemment détachées du serveur à l’aide d’une opération de **sp_detach_db** explicite ou sur des bases de données copiées.  
  
 **sp_attach_single_file_db** fonctionne uniquement sur les bases de données qui ont un seul fichier journal. Lorsque **sp_attach_single_file_db** attache la base de données au serveur, il génère un nouveau fichier journal. Si la base de données est en lecture seule, le journal est créé au même endroit que le précédent.  
  
> [!NOTE]  
>  Un instantané de base de données ne peut pas être détaché ni attaché.  
  
 N'utilisez pas cette procédure sur une base de données répliquée.  
  
## <a name="permissions"></a>Autorisations  
 Pour plus d’informations sur la façon dont les autorisations sont gérées lors de l’attachement d’une base de données, consultez [Create database &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple suivant, [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] est détaché, puis un des fichiers d'[!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] est attaché au serveur actif.  
  
```  
USE master;  
GO  
EXEC sp_detach_db @dbname = 'AdventureWorks2012';  
EXEC sp_attach_single_file_db @dbname = 'AdventureWorks2012',   
    @physname =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_Data.mdf';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
