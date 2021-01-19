---
title: Configurer l’option de configuration de serveur max worker threads | Microsoft Docs
description: Découvrez comment utiliser l’option max worker threads pour configurer le nombre de threads de travail disponibles pour SQL Server pour traiter certaines demandes.
ms.custom: contperf-fy20q4
ms.date: 04/14/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- worker threads [SQL Server]
- max worker threads option
ms.assetid: abeadfa4-a14d-469a-bacf-75812e48fac1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4d95d8f774d2b9daba6bd2a0e7a179dd901ef0ac
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171041"
---
# <a name="configure-the-max-worker-threads-server-configuration-option"></a>Configurer l'option de configuration de serveur max worker threads
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique explique comment configurer l'option de configuration de serveur **max worker threads** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L’option **max worker threads** permet de configurer le nombre de threads de travail disponibles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]à l’ensemble du processus pour traiter les demandes de requête, la connexion, la déconnexion et les requêtes d’application similaires.


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les services de thread natifs des systèmes d’exploitation pour garantir les conditions suivantes :

- Un ou plusieurs threads prennent simultanément en charge chaque réseau pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Un thread gère les points de contrôle de base de données.

- Un pool de threads gère tous les utilisateurs.

La valeur par défaut de **Nombre maximum de threads de travail** est 0. Cela permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de configurer automatiquement le nombre de threads de travail au démarrage. Ce paramètre par défaut convient à la plupart des systèmes. Cependant, selon votre configuration système, l'attribution d'une valeur spécifique à l'option **Nombre maximum de threads de travail** permet parfois d'accroître les performances.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Le nombre de demandes de requête peut dépasser la valeur définie pour dans **nombre maximal de threads de travail**, auquel cas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] regroupe les threads de travail afin que le prochain thread de travail disponible puisse traiter la demande. Un thread de travail est affecté uniquement à des requêtes actives et est libéré une fois la demande en service. Cela se produit même si la session utilisateur/connexion sur laquelle la requête a été effectuée reste ouverte. 

