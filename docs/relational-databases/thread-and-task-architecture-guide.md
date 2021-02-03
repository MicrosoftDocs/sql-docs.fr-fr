---
title: Guide d’architecture de thread et de tâche | Microsoft Docs
description: En savoir plus sur l’architecture des threads et des tâches dans SQL Server, notamment la planification des tâches, l’ajout à chaud du processeur et les meilleures pratiques pour l’utilisation d’ordinateurs avec plus de 64 UC.
ms.custom: ''
ms.date: 09/23/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, thread and task architecture
- thread and task architecture guide
- task scheduling
- working threads
- Large Deficit First scheduling
- LDF scheduling
- scheduling, SQL Server
- tasks, SQL Server
- threads, SQL Server
- quantum, SQL Server
ms.assetid: 925b42e0-c5ea-4829-8ece-a53c6cddad3b
author: pmasl
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3eb9fd5897029fedb0e53378ecbe5f6c12f635ef
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237656"
---
# <a name="thread-and-task-architecture-guide"></a>guide d’architecture de thread et de tâche
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]

## <a name="operating-system-task-scheduling"></a>Planification des tâches du système d'exploitation
Les threads sont les plus petites unités de traitement pouvant être exécutées par un système d'exploitation. Ils permettent de séparer la logique d’application en plusieurs chemins d’exécution parallèle. Les threads sont utiles lorsque des applications complexes comprennent un grand nombre de tâches à exécuter simultanément. 

Lorsqu'un système d'exploitation exécute une instance d'application, il crée une unité appelée processus pour gérer cette instance. Ce processus est associé à un thread d'exécution. Il s'agit de la série d'instructions de programmation exécutées par le code de l'application. Par exemple, si une application simple comprend un seul jeu d'instructions pouvant être exécutées en série, ce jeu d’instructions est traité comme une **tâche** unique et il n'existe qu'un seul chemin d'exécution, ou **thread**, dans toute l'application. Il est possible que les applications plus complexes aient plusieurs **tâches** pouvant être exécutées simultanément, mais pas en série. Une application peut le faire en démarrant des processus séparés pour chaque tâche, ce qui est une opération nécessitant de nombreuses ressources, ou en démarrant des threads séparés, ce qui nécessite relativement moins de ressources. De plus, l'exécution de chaque thread peut être programmée indépendamment des autres threads du même processus.

Les threads permettent aux applications complexes d'utiliser plus efficacement un processeur (UC), même sur des ordinateurs dotés d'une seule UC. Avec une UC, vous ne pouvez exécuter qu'un seul thread à la fois. Si un thread exécute une longue opération qui n'utilise pas l'UC, comme par exemple la lecture ou l'écriture sur le disque, un autre thread peut être exécuté avant la fin de la première opération. La possibilité d'exécuter des threads pendant que d'autres attendent la fin d'une opération permet à l'application d'optimiser l'utilisation de l'UC. Cela est particulièrement vrai pour les applications multi-utilisateurs qui utilisent beaucoup d'E/S disque, par exemple un serveur de bases de données. Les ordinateurs dotés de plusieurs UC peuvent exécuter simultanément un thread par UC. Ainsi, si un ordinateur comprend huit UC, il peut exécuter huit threads en même temps.

