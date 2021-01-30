---
description: sp_requestpeerresponse (Transact-SQL)
title: sp_requestpeerresponse (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_requestpeerresponse_TSQL
- sp_requestpeerresponse
helpviewer_keywords:
- sp_requestpeerresponse
ms.assetid: cbe13c22-4d7d-4a36-b194-7a13ce68ef27
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ce25686ab2b7947438d207a3acb5fc5b7f6549e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199969"
---
# <a name="sp_requestpeerresponse-transact-sql"></a>sp_requestpeerresponse (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Lorsque cette procédure est exécutée depuis un nœud dans une topologie d'égal à égal, elle demande une réponse de tous les nœuds de la topologie. En exécutant cette procédure et en vérifiant les réponses correspondantes, vous garantissez la remise de toutes les commandes précédentes aux nœuds qui répondent. Cette procédure stockée est exécutée sur le nœud demandeur sur n'importe quelle base de données.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_requestpeerresponse [ @publication = ] 'publication'  
    [ , [ @description = ] 'description'  
    [ , [ @request_id = ] request_id OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication dans une topologie d’égal à égal pour laquelle l’État est en cours de vérification. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @description = ] 'description'` Informations définies par l’utilisateur qui peuvent être utilisées pour identifier des demandes d’État individuelles. *Description* est de type **nvarchar (4000)**, avec NULL comme valeur par défaut.  
  
`[ @request_id = ] request_id` Retourne l’ID de la nouvelle demande. *request_id* est de **type int** et est un paramètre de sortie. Cette valeur peut être utilisée lors de l’exécution de [sp_helppeerresponses &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md) pour afficher toutes les réponses à une demande d’État.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_requestpeerresponse** est utilisé dans la réplication transactionnelle d’égal à égal.  
  
 **sp_requestpeerresponse** est utilisé pour s’assurer que toutes les commandes ont été reçues par tous les autres nœuds avant la restauration d’une base de données publiée dans une topologie d’égal à égal. Cette procédure est également utilisée lors de la réplication de modifications DDL (Data Definition Language) effectuées lorsqu'un nœud était hors ligne, pour déterminer le moment où les modifications arrivent sur les autres nœuds.  
  
 **sp_requestpeerresponse** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_requestpeerresponse**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_deletepeerrequesthistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-deletepeerrequesthistory-transact-sql.md)   
 [sp_helppeerrequests &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppeerrequests-transact-sql.md)  
  
  
