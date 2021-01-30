---
description: sp_gettopologyinfo (Transact-SQL)
title: sp_gettopologyinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_gettopologyinfo_TSQL
- sp_gettopologyinfo
helpviewer_keywords:
- sp_gettopologyinfo
ms.assetid: 8bbe8a06-a4aa-4219-8402-12db6a4682c6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e6762b60612523b8150b791c3fcef19a82079968
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197661"
---
# <a name="sp_gettopologyinfo-transact-sql"></a>sp_gettopologyinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations sur une topologie de réplication transactionnelle d'égal à égal. Exécutez [sp_requestpeertopologyinfo](../../relational-databases/system-stored-procedures/sp-requestpeertopologyinfo-transact-sql.md) pour collecter des informations avant d’exécuter cette procédure.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_gettopologyinfo [ @request_id = ] request_id  
```  
  
## <a name="arguments"></a>Arguments  
 [ @request_id =] *request_id*  
 ID d'une demande de statut de topologie. *request_id* est de **type int**, avec NULL comme valeur par défaut. Pour obtenir un ID, utilisez le @request_id paramètre OUTPUT de [sp_requestpeertopologyinfo](../../relational-databases/system-stored-procedures/sp-requestpeertopologyinfo-transact-sql.md) ou interrogez la table système [MSpeer_topologyrequest](../../relational-databases/system-tables/mspeer-topologyrequest-transact-sql.md) .  
  
## <a name="result-sets"></a>Jeux de résultats  
 sp_gettopologyinfo retourne un jeu de résultats contenant une seule colonne XML. Les données de la colonne XML sont les mêmes que celles de la table système [MSpeer_topologyresponse](../../relational-databases/system-tables/mspeer-topologyresponse-transact-sql.md) .  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 sp_gettopologyinfo est utilisé dans la réplication transactionnelle d'égal à égal. Exécutez [sp_requestpeertopologyinfo](../../relational-databases/system-stored-procedures/sp-requestpeertopologyinfo-transact-sql.md) avant d’exécuter sp_gettopologyinfo. Ces procédures sont utilisées par l'Assistant Configurer la topologie d'égal à égal, mais ils peuvent également être utilisés directement si vous avez besoin d'informations de topologie dans un format XML. Si vous préférez des résultats tabulaires, interrogez la table système [MSpeer_topologyresponse](../../relational-databases/system-tables/mspeer-topologyresponse-transact-sql.md) .  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle serveur fixe sysadmin ou au rôle de base de données fixe db_owner.  
  
## <a name="see-also"></a>Voir aussi  
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
