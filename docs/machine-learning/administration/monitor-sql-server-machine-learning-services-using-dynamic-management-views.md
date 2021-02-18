---
title: Surveiller les scripts à l’aide des vues de gestion dynamique
description: Utilisez les vues de gestion dynamique (DMV) pour surveiller l’exécution du script externe Python et R dans SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 91ae86607ee2127af21ae0d47f80580df32c93cd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337092"
---
# <a name="monitor-sql-server-machine-learning-services-using-dynamic-management-views-dmvs"></a>Surveiller SQL Server Machine Learning Services à l’aide de vues de gestion dynamique (DMV)
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Utilisez des vues de gestion dynamique pour surveiller l’exécution de scripts externes (Python et R), les ressources utilisées, diagnostiquer les problèmes et optimiser les performances dans SQL Server Machine Learning Services.

Dans cet article, vous trouverez les DMV spécifiques de SQL Server Machine Learning Services. Vous trouverez également des exemples de requêtes qui affichent les éléments suivants :

+ Paramètres et options de configuration avancées pour le Machine Learning
+ Sessions actives exécutant des scripts R ou Python externes
+ Statistiques d’exécution du runtime externe pour Python et R
+ Compteurs de performances pour les scripts externes
+ Utilisation de la mémoire pour le système d’exploitation, SQL Server et les pools de ressources externes
+ Configuration de la mémoire pour SQL Server et les pools de ressources externes
+ Pools de ressources Resource Governor, y compris les pools de ressources externes
+ Packages installés pour Python et R

Pour plus d’informations générales sur les DMV, consultez [Vues de gestion dynamique du système](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).

> [!TIP]
> Vous pouvez également utiliser les rapports personnalisés pour surveiller SQL Server Machine Learning Services. Pour plus d’informations, consultez [Surveiller le Machine Learning à l’aide de rapports personnalisés dans Management Studio](monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md).

## <a name="dynamic-management-views"></a>Vues de gestion dynamique (DMV)

Les vues de gestion dynamique suivantes peuvent être utilisées lors de l’analyse des charges de travail de Machine Learning dans SQL Server. Pour interroger les vues de gestion dynamique, vous devez disposer de l’autorisation `VIEW SERVER STATE` sur l’instance.

| Vue de gestion dynamique | Type | Description |
|-------------------------|------|-------------|
| [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md) | Exécution | Renvoie une ligne pour chaque compte de travail actif qui exécute un script externe. |
| [sys.dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md) | Exécution | Renvoie une ligne pour chaque type de demande de script externe. |
| [sys.dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) | Exécution | Renvoie une ligne par compteur de performance maintenu par le serveur. Si vous utilisez la condition de recherche `WHERE object_name LIKE '%External Scripts%'`, vous pouvez utiliser ces informations pour savoir combien de scripts ont été exécutés, quels scripts ont été exécutés avec quel mode d’authentification ou combien d’appels R ou Python ont été émis sur l’instance dans son ensemble. |
| [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) | gouverneur de ressources | Renvoie des informations sur l’état actuel des pools de ressources externes dans Resource Governor, la configuration actuelle des pools de ressources, ainsi que sur leurs statistiques. |
| [sys.dm_resource_governor_external_resource_pool_affinity](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pool-affinity-transact-sql.md) | gouverneur de ressources | Renvoie les informations d’affinité de l’UC relatives à la configuration actuelle du pool de ressources externes dans Resource Governor. Retourne une ligne par planificateur dans [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] où chaque planificateur est associé à un processeur. Vous pouvez utiliser cette vue pour surveiller la condition d'un planificateur ou pour identifier des tâches d'échappement. |

Pour obtenir des informations sur l’analyse des instances [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], consultez [Affichages catalogue](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md) et [Vues de gestion dynamiques pour Resource Governor](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md).

## <a name="settings-and-configuration"></a>Paramètres et configuration

Affichez les paramètres d’installation de Machine Learning Services et les options de configuration.

![Sortie issue de la requête sur les paramètres et la configuration](media/dmv-settings-and-configuration.png "Sortie issue de la requête sur les paramètres et la configuration")

