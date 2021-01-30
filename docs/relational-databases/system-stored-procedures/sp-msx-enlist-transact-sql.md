---
description: sp_msx_enlist (Transact-SQL)
title: sp_msx_enlist (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_enlist_TSQL
- sp_msx_enlist
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_enlist
ms.assetid: ceb3b2bc-0cc4-48d8-9bdc-6a809556e35f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13a8c58ffa932199452db856b04e880dbbe2ff95
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174441"
---
# <a name="sp_msx_enlist-transact-sql"></a>sp_msx_enlist (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute le serveur actif à la liste des serveurs disponibles sur le serveur maître.  
  
> [!CAUTION]  
>  La procédure stockée **sp_msx_enlist** modifie le registre. La modification manuelle du Registre n'est pas recommandée parce que des modifications inadaptées ou incorrectes peuvent provoquer de graves problèmes de configuration à votre système. Seuls des utilisateurs expérimentés peuvent utiliser regedit.exe pour modifier le Registre. Pour plus d'informations, consultez la documentation de Microsoft Windows.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_msx_enlist [@msx_server_name =] 'msx_server'   
     [, [@location =] 'location']  
```  
  
## <a name="arguments"></a>Arguments  
`[ @msx_server_name = ] 'msx_server'` Nom du serveur d’administration multiserveur (maître). *msx_server* est de **type sysname**, sans valeur par défaut.  
  
`[ @location = ] 'location'` Emplacement du serveur cible à ajouter. *location* est de type **nvarchar (100)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution de cette procédure sont accordées par défaut aux membres du rôle de serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant inscrit le serveur actif dans le serveur maître `AdventureWorks1`. L'emplacement du serveur actif est `Building 21, Room 309, Rack 5`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_msx_enlist N'AdventureWorks1',   
    N'Building 21, Room 309, Rack 5' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_msx_defect &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-msx-defect-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [xp_cmdshell &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)  
  
  
