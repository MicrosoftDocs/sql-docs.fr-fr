---
description: sys.dm_exec_query_plan (Transact-SQL)
title: sys.dm_exec_query_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_exec_query_plan_TSQL
- sys.dm_exec_query_plan
- dm_exec_query_plan
- sys.dm_exec_query_plan_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_plan dynamic management function
ms.assetid: e26f0867-9be3-4b2e-969e-7f2840230770
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 18d6e4e8e3c6fe097676d1bf65f337610127566a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094111"
---
# <a name="sysdm_exec_query_plan-transact-sql"></a>sys.dm_exec_query_plan (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Retourne le plan d'exécution de requêtes au format XML pour le traitement spécifié par le descripteur de plan. Le plan spécifié par le descripteur de plan peut être en cache ou en cours d'exécution.  
  
Le schéma XML de Showplan est publié et disponible sur [ce site Web de Microsoft](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409). Vous le trouverez également dans le répertoire d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.dm_exec_query_plan(plan_handle)  
```  
  
## <a name="arguments"></a>Arguments  
*plan_handle*  
Jeton qui identifie de façon unique un plan d’exécution de requête pour un lot exécuté et dont le plan réside dans le cache du plan, ou qui est en cours d’exécution. *plan_handle* est **de type varbinary (64)**.   

Le *plan_handle* peut être obtenu à partir des objets de gestion dynamique suivants :
  
-   [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)  
  
-   [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
-   [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)  

-   [sys.dm_exec_procedure_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md)  

-   [sys.dm_exec_trigger_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-trigger-stats-transact-sql.md)  
  
## <a name="table-returned"></a>Table retournée  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**dbid**|**smallint**|ID de la base de données de contexte qui était en fonction lorsque l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] correspondant à ce plan a été compilée. Pour les instructions SQL ad hoc et préparées, l'ID de la base de données où les instructions ont été compilées.<br /><br /> Colonne acceptant la valeur NULL.|  
|**objectid**|**int**|ID de l'objet (par exemple, procédure stockée ou fonction définie par l'utilisateur) pour ce plan de requête. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**number**|**smallint**|Entier servant à la numérotation des procédures stockées. Par exemple, un groupe de procédures pour l'application **orders** peuvent être appelées **orderproc;1**, **orderproc;2**, etc. Pour les traitements ad hoc et préparés, cette colonne est **null**.<br /><br /> Colonne acceptant la valeur NULL.|  
|**chiffrées**|**bit**|Indique si la procédure stockée correspondante est chiffrée.<br /><br /> 0 = Non chiffrée.<br /><br /> 1 = Chiffrée.<br /><br /> Colonne n'acceptant pas la valeur NULL.|  
|**query_plan**|**xml**|Contient la représentation Showplan au moment de la compilation du plan d’exécution de requête spécifié avec *plan_handle*. Le plan d'exécution de requêtes est au format XML. Un plan est généré pour chaque traitement contenant par exemple des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ad hoc, des appels de procédures stockées et des appels de fonctions définies par l'utilisateur.<br /><br /> Colonne acceptant la valeur NULL.|  
  
## <a name="remarks"></a>Notes  
 Aucune sortie du plan d'exécution de requêtes n'est retournée dans la colonne **query_plan** de la table retournée pour l'objet pour **sys.dm_exec_query_plan** dans les conditions suivantes :  
  
-   Si le plan de requête spécifié à l’aide de *plan_handle* a été supprimé de la mémoire cache des plans, la colonne **query_plan** de la table retournée est null. Ceci peut se produire par exemple s'il existe un délai entre le moment où le descripteur de plan est capturé et le moment de son utilisation avec **sys.dm_exec_query_plan**.  
  
-   Certaines instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne sont pas mises en mémoire cache, par exemple les instructions d'opérations en bloc ou les instructions contenant des littéraux de chaîne dont la taille est supérieure à 8 Ko. Il est impossible de récupérer pour de telles instructions des plans d'exécution de requêtes XML à l'aide de **sys.dm_exec_query_plan** sauf si le traitement est en cours d'exécution car elles n'existent pas dans la mémoire cache.  
  
-   Si une [!INCLUDE[tsql](../../includes/tsql-md.md)] procédure stockée ou un traitement contient un appel à une fonction définie par l’utilisateur ou un appel à du code SQL dynamique, par exemple à l’aide de exec (*String*), le plan d’exécution de requêtes XML compilé pour la fonction définie par l’utilisateur n’est pas inclus dans la table retournée par **sys.dm_exec_query_plan** pour le traitement ou la procédure stockée. Au lieu de cela, vous devez effectuer un appel distinct à **sys.dm_exec_query_plan** pour le descripteur de plan qui correspond à la fonction définie par l’utilisateur.  
  
 Lorsqu'une requête ad hoc utilise un paramétrage simple ou forcé, la colonne **query_plan** contient uniquement le texte de l'instruction, pas le plan de requête réel. Pour retourner le plan de requête, appelez **sys.dm_exec_query_plan** pour le descripteur de plan de la requête paramétrable préparée. Vous pouvez déterminer si la requête a été rendue paramétrable par le référencement de la colonne **sql** de la vue [sys.syscacheobjects](../../relational-databases/system-compatibility-views/sys-syscacheobjects-transact-sql.md) ou la colonne de texte de la vue de gestion dynamique [sys.dm_exec_sql_text](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md).  
  
> [!NOTE] 
> En raison d’une limitation du nombre de niveaux imbriqués autorisés dans le type de données **XML** , **sys.dm_exec_query_plan** ne peut pas retourner des plans de requête qui remplissent ou dépassent 128 niveaux d’éléments imbriqués. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette condition empêchait les retours par le plan de requête et générait l'erreur 6335. Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 et versions ultérieures, la colonne **query_plan** retourne la valeur null.   
> Vous pouvez utiliser la fonction de gestion dynamique [&#41;sys.dm_exec_text_query_plan &#40;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md) pour retourner la sortie du plan de requête au format texte.  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter **sys.dm_exec_query_plan**, un utilisateur doit être membre du rôle serveur fixe **sysadmin** ou disposer de l' `VIEW SERVER STATE` autorisation sur le serveur.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent l'utilisation de la vue de gestion dynamique **sys.dm_exec_query_plan**.  
  
 Pour afficher des plans d'exécution de requêtes XML, exécutez les requêtes suivantes dans l'éditeur de requêtes de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], puis cliquez sur **ShowPlanXML** dans la colonne **query_plan** de la table retournée par l'objet **sys.dm_exec_query_plan**. Le plan d'exécution de requêtes XML s'affiche dans le volet de résumé de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Pour enregistrer le Showplan XML dans un fichier, cliquez avec le bouton droit sur **ShowplanXml** dans la colonne **query_plan** , cliquez sur **enregistrer les résultats sous**, puis nommez le fichier au format \<*file_name*> . sqlplan ; par exemple, MyXMLShowplan. sqlplan.  
  
### <a name="a-retrieve-the-cached-query-plan-for-a-slow-running-transact-sql-query-or-batch"></a>R. Récupération du plan de requête mis en mémoire cache pour un traitement ou une requête Transact-SQL à exécution lente  
 Les plans de requête pour divers types de lots [!INCLUDE[tsql](../../includes/tsql-md.md)], par exemple les procédures stockées, les fonctions définies par l'utilisateur et les lots appropriés, sont mis en cache dans une zone de la mémoire appelée le cache de plan. Chaque plan de requête mis dans cette mémoire cache est différencié par un identificateur unique appelé descripteur de plan. Il est possible d'utiliser ce descripteur avec la vue de gestion dynamique **sys.dm_exec_query_plan** pour récupérer le plan d'exécution d'une requête ou d'un traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] particulier.  
  
 Si une requête ou un traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] s'exécute longtemps sur une connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifique, récupérez le plan d'exécution de cette requête ou de ce traitement pour trouver la raison de ce retard. L'exemple suivant montre la récupération du plan d'exécution de requêtes XML pour une requête ou un traitement s'exécutant lentement.  
  
> [!NOTE]  
>  Pour exécuter cet exemple, remplacez les valeurs de *session_id* et *plan_handle* par des valeurs spécifiques à votre serveur.  
  
 Récupérez tout d'abord à l'aide de la procédure stockée `sp_who` l'ID de processus serveur (SPID) pour le processus exécutant la requête ou le traitement.  
  
```sql  
USE master;  
GO  
exec sp_who;  
GO  
```  
  
 Le jeu de résultats renvoyé par `sp_who` indique que le SPID est `54`. Utilisez cet identificateur avec la vue de gestion dynamique `sys.dm_exec_requests` pour récupérer le descripteur de plan via la requête suivante :  
  
```sql  
USE master;  
GO  
SELECT * FROM sys.dm_exec_requests  
WHERE session_id = 54;  
GO  
```  
  
 La table retournée par **sys.dm_exec_requests** indique que le descripteur de plan pour la requête ou le traitement à exécution lente est `0x06000100A27E7C1FA821B10600` , que vous pouvez spécifier comme argument *plan_handle* avec `sys.dm_exec_query_plan` pour récupérer le plan d’exécution au format XML comme suit. Le plan d'exécution au format XML pour la requête ou le traitement à exécution lente se trouve dans la colonne **query_plan** de la table retournée par `sys.dm_exec_query_plan`.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_plan (0x06000100A27E7C1FA821B10600);  
GO  
```  
  
