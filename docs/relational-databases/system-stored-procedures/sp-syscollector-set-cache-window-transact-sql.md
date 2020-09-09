---
description: sp_syscollector_set_cache_window (Transact-SQL)
title: sp_syscollector_set_cache_window (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_syscollector_set_cache_window
- sp_syscollector_set_cache_window_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_set_cache_window stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 660f2749-392f-46bf-89f3-27764d848507
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81d1be542b93a1ceb7bd699e2dbc07a7f3884fa4
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89541489"
---
# <a name="sp_syscollector_set_cache_window-transact-sql"></a>sp_syscollector_set_cache_window (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit le nombre de tentatives de téléchargement de données en cas d'échec. Une nouvelle tentative de téléchargement en cas d'échec atténue le risque de perdre les données recueillies.  

  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_syscollector_set_cache_window [ @cache_window = ] cache_window   
```  
  
## <a name="arguments"></a>Arguments  
 [ @cache_window =] *cache_window*  
 Nombre de tentatives de téléchargement de données dans l'entrepôt de données de gestion en cas d'échec sans perte de données. *cache_window* est de **type int** avec 1 comme valeur par défaut. *cache_window* peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|-1|Mise en cache de toutes les données de téléchargement des échecs de téléchargement précédents.|  
|0|Ne pas mettre en cache de données après un échec de téléchargement.|  
|*n*|Mettez en cache les données des n échecs de téléchargement précédents, où *n* >= 1.|  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Vous devez désactiver le collecteur de données avant de modifier la configuration de la fenêtre de mise en cache. Cette procédure stockée échoue si le collecteur de données est activé. Pour plus d’informations, consultez [activer ou désactiver la collecte de données](../../relational-databases/data-collection/enable-or-disable-data-collection.md)et gérer la collecte de [données](../../relational-databases/data-collection/manage-data-collection.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'appartenance au rôle de base de données fixe dc_admin (avec autorisation EXECUTE) pour exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant désactive le collecteur de données, configure la fenêtre de cache pour conserver les données jusqu'à l'échec de trois téléchargements au maximum, puis active le collecteur de données.  
  
```sql  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_disable_collector;  
GO  
EXECUTE dbo.sp_syscollector_set_cache_window 3;  
GO  
EXECUTE dbo.sp_syscollector_enable_collector;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_syscollector_set_cache_directory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-directory-transact-sql.md)  
  
  
