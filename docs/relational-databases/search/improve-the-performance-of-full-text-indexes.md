---
description: Améliorer les performances des index de recherche en texte intégral
title: Améliorer les performances des index de recherche en texte intégral | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- performance [SQL Server], full-text search
- full-text queries [SQL Server], performance
- crawls [full-text search]
- full-text indexes [SQL Server], performance
- full-text search [SQL Server], performance
- batches [SQL Server], full-text search
ms.assetid: ef39ef1f-f0b7-4582-8e9c-31d4bd0ad35d
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d834493bac55e71e00fff11a9d27d7a0ee47387
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348635"
---
# <a name="improve-the-performance-of-full-text-indexes"></a>Améliorer les performances des index de recherche en texte intégral
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Cette rubrique décrit certaines causes courantes à l’origine de performances médiocres des requêtes et des index de recherche en texte intégral. Elle fournit également quelques suggestions pour éviter ces problèmes et améliorer les performances.
  
##  <a name="common-causes-of-performance-issues"></a><a name="causes"></a> Causes courantes des problèmes de performances
### <a name="hardware-resource-issues"></a>Problèmes liés aux ressources matérielles
Les performances de l’indexation de texte intégral et des requêtes de texte intégral sont influencées par les ressources matérielles, telles que la mémoire, la vitesse du disque et de l’UC, ainsi que l’architecture de la machine.  

La cause principale d’une baisse des performances de l’indexation de texte intégral se rapporte aux limites des ressources matérielles.  
  
-   **UC**. Si l’utilisation de l’UC par le processus hôte de démon de filtre (fdhost.exe) ou le processus (sqlservr.exe) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] approche des 100 pour cent, le processeur est le goulet d’étranglement.  
  
-   **Mémoire**. Une insuffisance de mémoire physique peut signaler l’existence d’un goulot d’étranglement au niveau de la mémoire.  

