---
title: Réglage automatique | Microsoft Docs
description: En savoir plus sur le paramétrage automatique dans SQL Server et Azure SQL Database
ms.custom: fasttrack-edit
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- performance tuning [SQL Server]
- automatic tuning
- aprc
- automatic plan regression correction
- last known good plan
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9c01a9e02576d666c39df13dc6e7e01f6d622a7d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169871"
---
# <a name="automatic-tuning"></a>Réglage automatique
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

L’optimisation automatique est une fonctionnalité de base de données qui fournit des insights sur les éventuels problèmes de performances des requêtes, recommande des solutions et corrige automatiquement les problèmes identifiés.

Le paramétrage automatique, introduit dans [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] , vous avertit chaque fois qu’un problème de performances potentiel est détecté et vous permet d’appliquer des actions correctives ou de [!INCLUDE[ssde_md](../../includes/ssde_md.md)] résoudre automatiquement les problèmes de performances. Le réglage automatique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vous permet d’identifier et de résoudre les problèmes de performances provoqués par les **régressions de choix de plan d’exécution de requête**. Le paramétrage automatique de [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] crée également des index nécessaires et supprime les index inutilisés. Pour plus d’informations sur les plans d’exécution de requête, consultez [plans d’exécution](../../relational-databases/performance/execution-plans.md).

[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Surveille les requêtes exécutées sur la base de données et améliore automatiquement les performances de la charge de travail. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]Dispose d’un mécanisme d’intelligence intégré qui peut automatiquement ajuster et améliorer les performances de vos requêtes en adaptant de manière dynamique la base de données à votre charge de travail. Deux fonctionnalités de réglage automatique sont disponibles :

-   La **Correction de plan automatique** identifie les plans d’exécution de requêtes problématiques, tels que les problèmes de [sensibilité des paramètres ou de détection des paramètres](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) , et corrige les problèmes de performances liés au plan d’exécution des requêtes en forçant le dernier plan correct connu avant la régression. **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

-   La **gestion automatique des index** identifie les index qui doivent être ajoutés à votre base de données, ainsi que les index qui doivent être supprimés. **S’applique à** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

## <a name="why-automatic-tuning"></a>À quoi sert le réglage automatique ?

Trois des tâches principales de l’administration de base de données classique sont la surveillance de la charge de travail, l’identification des [!INCLUDE[tsql_md](../../includes/tsql-md.md)] requêtes critiques et l’identification des index qui doivent être ajoutés pour améliorer les performances, ou les index rarement utilisés et pouvant être supprimés pour améliorer les performances. Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] fournit des informations détaillées sur les requêtes et les index que vous devez surveiller. Toutefois, la surveillance permanente d’une base de données est une tâche difficile et fastidieuse, en particulier lors du traitement de plusieurs bases de données. Il peut être impossible de gérer efficacement un grand nombre de bases de données. Au lieu de surveiller et de paramétrer manuellement votre base de données, vous pouvez envisager de déléguer certaines actions de surveillance et de paramétrage à la [!INCLUDE[ssde_md](../../includes/ssde_md.md)] fonctionnalité à l’aide du réglage automatique.

### <a name="how-does-automatic-tuning-work"></a>Comment fonctionne le réglage automatique ?

Le réglage automatique est un processus de surveillance et d’analyse continu qui apprend constamment sur les caractéristiques de votre charge de travail et identifie les problèmes potentiels et les améliorations.

![Processus de réglage automatique](./media/tuning-process.png)

Ce processus permet à la base de données de s’adapter de manière dynamique à votre charge de travail en recherchant les index et les plans susceptibles d’améliorer les performances de vos charges de travail et les index qui affectent vos charges de travail. En fonction de ces résultats, le paramétrage automatique applique des actions de paramétrage qui améliorent les performances de votre charge de travail. En outre, le réglage automatique surveille en permanence les performances de la base de données après avoir implémenté des modifications pour s’assurer qu’elle améliore les performances de votre charge de travail. Toute action qui n’a pas amélioré les performances est automatiquement rétablie. Ce processus de vérification est une fonctionnalité clé qui garantit que les modifications apportées par le réglage automatique ne diminuent pas les performances globales de votre charge de travail.

