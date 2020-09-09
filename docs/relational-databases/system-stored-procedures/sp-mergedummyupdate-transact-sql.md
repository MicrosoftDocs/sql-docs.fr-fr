---
description: sp_mergedummyupdate (Transact-SQL)
title: sp_mergedummyupdate (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_mergedummyupdate_TSQL
- sp_mergedummyupdate
helpviewer_keywords:
- sp_mergedummyupdate
ms.assetid: b834f7f6-9588-4d59-a3e2-83d8e8e722e1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39558295ad31cfccd1f4df70e89580e2615fea56
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89545958"
---
# <a name="sp_mergedummyupdate-transact-sql"></a>sp_mergedummyupdate (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Effectue une mise à jour fictive de la ligne indiquée, de sorte qu'elle soit renvoyée lors de la prochaine fusion. Cette procédure stockée peut être exécutée sur la base de données de publication du serveur de publication ou sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_mergedummyupdate [ @source_object =] 'source_object', [ @rowguid =] 'rowguid'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @source_object = ] 'source_object'` Nom de l’objet source. *source_object*est de type **nvarchar (386)**, sans valeur par défaut.  
  
`[ @rowguid = ] 'rowguid'` Identificateur de ligne. *rowguid* est de type **uniqueidentifier**et n’a pas de valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_mergedummyupdate** est utilisé dans la réplication de fusion.  
  
 **sp_mergedummyupdate** est utile si vous écrivez votre propre alternative à la Outil de résolution des conflits de réplication (Wzcnflct.exe).  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle de base de données fixe **db_owner** peuvent exécuter **sp_mergedummyupdate**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
