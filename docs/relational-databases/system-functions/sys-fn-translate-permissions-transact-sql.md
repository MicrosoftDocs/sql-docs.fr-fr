---
description: sys.fn_translate_permissions (Transact-SQL)
title: sys.fn_translate_permissions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_translate_permissions
- sys.fn_translate_permissions_TSQL
- fn_translate_permissions
- fn_translate_permissions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions bitmask [SQL Server]
- sys.fn_translate_permissions function
- fn_translate_permissions function
ms.assetid: ac97121f-2bd0-4f71-8e45-42c8584edbc5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 98997af48b4e1f51b18c5441d99f6abaec57b293
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187407"
---
# <a name="sysfn_translate_permissions-transact-sql"></a>sys.fn_translate_permissions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Convertit le masque de bits des autorisations retourné par la trace SQL en une table de noms d'autorisations.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sys.fn_translate_permissions ( level , perms )  
```  
  
## <a name="arguments"></a>Arguments  
 *level*  
 Type d'élément sécurisable auquel l'autorisation est appliquée. le *niveau* est **nvarchar (60)**.  
  
 *perms*  
 Masque de bits retourné dans la colonne d'autorisations. *perms* est **de type varbinary (16)**.  
  
## <a name="returns"></a>Retours  
 **table**  
  
## <a name="remarks"></a>Notes  
 La valeur retournée dans la colonne **permissions** d’une trace SQL est une représentation d’entier d’un masque de réutilisation utilisé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour calculer les autorisations effectives. Chacun des 25 types d'éléments sécurisables possède son propre jeu d'autorisations avec des valeurs numériques correspondantes. **sys.fn_translate_permissions** convertit ce masque de masque en une table de noms d’autorisations.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="example"></a>Exemple  
 La requête suivante utilise `sys.fn_builtin_permissions` pour afficher les autorisations qui s’appliquent aux certificats, puis utilise `sys.fn_translate_permissions` pour retourner les résultats du masque de sous-masque des autorisations.  
  
```  
SELECT * FROM sys.fn_builtin_permissions('CERTIFICATE');  
SELECT '0001' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0001);  
SELECT '0010' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0010);  
SELECT '0011' AS Input, * FROM sys.fn_translate_permissions('CERTIFICATE', 0011);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