## <a name="automatic-plan-correction"></a>Correction de plan automatique

La correction de plan automatique est une fonctionnalité de réglage automatique qui identifie la **régression de choix de plan d’exécution** et résout automatiquement le problème en forçant le dernier plan correct connu. Pour plus d’informations sur les plans d’exécution de requête et l’optimiseur de requête, consultez le [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md).

> [!IMPORTANT]
> La correction automatique des plans dépend de la Magasin des requêtes être activée dans la base de données pour le suivi de la charge de travail. 

### <a name="what-is-execution-plan-choice-regression"></a>Qu’est-ce que la régression de choix de plan d’exécution ?

[!INCLUDE[ssdenoversion_md](../../includes/ssdenoversion_md.md)]Peut utiliser différents plans d’exécution pour exécuter les [!INCLUDE[tsql_md](../../includes/tsql-md.md)] requêtes. Les plans de requête dépendent des statistiques, des index et d’autres facteurs. Le plan optimal à utiliser pour exécuter une [!INCLUDE[tsql_md](../../includes/tsql-md.md)] requête peut changer au fil du temps en fonction des modifications apportées à ces facteurs. Dans certains cas, le nouveau plan peut ne pas être mieux que le précédent, et le nouveau plan peut entraîner une régression des performances, telle qu’un problème de [sensibilité de paramètre ou](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) un problème lié à la détection de paramètres. 

 ![Régression de choix du plan d’exécution de requête](media/plan-choice-regression.png "Régression de choix du plan d’exécution de requête") 

Chaque fois que vous remarquez qu’une régression de choix de plan s’est produite, vous devez trouver un plan correct précédent et le forcer à être utilisé à la place de celui en cours. Cette opération peut être effectuée à l’aide de la `sp_query_store_force_plan` procédure. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]Dans [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] fournit des informations sur les plans régressés et les actions correctives recommandées. En outre, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] vous permet d’automatiser entièrement ce processus et de [!INCLUDE[ssde_md](../../includes/ssde_md.md)] corriger tout problème trouvé en rapport avec le changement de plan.

> [!IMPORTANT]
> La correction automatique du plan doit être utilisée dans l’étendue d’une mise à niveau de niveau de compatibilité de base de données, une fois qu’une ligne de base a été capturée, afin d’atténuer automatiquement les risques de mise à niveau de la charge Pour plus d’informations sur ce cas d’usage, consultez [maintenir la stabilité des performances lors de la mise à niveau vers une SQL Server plus récente](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). 

### <a name="automatic-plan-choice-correction"></a>Correction automatique du choix de plan

Le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] peut automatiquement basculer vers le dernier plan correct connu chaque fois qu’une régression de choix de plan est détectée.

![Correction du choix du plan d’exécution de requête](media/force-last-good-plan.png "Correction du choix du plan d’exécution de requête") 

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]Détecte automatiquement toute régression de choix de plan potentielle, y compris le plan qui doit être utilisé à la place du plan incorrect. Lorsque le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] applique le dernier plan correct connu avant la régression, il surveille automatiquement les performances du plan forcé. Si le plan forcé n’est pas mieux que le plan régressé, le nouveau plan est non forcé et le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] compile un nouveau plan. Si le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] vérifie que le plan forcé est mieux que le plan régressé, le plan forcé est conservé. Elle est conservée jusqu’à ce qu’une recompilation se produise (par exemple, lors de la prochaine mise à jour des statistiques ou modification du schéma). Pour plus d’informations sur le forçage du plan et les types de plans qui peuvent être forcés, consultez [limitations de forçage de plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md#plan-forcing-limitations).

> [!NOTE]
> Si l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance est redémarrée avant qu’une action de forçage de plan soit vérifiée, ce plan sera automatiquement non forcé. Dans le cas contraire, le forçage de plan est conservé lors des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] redémarrages.

