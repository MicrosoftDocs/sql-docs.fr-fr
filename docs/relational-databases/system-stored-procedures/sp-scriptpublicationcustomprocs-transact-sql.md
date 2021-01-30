---
description: sp_scriptpublicationcustomprocs (Transact-SQL)
title: sp_scriptpublicationcustomprocs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_scriptpublicationcustomprocs
- sp_scriptpublicationcustomprocs_TSQL
helpviewer_keywords:
- sp_scriptpublicationcustomprocs
ms.assetid: b06102d5-4284-4834-b126-bc0baea49be5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 733ca066ba7c11754fde7b82b228c7ac3796a42e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189969"
---
# <a name="sp_scriptpublicationcustomprocs-transact-sql"></a>sp_scriptpublicationcustomprocs (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un script pour les procédures personnalisées INSERT, UPDATE et DELETE pour tous les articles de table d'une publication dans laquelle l'option de schéma de génération automatique de procédure stockée est activée. **sp_scriptpublicationcustomprocs** est particulièrement utile pour configurer des abonnements pour lesquels l’instantané est appliqué manuellement.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_scriptpublicationcustomprocs [ @publication = ] 'publication_name'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication_name'` Nom de la publication. *publication_name* est de **type sysname** et n’a pas de valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats qui se compose d’une seule colonne **nvarchar (4000)** . L'ensemble de résultats forme l'instruction CREATE PROCEDURE complète, nécessaire à la création de la procédure stockée personnalisée.  
  
## <a name="remarks"></a>Notes  
 Les procédures personnalisées ne font pas l'objet de scripts pour les articles sans l'option de schéma (0x2) de génération automatique de procédure personnalisée.  
  
 Les procédures suivantes sont utilisées par **sp_scriptpublicationcustomprocs** pour créer des procédures pour l’abonné et ne doivent pas être exécutées directement :  
  
 **sp_script_reconciliation_delproc**  
  
 **sp_script_reconciliation_insproc**  
  
 **sp_script_reconciliation_vdelproc**  
  
 **sp_script_reconciliation_xdelproc**  
  
 **sp_scriptdelproc**  
  
 **sp_scriptinsproc**  
  
 **sp_scriptmappedupdproc**  
  
 **sp_scriptupdproc**  
  
 **sp_scriptvdelproc**  
  
 **sp_scriptvupdproc**  
  
 **sp_scriptxdelproc**  
  
 **sp_scriptxupdproc**  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation EXECUTE est accordée à **public**; une vérification de sécurité procédurale est effectuée à l’intérieur de cette procédure stockée pour restreindre l’accès aux membres du rôle serveur fixe **sysadmin** et **db_owner** rôle de base de données fixe dans la base de données actuelle.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
