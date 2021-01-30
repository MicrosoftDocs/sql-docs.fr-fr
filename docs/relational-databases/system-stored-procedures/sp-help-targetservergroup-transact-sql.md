---
description: sp_help_targetservergroup (Transact-SQL)
title: sp_help_targetservergroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_targetservergroup_TSQL
- sp_help_targetservergroup
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_targetservergroup
ms.assetid: ec3a4a68-b591-431c-9518-053ede522d0c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 200b6b0a8d6a6700f7aa98db0cde45cfc278dc53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186702"
---
# <a name="sp_help_targetservergroup-transact-sql"></a>sp_help_targetservergroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie tous les serveurs cibles du groupe spécifié. Si aucun groupe n'est spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie des informations sur tous les groupes de serveurs cibles.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_targetservergroup  
    [ [ @name = ] 'name' ]  
```  
  
## <a name="argument"></a>Argument  
`[ @name = ] 'name'` Nom du groupe de serveurs cibles pour lequel des informations doivent être retournées. *Name* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**servergroup_id**|**int**|Numéro d'identification du groupe de serveurs|  
|**name**|**sysname**|Nom du groupe de serveurs|  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution de cette procédure sont octroyées par défaut au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-information-for-all-target-server-groups"></a>R. Affichage d'informations pour tous les groupes de serveurs cibles  
 Cet exemple affiche une liste d'informations pour tous les groupes de serveurs cibles.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetservergroup ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-target-server-group"></a>B. Affichage d'informations pour un groupe de serveurs cibles spécifique  
 Cet exemple affiche une liste d'informations pour le groupe de serveurs cibles `Servers Maintaining Customer Information`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetservergroup   
    N'Servers Maintaining Customer Information' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_targetservergroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-targetservergroup-transact-sql.md)   
 [sp_delete_targetservergroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-targetservergroup-transact-sql.md)   
 [sp_update_targetservergroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-targetservergroup-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
