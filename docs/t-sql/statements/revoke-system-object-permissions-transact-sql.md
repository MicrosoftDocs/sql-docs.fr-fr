---
description: REVOKE – révocation d'autorisations d'objet système (Transact-SQL)
title: REVOKE - Révoquer des autorisations sur un objet système (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, system objects
- permissions [SQL Server], system objects
ms.assetid: 84983238-dd7d-45bd-99bb-52c9d8e96a87
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c3c0df1a646caa6562c640b119abe3817d15c094
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185924"
---
# <a name="revoke-system-object-permissions-transact-sql"></a>REVOKE – révocation d'autorisations d'objet système (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de révoquer des autorisations sur des objets système tels que des procédures stockées, des procédures stockées étendues, des fonctions et des vues, pour un principal.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
REVOKE { SELECT | EXECUTE } ON [sys.]system_object FROM principal   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 [**sys.**] .  
 Le qualificateur **sys** est obligatoire uniquement quand vous faites référence à des vues de catalogue ou à des vues de gestion dynamique.  
  
 *system_object*  
 Spécifie l'objet sur lequel l'autorisation doit être révoquée.  
  
 *principal*  
 Spécifie le principal pour lequel l'autorisation est révoquée.  
  
## <a name="remarks"></a>Notes  
 Cette instruction permet de révoquer des autorisations sur des procédures stockées, procédures stockées étendues, fonctions table, fonctions scalaires, vues, affichages catalogue, vues de compatibilité, vues INFORMATION_SCHEMA, vues de gestion dynamique et tables système installées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Chacun de ces objets système existe sous la forme d’un enregistrement unique dans la base de données des ressources (**mssqlsystemresource**). La base de données des ressources est en lecture seule. Un lien à l’objet est exposé sous la forme d’un enregistrement dans le schéma **sys** de chaque base de données.  
  
 La résolution de noms par défaut permet de résoudre les noms de procédures non qualifiés dans la base de données des ressources. Par conséquent, le qualificateur **sys.** est obligatoire uniquement quand vous spécifiez des vues de catalogue ou des vues de gestion dynamique.  
  
> [!CAUTION]  
>  La révocation d'autorisations sur des objets système entraîne l'échec des applications qui en dépendent. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utilise les affichages catalogue et peut ne pas fonctionner comme prévu si vous modifiez les autorisations par défaut sur les affichages catalogue.  
  
 La révocation d'autorisations sur des déclencheurs et sur des colonnes d'objets système n'est pas prise en charge.  
  
 Les autorisations sur les objets système sont conservées lors d'une mise à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Les objets système sont consultables dans l’affichage catalogue [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) .  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, l'autorisation `EXECUTE` sur `sp_addlinkedserver` est révoquée pour `public`.  
  
```sql  
REVOKE EXECUTE ON sys.sp_addlinkedserver FROM public;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.system_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [GRANT - Octroyer des autorisations sur un objet système &#40;Transact-SQL&#41;](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)   
 [DENY - Refuser des autorisations sur un objet système &#40;Transact-SQL&#41;](../../t-sql/statements/deny-system-object-permissions-transact-sql.md)  
  
  