-   **Disque**. Si la longueur moyenne de la file d'attente du disque est plus de deux fois supérieure au nombre de têtes de disque, le disque est engorgé. La première solution consiste à créer des catalogues de texte intégral distincts des fichiers et des journaux de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Placez les journaux, les fichiers de base de données et les catalogues de texte intégral sur des disques distincts. L’installation de disques plus rapides et l’utilisation de RAID peuvent également contribuer à améliorer les performances d’indexation.  
  
    > [!NOTE]  
    > À partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], le moteur d’indexation et de recherche en texte intégral peut utiliser la mémoire AWE parce qu’il fait partie du processus sqlservr.exe. Pour plus d’informations, consultez [Architecture de la recherche en texte intégral](../../relational-databases/search/full-text-search.md#architecture).  

### <a name="full-text-batching-issues"></a>Problèmes liés au traitement par lot de recherche en texte intégral
 Si le système ne rencontre aucun goulet d'étranglement matériel, les performances d'indexation de la recherche en texte intégral dépendent essentiellement des éléments suivants :  
  
-   Durée de création des traitements de texte intégral par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Rapidité d'utilisation de ces traitements par le démon de filtre.  

### <a name="full-text-index-population-issues"></a>Problèmes liés à l’alimentation d’un index de recherche en texte intégral
-   **Type d’alimentation**. Contrairement à un remplissage complet, le remplissage du suivi des modifications automatique, incrémentiel ou manuel n'a pas pour objectif de maximiser les ressources matérielles en vue d'obtenir une vitesse supérieure. Par conséquent, les suggestions de paramétrage fournies dans cette rubrique peuvent ne pas améliorer les performances de l’indexation de texte intégral lorsqu’une alimentation de suivi des modifications incrémentielle, manuelle ou automatique est utilisée.  
  
-   **Fusion principale**. À la fin d'une alimentation, une fusion finale est déclenchée ; les fragments d'index sont fusionnés entre eux dans un index de recherche en texte intégral. Il en résulte une amélioration des performances des requêtes dans la mesure où seul l'index principal doit être interrogé au lieu des fragments d'index ; par ailleurs, les statistiques de score sont plus appropriées pour un classement en fonction de la pertinence. Toutefois, la fusion principale peut nécessiter de nombreuses entrées/sorties, car de grandes quantités de données doivent être écrites et lues lors de la fusion des fragments d’index, sans pour autant bloquer les requêtes entrantes.  
  
    La fusion principale d'une grande quantité de données peut créer une transaction dont l'exécution est longue, ce qui retarde la troncation du journal des transactions pendant le point de contrôle. Dans ce cas, en mode de récupération complète, la taille du journal des transactions peut s'accroître considérablement. Avant de réorganiser un index de recherche en texte intégral de grande taille dans une base de données qui utilise le mode de récupération complète, il est fortement recommandé de vous assurer que votre journal des transactions contient un espace suffisant pour une transaction dont l'exécution est longue. Pour plus d’informations, consultez [Gérer la taille du fichier journal des transactions](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md).  
  
##  <a name="tune-the-performance-of-full-text-indexes"></a><a name="tuning"></a> Améliorer les performances des index de recherche en texte intégral  
Pour optimiser les performances de vos index de recherche en texte intégral, appliquez les bonnes pratiques suivantes :  
  
-   Pour utiliser tous les processeurs ou cœurs d’UC au maximum, définissez [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) '**max full-text crawl range**' en lui affectant le nombre d’UC présentes dans le système. Pour plus d’informations sur cette option de configuration, consultez [Maximum de la plage de l’analyse de texte intégral (option de configuration de serveur)](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md).  
  
-   Vérifiez si la table de base a un index cluster. Utilisez un type de données entier pour la première colonne de l'index cluster. Évitez d'utiliser les GUID dans la première colonne de l'index cluster. Un remplissage multiplage sur un index cluster peut produire la vitesse de remplissage la plus élevée. Nous recommandons que la colonne servant de clé de texte intégral corresponde à un type de données Integer.  
  
-   Mettez à jour les statistiques de la table de base à l'aide de l'instruction [UPDATE STATISTICS](../../t-sql/statements/update-statistics-transact-sql.md) . Le plus important est de mettre à jour les statistiques sur l'index cluster ou la clé de texte intégral pour un remplissage complet. De cette manière, un remplissage sur plusieurs plages peut générer les partitions adéquates sur la table.  
  
-   Avant d’effectuer une alimentation complète sur un ordinateur multiprocesseur de grande capacité, nous vous recommandons de limiter temporairement la taille du pool de mémoires tampons en définissant la valeur **mémoire maximum du serveur** de manière à laisser suffisamment de mémoire au processus fdhost.exe et au système d’exploitation. Pour plus d’informations, consultez [Estimation des besoins en mémoire du processus hôte de démon de filtre (fdhost.exe)](#estimate), plus loin dans cette rubrique.

-   Si vous utilisez une alimentation incrémentielle basée sur une colonne timestamp, créez un index secondaire sur la colonne **timestamp** afin d’améliorer les performances de l’alimentation incrémentielle.  
  
##  <a name="troubleshoot-the-performance-of-full-populations"></a><a name="full"></a> Résoudre les problèmes de performances des alimentations complètes  
### <a name="review-the-full-text-crawl-logs"></a>Passer en revue les journaux d’analyse de texte intégral
 Pour mieux diagnostiquer les problèmes de performances, examinez les journaux d’analyse de texte intégral.
 
Lorsqu'une erreur se produit durant une analyse, la fonction d'analyse de la recherche en texte intégral crée et conserve un journal de l'analyse sous forme de fichier texte. Chaque journal de l'analyse correspond à un catalogue de texte intégral particulier. Par défaut, les journaux d’analyse pour une instance donnée (dans cet exemple, l’instance par défaut) figurent dans le dossier `%ProgramFiles%\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\LOG`.
 
Le fichier journal de l'analyse respecte le modèle de dénomination suivant :  
  
`SQLFT<DatabaseID\><FullTextCatalogID\>.LOG[<n\>]`
  
Les parties variables du nom de fichier du journal d’analyse sont les suivantes.
-   \<**DatabaseID**> – L’ID d’une base de données. <**dbid**> est un nombre à 5 chiffres commençant par des zéros non significatifs.  
-   <**FullTextCatalogID**> - ID du catalogue de texte intégral. \<**catid**> est un nombre à cinq chiffres commençant par des zéros.  
-   <**n**> - Entier qui indique qu’il existe un ou plusieurs journaux d’analyse du même catalogue de texte intégral.  
  
 Par exemple, `SQLFT0000500008.2` est le fichier journal d’analyse pour une base de données ayant un ID de base de données = 5 et un ID de catalogue de texte intégral = 8. Le 2 à la fin du nom de fichier indique qu'il existe deux fichiers journaux d'analyse pour cette combinaison base de données/catalogue.  

### <a name="check-physical-memory-usage"></a>Vérifier l’utilisation de la mémoire physique  
 Durant une alimentation de texte intégral, `fdhost.exe` ou `sqlservr.exe` peuvent manquer partiellement ou complètement de mémoire.
-   Si le journal d’analyse de texte intégral indique que `fdhost.exe` est souvent redémarré ou que le code d’erreur 8007008 est retourné, cela signifie que l’un de ces processus manque de mémoire.
-   Si `fdhost.exe` produit des images mémoire, en particulier sur des ordinateurs multiprocesseurs de grande capacité, cela peut signifier qu’il manque de mémoire.  
-   Pour obtenir des informations sur les mémoires tampons utilisées par une analyse de texte intégral, consultez [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md).  
  
 Les causes possibles de problèmes de mémoire basse ou insuffisante sont les suivantes :  
  
-   **Mémoire insuffisante**. Si la quantité de mémoire physique disponible pendant une alimentation complète est égale à zéro, le pool de mémoires tampons [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consomme peut-être la plus grande part de la mémoire physique du système.  
  
     Le processus `sqlservr.exe` essaie de récupérer toute la mémoire disponible pour le pool de mémoires tampons, jusqu’à la limite maximale de mémoire configurée pour le serveur. Si l’allocation de **mémoire maximum du serveur** est trop importante, des insuffisances de mémoire et des échecs d’allocation de la mémoire partagée peuvent se produire pour le processus fdhost.exe.  
  
     Vous pouvez résoudre ce problème en définissant de façon appropriée la valeur **mémoire maximum du serveur** du pool de mémoires tampons [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Estimation des besoins en mémoire du processus hôte de démon de filtre (fdhost.exe)](#estimate), plus loin dans cette rubrique. La réduction de la taille de lot utilisée pour l'indexation de texte intégral peut également s'avérer utile.  

-   **Contention de mémoire**. Durant une alimentation de texte intégral sur un ordinateur multiprocesseur, il peut se produire un conflit de mémoire au niveau du pool de mémoires tampons pour fdhost.exe ou sqlservr.exe. Le manque de mémoire partagée qui en résulte provoque des nouvelles tentatives d'exécution de lots, des surexploitations de la mémoire et des vidages par le processus fdhost.exe.  

-   **Problème de pagination**. Si la taille du fichier d'échange est insuffisante, comme cela peut se produire sur un système qui dispose d'un petit fichier d'échange avec une croissance limitée, fdhost.exe ou sqlservr.exe risquent de manquer de mémoire. Si les journaux d'analyse n'indiquent pas de défaillances relatives à la mémoire, il est probable que les performances sont dégradées par une pagination excessive.  
  
### <a name="estimate-the-memory-requirements-of-the-filter-daemon-host-process-fdhostexe"></a><a name="estimate"></a> Estimer les besoins en mémoire du processus hôte de démon de filtre (fdhost.exe)  
 La quantité de mémoire requise par le processus fdhost.exe pour une alimentation dépend principalement du nombre de plages d'analyses de texte intégral utilisées, de la taille de la mémoire partagée entrante et du nombre maximal d'instances relatives à la mémoire partagée entrante.  
  
 La quantité de mémoire consommée (en octets) par l'hôte de démon de filtre peut être estimée approximativement à l'aide de la formule suivante :  
  
`number_of_crawl_ranges * ism_size * max_outstanding_isms * 2`  
  
 Les valeurs par défaut des variables contenues dans la formule précédente sont les suivantes :  
  
|**Variable**|**Valeur par défaut**|  
|------------------|-----------------------|  
|*number_of_crawl_ranges*|Nombre de processeurs|  
|*ism_size*|1 Mo pour les ordinateurs x86<br /><br /> 4 Mo, 8 Mo ou 16 Mo pour les ordinateurs x64, en fonction de la mémoire physique totale|  
|*max_outstanding_isms*|25 pour les ordinateurs x86<br /><br /> 5 pour les ordinateurs x64|  
  
 Le tableau suivant présente des indications expliquant comment estimer les besoins en mémoire de fdhost.exe. Les formules figurant dans ce tableau utilisent les valeurs suivantes :  
  
-   *F*, qui est une évaluation de la mémoire requise par fdhost.exe (en Mo).  
  
-   *T*, qui est la mémoire physique totale disponible sur le système (en Mo).  
  
-   *M*, qui correspond à la valeur **mémoire maximum du serveur** optimale.  
  
Pour obtenir des informations essentielles sur les formules suivantes, consultez les remarques qui suivent le tableau.  
  
|Plateforme|Estimation des besoins en mémoire de fdhost.exe en Mo : *F*^1|Formule de calcul de la mémoire maximum du serveur : *M*^2|  
|--------------|-----------------------------------------------------------|-----------------------------------------------------|  
|x86|*F* = *Nombre de plages d’analyse* \* 50|*M* =minimum(*T*, 2000) - F - 500|  
|x64|*F* = *Nombre de plages d’analyse* \* 10 \* 8|*M* = *T* - *F* - 500|  

**Remarques sur les formules**
1.  Si plusieurs alimentations complètes sont en cours, calculez séparément les besoins en mémoire de chaque processus fdhost.exe, comme *F1*, *F2*, etc. Ensuite, calculez *M* comme _T_ **-** sigma **(** _F_i **)** .  
2.  500 Mo est une estimation de la mémoire requise par les autres processus dans le système. Si le système effectue un travail supplémentaire, augmentez cette valeur en conséquence.  
3.  .*ism_size* est censé être de 8 Mo pour les plateformes x64.  
  
 #### <a name="example-estimate-the-memory-requirements-of-fdhostexe"></a>Exemple : estimation des besoins en mémoire de fdhost.exe  
  
 Cet exemple se rapporte à un ordinateur 64 bits avec 8 Go de RAM et 4 processeurs double cœur. Les premières estimations de calcul de la mémoire requise par fdhost.exe : *F*. Le nombre de plages d'analyse est `8`.  
  
 `F = 8*10*8 = 640`  
  
 Le calcul suivant obtient la valeur optimale de **mémoire maximum du serveur** -*M*. La mémoire physique totale disponible sur ce système en Mo -*T*- est `8192`.  
  
 `M = 8192-640-500 = 7052`  
  
 #### <a name="example-setting-max-server-memory"></a>Exemple : configuration de la mémoire maximum du serveur  
  
 Cet exemple utilise les instructions [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) et [RECONFIGURE](../../t-sql/language-elements/reconfigure-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] pour définir la **mémoire maximale du serveur** sur la valeur calculée pour *M* dans l’exemple précédent, `7052` :  
  
```sql  
USE master;  
GO  
EXEC sp_configure 'max server memory', 7052;  
GO  
RECONFIGURE;  
GO  
```  
  
Pour plus d’informations sur les options de mémoire du serveur, consultez [Mémoire du serveur (option de configuration de serveur)](../../database-engine/configure-windows/server-memory-server-configuration-options.md).
  
### <a name="check-cpu-usage"></a>Vérifier l’utilisation de l’UC  
Les performances des alimentations complètes ne sont pas optimales lorsque la consommation processeur moyenne est inférieure à environ 30 %. Voici quelques facteurs qui affectent la consommation processeur.  
  
-   Temps d’attente élevé pour les pages  
  
     Pour déterminer si le temps d’attente d’une page est élevé, exécutez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```sql  
    SELECT TOP 10 * FROM sys.dm_os_wait_stats ORDER BY wait_time_ms DESC;  
    ```  
  
     Le tableau suivant décrit les types de temps d'attente présentant un intérêt ici.  
  
    |Type d’attente|Description|Résolution possible|  
    |---------------|-----------------|-------------------------|  
    |PAGEIO_LATCH_SH (_EX ou _UP)|Cela pourrait indiquer un goulet d'étranglement ES, auquel cas vous devriez généralement constater également une durée de file d'attente de disque moyenne élevée.|Le déplacement de l'index de recherche en texte intégral vers un groupe de fichiers différent sur un disque différent peut aider à réduire le goulet d'étranglement ES.|  
    |PAGELATCH_EX (ou _UP)|Cela pourrait indiquer beaucoup de contention parmi les threads qui essaient d'écrire dans le même fichier de base de données.|L'ajout des fichiers au groupe de fichiers sur lequel l'index de texte intégral réside pourrait aider à alléger cette contention.|  
  
     Pour plus d’informations, consultez [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
-   Inefficacités dans l'analyse de la table de base  
  
     Un remplissage complet analyse la table de base pour produire des lots. Cette analyse de table peut être inefficace dans les scénarios suivantes :  
  
    -   Si la table de base a un pourcentage élevé de colonnes hors ligne qui sont indexées de texte intégral, l'analyse de la table de base pour produire des lots peut être le goulet d'étranglement. Dans ce cas, le déplacement des petites données dans la ligne à l’aide de **varchar(max)** ou **nvarchar(max)** peut améliorer la situation.  
  
    -   Si la table de base est très fragmentée, l'analyse peut être inefficace. Pour obtenir des informations sur le calcul des données hors ligne et de la fragmentation des index, consultez [sys.dm_db_partition_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md) et [sys.dm_db_index_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md).  
  
         Pour réduire la fragmentation, vous pouvez réorganiser ou reconstruire l'index cluster. Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
##  <a name="troubleshoot-slow-indexing-of-documents"></a><a name="filters"></a> Résoudre les problèmes de ralentissement de l’indexation des documents

> [!NOTE]
> Cette section décrit un problème qui affecte uniquement les clients qui indexent des documents (tels que les documents Microsoft Word) qui incorporent d’autres types de documents.

Le moteur d’indexation et de recherche en texte intégral utilise deux types de filtres lorsqu’il remplit un index de recherche en texte intégral : des filtres multithreads et des filtres monothreads.
-   Certains documents, tels que les documents [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word, sont filtrés à l'aide d'un filtre multithread.
-   D'autres, tels que les documents PDF (Portable Document Format) d'Adobe Acrobat, sont filtrés à l'aide d'un filtre monothread.  
  
 Pour des raisons de sécurité, les filtres sont chargés par les processus hôtes de démon de filtre. Une instance de serveur utilise un processus multithread pour tous les filtres multithreads et un processus monothread pour tous les filtres monothreads. Lorsqu'un document qui utilise un filtre multithread contient un document incorporé qui utilise un filtre monothread, le Moteur d'indexation et de recherche en texte intégral lance un processus monothread pour le document incorporé. Par exemple, quand il rencontre un document Word qui contient un document PDF, le Moteur d’indexation et de recherche en texte intégral utilise le processus multithread pour le contenu Word et lance un processus monothread pour le contenu PDF. Toutefois, un filtre monothread peut ne pas fonctionner correctement dans cet environnement et peut déstabiliser le processus de filtrage. Dans certaines circonstances, lorsque ce type d’incorporation est courant, cette déstabilisation peut provoquer des blocages du processus. Dans ce cas, le moteur d’indexation et de recherche en texte intégral réachemine tout document ayant subi un échec (par exemple, un document Word incorporant un contenu PDF) vers le processus de filtrage monothread. Si le réacheminement a lieu fréquemment, il en résulte une détérioration des performances du processus d'indexation de texte intégral.  
  
Pour contourner ce problème, marquez le filtre du document conteneur (le document Word dans cet exemple) en tant que filtre monothread. Pour ce faire, définissez la valeur de Registre **ThreadingModel** pour le filtre en spécifiant **Thread cloisonné**. Pour plus d’informations sur les threads uniques cloisonnés (STA), consultez le livre blanc intitulé [Présentation et utilisation des modèles de threads COM](/previous-versions/ms809971(v=msdn.10)).  
  
## <a name="see-also"></a>Voir aussi  
 [Mémoire du serveur (option de configuration de serveur)](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [Maximum de la plage de l’analyse de texte intégral (option de configuration de serveur)](../../database-engine/configure-windows/max-full-text-crawl-range-server-configuration-option.md)   
 [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md)   
 [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)   
 [sys.dm_fts_memory_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)   
 [Résoudre l'indexation de recherche en texte intégral](../../relational-databases/search/troubleshoot-full-text-indexing.md)  
 [Architecture de la recherche en texte intégral](../../relational-databases/search/full-text-search.md#architecture) 
