---
description: sp_helpntgroup (Transact-SQL)
title: sp_helpntgroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpntgroup
- sp_helpntgroup_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpntgroup
ms.assetid: 02b4f7c1-480a-436c-8bae-7a2488be45d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc378f5c07c4aa24c37c0212fd9f84fb6ef39f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210881"
---
# <a name="sp_helpntgroup-transact-sql"></a>sp_helpntgroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les groupes Windows ayant un compte dans la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpntgroup [ [ @ntname= ] 'name' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @ntname = ] 'name'` Nom du groupe Windows. *Name* est de **type sysname**, avec NULL comme valeur par défaut. le *nom* doit être un groupe Windows valide ayant accès à la base de données actuelle. Si le *nom* n’est pas spécifié, tous les groupes Windows ayant accès à la base de données active sont inclus dans la sortie.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**NTGroupName**|**sysname**|Nom du groupe Windows.|  
|**NTGroupId**|**smallint**|Identificateur du groupe.|  
|**SID**|**varbinary(85)**|Identificateur de sécurité (SID) de **NTGroupName**.|  
|**HasDbAccess**|**int**|1 = le groupe Windows a une autorisation d'accès à la base de données.|  
  
## <a name="remarks"></a>Notes  
 Pour afficher la liste des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rôles dans la base de données active, utilisez **sp_helprole**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
 L'exemple de code suivant affiche la liste des groupes Windows ayant accès à la base de données active.  
  
```  
EXEC sp_helpntgroup;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_helprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_revokedbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