### <a name="enabling-automatic-plan-choice-correction"></a>Activation de la correction automatique du choix de plan

Vous pouvez activer le paramétrage automatique par base de données et spécifier que le dernier plan correct doit être forcé chaque fois qu’une régression de modification de plan est détectée. Pour cela, utilisez la commande suivante :

```sql   
ALTER DATABASE <yourDatabase>
SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON ); 
```

Une fois que vous activez cette option, le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] force automatiquement toute recommandation dans laquelle le gain de processeur estimé est supérieur à 10 secondes, ou le nombre d’erreurs dans le nouveau plan est supérieur au nombre d’erreurs dans le plan recommandé, et vérifie que le plan forcé est mieux que le plan actuel.

### <a name="alternative---manual-plan-choice-correction"></a>Autre possibilité : la correction manuelle du choix de plan

Sans réglage automatique, les utilisateurs doivent régulièrement surveiller le système et rechercher les requêtes qui ont régressé. Si un plan a fait l’objet d’une régression, l’utilisateur doit trouver un plan correct précédent et le forcer à la place de celui en cours à l’aide de la `sp_query_store_force_plan` procédure. La meilleure pratique consiste à forcer le dernier plan correct connu car des plans plus anciens peuvent ne pas être valides en raison de modifications de statistiques ou d’index. L’utilisateur qui force le dernier plan correct connu doit surveiller les performances de la requête exécutée à l’aide du plan forcé et vérifier que le plan forcé fonctionne comme prévu. En fonction des résultats de la surveillance et de l’analyse, le plan doit être forcé ou l’utilisateur doit trouver une autre façon d’optimiser la requête, telle que la réécriture. Les plans forcés manuellement ne doivent pas être indéfiniment forcés, car l' [!INCLUDE[ssde_md](../../includes/ssde_md.md)] application doit être en mesure d’appliquer des plans optimaux. L’utilisateur ou l’administrateur de bases de droits doit finalement annuler l’application du plan à l’aide de la `sp_query_store_unforce_plan` procédure et laisser le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] plan optimal. 

> [!TIP]
> Vous pouvez également utiliser les **requêtes avec des plans forcés** magasin des requêtes vue pour rechercher et annuler les plans.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit toutes les vues et procédures nécessaires pour analyser les performances et résoudre les problèmes dans Magasin des requêtes.