### <a name="b-retrieve-every-query-plan-from-the-plan-cache"></a>B. Récupérer chaque plan de requête à partir du cache du plan  
 Pour récupérer un instantané de tous les plans de requête résidant dans la mémoire cache des plans, procurez-vous les descripteurs de tous les plans de requête dans la mémoire cache via une requête dans la vue de gestion dynamique `sys.dm_exec_cached_plans`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_cached_plans`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_query_plan` comme suit. La sortie du plan d'exécution de requêtes XML pour chaque plan actuellement dans la mémoire cache des plans se trouve dans la colonne `query_plan` de la table retournée.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_cached_plans AS cp 
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);  
GO  
```  
  
### <a name="c-retrieve-every-query-plan-for-which-the-server-has-gathered-query-statistics-from-the-plan-cache"></a>C. Récupération dans la mémoire cache des plans de chaque plan de requête pour lequel le serveur a regroupé des statistiques de requête  
 Pour récupérer un instantané de tous les plans de requête pour lesquels le serveur a regroupé des statistiques actuellement dans la mémoire cache des plans, procurez-vous les descripteurs de ces plans dans la mémoire cache via une requête formulée dans la vue de gestion dynamique `sys.dm_exec_query_stats`. Les descripteurs de plan sont stockés dans la colonne `plan_handle` de `sys.dm_exec_query_stats`. Utilisez ensuite l'opérateur CROSS APPLY pour transmettre les descripteurs à `sys.dm_exec_query_plan` comme suit. La sortie du plan d'exécution de requêtes XML pour chaque plan pour lequel le serveur a regroupé des statistiques actuellement dans la mémoire cache des plans se trouve dans la colonne `query_plan` de la table retournée.  
  
```sql  
USE master;  
GO  
SELECT * 
FROM sys.dm_exec_query_stats AS qs 
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle);  
GO  
```  
  
### <a name="d-retrieve-information-about-the-top-five-queries-by-average-cpu-time"></a>D. Récupération d'informations sur les cinq premières requêtes d'après le temps processeur moyen  
 L'exemple suivant retourne les plans et le temps processeur moyen pour les cinq premières requêtes.  
  
```sql  
SELECT TOP 5 total_worker_time/execution_count AS [Avg CPU Time],  
   plan_handle, query_plan   
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_query_plan(qs.plan_handle)  
ORDER BY total_worker_time/execution_count DESC;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)   
 [sys.dm_exec_text_query_plan &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)  
  
  