## <a name="sql-server-task-scheduling"></a>Planification de tâches SQL Server
Dans l’étendue de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], une **requête** est la représentation logique d’une requête ou d’un lot. Une requête représente également des opérations requises par des threads de système, telles qu’un point de contrôle ou un enregistreur de journal. Les requêtes existent dans différents états pendant toute leur durée de vie et peuvent accumuler les attentes lorsque les ressources requises pour exécuter la requête, par exemple des [verrouiller](../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md#locks) ou [verrous](../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md#latches), ne sont pas disponibles. Pour plus d’informations sur les états des requêtes, consultez [sys.dm_exec_requests](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).

Une **tâche** représente l’unité de travail à effectuer pour exécuter la requête. Une ou plusieurs tâches peuvent être attribuées à une seule requête. 
-  Les requêtes parallèles comporteront plusieurs tâches actives qui sont exécutées simultanément plutôt qu’en série, avec une **tâche parente** (ou une tâche qui coordonne) et plusieurs **tâches enfants**. Un plan d'exécution pour une requête parallèle peut avoir des branches en série - des zones du plan avec des opérateurs qui n'exécutent pas en parallèle. La tâche parente est également chargée de l’exécution de ces opérateurs en série.
-  Les requêtes en série n'auront qu'une seule tâche active à un moment donné de leur exécution.     
Les tâches existent dans différents états pendant toute leur durée de vie. Pour plus d’informations sur les états des tâches, consultez [sys.dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Les tâches dans l’état SUSPENDED attendent que les ressources requises pour exécuter la tâche soient disponibles. Pour plus d’informations sur les tâches en attente, consultez [sys. dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md).

Un **thread de travail** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], également appelé Worker ou thread, est une représentation logique d’un thread de système d’exploitation. Lors de l’exécution de **requêtes en série**, la [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] génère un Worker pour exécuter la tâche active (1:1). Lors de l’exécution de **requêtes parallèles** en [mode en ligne](../relational-databases/query-processing-architecture-guide.md#execution-modes), le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] affecte un worker pour coordonner les travailleurs enfants chargés de l’exécution des tâches qui leur sont attribuées (1:1), appelées **le thread parent** (ou coordonner le thread). Une tâche parente est associée à un thread parent. Le thread parent est le point d’entrée de la demande et existe même avant que le moteur n’analyse une requête. Les principales responsabilités du thread parent sont les suivantes : 
-  Coordonner une analyse parallèle.
-  Démarrer les threads de travail parallèles enfants.
-  Collecter les lignes des threads parallèles et les envoyer au client.
-  Effectuer des agrégations locales et globales.    

> [!NOTE]
> Si un plan de requête a des branches en série et parallèles, l’une des tâches parallèles est chargée de l’exécution de la branche en série. 

Le nombre de threads de travail générés pour chaque tâche dépend de ce qui suit :
-   Si la requête était éligible pour le parallélisme comme déterminé par l’optimiseur de requête.
-   Quel est le [degré de parallélisme (DOP)](../relational-databases/query-processing-architecture-guide.md#DOP) effectif disponible dans le système en fonction de la charge actuelle. Cela peut diverger du degré de parallélisme estimé, qui est basé sur la configuration du serveur pour le degré maximal de parallélisme (MAXDOP). Par exemple, la configuration du serveur pour MAXDOP peut être 8, mais le DOP disponible au moment de l’exécution peut être de 2 seulement, ce qui affecte les performances des requêtes. 

> [!NOTE]
> La limite du **degré maximal de parallélisme (MAXDOP)** est spécifiée par tâche, pas par requête. Cela signifie que lors d’une exécution de requête parallèle, une seule requête peut générer plusieurs tâches jusqu’à la limite de MAXDOP et que chaque tâche utilisera un worker. Pour plus d’informations sur MAXDOP, consultez [Configurer l’option de configuration serveur du degré maximal de parallélisme](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

Un **planificateur**, également appelé planificateur SOS, gère les threads de travail nécessitant un temps de traitement pour effectuer le travail résultant de tâches. Chaque planificateur est mappé à un processeur (UC) individuel. La durée pendant laquelle un Worker peut rester actif dans un planificateur est appelée quantum de système d’exploitation, avec un maximum de 4 ms. Une fois que son temps de quantum a expiré, un thread de travail consacre son temps à d’autres threads ayant besoin d’accéder aux ressources de l’UC et change d’état. Cette coopération entre les Workers pour optimiser l’accès aux ressources de l’UC est appelée **planification coopérative**, également connue sous le nom de planification non préemptive. À son tour, la modification de l’état du Worker est propagée à la tâche associée à ce Worker ainsi qu’à la requête associée à la tâche. Pour plus d’informations sur les états des Workers, consultez [sys.dm_os_workers](../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md). Pour plus d’informations sur les planificateurs, consultez [sys.dm_os_schedulers](../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). 

En résumé, une **requête** peut générer une ou plusieurs **tâches** pour effectuer des unités de travail. Chaque tâche est assignée à un **thread de travail** qui est responsable de l’exécution de la tâche. Chaque thread de travail doit être planifié (placé sur un **planificateur**) pour l’exécution active de la tâche. 

> [!NOTE]
> Examinez le cas suivant :   
> -  Worker 1 est une tâche de longue durée, par exemple une requête Read qui utilise des tables en lecture anticipée sur des tables en mémoire. Worker 1 trouve les pages de données requises qui se trouvent déjà dans le pool de mémoires tampons. Il n’est donc pas nécessaire d’attendre les opérations d’E/S et peut consommer son quantum complet avant de générer.   
> -  Worker 2 exécute des tâches subordonnées plus courtes et, par conséquent, doit être généré avant l’épuisement de son quantum complet.     
>
> Dans ce scénario et jusqu’à [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], Worker 1 est autorisé à monopoliser le planificateur en faisant plus de temps quantum global.   
> À partir de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], la planification coopérative prend en compte la planification du déficit volumineux (LDF). Avec la planification LDF, les modèles d’utilisation quantum sont surveillés et un thread de travail ne monopolise pas un planificateur. Dans le même scénario, Worker 2 est autorisé à consommer des quantums répétitifs avant que Worker 1 ne soit autorisé à plus de quantum, ce qui empêche Worker 1 de monopoliser le planificateur dans un modèle peu convivial.

### <a name="scheduling-parallel-tasks"></a>Planification de tâches parallèles
Imaginez une [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] configurée avec MaxDOP 8, et l’affinité du processeur est configurée pour 24 UC (planificateurs) sur les nœuds NUMA 0 et 1. Les planificateurs de 0 à 11 appartiennent au nœud NUMA 0, les planificateurs de 12 à 23 appartiennent au nœud NUMA 1. Une application envoie la requête suivante (request) au [!INCLUDE[ssde_md](../includes/ssde_md.md)] :

```sql
SELECT h.SalesOrderID, h.OrderDate, h.DueDate, h.ShipDate
FROM Sales.SalesOrderHeaderBulk AS h 
INNER JOIN Sales.SalesOrderDetailBulk AS d ON h.SalesOrderID = d.SalesOrderID 
WHERE (h.OrderDate >= '2014-3-28 00:00:00');
```

> [!TIP]
> L’exemple de requête peut être exécuté à l’aide de la base de données [AdventureWorks2016_EXT sample database](../samples/adventureworks-install-configure.md). Les tables `Sales.SalesOrderHeader` et `Sales.SalesOrderDetail` ont été augmentées 50 fois et renommées en `Sales.SalesOrderHeaderBulk` et `Sales.SalesOrderDetailBulk`.

Le plan d’exécution affiche une [jointure de hachage](../relational-databases/performance/joins.md#hash) entre deux tables, et chacun des opérateurs exécutés en parallèle, comme indiqué par le cercle jaune avec deux flèches. Chaque opérateur de parallélisme est une branche différente dans le plan. Par conséquent, il existe trois branches dans le plan d’exécution ci-dessous. 

![Plan de requête en parallèle](../relational-databases/media/schedule-parallel-query-plan.png)

> [!NOTE]
> Si vous visualisez un plan d’exécution comme une arborescence, une **branche** est une zone du plan qui regroupe un ou plusieurs opérateurs entre les opérateurs de parallélisme, également appelés itérateurs d’échange. Pour plus d’informations sur les opérateurs de plan, consultez le [Guide de référence des opérateurs Showplan logiques et physiques](../relational-databases/showplan-logical-and-physical-operators-reference.md). 

Bien qu’il y ait trois branches dans le plan d’exécution, à tout moment pendant l’exécution, seules deux branches peuvent s’exécuter simultanément dans ce plan d’exécution :
1.  La branche dans laquelle une *analyse d’index cluster* est utilisée sur `Sales.SalesOrderHeaderBulk` (entrée de build de la jointure) s’exécute seule.
2.  Ensuite, la branche dans laquelle une *Analyse d’index cluster*  est utilisée sur le `Sales.SalesOrderDetailBulk` (entrée build de la jointure) s’exécute simultanément avec la branche dans laquelle le *Bitmap* a été créé et où le *Hash Match* s’exécute.

Le plan d'exécution de requêtes XML montre que 16 threads de travail ont été réservés et utilisés sur le nœud NUMA 0 :

```xml
<ThreadStat Branches="2" UsedThreads="16">
  <ThreadReservation NodeId="0" ReservedThreads="16" />
</ThreadStat>
```

La réservation de thread garantit que le [!INCLUDE[ssde_md](../includes/ssde_md.md)] dispose de suffisamment de threads de travail pour effectuer toutes les tâches qui seront nécessaires pour la requête. Les threads peuvent être réservés sur plusieurs nœuds NUMA ou être réservés dans un seul nœud NUMA. La réservation de thread est effectuée au moment de l’exécution avant le démarrage de l’exécution, et dépend de la charge du planificateur. Le nombre de threads de travail réservés est dérivé de façon générique à partir de la formule ***d’exécution DOP* * *parallélisme de Runtime*** et exclut le thread de travail parent. Chaque branche est limitée à un nombre de threads de travail égaux à MaxDOP. Dans cet exemple, il existe deux branches simultanées et MaxDOP a pour valeur 8, par conséquent **2 * 8 = 16**.

Pour référence, observez le plan d’exécution en direct à partir de [Statistiques des Requêtes en direct](../relational-databases/performance/live-query-statistics.md), où une branche est terminée et que deux branches s’exécutent simultanément.

![Plan de requête parallèle en direct](../relational-databases/media/schedule-parallel-query-live-plan.png)

L' [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] assignera un thread de travail pour exécuter une tâche active (1:1), qui peut être observée pendant l’exécution de la requête en interrogeant la DMV [sys. dm_os_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md), comme indiqué dans l’exemple suivant :

```sql
SELECT parent_task_address, task_address, 
       task_state, scheduler_id, worker_address
FROM sys.dm_os_tasks
WHERE session_id = <insert_session_id>
ORDER BY parent_task_address, scheduler_id;
```

> [!TIP]
> La colonne `parent_task_address` est toujours NULL pour la tâche parente. 

> [!TIP]
> Sur un [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]très occupé, il est possible de voir un certain nombre de tâches actives sur la limite définie par les threads réservés. Ces tâches peuvent appartenir à une branche qui n’est plus utilisée et qui sont dans un état transitoire, en attente de nettoyage. 

[!INCLUDE[ssResult](../includes/ssresult-md.md)] Notez qu’il y a 17 tâches actives pour les branches qui sont en cours d’exécution : 16 tâches enfants correspondant aux threads réservés, plus la tâche parente ou la tâche de coordination.

|parent_task_address|task_address|task_state|scheduler_id|worker_address|
|--------|--------|--------|--------|--------|
|NULL|**0x000001EF4758ACA8**|SUSPENDED|3|0x000001EFE6CB6160|
|0x000001EF4758ACA8|0x000001EFE43F3468|SUSPENDED|0|0x000001EF6DB70160|
|0x000001EF4758ACA8|0x000001EEB243A4E8|SUSPENDED|0|0x000001EF6DB7A160|
|0x000001EF4758ACA8|0x000001EC86251468|SUSPENDED|5|0x000001EEC05E8160|
|0x000001EF4758ACA8|0x000001EFE3023468|SUSPENDED|5|0x000001EF6B46A160|
|0x000001EF4758ACA8|0x000001EFE3AF1468|SUSPENDED|6|0x000001EF6BD38160|
|0x000001EF4758ACA8|0x000001EFE4AFCCA8|SUSPENDED|6|0x000001EF6ACB4160|
|0x000001EF4758ACA8|0x000001EFDE043848|SUSPENDED|7|0x000001EEA18C2160|
|0x000001EF4758ACA8|0x000001EF69038108|SUSPENDED|7|0x000001EF6AEBA160|
|0x000001EF4758ACA8|0x000001EFCFDD8CA8|SUSPENDED|8|0x000001EFCB6F0160|
|0x000001EF4758ACA8|0x000001EFCFDD88C8|SUSPENDED|8|0x000001EF6DC46160|
|0x000001EF4758ACA8|0x000001EFBCC54108|SUSPENDED|9|0x000001EFCB886160|
|0x000001EF4758ACA8|0x000001EC86279468|SUSPENDED|9|0x000001EF6DE08160|
|0x000001EF4758ACA8|0x000001EFDE901848|SUSPENDED|10|0x000001EFF56E0160|
|0x000001EF4758ACA8|0x000001EF6DB32108|SUSPENDED|10|0x000001EFCC3D0160|
|0x000001EF4758ACA8|0x000001EC8628D468|SUSPENDED|11|0x000001EFBFA4A160|
|0x000001EF4758ACA8|0x000001EFBD3A1C28|SUSPENDED|11|0x000001EF6BD72160|

Notez que chacune des 16 tâches enfants a un thread de travail différent affecté (vu dans la colonne `worker_address`), mais que tous les workers sont affectés au même pool de huit planificateurs (0, 5, 6, 7, 8, 9, 10, 11) et que la tâche parente est affectée à un planificateur en dehors de ce pool (3).

> [!IMPORTANT]
> Une fois le premier ensemble de tâches parallèles sur une branche donnée planifiée, le [!INCLUDE[ssde_md](../includes/ssde_md.md)] utilise ce même pool de planificateurs pour toutes les tâches supplémentaires sur d’autres branches. Cela signifie que le même ensemble de planificateurs sera utilisé pour toutes les tâches parallèles dans le plan d’exécution entier, limité uniquement par MaxDOP.  
> Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] essaiera toujours d’affecter des planificateurs du même nœud NUMA pour l’exécution des tâches et de les affecter de manière séquentielle (en mode tourniquet) si les planificateurs sont disponibles. Toutefois, le thread de travail affecté à la tâche parente peut être placé dans un autre nœud NUMA à partir d’autres tâches.

Un thread de travail ne peut rester actif dans le planificateur que pour la durée de son quantum (4 ms) et doit générer son planificateur après que ce quantum se soit écoulé, de sorte qu’un thread de travail affecté à une autre tâche peut devenir actif. Quand le quantum d’un worker arrive à expiration et qu’il n’est plus actif, la tâche correspondante est placée dans une file d’attente FIFO dans un état EXÉCUTABLE, en supposant que la tâche ne nécessitera pas l'accès à des ressources qui ne sont pas disponibles pour le moment, comme un verrou ou une serrure, auquel cas la tâche serait placée dans un état SUSPENDU au lieu d’EXÉCUTABLE, jusqu'à ce que ces ressources soient disponibles.  

> [!TIP] 
> Pour la sortie de la vue DMV indiquée ci-dessus, toutes les tâches actives sont en état SUSPENDU. Pour plus d’informations sur les tâches en attente, vous pouvez interroger la DMV [sys. dm_os_waiting_tasks](../relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql.md). 

En résumé, une demande parallèle génère plusieurs tâches. Chaque tâche doit être affectée à un seul thread de travail. Chaque thread de travail doit être affecté à un seul planificateur. Ainsi, le nombre de planificateurs en cours d’utilisation ne peut pas dépasser le nombre de tâches parallèles par branche, que définit l’indicateur de requête ou la configuration MaxDOP. Le thread de coordination ne contribue pas à la limite MaxDOP. 

### <a name="allocating-threads-to-a-cpu"></a>Allocation de threads à une UC
Par défaut, chaque instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] commence chaque thread, et le système d’exploitation répartit les threads à partir des instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] entre les processeurs (UC) sur un ordinateur en fonction de la charge. Si l'affinité de processus a été activée au niveau du système d'exploitation, ce dernier attribue chaque thread à une UC spécifique. En revanche, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] attribue des **threads de travail** [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]aux **planificateurs** qui distribuent les threads de manière équitable entre les processeurs en mode tourniquet (round robin).
    
