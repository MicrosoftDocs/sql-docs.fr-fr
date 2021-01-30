---
description: sp_help_peerconflictdetection (Transact-SQL)
title: sp_help_peerconflictdetection (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_help_peerconflictdetection
- sp_help_peerconflictdetection_TSQL
helpviewer_keywords:
- sp_help_peerconflictdetection
ms.assetid: 59e04107-5eaa-44a1-beb6-ac4f2dbbcb28
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 90cd963b22cffd11c400f18a91f2badefff2e799
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199607"
---
# <a name="sp_help_peerconflictdetection-transact-sql"></a>sp_help_peerconflictdetection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations relatives aux paramètres de la détection de conflit pour une publication impliquée dans une topologie de réplication transactionnelle d'égal à égal.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_peerconflictdetection [ @publication = ] 'publication'  
    [ ,[ @timeout = ] timeout ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @publication =] '*publication*'  
 Nom de la publication pour laquelle renvoyer les informations. *publication* est de **type sysname**, sans valeur par défaut.  
  
 [ @timeout =] *délai d’expiration*  
 Spécifie la durée, en secondes, du délai d'expiration de la procédure durant l'attente d'une réponse de chaque nœud dans la topologie. Si la topologie contient un Abonné en lecture seule, spécifier une valeur de délai d'attente n'est pas valide. Les Abonnés en lecture seule ne répondent jamais à un appel de cette procédure. *timeout* est de **type int**, avec 60 comme valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
 sp_help_peerconflictdetection retourne trois jeux de résultats. Ces résultats sont décrits dans les rubriques suivantes :  
  
-   [MSpeer_conflictdetectionconfigrequest](../../relational-databases/system-tables/mspeer-conflictdetectionconfigrequest-transact-sql.md)  
  
-   [MSpeer_conflictdetectionconfigresponse](../../relational-databases/system-tables/mspeer-conflictdetectionconfigresponse-transact-sql.md)  
  
-   [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md)  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_help_peerconflictdetection est utilisé dans la réplication transactionnelle d'égal à égal.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner.  
  
## <a name="see-also"></a>Voir aussi  
 [Détection de conflit dans la réplication d’égal à égal](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)   
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
