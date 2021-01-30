---
description: sp_delete_targetserver (Transact-SQL)
title: sp_delete_targetserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_targetserver
- sp_delete_targetserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_targetserver
ms.assetid: cc438701-ad91-419d-9f23-ebc4c548c700
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2f9e77a548e32e32c41d7ed6b45621ac5f4a2594
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199149"
---
# <a name="sp_delete_targetserver-transact-sql"></a>sp_delete_targetserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Supprime le serveur spécifié de la liste des serveurs cibles disponibles.  
   
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_targetserver [ @server_name = ] 'server'   
     [ , [ @clear_downloadlist = ] clear_downloadlist ]  
     [ , [ @post_defection = ] post_defection ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @server_name = ] 'server'` Nom du serveur à supprimer en tant que serveur cible disponible. *Server* est de type **nvarchar (30)**, sans valeur par défaut.  
  
`[ @clear_downloadlist = ] clear_downloadlist` Spécifie s’il faut effacer la liste de téléchargement du serveur cible. *clear_downloadlist* est de type **bit**, avec **1** comme valeur par défaut. Lorsque *clear_downloadlist* est **1**, la procédure efface la liste de téléchargement du serveur avant de supprimer le serveur. Lorsque *clear_downloadlist* a la **valeur 0**, la liste de téléchargement n’est pas effacée.  
  
`[ @post_defection = ] post_defection` Spécifie s’il faut poster une instruction de désinscription sur le serveur cible. *post_defection* est de type **bit**, avec 1 comme valeur par défaut. Lorsque *post_defection* a la valeur **1**, la procédure publie une instruction de désinscription sur le serveur cible avant de supprimer le serveur. Lorsque *post_defection* a la **valeur 0**, la procédure ne publie pas d’instruction de désinscription sur le serveur cible.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 La méthode normale pour supprimer un serveur cible consiste à appeler **sp_msx_defect** sur le serveur cible. Utilisez **sp_delete_targetserver** uniquement lorsqu’une désinscription manuelle est nécessaire.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, les utilisateurs doivent disposer du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous supprime le serveur `LONDON1` de la liste des serveurs de travail disponibles.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_targetserver  
  @server_name = N'LONDON1' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_help_targetserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-targetserver-transact-sql.md)   
 [sp_msx_defect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-msx-defect-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