Dans [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] , vous pouvez rechercher des régressions de choix de plan à l’aide d’magasin des requêtes vues système. À compter de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] , le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] détecte et affiche les régressions de choix de plan potentielles et les actions recommandées qui doivent être appliquées dans le [sys.dm_db_tuning_recommendations &#40;DMV Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) . La DMV affiche des informations sur le problème, l’importance du problème et des détails tels que la requête identifiée, l’ID du plan régressé, l’ID du plan utilisé comme ligne de base pour la comparaison et l' [!INCLUDE[tsql_md](../../includes/tsql-md.md)] instruction qui peut être exécutée pour résoudre le problème.

| type | description | DATETIME | score | details | ... |
| --- | --- | --- | --- | --- | --- |
| `FORCE_LAST_GOOD_PLAN` | Temps processeur passé de 4 ms à 14 ms | 3/17/2017 | 83 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |
| `FORCE_LAST_GOOD_PLAN` | Temps processeur passé de 37 ms à 84 ms | 3/16/2017 | 26 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |

Certaines colonnes de cette vue sont décrites dans la liste suivante :
 - Type de l’action recommandée `FORCE_LAST_GOOD_PLAN` .
 - Description qui contient des informations expliquant pourquoi [!INCLUDE[ssde_md](../../includes/ssde_md.md)] ce changement de plan est une régression potentielle des performances.
 - Date et heure de détection de la régression potentielle.
 - Score de cette recommandation.
 - Détails sur les problèmes tels que l’ID du plan détecté, l’ID du plan régressé, l’ID du plan qui doit être forcé à résoudre le problème, le [!INCLUDE[tsql_md](../../includes/tsql-md.md)] script qui peut être appliqué pour résoudre le problème, etc. Les détails sont stockés au [format JSON](../json/json-data-sql-server.md).

Utilisez la requête suivante pour obtenir un script qui résout le problème et des informations supplémentaires sur le gain estimé :

```sql   
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressedPlanId int '$.regressedPlanId',
            recommendedPlanId int '$.recommendedPlanId',
            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,
            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float
          ) AS planForceDetails;
```

[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]     

| reason | score | script | ID de requête \_ | ID de plan actuel \_ | ID de plan recommandé \_ | \_gain estimé | risque d’erreur \_
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Temps processeur passé de 3 ms à 46 ms | 36 | EXEC SP \_ query \_ Store \_ force \_ plan 12, 17 ; | 12 | 28 | 17 | 11,59 | 0

La colonne `estimated_gain` représente le nombre estimé de secondes qui seraient enregistrées si le plan recommandé est utilisé pour l’exécution de la requête au lieu du plan actuel. Le plan recommandé doit être forcé au lieu du plan actuel si le gain est supérieur à 10 secondes. S’il y a plus d’erreurs (par exemple, de délais d’attente ou d’exécutions abandonnées) dans le plan actuel que dans le plan recommandé, la colonne `error_prone` est définie sur la valeur `YES` . Un plan sujette aux erreurs est une autre raison pour laquelle le plan recommandé doit être forcé à la place de l’offre actuelle.

Bien que le [!INCLUDE[ssde_md](../../includes/ssde_md.md)] fournit toutes les informations nécessaires pour identifier les régressions de choix de plan, la surveillance continue et la résolution des problèmes de performances peuvent devenir un processus fastidieux. Le réglage automatique facilite grandement ce processus.

> [!NOTE]
> Les données de la `sys.dm_db_tuning_recommendations` DMV ne sont pas conservées entre les redémarrages de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.

## <a name="automatic-index-management"></a>Gestion automatique des index

Dans [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] , la gestion des index est simple, car elle [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] apprend sur votre charge de travail et garantit que vos données sont toujours indexées de manière optimale. Pour assurer des performances optimales de votre charge de travail, l’index doit être correctement conçu. La gestion automatique des index peut vous aider à les optimiser. La gestion automatique des index permet de résoudre les problèmes de performances dans les bases de données indexées de manière incorrecte, ou de conserver et d’améliorer les index dans la structure de base de données existante. Le paramétrage automatique de [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] effectue les actions suivantes :

 - Identifie les index qui peuvent améliorer les performances de vos [!INCLUDE[tsql_md](../../includes/tsql-md.md)] requêtes qui lisent les données des tables.
 - Identifie les index ou index redondants qui n’ont pas été utilisés pendant une période plus longue qui pourrait être supprimée. La suppression des index inutiles améliore les performances des requêtes qui mettent à jour les données dans les tables.

### <a name="why-do-you-need-index-management"></a>Pourquoi utiliser la gestion des index ?

Les index accélèrent certaines de vos requêtes qui lisent les données des tables, mais ils peuvent ralentir les requêtes qui mettent à jour les données. Vous devez analyser soigneusement quand créer un index et les colonnes que vous devez inclure dans l’index. Certains index peuvent ne plus être nécessaires après un certain temps. Par conséquent, vous devez identifier et supprimer régulièrement ces index qui n’apportent aucun avantage. Si vous ignorez les index inutilisés, les performances des requêtes qui mettent à jour les données seraient réduites sans tirer parti des requêtes qui lisent les données. Les index inutilisés affectent également les performances globales du système, car les mises à jour supplémentaires impliquent une journalisation inutile.

Pour trouver l’ensemble optimal d’index qui améliore les performances des requêtes qui lisent les données de vos tables et ont un impact minimal sur les mises à jour, vous aurez peut-être besoin d’une analyse continue complexe.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] utilise l’intelligence intégrée et des règles avancées qui analysent vos requêtes, identifient les index qui seraient optimaux pour vos charges de travail actuelles et identifient les index qui devront peut-être être supprimés. Azure SQL Database garantit que vous disposez d’un ensemble minimal d’index qui optimisent les requêtes qui lisent les données, avec un impact minimal sur les autres requêtes.

