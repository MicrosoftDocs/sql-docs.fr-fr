---
description: sp_configure_peerconflictdetection (Transact-SQL)
title: sp_configure_peerconflictdetection (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_configure_peerconflictdetection_TSQL
- sp_configure_peerconflictdetection
helpviewer_keywords:
- sp_configure_peerconflictdetection
ms.assetid: 45117cb2-3247-433f-ba3d-7fa19514b1c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 58e583191ba0c18824715664162f89a7911c466a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198710"
---
# <a name="sp_configure_peerconflictdetection-transact-sql"></a>sp_configure_peerconflictdetection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Configure la détection de conflit pour une publication impliquée dans une topologie de réplication transactionnelle d'égal à égal. Pour plus d’informations, voir [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md). Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_configure_peerconflictdetection [ @publication = ] 'publication'  
    [ , [ @action = ] 'action']  
    [ , [ @originator_id = ] originator_id ]  
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @continue_onconflict = ] 'continue_onconflict']  
    [ , [ @local = ] 'local']  
    [ , [ @timeout = ] timeout ]  
  
```  
  
## <a name="arguments"></a>Arguments  
 [ @publication =] '*publication*'  
 Nom de la publication pour laquelle configurer la détection de conflit. *publication* est de **type sysname**, sans valeur par défaut.  
  
 [ @action =] '*action*'  
 Spécifie s'il faut activer ou désactiver la détection de conflit pour une publication. *action* est de type **nvarchar (5)** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**enable**|Active la détection de conflit pour une publication.|  
|**disable**|Désactive la détection de conflit pour une publication.|  
|NULL (par défaut)||  
  
 [ @originator_id =] *originator_id*  
 Spécifie un ID pour un nœud dans une topologie d'égal à égal. *originator_id* est de **type int**, avec NULL comme valeur par défaut. Cet ID est utilisé pour la détection de conflit si *action* a la valeur **activer**. Spécifiez un ID positif différent de zéro qui n'a jamais été utilisé dans la topologie. Pour obtenir la liste des ID qui ont déjà été utilisés, interrogez la table système [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) .  
  
 [ @conflict_retention =] *conflict_retention*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @continue_onconflict =] '*continue_onconflict*']  
 Détermine si l'Agent de distribution continue à traiter les modifications lorsqu'un conflit est détecté. *continue_onconflict* est de type **nvarchar (5),** avec false comme valeur par défaut.  
  
> [!CAUTION]  
>  Nous vous recommandons de conserver la valeur par défaut FALSE. Lorsque cette option a la valeur TRUE, l'Agent de distribution tente de converger les données dans la topologie en appliquant la ligne en conflit du nœud doté de l'ID d'appelant le plus élevé. Cette méthode ne garantit pas la convergence. Vous devez vous assurer que la topologie est cohérente après la détection d'un conflit. Pour plus d'informations, consultez « Gestion des conflits » dans [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md).  
  
 [ @local =] '*local*'  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @timeout =] *délai d’expiration*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_configure_peerconflictdetection est utilisé dans la réplication transactionnelle d'égal à égal. Pour utiliser la détection de conflit, tous les nœuds doivent exécuter [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou versions ultérieures, et la détection doit être activée pour tous les nœuds.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner.  
  
## <a name="see-also"></a>Voir aussi  
 [Détection de conflit dans la réplication d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)   
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