Exécutez la requête ci-dessous pour obtenir cette sortie. Pour plus d’informations sur les vues et les fonctions utilisées, consultez [sys. dm_server_registry](../../relational-databases/system-dynamic-management-views/sys-dm-server-registry-transact-sql.md), [sys. configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)et [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md).

```sql
SELECT CAST(SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS INT) AS IsMLServicesInstalled
    , CAST(value_in_use AS INT) AS ExternalScriptsEnabled
    , COALESCE(SIGN(SUSER_ID(CONCAT (
                    CAST(SERVERPROPERTY('MachineName') AS NVARCHAR(128))
                    , '\SQLRUserGroup'
                    , CAST(serverproperty('InstanceName') AS NVARCHAR(128))
                    ))), 0) AS ImpliedAuthenticationEnabled
    , COALESCE((
            SELECT CAST(r.value_data AS INT)
            FROM sys.dm_server_registry AS r
            WHERE r.registry_key LIKE 'HKLM\Software\Microsoft\Microsoft SQL Server\%\SuperSocketNetLib\Tcp'
            AND r.value_name = 'Enabled'
            ), - 1) AS IsTcpEnabled
FROM sys.configurations
WHERE name = 'external scripts enabled';
```

La requête retourne les colonnes suivantes :

| Colonne                       | Description |
|------------------------------|-------------|
| IsMLServicesInstalled        | Renvoie la valeur 1 si SQL Server Machine Learning Services est installé pour l’instance. Dans le cas contraire, renvoie la valeur 0. |
| ExternalScriptsEnabled       | Renvoie la valeur 1 si les scripts externes sont activés pour l’instance. Dans le cas contraire, renvoie la valeur 0. |
| ImpliedAuthenticationEnabled | Renvoie la valeur 1 si l’authentification implicite est activée. Dans le cas contraire, renvoie la valeur 0. La configuration de l’authentification implicite est contrôlée en vérifiant si une connexion existe pour SQLRUserGroup. |
| IsTcpEnabled                 | Renvoie la valeur 1 si le protocole TCP/IP est activé pour l’instance. Dans le cas contraire, renvoie la valeur 0. Pour plus d’informations, consultez [Configuration des protocoles réseau par défaut de SQL Server](../../database-engine/configure-windows/default-sql-server-network-protocol-configuration.md). |

## <a name="active-sessions"></a>Sessions actives

Affiche les sessions actives exécutant des scripts externes.

![Sortie issue de la requête sur les paramètres actifs](media/dmv-active-sessions.png "Sortie issue de la requête sur les paramètres actifs")

Exécutez la requête ci-dessous pour obtenir cette sortie. Pour plus d’informations sur les vues de gestion dynamique utilisées, consultez [sys. dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md), [sys. dm_external_script_requests](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) et [sys. dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md).

```sql
SELECT r.session_id, r.blocking_session_id, r.status, DB_NAME(s.database_id) AS database_name
    , s.login_name, r.wait_time, r.wait_type, r.last_wait_type, r.total_elapsed_time, r.cpu_time
    , r.reads, r.logical_reads, r.writes, er.language, er.degree_of_parallelism, er.external_user_name
FROM sys.dm_exec_requests AS r
INNER JOIN sys.dm_external_script_requests AS er
ON r.external_script_request_id = er.external_script_request_id
INNER JOIN sys.dm_exec_sessions AS s
ON s.session_id = r.session_id;
```

La requête retourne les colonnes suivantes :

