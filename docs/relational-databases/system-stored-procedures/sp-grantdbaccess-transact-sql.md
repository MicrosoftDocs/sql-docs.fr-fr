---
description: sp_grantdbaccess (Transact-SQL)
title: sp_grantdbaccess (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grantdbaccess
- sp_grantdbaccess_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grantdbaccess
ms.assetid: 3eb09513-03f1-42f8-9917-3a1f3a579bec
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 5a08a424a09d8f0178137b5a83bb64b44f5b0b04
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208985"
---
# <a name="sp_grantdbaccess-transact-sql"></a>sp_grantdbaccess (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute un utilisateur à la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [Create User](../../t-sql/statements/create-user-transact-sql.md) .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_grantdbaccess [ @loginame = ] 'login'  
    [ , [ @name_in_db = ] 'name_in_db' [ OUTPUT ] ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @loginame = ] 'login_ '` Nom du groupe Windows, de la connexion Windows ou de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion à mapper au nouvel utilisateur de la base de données. Les noms des groupes Windows et des connexions Windows doivent être qualifiés par un nom de domaine Windows sous la forme *domaine* \\ *login*, par exemple **LONDON\Joeb**. La connexion ne peut pas être déjà associée à un utilisateur de la base de données. *login* est de **type sysname** et n’a pas de valeur par défaut.  
  
``[ @name_in_db = ] 'name_in_db' [ OUTPUT]`` Nom du nouvel utilisateur de base de données. *name_in_db* est une variable de sortie avec un type de données **sysname** et une valeur par défaut null. S’il n’est pas spécifié, la *connexion* est utilisée. S’il est spécifié comme variable de sortie avec une valeur NULL, **\@ name_in_db** est défini sur *login*. *name_in_db* ne doit pas déjà exister dans la base de données active.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_grantdbaccess** appelle Create User, qui prend en charge des options supplémentaires. Pour plus d’informations sur la création d’utilisateurs de base de données, consultez [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md). Pour supprimer un utilisateur de base de données d’une base de données, utilisez [DROP USER](../../t-sql/statements/drop-user-transact-sql.md).  
  
 **sp_grantdbaccess** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** ou le rôle de base de données fixe **db_accessadmin** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise `CREATE USER` pour ajouter un utilisateur de base de données pour la connexion Windows `Edmonds\LolanSo` à la base de données active. Le nouvel utilisateur se nomme `Lolan`. Il s'agit de la méthode recommandée pour la création d'un utilisateur de base de données.  
  
```sql
CREATE USER Lolan FOR LOGIN [Edmonds\LolanSo];  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
