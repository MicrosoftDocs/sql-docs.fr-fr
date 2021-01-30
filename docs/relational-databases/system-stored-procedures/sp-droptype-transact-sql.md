---
description: sp_droptype (Transact-SQL)
title: sp_droptype (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droptype_TSQL
- sp_droptype
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droptype
ms.assetid: e78464ac-2370-4c4e-9cc0-06aebc07cec5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7614a0b11a22ed7680fa98a9fbe93663d175c1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186990"
---
# <a name="sp_droptype-transact-sql"></a>sp_droptype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime un type de données alias de **systypes**.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_droptype [ @typename = ] 'type'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @typename = ] 'type'` Nom d’un type de données alias dont vous êtes propriétaire. *type* **sysname**, sans valeur par défaut.  
  
## <a name="return-code-type"></a>Type des codes renvoyés  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Le type de données de l’alias de **type** ne peut pas être supprimé si des tables ou d’autres objets de base de données y font référence.  
  
> [!NOTE]  
>  Vous ne pouvez pas supprimer un type de données alias si celui-ci est utilisé dans une définition de table ou si une règle ou une valeur par défaut lui est associée.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **db_owner** ou le rôle de base de données fixe **db_ddladmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant supprime le type de données alias `birthday`.  
  
> [!NOTE]  
>  Ce type de données alias doit déjà exister, faute de quoi cet exemple renvoie un message d'erreur.  
  
```  
USE master;  
GO  
EXEC sp_droptype 'birthday';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sp_addtype &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addtype-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
