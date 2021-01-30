---
description: sp_trace_setfilter (Transact-SQL)
title: sp_trace_setfilter (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_trace_setfilter
- sp_trace_setfilter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_trace_setfilter
ms.assetid: 11e7c7ac-a581-4a64-bb15-9272d5c1f7ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b068b4659b3c3205aa52d6ebffc0daf680c3cd9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209713"
---
# <a name="sp_trace_setfilter-transact-sql"></a>sp_trace_setfilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Applique un filtre à une trace. **sp_trace_setfilter** ne peuvent être exécutées que sur des traces existantes qui sont arrêtées (l'*État* est **0**). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur si cette procédure stockée est exécutée sur une trace qui n’existe pas ou dont l' *État* n’est pas **égal à 0**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez plutôt des événements étendus.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_trace_setfilter [ @traceid = ] trace_id   
          , [ @columnid = ] column_id  
          , [ @logical_operator = ] logical_operator  
          , [ @comparison_operator = ] comparison_operator  
          , [ @value = ] value  
```  
  
## <a name="arguments"></a>Arguments  
`[ @traceid = ] trace_id` ID de la trace à laquelle le filtre est défini. *trace_id* est de **type int**, sans valeur par défaut. L’utilisateur emploie cette *trace_id* valeur pour identifier, modifier et contrôler la trace.  
  
`[ @columnid = ] column_id` ID de la colonne sur laquelle le filtre est appliqué. *column_id* est de **type int**, sans valeur par défaut. Si *column_id* a la valeur null, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] efface tous les filtres pour la trace spécifiée.  
  
`[ @logical_operator = ] logical_operator` Spécifie si l’opérateur AND (**0**) ou or (**1**) est appliqué. *logical_operator* est de **type int**, sans valeur par défaut.  
  
`[ @comparison_operator = ] comparison_operator` Spécifie le type de comparaison à effectuer. *comparison_operator* est de **type int**, sans valeur par défaut. Le tableau contient les opérateurs de comparaison et leurs valeurs.  
  
|Valeur|Opérateur de comparaison|  
|-----------|-------------------------|  
|**0**|= (Égal à)|  
|**1**|<>  (différent de)|  
|**2**|> (Supérieur à)|  
|**3**|< (Inférieur à)|  
|**4**|>= (supérieur ou égal à)|  
|**5**|<= (inférieur ou égal à)|  
|**6**|LIKE|  
|**7**|Ne correspond pas à|  
  
`[ @value = ] value` Spécifie la valeur sur laquelle filtrer. Le type de données de la *valeur* doit correspondre au type de données de la colonne à filtrer. Par exemple, si le filtre est défini sur une colonne d’ID d’objet qui est un type de données **int** , la *valeur* doit être **int**. Si la *valeur* est **nvarchar** ou **varbinary**, elle peut avoir une longueur maximale de 8000.  
  
 Lorsque l'opérateur de comparaison est LIKE ou NOT LIKE, l'opérateur logique peut inclure « % » ou tout autre filtre approprié pour l'opération LIKE.  
  
 Vous pouvez spécifier NULL comme *valeur* pour filtrer les événements avec des valeurs de colonne null. Seuls les opérateurs **0** (= Equal) et **1** (<> différent) sont valides avec la valeur null. Dans ce cas, ils sont équivalents aux opérateurs IS NULL et IS NOT NULL de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Pour appliquer le filtre entre une plage de valeurs de colonne, **sp_trace_setfilter** doit être exécutée deux fois, une fois avec un opérateur de comparaison supérieur ou égal à (' >= ') et une autre fois avec un opérateur d’infériorité ou d’égalité (' <= ').  
  
 Pour plus d’informations sur les types de données des colonnes de données, consultez la [SQL Server référence](../../relational-databases/event-classes/sql-server-event-class-reference.md)de la classe d’événements.  
  
## <a name="return-code-values"></a>Codet de retour  
 Le tableau suivant décrit les valeurs de code que les utilisateurs peuvent recevoir à la fin de l'exécution de la procédure stockée.  
  
|Code de retour|Description|  
|-----------------|-----------------|  
|0|Aucune erreur.|  
|1|Erreur inconnue.|  
|2|La trace est en cours d'exécution. La modification de la Trace à cet instant précis entraîne une erreur.|  
|4|La colonne spécifiée n’est pas valide.|  
|5|La colonne spécifiée n'est pas autorisée pour le filtrage. Cette valeur est retournée uniquement à partir de **sp_trace_setfilter**.|  
|6|L'opérateur de comparaison spécifié n'est pas valide.|  
|7|L'opérateur logique spécifié n'est pas valide.|  
|9|Le descripteur de trace spécifié n'est pas valide.|  
|13|Mémoire insuffisante. Renvoyé lorsqu'il n'y a pas assez de mémoire pour exécuter l'action spécifiée.|  
|16|La fonction n'est pas valide pour cette trace.|  
  
## <a name="remarks"></a>Notes  
 **sp_trace_setfilter** est une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procédure stockée qui effectue un grand nombre des actions précédemment exécutées par les procédures stockées étendues disponibles dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilisez **sp_trace_setfilter** au lieu des procédures stockées étendues du **\* filtre xp_trace_set** pour créer, appliquer, supprimer ou manipuler des filtres sur des traces. Pour plus d’informations, consultez [Filtrer une trace](../../relational-databases/sql-trace/filter-a-trace.md).  
  
 Tous les filtres d’une colonne particulière doivent être activés ensemble au cours d’une exécution de **sp_trace_setfilter**. Par exemple, si un utilisateur décide d'appliquer deux filtres sur la colonne Nom d'application et un filtre sur la colonne Nom d'utilisateur, il doit spécifier les filtres sur le nom d'application de manière séquentielle. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur si l'utilisateur tente de spécifier un filtre sur le nom d'application dans un appel de procédure stockée, suivi d'un filtre sur le nom d'utilisateur, puis d'un autre filtre sur le nom d'application.  
  
 Les paramètres de toutes les procédures stockées trace SQL (**sp_trace_xx**) sont strictement typés. Si ces paramètres ne sont pas appelés à l'aide des types de données appropriés pour les paramètres d'entrée tels qu'ils sont spécifiés dans la description de l'argument, la procédure stockée retourne une erreur.  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation ALTER TRACE.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant définit trois filtres sur Trace `1`. Les filtres `N'SQLT%'` et `N'MS%'` opèrent sur une colonne (`AppName`, valeur `10`) à l'aide de l'opérateur de comparaison « `LIKE` ». Le filtre `N'joe'` opère sur une autre colonne (`UserName`, valeur `11`) à l'aide de l'opérateur de comparaison « `EQUAL` ».  
  
```  
sp_trace_setfilter  1, 10, 0, 6, N'SQLT%';  
sp_trace_setfilter  1, 10, 0, 6, N'MS%';  
sp_trace_setfilter  1, 11, 0, 0, N'joe';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fn_trace_getfilterinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getfilterinfo-transact-sql.md)   
 [sys.fn_trace_getinfo &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [Trace SQL](../../relational-databases/sql-trace/sql-trace.md)  
  
  
