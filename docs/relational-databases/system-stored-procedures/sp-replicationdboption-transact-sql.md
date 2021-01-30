---
description: sp_replicationdboption (Transact-SQL)
title: sp_replicationdboption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replicationdboption_TSQL
- sp_replicationdboption
helpviewer_keywords:
- sp_replicationdboption
ms.assetid: d021864e-3f21-4d1a-89df-6c1086f753bf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e56271516d9ef2df60e993cd73963d074b503d42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211879"
---
# <a name="sp_replicationdboption-transact-sql"></a>sp_replicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Définit une option de base de données de réplication pour la base de données spécifiée. Cette procédure stockée est exécutée sur n'importe quelle base de données de l'abonné au niveau du serveur de publication ou de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replicationdboption [ @dbname= ] 'db_name'   
        , [ @optname= ] 'optname'   
        , [ @value= ] 'value'   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
    [ , [ @from_scripting = ] from_scripting ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbname = ] 'dbname'` Base de données pour laquelle l’option de base de données de réplication est définie. *db_name* est de **type sysname**, sans valeur par défaut.  
  
`[ @optname = ] 'optname'` Option de base de données de réplication à activer ou à désactiver. *nom_d* 'est est de **type sysname** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**publication de fusion**|La base de données peut être utilisée pour les publications de fusion.|  
|**édition**|La base de données peut être utilisée pour les autres types de publications.|  
|**Inscrivez**|La base de données est une base de données d'abonnement.|  
|**sync with backup**|La base de données est activée pour la sauvegarde coordonnée. Pour plus d’informations, consultez [activer des sauvegardes coordonnées pour la réplication transactionnelle &#40;la programmation Transact-SQL de la réplication&#41;](../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md).|  
  
`[ @value = ] 'value'` Indique s’il faut activer ou désactiver l’option de base de données de réplication donnée. la valeur est de **type sysname** et peut avoir la *valeur* **true** ou **false**. Lorsque cette valeur est **false** et que *nom_d* 'est la **publication de fusion**, les abonnements à la base de données publiée de fusion sont également supprimés.  
  
`[ @ignore_distributor = ] ignore_distributor` Indique si cette procédure stockée est exécutée sans se connecter au serveur de distribution. *ignore_distributor* est de **bit**, avec **0** comme valeur par défaut, ce qui signifie que le serveur de distribution doit être connecté et mis à jour avec le nouvel état de la base de données de publication. La valeur **1** doit être spécifiée uniquement si le serveur de distribution est inaccessible et que **sp_replicationdboption** est utilisé pour désactiver la publication.  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replicationdboption** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
 Cette procédure crée ou supprime des tables système de réplication spécifiques, des comptes de sécurité, etc., en fonction des options choisies. Définit le **is_published** correspondant (réplication transacational ou d’instantané), **is_merge_published** (réplication de fusion) ou **is_distributor** bits dans la table système **Master. databases** et crée les tables système nécessaires.  
  
 Pour désactiver la publication, la base de données de publication doit être en ligne. Si un instantané existe pour la base de données de publication, elle doit être supprimée pour pouvoir désactiver la publication. Un instantané de base de données est une copie en lecture seule hors ligne d'une base de données et n'est pas lié à un instantané de réplication. Pour plus d’informations, consultez [Instantanés de base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_replicationdboption**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Supprimer une publication](../../relational-databases/replication/publish/delete-a-publication.md)   
 [Désactiver la publication et la distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
