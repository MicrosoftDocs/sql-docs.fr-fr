---
description: sp_changeobjectowner (Transact-SQL)
title: sp_changeobjectowner (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changeobjectowner_TSQL
- sp_changeobjectowner
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changeobjectowner
ms.assetid: 45b3dc1c-1cde-45b7-a248-5195c12973e9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 482fcbdb2d7c1b79eb0a8b5ed88d1295785b994b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159829"
---
# <a name="sp_changeobjectowner-transact-sql"></a>sp_changeobjectowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie le propriétaire d'un objet dans la base de données active.  
  
> [!IMPORTANT]
>  Cette procédure stockée fonctionne uniquement avec les objets disponibles dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez à la place [ALTER Schema](../../t-sql/statements/alter-schema-transact-sql.md) ou [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) . **sp_changeobjectowner** modifie à la fois le schéma et le propriétaire. Pour maintenir la compatibilité avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette procédure stockée modifie les propriétaires d'objet uniquement si les schémas, dont les noms sont identiques aux noms d'utilisateur de base de données, appartiennent à la fois au propriétaire actuel et au nouveau propriétaire.  
> 
> [!IMPORTANT]
>  Une nouvelle autorisation a été ajoutée à cette procédure stockée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changeobjectowner [ @objname = ] 'object' , [ @newowner = ] 'owner'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @objname = ] 'object'` Nom d’une table, d’une vue, d’une fonction définie par l’utilisateur ou d’une procédure stockée existante dans la base de données active. l' *objet* est de type **nvarchar (776)**, sans valeur par défaut. l' *objet* peut être qualifié avec le propriétaire de l’objet existant, sous la forme _existing_owner_**.** _objet_ si le schéma et son propriétaire ont le même nom.  
  
`[ @newowner = ] 'owner_ '` Nom du compte de sécurité qui sera le nouveau propriétaire de l’objet. *owner* est de **type sysname**, sans valeur par défaut. le *propriétaire* doit être un utilisateur de base de données, un rôle de serveur, une [!INCLUDE[msCoName](../../includes/msconame-md.md)] connexion Windows ou un groupe Windows valide ayant accès à la base de données actuelle. Un utilisateur de base de données est créé si le nouveau propriétaire est un utilisateur Windows ou un groupe Windows pour lequel il n'existe pas de principal de base de données correspondant.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changeobjectowner** supprime toutes les autorisations existantes de l’objet. Vous devrez réappliquer toutes les autorisations que vous souhaitez conserver après l’exécution de **sp_changeobjectowner**. Par conséquent, nous vous recommandons de générer un script pour les autorisations existantes avant d’exécuter **sp_changeobjectowner**. Après avoir modifié la propriété de l'objet, vous pouvez utiliser le script afin de réappliquer les autorisations. Vous devez modifier le propriétaire de l'objet dans le script des autorisations avant l'exécution.  
  
 Pour modifier le propriétaire d'un élément sécurisable, utilisez ALTER AUTHORIZATION. Pour modifier un schéma, utilisez ALTER SCHEMA.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** , ou l’appartenance au rôle de base de données fixe **db_ddladmin** et au rôle de base de données fixe **db_securityadmin** , ainsi que l’autorisation Control sur l’objet.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie le propriétaire de la `authors` table en `Corporate\GeorgeW` .  
  
```  
EXEC sp_changeobjectowner 'authors', 'Corporate\GeorgeW';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
