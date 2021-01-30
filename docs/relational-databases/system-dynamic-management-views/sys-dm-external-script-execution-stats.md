---
description: sys.dm_external_script_execution_stats
title: sys.dm_external_script_execution_stats | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- sys.dm_external_script_execution_stats
- sys.dm_external_script_execution_stats_TSQL
- dm_external_script_execution_stats
- dm_external_script_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_execution_stats dynamic management view
ms.assetid: 2e99f026-ceb2-42a2-a549-c71d31ed0cf4
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 2885f83185218d389d2a6fcf7dce6c3fbd097286
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201347"
---
# <a name="sysdm_external_script_execution_stats"></a>sys.dm_external_script_execution_stats
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Renvoie une ligne pour chaque type de demande de script externe. Les demandes de script externe sont regroupées par le langage de script externe pris en charge. Une ligne est générée pour chaque fonction enregistrée de script externe. Les fonctions de script externe ne sont pas enregistrées, sauf si elle sont envoyées par un processus parent, comme `rxExec`.
  
> [!NOTE]  
> Cette vue de gestion dynamique (DMV) est disponible uniquement si vous avez installé et activé la fonctionnalité qui prend en charge l’exécution de scripts externes. Pour plus d’informations, consultez [R services dans SQL Server 2016](../../machine-learning/r/sql-server-r-services.md), [machine learning services (r, Python) dans SQL Server 2017 et versions ultérieures](../../machine-learning/sql-server-machine-learning-services.md) et [Azure Managed instance machine learning services](/azure/azure-sql/managed-instance/machine-learning-services-overview).
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|langage|**nvarchar**|Nom du langage enregistré de script externe. Chaque script externe doit spécifier le langage dans la demande de requête utilisé pour le démarrage du lanceur associé. |  
|counter_name|**nvarchar**|Nom de la fonction enregistrée de script externe. N'accepte pas la valeur NULL.|  
|counter_value|**integer**|Nombre total d’instance appelée de la fonction enregistrée de script externe sur le serveur. Cette valeur, cumulative, commence par l’horodatage d’installation de la fonction sur l’instance. Elle ne peut pas être réinitialisée.|  

## <a name="permissions"></a>Autorisations

 Nécessite l'autorisation VIEW SERVER STATE sur le serveur.  
  
> [!NOTE]  
> Les utilisateurs qui exécutent des scripts externes doivent avoir l’autorisation supplémentaire EXECUTE ANY EXTERNAL SCRIPT, toutefois, cette vue de gestion dynamique peut être utilisée par les administrateurs sans cette autorisation.
  
## <a name="remarks"></a>Notes

  Cette vue de gestion dynamique est fournie pour la télémétrie interne, afin de contrôler l’utilisation générale de la nouvelle fonction d’exécution de script externe fournie dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. La télémétrie démarre au lancement de LaunchPad et incrémente un compteur sur disque à chaque fois qu’une fonction enregistrée de script externe est appelée.

En règle générale, les compteurs de performance demeurent valides tant que le processus qui les a générés reste actif. Par conséquent, une demande sur une vue de gestion dynamique ne peut pas faire état des données des services qui ne sont plus en cours d’exécution. Par exemple, si un lanceur exécute un script externe et les complète très rapidement, une DMV conventionnelle peut ne pas afficher de données.

Par conséquent, les compteurs suivis par cette vue de gestion dynamique sont conservés en cours d’exécution, tandis que l’état pour sys.dm_external_script_requests est préservé par l’utilisation d’écritures sur le disque, même si l’instance est arrêtée.

### <a name="counter-values"></a>Valeurs de compteur

Dans SQL Server 2016, le seul langage externe pris en charge est R et les requêtes de script externe sont gérées par [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] . Dans SQL Server 2017 et versions ultérieures, et sur Azure SQL Managed Instance, R et Python sont des langages externes pris en charge et les demandes de script externe sont gérées par [!INCLUDE[rsql_productname_md](../../includes/rsql-productnamenew-md.md)] .

Pour R, cette vue de gestion dynamique (DMV) suit le nombre d’appels R effectués sur une instance. Par exemple, si `rxLinMod` est appelé et s’exécute en parallèle, le compteur est incrémenté d’une unité.

Pour le langage R, les valeurs de compteur affichées dans le champ *counter_name* représentent le nom des fonctions ScaleR enregistrées. Les valeurs du champ *counter_value* représentent le nombre cumulé des instances de fonctions spécifiques ScaleR. 

Pour Python, cette vue de gestion dynamique (DMV) suit le nombre d’appels python effectués sur une instance.

Le comptage commence quand la fonction est installée et activée sur l’instance. Il est cumulé jusqu’à ce que le fichier qui gère l’état soit supprimé ou remplacé par un administrateur. Par conséquent, il n’est généralement pas possible de réinitialiser les valeurs de *counter_value*. Si vous souhaitez contrôler l’utilisation par session, période calendaire ou tout autre intervalle, nous vous recommandons de capturer les nombres dans un tableau.

### <a name="registration-of-external-script-functions-in-r"></a>Inscription de fonctions de script externe dans R

R prend en charge des scripts arbitraires, et la communauté R fournit plusieurs milliers de packages, chacun avec ses propres fonctions et méthodes. Toutefois, cette vue de gestion dynamique surveille uniquement les fonctions scaler qui sont installées avec SQL Server 2016 R services.

L’inscription de ces fonctions est effectuée lors de leur installation ; les fonctions inscrites ne peuvent pas être ajoutées ou supprimées.

## <a name="examples"></a>Exemples  
  
### <a name="viewing-the-number-of-r-scripts-run-on-the-server"></a>Affichage du nombre de scripts R exécutés sur le serveur

 L’exemple suivant affiche le nombre cumulé d’exécutions du script externe pour le langage R.  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'R';
```  

### <a name="viewing-the-number-of-python-scripts-run-on-the-server"></a>Affichage du nombre de scripts Python exécutés sur le serveur

L’exemple suivant affiche le nombre cumulé d’exécutions de script externes pour le langage Python.  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'Python';
```  

## <a name="see-also"></a>Voir aussi

+ [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [Fonctions et vues de gestion dynamique relatives aux exécutions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  
