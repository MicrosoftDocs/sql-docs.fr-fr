---
description: sp_validatelogins (Transact-SQL)
title: sp_validatelogins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_validatelogins
- sp_validatelogins_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_validatelogins
ms.assetid: 6ac52e21-e20d-469b-ad40-5aa091e06b61
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 46ed2b0577918630cc9c938047b740618eda45a3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201848"
---
# <a name="sp_validatelogins-transact-sql"></a>sp_validatelogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de signaler des informations sur les utilisateurs et les groupes Windows qui sont mappés sur des principaux [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mais qui n'existent plus dans l'environnement Windows.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_validatelogins  
```  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**SID**|**varbinary(85)**|Identificateur de sécurité de l'utilisateur ou groupe Windows.|  
|**NT Login**|**sysname**|Nom de l’utilisateur ou du groupe Windows.|  
  
## <a name="remarks"></a>Notes  
 Si le principal orphelin au niveau serveur possède un utilisateur de base de données, celui-ci doit être supprimé pour que le principal de serveur orphelin puisse être supprimé. Pour supprimer un utilisateur de base de données, utilisez [DROP USER](../../t-sql/statements/drop-user-transact-sql.md). Si le principal au niveau serveur possède des éléments sécurisables dans la base de données, la propriété des éléments sécurisables doit être transférée ou ils doivent être supprimés. Pour transférer la propriété des éléments sécurisables de base de données, utilisez [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
 Pour supprimer les mappages aux utilisateurs et groupes Windows qui n’existent plus, utilisez [Drop login](../../t-sql/statements/drop-login-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle serveur fixe **sysadmin** ou **securityadmin** .  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, les utilisateurs et les groupes Windows qui n'existent plus mais qui bénéficient toujours d'un accès à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont affichés.  
  
```  
EXEC sp_validatelogins;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [DROP LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/drop-login-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)  
  
  
