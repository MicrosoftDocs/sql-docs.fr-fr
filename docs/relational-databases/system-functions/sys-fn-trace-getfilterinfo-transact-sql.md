---
description: sys.fn_trace_getfilterinfo (Transact-SQL)
title: sys.fn_trace_getfilterinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_trace_getfilterinfo
- fn_trace_getfilterinfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], filters
- sys.fn_trace_getfilterinfo function
- filters [SQL Server], traces
- fn_trace_getfilterinfo function
ms.assetid: 09fe4a28-ff8a-4655-9da1-4654d5bc514d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 889a10c6ae999a5211120e52d7b903d04c5ec70e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097448"
---
# <a name="sysfn_trace_getfilterinfo-transact-sql"></a>sys.fn_trace_getfilterinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Retourne des informations relatives aux filtres appliqués à une trace spécifiée.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt des événements étendus.  
  
 
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn_trace_getfilterinfo ( trace_id )  
```  
  
## <a name="arguments"></a>Arguments  
 *trace_id*  
 ID de la trace. *trace_id* est de **type int**, sans valeur par défaut.  
  
## <a name="tables-returned"></a>Tables retournées  
 Retourne les informations suivantes. Pour plus d’informations sur les colonnes, consultez [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ColumnID**|**int**|ID de la colonne sur laquelle le filtre est appliqué.|  
|**logical_operator**|**int**|Indique si l'opérateur AND ou OR est appliqué.|  
|**comparison_operator**|**int**|Spécifie le type de comparaison effectué :<br /><br /> 0 = Égal<br /><br /> 1 = Différent de<br /><br /> 2 = Supérieur à<br /><br /> 3 = Inférieur à<br /><br /> 4 = Supérieur ou égal à<br /><br /> 5 = Inférieur ou égal à<br /><br /> 6 = Identique<br /><br /> 7 = Non identique|  
|**value**|**sql_variant**|Indique la valeur sur laquelle le filtre est appliqué.|  
  
## <a name="remarks"></a>Notes  
 L’utilisateur définit *trace_id* valeur pour identifier, modifier et contrôler la trace. Lorsqu’il reçoit l’ID d’une trace spécifique, **fn_trace_getfilterinfo** retourne des informations sur n’importe quel filtre de cette trace. Si la trace spécifiée n'a pas de filtre, cette fonction retourne un ensemble de lignes vide. Lorsqu'un identificateur non valide lui est passé, cette fonction renvoie un ensemble de lignes vide. Pour obtenir des informations similaires sur les traces, consultez [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER TRACE sur le serveur.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne des informations sur tous les filtres de la trace numéro 2.  
  
```  
SELECT * FROM fn_trace_getfilterinfo(2) ;  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Créer une trace &#40;Transact-SQL&#41;](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)   
 [sp_trace_setfilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)   
 [sp_trace_create &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [sp_trace_generateevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-generateevent-transact-sql.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setstatus &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setstatus-transact-sql.md)   
 [sys.fn_trace_geteventinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-geteventinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [sys.fn_trace_gettable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-gettable-transact-sql.md)  
  
  