| Colonne                | Description |
|-----------------------|-------------|
| session_id            | Identifie la session associée à chaque connexion principale active. |
| blocking_session_id   | ID de la session qui bloque la demande. Si cette colonne est NULL, la demande n'est pas bloquée, ou les informations de session de la session bloquant la demande ne sont pas disponibles (ou ne peuvent pas être identifiées). |
| status                | Statut de la demande. |
| database_name         | Nom de la base de données active pour chaque session. |
| login_name            | Nom de connexion SQL Server sous lequel la session s’exécute actuellement. |
| wait_time             | Si la demande est actuellement bloquée, cette colonne retourne la durée de l'attente, en millisecondes. N'accepte pas la valeur NULL. |
| wait_type             | Si la demande est actuellement bloquée, cette colonne retourne le type d'attente. Pour plus d’informations sur les types d’attentes, consultez [sys.dm_os_wait_stats](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md). |
| last_wait_type        | Si la demande a été bloquée précédemment, cette colonne indique le type de la dernière attente. |
| total_elapsed_time    | Temps total écoulé en millisecondes depuis l'arrivée de la demande. |
| cpu_time              | Quantité de temps UC (en millisecondes) utilisée par la demande. |
| lectures                 | Nombre de lectures effectuées par la demande. |
| logical_reads         | Nombre de lectures logiques effectuées par la demande. |
| écritures                | Nombre d'écritures effectuées par la demande. |
| langage              | Mot clé qui représente un langage de script pris en charge. |
| degree_of_parallelism | Nombre indiquant le nombre de traitements parallèles qui ont été créés. Cette valeur peut être différente du nombre de traitements parallèles qui ont été demandés. |
| external_user_name    | Le compte de travail Windows sous lequel le script a été exécuté. |

## <a name="execution-statistics"></a>Statistiques d'exécution

Affiche les statistiques d’exécution du runtime externe pour Python et R. Seules les statistiques des fonctions de package RevoScaleR, revoscalepy ou microsoftml sont actuellement disponibles.

![Sortie issue de la requête sur les statistiques d’exécution](media/dmv-execution-statistics.png "Sortie issue de la requête sur les statistiques d’exécution")

Exécutez la requête ci-dessous pour obtenir cette sortie. Pour plus d’informations sur la vue de gestion dynamique utilisée, consultez [sys. dm_external_script_execution_stats](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-execution-stats.md). La requête renvoie uniquement les fonctions qui ont été exécutées plusieurs fois.

```sql
SELECT language, counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE counter_value > 0
ORDER BY language, counter_name;
```

La requête retourne les colonnes suivantes :

| Colonne        | Description |
|---------------|-------------|
| langage      | Nom du langage enregistré de script externe. |
| counter_name  | Nom de la fonction enregistrée de script externe. |
| counter_value | Nombre total d’instance appelée de la fonction enregistrée de script externe sur le serveur. Cette valeur, cumulative, commence par l’horodatage d’installation de la fonction sur l’instance. Elle ne peut pas être réinitialisée. |

## <a name="performance-counters"></a>Compteurs de performance

Affiche les compteurs de performance liés à l’exécution de scripts externes.

![Sortie issue de la requête sur les compteurs de performance](media/dmv-performance-counters.png "Sortie issue de la requête sur les compteurs de performance")

Exécutez la requête ci-dessous pour obtenir cette sortie. Pour plus d’informations sur la vue de gestion dynamique utilisée, consultez [sys. dm_os_performance_counters](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md).

```sql
SELECT counter_name, cntr_value
FROM sys.dm_os_performance_counters 
WHERE object_name LIKE '%External Scripts%'
```

**sys. dm_os_performance_counters** génère les compteurs de performances suivants pour les scripts externes :

| Compteur                   | Description |
|---------------------------|-------------|
| Nombre total d’exécutions          | Nombre de processus externes démarrés par des appels distants ou locaux. |
| Exécutions parallèles       | Nombre de fois où un script contenait la spécification _\@parallel_ et où [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] a été en mesure de générer et d’utiliser un plan de requête parallèle. |
| Exécutions avec diffusion en continu      | Nombre de fois où la fonctionnalité de diffusion en continu a été appelée. |
| Exécutions de contexte de calcul SQL         | Nombre d’exécutions de scripts externes pour lesquelles l’appel a été instancié à distance et SQL Server a été utilisé comme contexte de calcul. |
| Connexions avec authentification implicite Connexions      | Nombre de fois où un appel de bouclage ODBC a été effectué avec l’authentification implicite. Autrement dit, [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] a exécuté l’appel au nom de l’utilisateur ayant envoyé la demande de script. |
| Durée totale d’exécution (ms) | Temps écoulé entre l’appel et la fin de l’appel. |
| Erreurs d’exécution          | Nombre de fois où des scripts ont signalé des erreurs. Ce nombre n’inclut pas les erreurs R ou Python. |