### <a name="automatic-index-management"></a>Gestion automatique des index

En plus de la détection, [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] peut appliquer automatiquement les recommandations identifiées. Si vous constatez que les règles intégrées améliorent les performances de votre base de données, vous pouvez autoriser la [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] gestion automatique de vos index.

Pour activer le réglage automatique dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et permettre à la fonctionnalité de réglage automatique de gérer entièrement votre charge de travail, consultez [activer le réglage automatique dans Azure SQL Database à l’aide de portail Azure](/azure/sql-database/sql-database-automatic-tuning-enable).

Lorsque le [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] applique une recommandation créer un index ou Drop Index, il surveille automatiquement les performances des requêtes qui sont affectées par l’index. Le nouvel index sera conservé uniquement si les performances des requêtes affectées sont améliorées. L’index supprimé sera automatiquement recréé s’il y a des requêtes qui s’exécutent plus lentement en raison de l’absence de l’index.

### <a name="automatic-index-management-considerations"></a>Considérations sur la gestion automatique des index

Les actions requises pour créer des index nécessaires dans [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] peuvent consommer des ressources et affecter temporellement les performances de la charge de travail. Pour réduire l’impact de la création d’index sur les performances de la charge de travail, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  recherche une fenêtre de temps appropriée pour toute opération de gestion d’index. L’action de paramétrage est reportée si la base de données a besoin de ressources pour exécuter votre charge de travail et qu’elle est redémarrée lorsque la base de données dispose de suffisamment de ressources inutilisées pouvant être utilisées pour la tâche de maintenance. Une fonctionnalité importante dans la gestion automatique des index est la vérification des actions. Lorsque [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  crée ou supprime un index, un processus d’analyse analyse les performances de votre charge de travail pour vérifier que l’action a amélioré les performances globales. S’il n’a pas été amélioré, l’action est immédiatement annulée. Ainsi, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  les actions de réglage automatique n’ont pas d’impact négatif sur les performances de votre charge de travail. Les index créés par le réglage automatique sont transparents pour l’opération de maintenance sur le schéma sous-jacent. Les modifications du schéma, par exemple le fait de supprimer ou de renommer des colonnes, ne sont pas bloquées par la présence d’index créés automatiquement. Les index créés automatiquement par [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  sont supprimés immédiatement lorsque la ou les colonnes associées sont supprimées.

### <a name="alternative---manual-index-management"></a>Alternative-gestion manuelle des index

Sans la gestion automatique des index, un utilisateur ou un administrateur de bases de données doit interroger manuellement l' [sys.dm_db_missing_index_details &#40;la vue Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) ou utiliser le rapport du tableau de bord des performances dans [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] pour rechercher des index susceptibles d’améliorer les performances, créer des index à l’aide des détails fournis dans cette vue et surveiller manuellement les performances de la requête. Pour rechercher les index qui doivent être supprimés, les utilisateurs doivent surveiller les statistiques d’utilisation opérationnelle des index pour trouver des index rarement utilisés.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] simplifie ce processus. [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] analyse votre charge de travail, identifie les requêtes qui peuvent être exécutées plus rapidement avec un nouvel index et identifie les index inutilisés ou en double. Pour plus d’informations sur l’identification des index à modifier, voir [Rechercher des recommandations d’index dans le portail Azure](/azure/sql-database/sql-database-advisor-portal).

## <a name="see-also"></a>Voir aussi  
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_automatic_tuning_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)  
 [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)     
 [sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)           
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [Fonctions JSON](../json/json-data-sql-server.md)    
 [Plans d’exécution](../../relational-databases/performance/execution-plans.md)    
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Outils de surveillance et d’optimisation des performances](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Analyse des performances à l’aide du magasin de requêtes](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
 [Assistant Paramétrage de requêtes](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)