-   L’option de configuration du serveur pour le **nombre maximal de threads de travail** ne limite pas tous les threads qui peuvent être générés dans le système. Les threads requis pour des tâches telles que LazyWriter, Checkpoint, Logwriter, Service Broker, Lock Manager ou autres sont générés en dehors de cette limite. Les groupes de disponibilité utilisent certains des threads de travail dans la limite maximale de threads de travail  **mais utilisent également** des threads système (voir [Utilisation des threads par les groupes de disponibilité](../availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md#ThreadUsage)) si le nombre de threads configurés est dépassé, la requête suivante fournit des informations sur les tâches système qui ont généré les threads supplémentaires.  
  
 ```sql  
 SELECT  s.session_id, r.command, r.status,  
    r.wait_type, r.scheduler_id, w.worker_address,  
    w.is_preemptive, w.state, t.task_state,  
    t.session_id, t.exec_context_id, t.request_id  
 FROM sys.dm_exec_sessions AS s  
 INNER JOIN sys.dm_exec_requests AS r  
    ON s.session_id = r.session_id  
 INNER JOIN sys.dm_os_tasks AS t  
    ON r.task_address = t.task_address  
 INNER JOIN sys.dm_os_workers AS w  
    ON t.worker_address = w.worker_address  
 WHERE s.is_user_process = 0;  
 ```  
  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous suspectez un problème de performance, celui ne vient probablement pas de la disponibilité des threads. La cause est plus probablement liée aux activités qui occupent les thread de travail et ne les mettent pas en production. Les exemples incluent des requêtes de longue durée ou des goulots d’étranglement sur le système (E/S, blocage, attentes de verrous, attentes réseau) qui entraînent des requêtes à attente longue. Nous vous conseillons d’identifier la cause racine d’un problème de performance avant de changer le paramètre max worker threads. Pour plus d’informations sur l’évaluation de la performance, consultez [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md).
  
-   Le regroupement de threads permet d'optimiser les performances lorsque de nombreux clients sont connectés au serveur. Habituellement, un thread de système d'exploitation séparé est créé pour chaque demande de requête. Cependant, s'il existe des centaines de connexions au serveur, l'utilisation d'un thread par demande de requête peut consommer de grandes quantités de ressources système. L'option **Nombre maximum de threads de travail** permet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de créer un pool de threads de travail afin de servir un grand nombre de demandes de requête, ce qui améliore les performances.  
  
-   Le tableau suivant indique le nombre maximal de threads de travail configurés automatiquement (lorsqu la valeur est définie sur 0) pour différentes combinaisons d’UC, d’architecture d’ordinateur et de versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à l’aide de la formule : **_Nombre maximal de Workers par défaut_ + ((* UC logiques * - 4) * *Workers par UC*)**.  
  
    |Nombre d'unités centrales|Ordinateur 32 bits (jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])|Ordinateur 64 bits (jusqu’à [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1)|Ordinateur 64 bits (à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])|   
    |------------|------------|------------|------------|  
    |\<= 4|256|512|512|   
    |8|288|576|576|   
    |16|352|704|704|   
    |32|480|960|960|   
    |64|736|1472|1472|   
    |128|1248|2496|4480|   
    |256|2272|4544|8576|   
    
    Jusqu’à [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP1, le nombre de *Workers par UC* dépend uniquement de l’architecture (32 bits ou 64 bits) :
    
    |Nombre d'unités centrales|Ordinateur 32 bits <sup>1</sup>|Ordinateur 64 bits|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4|256 + ((UC logiques - 4) * 8)|512 <sup>2</sup> + ((UC logiques - 4) * 16)|   
    
    À partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], le nombre de *Workers par UC* dépend de l’architecture et du nombre de processeurs (entre 4 et 64, ou supérieur à 64) :
    
    |Nombre d'unités centrales|Ordinateur 32 bits <sup>1</sup>|Ordinateur 64 bits|   
    |------------|------------|------------|   
    |\<= 4|256|512|   
    |\> 4 et \<= 64|256 + ((UC logiques - 4) * 8)|512 <sup>2</sup> + ((UC logiques - 4) * 16)|   
    |\> 64|256 + ((UC logiques - 4) * 32)|512 <sup>2</sup> + ((UC logiques - 4) * 32)|   
  
    <sup>1</sup> À partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut plus être installé sur un système d’exploitation 32 bits. Les valeurs d’ordinateur 32 bits sont répertoriées pour aider les clients exécutant [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions antérieures. Nous vous recommandons d'utiliser 1 024 comme nombre maximal de threads de travail pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécutée sur un ordinateur 32 bits.
    
    <sup>2</sup> À partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], la valeur *Nombre maximal de Workers* par défaut est divisée par 2 pour les machines avec moins de 2 Go de mémoire.
  
    > [!TIP]  
    > Pour obtenir des recommandations concernant l’utilisation de plus de 64 unités centrales, consultez [Recommandations pour l’exécution de SQL Server sur des ordinateurs comportant plus de 64 unités centrales](../../relational-databases/thread-and-task-architecture-guide.md#best-practices-for-running-sql-server-on-computers-that-have-more-than-64-cpus).  
  
-   Lorsque tous les threads de travail traitent de longues requêtes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut sembler ne plus répondre jusqu'à ce qu'un thread de travail soit terminé et devienne disponible. Même s'il ne s'agit pas d'une défaillance, ce comportement peut parfois être indésirable. Si un processus semble ne pas répondre et si aucune nouvelle requête n'est traitée, connectez-vous à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de la connexion administrateur dédiée (DAC) et terminez le processus. Pour éviter cette situation, augmentez la valeur de l'option max worker threads.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction `RECONFIGURE`, un utilisateur doit disposer de l’autorisation de niveau serveur `ALTER SETTINGS`. L’autorisation `ALTER SETTINGS` est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin**.  
  
##  <a name="using-ssmanstudiofull"></a><a name="SSMSProcedure"></a> Utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Pour configurer l'option max worker threads  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Processeurs** .  
  
3.  Dans la zone du **nombre maximum de threads de worker**, tapez ou sélectionnez une valeur comprise entre 128 et 32 767.  
  
> [!TIP]
> Utilisez l'option de définition du **nombre maximal de threads de travail** pour définir le nombre de threads de travail disponibles pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le paramètre par défaut de **max worker threads** est adapté à la plupart des systèmes. Cependant, selon votre configuration système, l'attribution d'une valeur plus faible à l'option **max worker threads** (Nombre maximum de threads de travail) permet parfois d'accroître les performances.
> Pour plus d’informations, consultez [Recommandations](#Recommendations) dans cette page.
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-max-worker-threads-option"></a>Pour configurer l'option max worker threads  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `max worker threads` la valeur `900`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'max worker threads', 900 ;  
GO  
RECONFIGURE;  
GO  
```  
  
##  <a name="follow-up-after-you-configure-the-max-worker-threads-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l'option Nombre maximum de threads de travail  
 Le changement prend effet immédiatement après l’exécution de [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md), sans nécessiter le redémarrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Connexion de diagnostic pour les administrateurs de base de données](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)