## <a name="memory-usage"></a>Utilisation de la mémoire

Affichez des informations sur la mémoire utilisée par le système d’exploitation, SQL Server et les pools externes.

![Sortie issue de la requête sur l’utilisation de la mémoire](media/dmv-memory-usage.png "Sortie issue de la requête sur l’utilisation de la mémoire")

Exécutez la requête ci-dessous pour obtenir cette sortie. Pour plus d’informations sur les vues de gestion dynamique utilisées, consultez [sys. dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md) et [sys. dm_os_sys_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md).

```sql
SELECT physical_memory_kb, committed_kb
    , (SELECT SUM(peak_memory_kb)
        FROM sys.dm_resource_governor_external_resource_pools AS ep
        ) AS external_pool_peak_memory_kb
FROM sys.dm_os_sys_info;
```

La requête retourne les colonnes suivantes :

| Colonne                       | Description                                                                                                           |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| physical_memory_kb           | Quantité totale de mémoire physique sur l’ordinateur.                                                                   |
| committed_kb                 | Mémoire validée, en kilo-octets (Ko), dans le gestionnaire de mémoire. Elle ne comprend pas la mémoire réservée dans le gestionnaire de mémoire. |
| external_pool_peak_memory_kb | Somme de la quantité maximale de mémoire utilisée, en kilo-octets, pour tous les pools de ressources externes.                          |

## <a name="memory-configuration"></a>Configuration de la mémoire

Affiche des informations sur la configuration de mémoire maximale en pourcentage de pools de ressources externes et SQL Server. Si SQL Server s’exécute avec la valeur par défaut de `max server memory (MB)`, cela représente 100 % de la mémoire du système d’exploitation.

![Sortie issue de la requête sur la configuration de la mémoire](media/dmv-memory-configuration.png "Sortie issue de la requête sur la configuration de la mémoire")

Exécutez la requête ci-dessous pour obtenir cette sortie. Pour plus d’informations sur les vues utilisées, consultez [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) et [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md).

```sql
SELECT 'SQL Server' AS name
    , CASE CAST(c.value AS BIGINT)
        WHEN 2147483647 THEN 100
        ELSE (SELECT CAST(c.value AS BIGINT) / (physical_memory_kb / 1024.0) * 100 FROM sys.dm_os_sys_info)
        END AS max_memory_percent
FROM sys.configurations AS c
WHERE c.name LIKE 'max server memory (MB)'
UNION ALL
SELECT CONCAT ('External Pool - ', ep.name) AS pool_name, ep.max_memory_percent
FROM sys.dm_resource_governor_external_resource_pools AS ep;
```

La requête retourne les colonnes suivantes :

| Colonne             | Description |
|--------------------|-------------|
| name               | Nom du pool de ressources externes ou SQL Server. |
| max_memory_percent | Mémoire maximale que SQL Server ou le pool de ressources externes peut utiliser. |

## <a name="resource-pools"></a>Pools de ressources

Dans [SQL Server Resource Governor](../../relational-databases/resource-governor/resource-governor.md), un [pool de ressources](../../relational-databases/resource-governor/resource-governor-resource-pool.md) représente un sous-ensemble des ressources physiques d’une instance. Vous pouvez spécifier des limites sur la quantité d’UC, les E/S physiques et la mémoire que les demandes d’application entrantes, y compris l’exécution des scripts externes, peuvent utiliser avec le pool de ressources. Affichez les pools de ressources utilisés pour les scripts SQL Server et les scripts externes.

![Sortie issue de la requête sur les pools de ressources](media/dmv-resource-pools.png "Sortie issue de la requête sur les pools de ressources")

Exécutez la requête ci-dessous pour obtenir cette sortie. Pour plus d’informations sur les vues de gestion dynamique utilisées, consultez [sys.dm_resource_governor_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md) et [sys.dm_resource_governor_external_resource_pools](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-external-resource-pools.md).

