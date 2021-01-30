---
description: sp_create_plan_guide_from_handle (Transact-SQL)
title: sp_create_plan_guide_from_handle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_plan_guide_from_handle_TSQL
- sp_create_plan_guide_from_handle
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide_from_handle
ms.assetid: 02cfb76f-a0f9-4b42-a880-1c3e7d64fe41
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2020abc49ccd29491f99adc77910868071e16e00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186219"
---
# <a name="sp_create_plan_guide_from_handle-transact-sql"></a>sp_create_plan_guide_from_handle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée un ou plusieurs repères de plan à partir d'un plan de requête dans le cache du plan. Vous pouvez appliquer cette procédure stockée pour garantir que l'optimiseur de requête utilise toujours un plan de requête spécifique pour une requête spécifiée. Pour plus d'informations sur les repères de plan, consultez [Plan Guides](../../relational-databases/performance/plan-guides.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_create_plan_guide_from_handle [ @name = ] N'plan_guide_name'  
    , [ @plan_handle = ] plan_handle  
    , [ [ @statement_start_offset = ] { statement_start_offset | NULL } ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @name =] N'*plan_guide_name*'  
 Nom du repère de plan. Les noms des repères de plan sont limités à la base de données active. *plan_guide_name* doit respecter les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md) et ne peut pas commencer par le signe dièse (#). La longueur maximale de *plan_guide_name* est de 124 caractères.  
  
 [ @plan_handle =] *plan_handle*  
 Identifie un traitement dans le repère de plan. *plan_handle* est **de type varbinary (64)**. vous pouvez obtenir *plan_handle* à partir de la vue de gestion dynamique [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) .  
  
 [ @statement_start_offset =] { *statement_start_offset* | NULL}]  
 Identifie la position de départ de l’instruction dans le lot du *plan_handle* spécifié. *statement_start_offset* est de **type int**, avec NULL comme valeur par défaut.  
  
 Le décalage de l’instruction correspond à la colonne statement_start_offset de la vue de gestion dynamique [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) .  
  
 Lorsque la valeur NULL est spécifiée ou qu'un décalage d'instruction n'est pas spécifié, un repère de plan est créé pour chaque instruction du lot à l'aide du plan de requête pour le descripteur de plan spécifié. Les repères de plan obtenus sont équivalents à ceux qui utilisent l'indicateur de requête USE PLAN pour forcer l'utilisation d'un plan spécifique.  
  
## <a name="remarks"></a>Notes  
 Un repère de plan ne peut pas être créé pour tous les types d'instructions. Si un repère de plan ne peut pas être créé pour une instruction du lot, la procédure stockée ignore l'instruction et passe à la suivante dans le lot. Si une instruction apparaît plusieurs fois dans le même lot, le plan de la dernière occurrence est activé et les plans précédents de l'instruction sont désactivés. Si aucune instruction dans le lot ne peut être utilisée dans un repère de plan, l'erreur 10532 est générée et l'instruction échoue. Nous vous recommandons de toujours obtenir le descripteur de plan à partir de la vue de gestion dynamique sys.dm_exec_query_stats pour empêcher toute occurrence de cette erreur.  
  
> [!IMPORTANT]  
>  L'instruction sp_create_plan_guide_from_handle crée des repères de plan basés sur les plans qui apparaissent dans le cache du plan. Autrement dit, le texte du lot, les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et le plan d'exécution XML sont transférés, caractère par caractère (y compris les valeurs littérales passées à la requête), du cache du plan au repère de plan obtenu. Ces chaînes de texte peuvent contenir des informations sensibles stockées ensuite dans les métadonnées de la base de données. Les utilisateurs disposant des autorisations appropriées peuvent afficher ces informations à l’aide de l’affichage catalogue sys.plan_guides et de la boîte de dialogue **Propriétés du repère de plan** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Pour garantir qu'aucune information sensible n'est divulguée par le biais d'un repère de plan, nous vous recommandons d'examiner les repères de plan créés à partir du cache du plan.  
  
