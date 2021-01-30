---
description: sp_markpendingschemachange (Transact-SQL)
title: sp_markpendingschemachange (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_markpendingschemachange
- sp_markpendingschemachange_TSQL
helpviewer_keywords:
- sp_markpendingschemachange
ms.assetid: 01100309-7bef-4154-85bf-f18489577e37
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 412beb1a5afa4fdb24cab38df9e6251c6b4d5ed2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185375"
---
# <a name="sp_markpendingschemachange-transact-sql"></a>sp_markpendingschemachange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Procédure utilisée pour la capacité de prise en charge des publications de fusion permettant à un administrateur d'ignorer certaines modifications de schéma en attente pour qu'elles ne soient pas répliquées. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!CAUTION]  
>  Cette procédure stockée peut entraîner des modifications de schéma ne devant pas être répliquées. Elle ne doit être utilisée pour résoudre des problèmes qu'après avoir essayé la réinitialisation, ou d'autres méthodes trop coûteuses en termes de performance.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_markpendingschemachange [@publication = ] 'publication'  
    [ , [ @schemaversion = ] schemaversion ]  
    [ , [ @status = ] 'status' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @schemaversion = ] schemaversion` Identifie une modification de schéma en attente. *SchemaVersion* est de **type int**, avec **0** comme valeur par défaut. Utilisez [sp_enumeratependingschemachanges &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-enumeratependingschemachanges-transact-sql.md) pour répertorier les modifications de schéma en attente pour la publication.  
  
`[ @status = ] 'status'` Indique si une modification de schéma en attente sera ignorée. l' *État* est **nvarchar (10)** et sa valeur par défaut est **actif**. Si la valeur de l' *État* est **ignorée**, la modification de schéma sélectionnée n’est pas répliquée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_markpendingschemachange** est utilisé avec la réplication de fusion.  
  
 **sp_markpendingschemachange** est une procédure stockée destinée à la prise en charge de la réplication de fusion et ne doit être utilisée que lorsque d’autres actions correctives, telles que la réinitialisation, n’ont pas pu corriger la situation ou sont trop coûteuses en termes de performances.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_markpendingschemachange**.  
  
## <a name="see-also"></a>Voir aussi  
 [sysmergeschemachange &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysmergeschemachange-transact-sql.md)  
  
  