Pour exécuter des travaux multitâches, par exemple lorsque plusieurs applications accèdent au même ensemble d’UC, le système d’exploitation déplace parfois les threads de travail entre les différentes UC. Même si cela est efficace du point de vue du système d'exploitation, cette activité peut réduire les performances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] du fait des charges système élevées, puisque chaque cache de processeur est rechargé par des données de façon répétée. L'affectation d’UC à des threads spécifiques permet d'améliorer les performances dans ces conditions en éliminant les rechargements de processeurs et en réduisant la migration des threads entre les UC (réduisant ainsi les changements de contexte) ; une telle association entre un thread et un processeur est appelée affinité du processeur. Si l'affinité a été activée, le système d'exploitation attribue chaque thread à une UC spécifique. 

L’[option de masque d’affinité](../database-engine/configure-windows/affinity-mask-server-configuration-option.md) est définie à l’aide d’[ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md). Lorsque le masque d'affinité n'est pas défini, l'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] alloue les threads de travail de manière équitable entre les planificateurs qui n'ont pas été masqués.

> [!CAUTION]
> Évitez de configurer l'affinité d’UC dans le système d'exploitation et le masque d'affinité dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Ces paramètres tentent d'obtenir le même résultat et, si les configurations sont incohérentes, les résultats risquent d'être imprévisibles. Pour plus d'informations, consultez l’[option de masque d'affinité](../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

Le regroupement de threads permet d'optimiser les performances lorsque de nombreux clients sont connectés au serveur. Habituellement, un thread de système d'exploitation séparé est créé pour chaque demande de requête. Cependant, s'il existe des centaines de connexions au serveur, l'utilisation d'un thread par demande de requête peut consommer de grandes quantités de ressources système. L'[option Nombre maximum de threads de travail](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) permet à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de créer un pool de threads de travail afin de servir un grand nombre de demandes de requête, ce qui améliore les performances. 

### <a name="using-the-lightweight-pooling-option"></a>Utilisation de l'option Regroupement léger
La surcharge qu’implique le basculement des contextes de thread peut ne pas être significative. La plupart des instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne perçoivent aucune différence de performances entre les valeurs 0 et 1 de l'option de regroupement léger. Les seules instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] susceptibles de tirer parti du [regroupement léger](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md) sont celles exécutées sur un ordinateur qui présente les caractéristiques suivantes :    
* Un serveur multiprocesseur de grande taille
* Exécution de toutes les UC près de leur capacité maximale
* Fréquence élevée de basculement des contextes

Ces systèmes peuvent augmenter légèrement leurs performances si la valeur 1 est attribuée à l’option Regroupement léger.

> [!IMPORTANT]
> N'utilisez pas la planification en mode fibre pour les opérations courantes. Cela peut réduire les performances en bloquant les avantages habituels du basculement de contexte. Par ailleurs, certains composants de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne peuvent pas fonctionner correctement en mode fibre. Pour plus d’informations, reportez-vous au [regroupement léger](../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

## <a name="thread-and-fiber-execution"></a>Exécution de threads et de fibres
Microsoft Windows utilise un système de priorité numérique qui va de 1 à 31 pour planifier l'exécution des threads. Le zéro est réservé pour le système d'exploitation. Lorsque plusieurs threads attendent d'être exécutés, Windows envoie le thread dont la priorité est la plus élevée.

Par défaut, chaque instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est une priorité 7 (priorité dite normale). Ceci octroie aux threads [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] une priorité suffisamment élevée pour obtenir les ressources UC dont ils ont besoin sans pénaliser les autres applications. 

> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../includes/ssnotedepfuturedontuse-md.md)]  

