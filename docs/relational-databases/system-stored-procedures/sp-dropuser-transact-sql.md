---
description: sp_dropuser (Transact-SQL)
title: sp_dropuser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_dropuser
- sp_dropuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropuser
ms.assetid: e28f18f9-7ecf-4568-89f4-fe5c520df386
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f44b851dc2d60899f27c8419dfdb557951043683
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89536154"
---
# <a name="sp_dropuser-transact-sql"></a>sp_dropuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un utilisateur de base de données de la base de données active. **sp_dropuser** assure la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [DROP USER](../../t-sql/statements/drop-user-transact-sql.md) .  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_dropuser [ @name_in_db = ] 'user'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name_in_db = ] 'user'` Nom de l’utilisateur à supprimer. *User* est de **type sysname**, sans valeur par défaut. l' *utilisateur* doit exister dans la base de données active. Lorsque vous spécifiez une connexion Windows, utilisez le nom sous lequel la base de données connaît cette connexion.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_dropuser** exécute **sp_revokedbaccess** pour supprimer l’utilisateur de la base de données actuelle.  
  
 Utilisez **sp_helpuser** pour afficher la liste des noms d’utilisateurs qui peuvent être supprimés de la base de données actuelle.  
  
 Lorsqu'un utilisateur de base de données est supprimé, les alias affectés à cet utilisateur sont également supprimés. Si l'utilisateur possède un schéma vide dont le nom est identique à celui de l'utilisateur, le schéma est supprimé. Si l'utilisateur possède d'autres éléments sécurisables dans la base de données, il n'est pas supprimé. La propriété des objets doit être auparavant transmise à un autre principal. Pour plus d’informations, consultez [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md). La suppression d'un utilisateur de base de données entraîne également la suppression automatique des autorisations associées à cet utilisateur ainsi que tous les rôles de base de données dont l'utilisateur est membre.  
  
 **sp_dropuser** ne peut pas être utilisé pour supprimer le propriétaire de la base de données (**dbo**) **INFORMATION_SCHEMA** les utilisateurs ou l’utilisateur **invité** des bases de données **Master** ou **tempdb** . Dans les bases de données non-système, `EXEC sp_dropuser 'guest'` révoque l’autorisation Connect de l’utilisateur **invité**. Cependant, l'utilisateur lui-même n'est pas supprimé.  
  
 **sp_dropuser** ne peut pas être exécutée dans une transaction définie par l’utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY USER sur la base de données.  
  
## <a name="examples"></a>Exemples  
 Le code exemple suivant supprime l'utilisateur `Albert` de la base de données active.  
  
```  
EXEC sp_dropuser 'Albert';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [sp_revokedbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
