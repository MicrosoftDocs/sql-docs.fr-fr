---
description: sp_lock (Transact-SQL)
title: sp_lock (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_lock_TSQL
- sp_lock
dev_langs:
- TSQL
helpviewer_keywords:
- sp_lock
ms.assetid: 9eaa0ec2-2ad9-457c-ae48-8da92a03dcb0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 09e753e8b821c3ad8029a7ed4d81bbe183160279
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185380"
---
# <a name="sp_lock-transact-sql"></a>sp_lock (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Affiche des informations sur les verrous.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Pour obtenir des informations sur les verrous dans le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] , utilisez la vue de gestion dynamique [sys.dm_tran_locks](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_lock [ [ @spid1 = ] 'session ID1' ] [ , [@spid2 = ] 'session ID2' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @spid1 = ] 'session ID1'`[!INCLUDE[ssDE](../../includes/ssde-md.md)]Numéro d’ID de session de **sys.dm_exec_sessions** pour lequel l’utilisateur souhaite verrouiller des informations. la *session ID1* est de **type int** avec NULL comme valeur par défaut. Exécutez **sp_who** pour obtenir des informations sur les processus de la session. Si la *session ID1* n’est pas spécifiée, des informations sur tous les verrous s’affichent.  
  
`[ @spid2 = ] 'session ID2'` Est un autre [!INCLUDE[ssDE](../../includes/ssde-md.md)] numéro d’ID de session de **sys.dm_exec_sessions** qui peut avoir un verrou en même temps que la *session ID1* et sur lequel l’utilisateur souhaite également obtenir des informations. la *session ID2* est de **type int** avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le **sp_lock** jeu de résultats contient une ligne pour chaque verrou détenu par les sessions spécifiées dans les paramètres **\@ spid1** et **\@ SPID2** . Si ni **\@ spid1** ni **\@ SPID2** n’est spécifié, le jeu de résultats signale les verrous pour toutes les sessions actuellement actives dans l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**spid**|**smallint**|[!INCLUDE[ssDE](../../includes/ssde-md.md)]Numéro d’ID de session du processus demandant le verrou.|  
|**dbid**|**smallint**|Numéro d'identification de la base de données qui contient le verrou. Vous pouvez utiliser la fonction DB_NAME() pour identifier la base de données.|  
|**ID**|**int**|Numéro d'identification de l'objet sur lequel le verrou est maintenu. Vous pouvez utiliser la fonction OBJECT_NAME() dans la base de données associée pour identifier l'objet. La valeur 99 indique l'existence d'un verrou sur l'une des pages système utilisées pour enregistrer l'allocation des pages dans une base de données.|  
|**IndId**|**smallint**|Numéro d'identification de l'index sur lequel le verrou est maintenu.|  
|**Type**|**nchar (4)**|Type du verrou :<br /><br /> RID = verrou sur une seule ligne d'une table, identifiée par un identificateur de ligne (RID).<br /><br /> KEY = verrou dans un index qui protège une plage de clés dans les transactions sérialisables.<br /><br /> PAG = verrou sur une page de données ou d'index.<br /><br /> EXT = verrou sur une extension.<br /><br /> TAB = verrou sur une table complète comprenant l'ensemble des index et des données.<br /><br /> DB = verrou sur une base de données.<br /><br /> FIL = verrou sur un fichier de base de données.<br /><br /> APP = verrou sur une ressource spécifiée par l'application.<br /><br /> MD = verrous sur des métadonnées ou informations de catalogue.<br /><br /> HBT = verrou sur un segment de mémoire ou un arbre B (B-Tree) (HoBT). Ces informations sont incomplètes dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> AU = verrou sur une unité d'allocation. Ces informations sont incomplètes dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Ressource**|**nchar(32)**|Valeur identifiant la ressource verrouillée. Le format de la valeur dépend du type de ressource identifié dans la colonne **type** :<br /><br /> **Type** Valeur : valeur de **ressource**<br /><br /> RID : identificateur de la forme fileid:pagenumber:rid, où fileid désigne le fichier contenant la page, pagenumber la page comportant la ligne et rid la ligne spécifique de la page. fileid correspond à la colonne **file_id** de l’affichage catalogue **sys.database_files** .<br /><br /> CLÉ : nombre hexadécimal utilisé en interne par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] .<br /><br /> PAG : nombre au format fileid:pagenumber, où fileid identifie le fichier contenant la page et pagenumber la page elle-même.<br /><br /> EXT : nombre identifiant la première page de l'étendue. Ce nombre est de la forme fileid:pagenumber.<br /><br /> ONGLET : aucune information n’est fournie car la table est déjà identifiée dans la colonne **objID** .<br /><br /> DB : aucune information n’est fournie car la base de données est déjà identifiée dans la colonne **dbid** .<br /><br /> FIL : identificateur du fichier, qui correspond à la **file_id** colonne dans l’affichage catalogue **sys.database_files** .<br /><br /> APP : identificateur unique de la ressource d'application verrouillée. Au format DbPrincipleId : \<first two to 16 characters of the resource string> \<hashed value> .<br /><br /> MD : varie d'un type de ressource à l'autre. Pour plus d’informations, consultez la description de la colonne **resource_description** dans [sys.dm_tran_locks &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md).<br /><br /> HBT : aucune information n'est fournie. Utilisez plutôt la vue de gestion dynamique **sys.dm_tran_locks** .<br /><br /> AU : aucune information n'est fournie. Utilisez plutôt la vue de gestion dynamique **sys.dm_tran_locks** .|  
|**Mode**|**nvarchar(8)**|Le mode de verrou est demandé. Valeurs possibles :<br /><br /> NULL = aucun accès n'est accordé à la ressource. Sert d'espace réservé.<br /><br /> Sch-S = stabilité du schéma. Garantit que l'élément d'un schéma, tel qu'une table ou un index, n'est pas supprimé alors qu'une session contient un verrou de stabilité du schéma sur l'élément du schéma.<br /><br /> Sch-M = modification du schéma. Doit être détenu par toute session destinée à modifier le schéma de la ressource spécifiée. Garantit qu'aucune autre session ne fait référence à l'objet indiqué.<br /><br /> S = partage. La session détenant le verrou peut disposer d'un accès partagé à la ressource.<br /><br /> U = mise à jour. Indique qu'un verrouillage de mise à jour a été posé sur des ressources qui peuvent finalement être mises à jour. Utilisé pour éviter les formes courantes de blocages qui se produisent lorsque plusieurs sessions verrouillent des ressources en vue d'une mise à jour éventuelle.<br /><br /> X = exclusif. La session détenant le verrou peut disposer d'un accès exclusif à la ressource.<br /><br /> IS = partage intentionnel. Indique l'intention de placer des verrous S sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> IU = mise à jour intentionnelle. Indique l'intention de placer des verrous U sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> IX = exclusion intentionnelle. Indique l'intention de placer des verrous X sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> SIU = mise à jour intentionnelle partagée. Signale des accès partagés à une ressource dans le but de poser des verrous de mise à jour sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> SIX = partage intentionnel exclusif. Signale des accès partagés à une ressource dans le but de poser des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> UIX = mise à jour exclusive intentionnelle. Signale un verrou de mise à jour sur une ressource dans le but de poser des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> BU = mise à jour en bloc. Utilisé par les opérations en bloc.<br /><br /> RangeS_S = verrou de groupes de clés partagés et de ressources partagées. Indique une analyse de plage sérialisable.<br /><br /> RangeS_U = verrou de groupes de clés partagés et de ressources de mise à jour. Indique une analyse de mise à jour sérialisable.<br /><br /> RangeI_N = insérer le verrou de groupes de clés et de ressources NULL. Utilisé pour tester les étendues avant l'insertion d'une nouvelle clé dans un index.<br /><br /> RangeI_S = verrou de conversion de groupes de clés. Créé par une superposition des verrous RangeI_N et S.<br /><br /> RangeI_U = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et U.<br /><br /> RangeI_X = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et X.<br /><br /> RangeX_S = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et RangeS_S.<br /><br /> RangeX_U = verrou de conversion de groupes de clés créé par une superposition des verrous RangeI_N et RangeS_U.<br /><br /> RangeX_X = verrou de groupes de clés exclusifs et de ressources exclusives. Verrou de conversion utilisé lors de la mise à jour d'une clé dans une étendue.|  
|**État**|**nvarchar(5**|État de la demande de verrouillage :<br /><br /> CNVRT : le verrou est converti depuis un autre mode, mais la conversion est bloquée par un autre processus qui maintient un verrou dont le mode est en conflit.<br /><br /> GRANT : un verrou a été obtenu.<br /><br /> WAIT : le verrou est bloqué par un autre processus qui maintient un verrou dont le mode est en conflit.|  
  
## <a name="remarks"></a>Notes  
 Les utilisateurs peuvent contrôler le verrouillage des opérations de lecture en utilisant :  
  
-   SET TRANSACTION ISOLATION LEVEL pour spécifier le niveau de verrouillage d'une session. Pour plus d’instructions sur la syntaxe et les restrictions, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).  
  
-   des indicateurs de table de verrouillage pour spécifier le niveau de verrouillage d'une référence spécifique d'une table dans une clause FROM. Pour connaître la syntaxe et les restrictions, consultez [indicateurs de Table &#40;&#41;Transact-SQL ](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Toutes les transactions distribuées qui ne sont pas associées à une session sont des transactions orphelines. [!INCLUDE[ssDE](../../includes/ssde-md.md)]Affecte à toutes les transactions distribuées orphelines la valeur SPID-2, ce qui permet à un utilisateur d’identifier plus facilement les transactions distribuées bloquantes. Pour plus d’informations, consultez [Utiliser les transactions marquées pour récupérer des bases de données associées uniformément &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation VIEW SERVER STATE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-locks"></a>R. Affichage de tous les verrous  
 L’exemple suivant affiche des informations sur tous les verrous actuellement détenus dans une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .  
  
```sql  
USE master;  
GO  
EXEC sp_lock;  
GO  
```  
  
### <a name="b-listing-a-lock-from-a-single-server-process"></a>B. Affichage des verrous d'un processus de serveur unique  
 L'exemple suivant affiche des informations, notamment à propos des verrous, sur l'ID de processus `53`.  
  
```sql  
USE master;  
GO  
EXEC sp_lock 53;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
 [DB_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/db-name-transact-sql.md)   
 [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md)   
 [OBJECT_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/object-name-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sys.dm_os_tasks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md)   
 [sys.dm_os_threads &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-threads-transact-sql.md)  
  
  
