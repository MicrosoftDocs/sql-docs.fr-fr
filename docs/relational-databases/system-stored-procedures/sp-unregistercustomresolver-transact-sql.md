---
description: sp_unregistercustomresolver (Transact-SQL)
title: sp_unregistercustomresolver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_unregistercustomresolver_TSQL
- sp_unregistercustomresolver
helpviewer_keywords:
- sp_unregistercustomresolver
ms.assetid: 08bd20c8-c6be-4be2-be9f-2b5e1d7bee43
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1a8bb2c7384312d9782bdb6169d17ace6eae7c6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202334"
---
# <a name="sp_unregistercustomresolver-transact-sql"></a>sp_unregistercustomresolver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Annule l'inscription d'un module de logique métier précédemment inscrit. La logique métier peut prendre la forme d'un composant COM ou d'un assemby [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework. Cette procédure stockée est exécutée sur le serveur de distribution dans lequel la logique métier personnalisée a été inscrite.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_unregistercustomresolver [ @article_resolver = ] 'article_resolver'   
```  
  
## <a name="arguments"></a>Arguments  
`[ @article_resolver = ] 'article_resolver'` Spécifie le nom de la logique métier personnalisée en cours d’annulation d’inscription. *article_resolver* est de type **nvarchar (255)**, sans valeur par défaut. Si la logique métier en cours de suppression est un composant COM, ce paramètre est le nom convivial du composant. Si la logique métier est un assembly .NET Framework, ce paramètre correspond au nom de l'assembly.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_unregistercustomresolver** est utilisé dans la réplication de fusion.  
  
 Utilisez [sp_enumcustomresolvers](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) sur n’importe quel serveur de la topologie de réplication pour retourner la liste des modules de logique métier personnalisés ou des programmes de résolution com disponibles pour la topologie.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_unregistercustomresolver**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_lookupcustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