L'option de configuration [Renforcement de priorité](../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md) peut être utilisée pour augmenter la priorité des threads d'une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à 13. (priorité dite élevée). Cette valeur donne aux threads [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] une priorité plus élevée que la plupart des autres applications. Par conséquent, les threads [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] auront plutôt tendance à être distribués chaque fois qu'ils sont prêts à être exécutés et ne seront pas devancés par les threads d'autres applications. Ceci peut améliorer les performances lorsqu'un serveur n'exécute que des instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et aucune autre application. Cependant, si une opération nécessitant beaucoup de mémoire se produit dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], les autres applications n'auront sans doute pas une priorité suffisamment élevée pour devancer le thread [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

Si vous utilisez plusieurs instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur et activez le renforcement de priorité pour certaines d'entre elles uniquement, les performances des instances exécutées avec une priorité normale peuvent être compromises. Les performances d’autres applications et composants sur le serveur peuvent aussi diminuer si l’option Renforcement de priorité est activée. C'est pourquoi elle doit être utilisée dans certaines conditions bien définies.


## <a name="hot-add-cpu"></a>Ajout d’un processeur à chaud
L'ajout d'un processeur à chaud est la capacité d'ajouter dynamiquement des processeurs à un système en cours d'exécution. L'ajout de processeurs peut s'effectuer physiquement en ajoutant du matériel, logiquement en partitionnant du matériel en ligne ou virtuellement par l'intermédiaire d'une couche de virtualisation. À compter de [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prend en charge l'ajout de processeurs à chaud.

Spécifications pour l'ajout de processeurs à chaud :  
* Nécessite un matériel prenant en charge l'ajout de processeurs à chaud.
* Nécessite l'édition 64 bits de Windows Server 2008 Datacenter ou le système d'exploitation Windows Server 2008 Enterprise Edition pour systèmes basés sur Itanium.
* Exige [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise.
* [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne peut pas être configuré pour utiliser la configuration NUMA logicielle. Pour plus d’informations sur la configuration NUMA logicielle, consultez [Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md).

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne commence pas automatiquement à utiliser les processeurs une fois que ceux-ci ont été ajoutés. Cela empêche [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] d'utiliser des processeurs qui peuvent être ajoutés à d'autres fins. Après avoir ajouté des processeurs (UC), exécutez l'instruction [RECONFIGURE](../t-sql/language-elements/reconfigure-transact-sql.md) pour que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] reconnaisse les nouveaux processeurs comme des ressources disponibles.

> [!NOTE]
> Si le [masque d’affinité 64](../database-engine/configure-windows/affinity64-mask-server-configuration-option.md) est configuré, il doit être modifié pour utiliser les nouveaux processeurs.
 
## <a name="best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus"></a>Recommandations pour l'exécution de SQL Server sur des ordinateurs comportant plus de 64 unités centrales

### <a name="assigning-hardware-threads-with-cpus"></a>Affectation de threads matériels avec des unités centrales
N’utilisez pas les options de configuration de serveur masque d’affinité (affinity mask) et masque d’affinité 64 (affinity64 mask) pour lier les processeurs à des threads spécifiques. Ces options sont limitées à 64 unités centrales. Utilisez l’option `SET PROCESS AFFINITY` d’[ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md) à la place.

### <a name="managing-the-transaction-log-file-size"></a>Gestion de la taille du fichier journal des transactions
Ne comptez pas sur la croissance automatique pour augmenter la taille du fichier journal de transactions. L'augmentation du journal des transactions doit être un processus série. L’extension du journal peut empêcher la poursuite d’opérations d’écriture de transactions jusqu’à ce que l’extension du journal soit terminée. Pré-allouez plutôt l'espace des fichiers journaux en définissant leur taille avec une valeur suffisamment élevée pour prendre en charge la charge de travail habituelle de l'environnement.

### <a name="setting-max-degree-of-parallelism-for-index-operations"></a>Définition du degré maximal de parallélisme pour les opérations d'index
Les performances des opérations d'index telles que la création ou la reconstruction d'index peuvent être améliorées sur les ordinateurs qui possèdent de nombreuses unités centrales en définissant temporairement le mode de récupération de la base de données sur le mode de récupération simple ou de journalisation en bloc. Ces opérations d'index peuvent générer une activité de journal significative et les contentions de journal peuvent affecter le meilleur choix de degré de parallélisme (DOP) effectué par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

En plus de l’ajustement de l’option de configuration serveur du **degré maximal de parallélisme (MAXDOP)** , envisagez d’ajuster le parallélisme pour des opérations d’index avec l’[option MAXDOP](../t-sql/statements/alter-index-transact-sql.md). Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../relational-databases/indexes/configure-parallel-index-operations.md). Pour plus d’informations sur l’option relative au degré maximal de parallélisme, consultez [Configurer l’option de configuration serveur Degré maximal de parallélisme](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

### <a name="setting-the-maximum-number-of-worker-threads"></a>Configuration du nombre maximal de threads de travail
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] configurera l’option de configuration serveur **max worker threads** de façon dynamique au démarrage. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise le nombre d’UC disponibles et l’architecture système pour déterminer cette configuration serveur pendant le démarrage à l’aide d’une [formule](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md#Recommendations) documentée.

Seul un administrateur de base de données expérimenté ou un spécialiste SQL Server agréé doit changer cette option avancée. Si vous suspectez un problème de performance, celui ne vient probablement pas de la disponibilité des threads. Le problème est plus vraisemblablement causé par des opérations d’E/S qui contraignent les threads de worker à attendre. Nous vous conseillons d’identifier la cause racine d’un problème de performance avant de changer le paramètre max worker threads. Cependant, si vous devez définir manuellement max worker threads, cette valeur de configuration doit toujours correspondre à au moins sept fois le nombre d’UC présentes sur le système. Pour plus d’informations, consultez [Configurer l’option max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).

### <a name="using-sql-trace-and-sql-server-profiler"></a>Utilisation de Trace SQL et du Générateur de profils SQL
Nous vous recommandons de ne pas utiliser Trace SQL ni SQL Server Profiler dans un environnement de production. La surcharge induite par l'exécution de ces outils augmente également proportionnellement au nombre d'unités centrales. Si vous devez utiliser Trace SQL dans un environnement de production, limitez le nombre d'événements de trace au minimum. Profilez et testez avec soin chaque événement de trace sous charge et évitez d'utiliser des combinaisons d'événements qui affectent les performances de façon significative.

> [!IMPORTANT]
> Trace SQL et [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] sont dépréciés. L’espace de noms *Microsoft.SqlServer.Management.Trace* qui contient les objets Trace et Replay Microsoft SQL Server est également déconseillé. 
> [!INCLUDE[ssNoteDepFutureAvoid](../includes/ssnotedepfutureavoid-md.md)] 
> Utilisez plutôt des événements étendus. Pour plus d’informations sur les [événements étendus](../relational-databases/extended-events/extended-events.md), consultez [Démarrage rapide : Événements étendus dans SQL Server](../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md) et [SSMS XEvent Profiler](../relational-databases/extended-events/use-the-ssms-xe-profiler.md).

> [!NOTE]
> [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] pour les charges de travail Analysis Services n’est PAS déprécié et continuera à être pris en charge.

### <a name="setting-the-number-of-tempdb-data-files"></a>Définition du nombre de fichiers de données TempDB
Le nombre de fichiers dépend du nombre de processeurs (logiques) sur l’ordinateur. En règle générale, si le nombre de processeurs logiques est inférieur ou égal à huit, utilisez le même nombre de fichiers de données que de processeurs logiques. Si le nombre de processeurs logiques est supérieur à huit, utilisez huit fichiers de données et, si le conflit persiste, augmentez le nombre de fichiers de données par multiples de quatre pour réduire le conflit à un niveau acceptable ou bien modifiez la charge de travail/le code. N’oubliez pas non plus les autres suggestions pour tempdb, disponibles dans [Optimisation des performances de tempdb dans SQL Server](../relational-databases/databases/tempdb-database.md#optimizing-tempdb-performance-in-sql-server). 

Toutefois, en prenant soigneusement en compte les besoins de concurrence de TempDB, vous pouvez réduire les frais généraux de gestion de la base de données. Par exemple, si un système comporte 64 unités centrales et qu'habituellement seules 32 requêtes utilisent tempdb, l'augmentation du nombre de fichiers tempdb à 64 n'améliorera pas les performances.

### <a name="sql-server-components-that-can-use-more-than-64-cpus"></a>Composants SQL Server qui peuvent utiliser plus de 64 unités centrales
Le tableau suivant liste les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et indique s'ils peuvent utiliser plus de 64 UC.

|Nom du processus   |Programme exécutable |Utilisation de plus de 64 unités centrales |  
|----------|----------|----------|  
|Moteur de base de données SQL Server |Sqlserver.exe  |Oui |  
|Reporting Services |Rs.exe |Non |  
|Analysis Services  |As.exe |Non |  
|Integration Services   |Is.exe |Non |  
|Service Broker |Sb.exe |Non |  
|Recherche en texte intégral   |Fts.exe    |Non |  
|SQL Server Agent   |Sqlagent.exe   |Non |  
|SQL Server Management Studio   |Ssms.exe   |Non |  
|Programme d'installation de SQL Server   |Setup.exe  |Non |  
