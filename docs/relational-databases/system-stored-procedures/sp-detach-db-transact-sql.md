---
description: sp_detach_db (Transact-SQL)
title: sp_detach_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/30/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_detach_db
- sp_detach_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_detach_db
- detaching databases [SQL Server]
ms.assetid: abcb1407-ff78-4c76-b02e-509c86574462
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1d4b484cd6d440b14b7639b513fba7e53f615443
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200760"
---
# <a name="sp_detach_db-transact-sql"></a>sp_detach_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Détache d'une instance de serveur une base de données qui n'est pas en cours d'utilisation, et exécute accessoirement UPDATE STATISTICS sur toutes les tables avant de détacher la base de données.  
  
> [!IMPORTANT]  
>  Pour détacher une base de données répliquée, celle-ci ne doit pas être publiée. Pour plus d'informations, consultez la section « Notes », plus loin dans cette rubrique.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_detach_db [ @dbname= ] 'database_name'   
    [ , [ @skipchecks= ] 'skipchecks' ]   
    [ , [ @keepfulltextindexfile = ] 'KeepFulltextIndexFile' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbname = ] 'database_name'` Nom de la base de données à détacher. *database_name* est une valeur de **type sysname** , avec NULL comme valeur par défaut.  
  
`[ @skipchecks = ] 'skipchecks'` Spécifie si les statistiques de mise à jour doivent être ignorées ou exécutées. *option skipchecks* est une valeur **nvarchar (10)** , avec NULL comme valeur par défaut. Pour ignorer UPDATE STATISTICs, spécifiez **true**. Pour exécuter explicitement UPDATE STATISTICs, spécifiez **false**.  
  
 Par défaut, UPDATE STATISTICS est exécuté pour mettre à jour les informations relatives aux données des tables et des index. L'exécution de UPDATE STATISTICS est utile pour les bases de données qui doivent être placées sur des supports en lecture seule.  
  
`[ @keepfulltextindexfile = ] 'KeepFulltextIndexFile'` Spécifie que le fichier d’index de recherche en texte intégral associé à la base de données en cours de détachement ne sera pas supprimé pendant l’opération de détachement de la base de données. *Keepfulltextindexfile* est une valeur **nvarchar (10)** avec **true** comme valeur par défaut. Si *keepfulltextindexfile* a la **valeur false**, tous les fichiers d’index de recherche en texte intégral associés à la base de données et les métadonnées de l’index de recherche en texte intégral sont supprimés, sauf si la base de données est en lecture seule. Si la valeur est NULL ou **true**, les métadonnées associées au texte intégral sont conservées.  
  
> [!IMPORTANT]
>  Le paramètre **\@ keepfulltextindexfile** sera supprimé dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Évitez d'utiliser ce paramètre dans de nouveaux travaux de développement, et modifiez dès que possible les applications qui utilisent actuellement ce paramètre.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Lorsqu'une base de données est détachée, toutes ses métadonnées sont supprimées. Si la base de données était la base de données par défaut de tous les comptes de connexion, **Master** devient la base de données par défaut.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’affichage de la base de données par défaut de tous les comptes de connexion, consultez [sp_helplogins &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md). Si vous disposez des autorisations requises, vous pouvez utiliser [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) pour affecter une nouvelle base de données par défaut à une connexion.  
  
## <a name="restrictions"></a>Restrictions  
 Une base de données ne peut pas être détachée Si l’une des conditions suivantes est vraie :  
  
-   La base de données est en cours d'utilisation. Pour plus d'informations, consultez « Obtention d'un accès exclusif », plus loin dans cette rubrique.  
  
-   Si la base de données est répliquée, elle est publiée.  
  
     Avant de pouvoir détacher la base de données, vous devez désactiver la publication en exécutant [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md).  
  
    > [!NOTE]  
    >  Si vous ne pouvez pas utiliser **sp_replicationdboption**, vous pouvez supprimer la réplication en exécutant [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md).  
  
-   Un instantané existe sur la base de données.  
  
     Avant de pouvoir détacher la base de données, vous devez supprimer tous ses instantanés. Pour plus d’informations, consultez [Supprimer un instantané de base de données &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
    > [!NOTE]  
    >  Un instantané de base de données ne peut pas être détaché ni attaché.  
  
-   La base de données fait l'objet d'une mise en miroir.  
  
     La base de données ne peut pas être détachée tant que la session de mise en miroir de la base de données n'a pas été interrompue. Pour plus d’informations, consultez [Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
-   La base de données est suspecte.  
  
     Vous devez placer une base de données suspecte en mode urgence avant de pouvoir la détacher. Pour plus d’informations sur la manière de mettre une base de données en mode urgence, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
-   La base de données est une base de données système.  
  
## <a name="obtaining-exclusive-access"></a>Obtention d'un accès exclusif  
 Un accès exclusif à la base de données est nécessaire pour procéder au détachement de la base de données. Si la base de données que vous voulez détacher est en cours d'utilisation, avant de procéder à son détachement, passez-la en mode SINGLE_USER pour obtenir un accès exclusif.

 Avant d'affecter la valeur SINGLE_USER à la base de données, vérifiez que l'option AUTO_UPDATE_STATISTICS_ASYNC a la valeur OFF. Si la valeur de cette option est ON, le thread d'arrière-plan utilisé pour mettre à jour les statistiques se connecte à la base de données et vous ne pourrez pas accéder à celle-ci en mode mono-utilisateur. Pour plus d’informations, consultez [définir une base de données en mode mono-utilisateur](../databases/set-a-database-to-single-user-mode.md).

 Par exemple, l' `ALTER DATABASE` instruction suivante obtient un accès exclusif à la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de données une fois que tous les utilisateurs actuels se sont déconnectés de la base de données.  
  
```  
USE master;  
ALTER DATABASE AdventureWorks2012  
SET SINGLE_USER;  
GO  
```  
  
> [!NOTE]  
>  Pour forcer les utilisateurs actuels à se déconnecter de la base de données immédiatement ou dans un nombre de secondes spécifié, utilisez également l’option ROLLBACK : ALTER DATABASE *database_name* SET SINGLE_USER avec Rollback *rollback_option*. Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="reattaching-a-database"></a>Rattachement d'une base de données  
 Les fichiers détachés restent et peuvent être rattachés à l'aide de CREATE DATABASE (avec l'option FOR ATTACH ou FOR ATTACH_REBUILD_LOG). Vous pouvez les déplacer sur un autre serveur et les y attacher.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou l’appartenance au rôle **db_owner** de la base de données.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant détache la [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] base de données avec *option skipchecks* défini sur true.  
  
```  
EXEC sp_detach_db 'AdventureWorks2012', 'true';  
```  
  
 L'exemple suivant détache la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] et conserve les fichiers d'index de texte intégral et les métadonnées de l'index de texte intégral. Cette commande exécute UPDATE STATISTICS, ce qui correspond au comportement par défaut.  
  
```  
exec sp_detach_db @dbname='AdventureWorks2012'  
    , @keepfulltextindexfile='true';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)   
 [Détacher une base de données](../../relational-databases/databases/detach-a-database.md)  
  
