---
description: sp_adjustpublisheridentityrange (Transact-SQL)
title: sp_adjustpublisheridentityrange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adjustpublisheridentityrange_TSQL
- sp_adjustpublisheridentityrange
helpviewer_keywords:
- sp_adjustpublisheridentityrange
ms.assetid: 64f111fd-fb7d-4459-93f7-65f0f8dd7efe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5855ffeceff68c10e0eac2089a95d2b0184cd327
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198347"
---
# <a name="sp_adjustpublisheridentityrange-transact-sql"></a>sp_adjustpublisheridentityrange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajuste la plage d'identités sur une publication et réaffecte de nouvelles plages en fonction de la valeur de seuil définie pour la publication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adjustpublisheridentityrange [ [ @publication = ] 'publication' ]  
    [ , [ @table_name = ] 'table_name' ]  
    [ , [ @table_owner= ] 'table_owner' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication dans laquelle de nouvelles plages d’identité sont réallouées. *publication* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_name = ] 'table_name'` Nom de la table dans laquelle de nouvelles plages d’identité sont réallouées. *table_name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @table_owner = ] 'table_owner'` Propriétaire de la table sur le serveur de publication. *TABLE_OWNER* est de **type sysname**, avec NULL comme valeur par défaut. Si *TABLE_OWNER* n’est pas spécifié, le nom de l’utilisateur actuel est utilisé.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adjustpublisheridentityrange** est utilisé dans tous les types de réplications.  
  
 Lorsque le paramètre d'affectation automatique de plage d'identités est activé pour une publication, l'Agent de distribution ou de fusion est responsable de l'ajustement automatique de la plage d'identités en fonction de la valeur de seuil de la publication. Toutefois, si, pour une raison quelconque, le Agent de distribution ou le Agent de fusion n’a pas été exécuté pendant un certain temps, et que la ressource de plage d’identité a été consommée de manière intensive jusqu’au point de seuil, les utilisateurs peuvent appeler **sp_adjustpublisheridentityrange** pour allouer une nouvelle plage de valeurs à un serveur de publication.  
  
 Lors de l’exécution de **sp_adjustpublisheridentityrange**, la *publication* ou l' *table_name* doit être spécifié. Si les deux sont spécifiés ou aucun des deux, un message d'erreur est renvoyé.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_adjustpublisheridentityrange**.  
  
## <a name="see-also"></a>Voir aussi  
 [Répliquer les colonnes d’identité](../../relational-databases/replication/publish/replicate-identity-columns.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