```sql
SELECT CONCAT ('SQL Server - ', p.name) AS pool_name
    , p.total_cpu_usage_ms, p.read_io_completed_total, p.write_io_completed_total
FROM sys.dm_resource_governor_resource_pools AS p
UNION ALL
SELECT CONCAT ('External Pool - ', ep.name) AS pool_name
    , ep.total_cpu_user_ms, ep.read_io_count, ep.write_io_count
FROM sys.dm_resource_governor_external_resource_pools AS ep;
```

La requête retourne les colonnes suivantes :

| Colonne                   | Description  |
|--------------------------|--------------|
| pool_name                | Nom du pool de ressources. Les pools de ressources SQL Server sont préfixés avec `SQL Server` et les pools de ressources externes avec `External Pool`. |
| total_cpu_usage_hours    | L’utilisation cumulative du processeur, en millisecondes, depuis que les statistiques de Resource Governor ont été réinitialisées. |
| read_io_completed_total  | Total d’entrées/sorties de lecture terminées depuis que les statistiques de Resource Governor ont été réinitialisées.              |
| write_io_completed_total | Total d’entrées/sorties d’écriture terminées depuis que les statistiques de Resource Governor ont été réinitialisées.             |

## <a name="installed-packages"></a>Packages installés

Vous pouvez afficher les packages R et Python qui sont installés dans SQL Server Machine Learning Services en exécutant un script R ou Python qui les génère.

### <a name="installed-packages-for-r"></a>Packages installés pour R

Affiche les packages R installés dans SQL Server Machine Learning Services.

![Sortie issue de la requête R sur les packages installés](media/dmv-installed-packages-r.png "Sortie issue de la requête R sur les packages installés")

Exécutez la requête ci-dessous pour obtenir cette sortie. La requête utilise un script R pour déterminer les packages R installés avec SQL Server.

```sql
EXECUTE sp_execute_external_script @language = N'R'
, @script = N'
OutputDataSet <- data.frame(installed.packages()[,c("Package", "Version", "Depends", "License", "LibPath")]);'
WITH result sets((Package NVARCHAR(255), Version NVARCHAR(100), Depends NVARCHAR(4000)
    , License NVARCHAR(1000), LibPath NVARCHAR(2000)));
```

Les colonnes renvoyées sont les suivantes :

| Colonne  | Description                                                 |
|---------|-------------------------------------------------------------|
| Package | Nom du package installé.                              |
| Version | Version du package.                                     |
| Dépend | Répertorie le ou les packages dont dépend le package installé. |
| Licence | Licence du package installé.                          |
| LibPath | Répertoire dans lequel se trouve le package.                   |

### <a name="installed-packages-for-python"></a>Packages installés pour Python

Affiche les packages Python installés dans SQL Server Machine Learning Services.

![Sortie issue de la requête Python sur les packages installés](media/dmv-installed-packages-python.png "Sortie issue de la requête Python sur les packages installés")

Exécutez la requête ci-dessous pour obtenir cette sortie. La requête utilise un script Python pour déterminer les packages Python installés avec SQL Server.

```sql
EXECUTE sp_execute_external_script @language = N'Python'
, @script = N'
import pkg_resources
import pandas
OutputDataSet = pandas.DataFrame(sorted([(i.key, i.version, i.location) for i in pkg_resources.working_set]))'
WITH result sets((Package NVARCHAR(128), Version NVARCHAR(128), Location NVARCHAR(1000)));
```

Les colonnes renvoyées sont les suivantes :

| Colonne   | Description                               |
|----------|-------------------------------------------|
| Package  | Nom du package installé.            |
| Version  | Version du package.                   |
| Emplacement | Répertoire dans lequel se trouve le package. |

## <a name="next-steps"></a>Étapes suivantes

+ [Événements étendus pour le Machine Learning](extended-events.md)
+ [Vues de gestion dynamique relatives à Resource Governor](../../relational-databases/system-dynamic-management-views/resource-governor-related-dynamic-management-views-transact-sql.md)
+ [Vues de gestion dynamique système](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [Surveiller le Machine Learning à l’aide de rapports personnalisés dans Management Studio](monitor-sql-server-machine-learning-services-using-custom-reports-management-studio.md)