## <a name="creating-plan-guides-for-multiple-statements-within-a-query-plan"></a>Création de repères de plan pour plusieurs instructions dans un plan de requête  
 Comme l'instruction sp_create_plan_guide, l'instruction sp_create_plan_guide_from_handle supprime du cache du plan le plan de requête du lot ou module ciblé. Cette suppression permet de garantir que tous les utilisateurs commencent à utiliser le nouveau repère de plan. Lorsque vous créez un repère de plan pour plusieurs instructions dans un plan de requête unique, vous pouvez différer la suppression du plan du cache en créant tous les repères de plan dans une transaction explicite. Cette méthode permet au plan de rester dans le cache jusqu'à ce que la transaction soit terminée et qu'un repère de plan soit créé pour chaque instruction spécifiée. Voir l'exemple B.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation `VIEW SERVER STATE`. De plus, des autorisations individuelles sont requises pour chaque repère de plan créé à l'aide de l'instruction sp_create_plan_guide_from_handle. Pour créer un repère de plan de type OBJECT, vous devez disposer `ALTER` de l’autorisation sur l’objet référencé. Pour créer un repère de plan de type SQL ou TEMPLATE, vous devez disposer de `ALTER` l’autorisation sur la base de données actuelle. Pour déterminer le type de repère de plan qui sera créé, exécutez la requête suivante :  
  
```sql  
SELECT cp.plan_handle, sql_handle, st.text, objtype   
FROM sys.dm_exec_cached_plans AS cp  
JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st;  
```  
  
 Dans la ligne qui contient l'instruction pour laquelle vous créez le repère de plan, examinez la colonne `objtype` dans le jeu de résultats. La valeur `Proc` indique que le repère de plan est de type OBJECT. D'autres valeurs, telles que `AdHoc` ou `Prepared`, indiquent que le repère de plan est de type SQL.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-plan-guide-from-a-query-plan-in-the-plan-cache"></a>R. Création d'un repère de plan à partir d'un plan de requête dans le cache du plan  
 L'exemple suivant crée un repère de plan pour une instruction SELECT unique en spécifiant un plan de requête à partir du cache du plan. L'exemple commence par exécuter une instruction `SELECT` simple pour laquelle le repère de plan sera créé. Le plan de cette requête est examiné à l'aide des vues de gestion dynamique `sys.dm_exec_sql_text` et `sys.dm_exec_text_query_plan`. Le repère de plan est ensuite créé pour la requête en spécifiant le plan de requête dans le cache du plan associé à la requête. La dernière instruction dans l'exemple vérifie que le repère de plan existe.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT WorkOrderID, p.Name, OrderQty, DueDate  
FROM Production.WorkOrder AS w   
JOIN Production.Product AS p ON w.ProductID = p.ProductID  
WHERE p.ProductSubcategoryID > 4  
ORDER BY p.Name, DueDate;  
GO  
-- Inspect the query plan by using dynamic management views.  
SELECT * FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle)  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
-- Create a plan guide for the query by specifying the query plan in the plan cache.  
DECLARE @plan_handle varbinary(64);  
DECLARE @offset int;  
SELECT @plan_handle = plan_handle, @offset = qs.statement_start_offset  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
  
EXECUTE sp_create_plan_guide_from_handle   
    @name =  N'Guide1',  
    @plan_handle = @plan_handle,  
    @statement_start_offset = @offset;  
GO  
-- Verify that the plan guide is created.  
SELECT * FROM sys.plan_guides  
WHERE scope_batch LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
```  
  
### <a name="b-creating-multiple-plan-guides-for-a-multistatement-batch"></a>B. Création de plusieurs repères de plan pour un lot à instructions multiples  
 L'exemple suivant crée un repère de plan pour deux instructions dans un lot à instructions multiples. Les repères de plan sont créés dans une transaction explicite afin que le plan de requête du lot ne soit pas supprimé du cache du plan après la création du premier repère de plan. L'exemple commence par exécuter un lot à instructions multiples. Le plan du lot est examiné à l'aide de vues de gestion dynamique. Notez qu'une ligne est retournée pour chaque instruction du lot. Un repère de plan est ensuite créé pour la première et la troisième instruction dans le lot en spécifiant le paramètre `@statement_start_offset`. La dernière instruction dans l'exemple vérifie que les repères de plan existent.  
  
 [!code-sql[PlanGuides#Create_From_Handle2](../../relational-databases/system-stored-procedures/codesnippet/tsql/sp-create-plan-guide-fro_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [Repères de plan](../../relational-databases/performance/plan-guides.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_text_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
  
  
