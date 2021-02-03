---
title: Guide d’architecture de gestion de la mémoire | Microsoft Docs
description: En savoir plus sur l’architecture de gestion de mémoire dans SQL Server, notamment les modifications apportées à la gestion de la mémoire dans les versions précédentes.
ms.custom: ''
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- guide, memory management architecture
- memory management architecture guide
- PMO
- Partitioned Memory Objects
- cmemthread
- AWE
- SPA, Single Page Allocator
- MPA, Multi Page Allocator
- memory allocation, SQL Server
- memory pressure, SQL Server
- stack size, SQL Server
- buffer manager, SQL Server
- buffer pool, SQL Server
- resource monitor, SQL Server
ms.assetid: 7b0d0988-a3d8-4c25-a276-c1bdba80d6d5
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 52cfa79cfa88646fea2bdd2d58168cbfdd60e57d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236595"
---
# <a name="memory-management-architecture-guide"></a>guide d’architecture de gestion de la mémoire

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

## <a name="windows-virtual-memory-manager"></a>Gestionnaire de mémoire virtuelle Windows  
Les zones d'espace d'adressage validées sont mappées à la mémoire physique disponible par le Gestionnaire de mémoire virtuelle Windows (VMM).  
  
Pour plus d’informations sur la quantité de mémoire physique prise en charge par les divers systèmes d’exploitation, consultez la documentation Windows sur les [limites de la mémoire selon les versions de Windows](/windows/desktop/Memory/memory-limits-for-windows-releases).  
  
Les systèmes de mémoire virtuelle autorisent le surengagement de la mémoire physique, de sorte que le rapport entre la mémoire virtuelle et la mémoire physique peut être supérieur à 1:1. Il est alors possible d'exécuter des programmes plus volumineux sur des ordinateurs offrant diverses configurations de la mémoire physique. Toutefois, dans la plupart des cas, l'utilisation d'une quantité de mémoire virtuelle nettement plus importante que les plages de travail moyennes combinées de tous les processus peut entraîner une détérioration des performances. 

## <a name="sql-server-memory-architecture"></a>Architecture de la mémoire de SQL Server

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] acquiert et libère la mémoire dynamiquement selon ses besoins. En règle générale, un administrateur ne doit plus spécifier la quantité de mémoire à allouer à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], même si cette option existe toujours et est obligatoire dans certains environnements.

L'un des objectifs principaux de tous les logiciels de base de données est de réduire les E/S disque, car les opérations de lecture et écriture sur le disque font partie des opérations les plus consommatrices de ressources. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] crée un pool de mémoires tampons en mémoire afin d’y garder les pages lues à partir de la base de données. Une grande partie du code [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vise à réduire au minimum le nombre d’opérations de lecture et d’écriture physiques entre le disque et le pool de mémoires tampons. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tente d’atteindre un équilibre entre deux objectifs :

* Empêcher le pool de mémoires tampons d'atteindre une taille susceptible de priver le système de mémoire.
* Réduire les E/S physiques vers les fichiers de la base de données en augmentant la taille du pool de mémoires tampons.

> [!NOTE]
> Dans un système surchargé, certaines requêtes volumineuses dont l'exécution nécessite une importante quantité de mémoire ne peuvent pas obtenir la quantité minimale de mémoire requise et reçoivent une erreur de temporisation pendant qu'elles attendent des ressources mémoire. Pour résoudre ce problème, augmentez la valeur de [l'option query wait](../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Pour une requête parallèle, envisagez de réduire [l'option max degree of parallelism](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)(Degré maximum de parallélisme).
 
> [!NOTE]
> Dans un système où une charge très lourde pèse sur les ressources mémoire, les requêtes comportant jointure de fusion, tri et bitmap dans le plan de requête peuvent éliminer le bitmap si elles n'obtiennent pas la mémoire minimale nécessaire pour ce bitmap. Ceci peut affecter les performances de la requête, et, si le processus de tri ne tient pas en mémoire, ceci peut accroître l'utilisation des tables de travail dans la base de données tempdb, ce qui augmente le volume de tempdb. Pour résoudre ce problème, ajoutez de la mémoire physique ou paramétrez les requêtes de façon qu'elles utilisent un autre plan de requête plus rapide.
 
### <a name="providing-the-maximum-amount-of-memory-to-ssnoversion"></a>Apport de la quantité maximale de mémoire à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

L’utilisation d’AWE et du privilège de verrouillage des pages en mémoire vous permet de fournir les quantités de mémoire suivantes au moteur de base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . 

> [!NOTE]
> Le tableau suivant comprend une colonne pour les versions 32 bits qui ne sont plus disponibles.

|Stratégie de mémoire|32 bits <sup>1</sup> |64 bits|
|-------|-------|-------| 
|Mémoire conventionnelle |Toutes les éditions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . jusqu'à la limite d'espace d'adressage virtuel de processus : <br>- 2 Go<br>- 3 Go avec le paramètre d’amorçage /3 gb <sup>2</sup> <br>- 4 Go sur WOW64 <sup>3</sup> |Toutes les éditions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . jusqu'à la limite d'espace d'adressage virtuel de processus : <br>- 7 To avec l’architecture IA64 (IA64 non pris en charge dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] et les versions ultérieures)<br>- Maximum du système d’exploitation avec architecture x64 <sup>4</sup>
|Mécanisme AWE (Permet à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] d'aller au-delà de la limite d'espace d'adressage virtuel de processus sur une plateforme 32 bits.) |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] éditions Standard, Entreprise et Développeur : le pool de mémoires tampons peut accéder à 64 Go de mémoire maximum.|Non applicable <sup>5</sup> |
|Privilège de verrouillage des pages en mémoire du système d’exploitation (permet de verrouiller la mémoire physique, empêchant ainsi la pagination par le système d’exploitation de la mémoire verrouillée). <sup>6</sup> |[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] éditions Standard, Entreprise et Développeur : requis pour que le processus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise le mécanisme AWE. La mémoire allouée par le biais du mécanisme AWE ne peut pas être dépaginée. <br> L'accord de ce privilège sans l'activation de AWE n'a aucun effet sur le serveur. | Option uniquement utilisée en cas de nécessité, à savoir s’il y a des raisons de penser que le processus sqlservr est hors page. Dans ce cas, l’erreur 17890, qui ressemble à l’exemple ci-dessous, est signalée dans le journal des erreurs : `A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`|

<sup>1</sup> les versions 32 bits ne sont pas disponibles à partir de la version [!INCLUDE[ssSQL14](../includes/sssql14-md.md)].  
<sup>2</sup> /3gb est un paramètre d’amorçage de système d’exploitation.  
<sup>3</sup> WOW64 (Windows on Windows 64) est un mode dans lequel [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 32 bits s’exécute sur un système d’exploitation 64 bits.  
<sup>4</sup> [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] édition Standard prend en charge jusqu’à 128 Go. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition prend en charge le maximum du système d’exploitation.  
<sup>5</sup> Notez que l’option sp_configure awe enabled est présente sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]64 bits, mais qu’elle est ignorée.    
<sup>6</sup> Si le privilège de verrouillage des pages en mémoire (LPIM) est accordé (sur la version 32 bits pour la prise en charge d’AWE ou sur la version 64 bits par elle-même), nous recommandons de définir également la mémoire de serveur maximale. Pour plus d’informations sur LPIM, consultez [Mémoire du serveur (option de configuration de serveur)](../database-engine/configure-windows/server-memory-server-configuration-options.md#lock-pages-in-memory-lpim).

> [!NOTE]
> Les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peuvent s’exécuter sur un système d’exploitation 32 bits. L’accès à plus de 4 gigaoctets (Go) de mémoire sur un système d’exploitation 32 bits nécessite AWE (Address Windowing Extensions) pour gérer la mémoire. Cela n’est pas nécessaire lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est exécuté sur des systèmes d’exploitation 64 bits. Pour plus d’informations sur AWE, consultez [Espace d’adressage de processus](/previous-versions/sql/sql-server-2008-r2/ms189334(v=sql.105)) et [Gestion de la mémoire pour des bases de données volumineuses](/previous-versions/sql/sql-server-2008-r2/ms191481(v=sql.105)) dans la documentation de [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)].   

<a name="changes-to-memory-management-starting-2012-11x-gm"></a>

## <a name="changes-to-memory-management-starting-with-sssql11"></a>Changements apportés à la gestion de la mémoire à compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]

Dans les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]), la mémoire était allouée selon cinq mécanismes différents :
-  **Allocateur de page unique (SPA)** , comprenant uniquement les allocations de mémoire inférieures ou égales à 8 Ko dans le processus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Les options de configuration *Mémoire maximum du serveur (Mo)* et *Mémoire minimum du serveur (Mo)* déterminaient les limites de la mémoire physique consommée par SPA. Le pool de tampons était aussi le mécanisme pour SPA et le plus grand consommateur d’allocations de pages uniques.
-  **Allocateur de plusieurs pages (MPA)** , pour les allocations de mémoire demandant plus de 8 Ko.
-  **Allocateur du CLR**, comprenant les segments de mémoire du CLR SQL et ses allocations globales créées durant l’initialisation du CLR.
-  Allocations de mémoire pour les **[piles de threads](../relational-databases/memory-management-architecture-guide.md#stacksizes)** dans le processus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].
-  **Allocations Windows directes (DWA)** , pour les demandes d’allocation de mémoire apportées directement à Windows. Il s’agit notamment de l’utilisation des segments de mémoire Windows et des allocations virtuelles directes effectuées par les modules chargés dans le processus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Les allocations à partir des DLL de procédure stockée étendue, les objets créés au moyen de procédures Automation (appels sp_OA) et les allocations à partir de fournisseurs de serveur lié sont des exemples de demandes d’allocation de mémoire.

À compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], les allocations de page unique, les allocations de plusieurs pages et les allocations du CLR sont toutes consolidées dans un **Allocateur de pages de « toute taille »** et sont incluent dans les limites de mémoire contrôlées par les options de configuration *mémoire maximum du serveur (Mo)* et *mémoire minimum du serveur (Mo)* . Ce changement offre des capacités de redimensionnement plus précises pour tous les besoins en mémoire transitant par le Gestionnaire de mémoire de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 

> [!IMPORTANT]
> Examinez attentivement vos configurations *Mémoire maximum du serveur (Mo)* et *Mémoire minimum du serveur (Mo)* après la mise à niveau vers [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]. En effet, à compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], ces configurations comprennent et représentent plus d’allocations de mémoire que celles des versions antérieures. Ces changements s’appliquent aux versions 32 bits et 64 bits de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] et [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], ainsi qu’aux versions 64 bits de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] jusqu’à [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)].

Le tableau suivant indique si un type spécifique d’allocation de mémoire est contrôlé par les options de configuration *Mémoire maximum du serveur (Mo)* et *Mémoire minimum du serveur (Mo)* :

|Type d’allocation de mémoire| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| À compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|
|-------|-------|-------|
|Allocation de page unique|Oui|Oui, regroupées dans des allocations de pages de « toute taille »|
|Allocation de plusieurs pages|Non|Oui, regroupées dans des allocations de pages de « toute taille »|
|Allocation du CLR|Non|Oui|
|Mémoire de piles de threads|Non|Non|
|Allocations directes de Windows|Non|Non|

À compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut allouer plus de mémoire que la valeur spécifiée dans le paramètre max server memory. Ce comportement peut se produire quand la valeur de **_Mémoire totale du serveur (Ko)_** a déjà atteint le paramètre **_Mémoire du serveur cible (Ko)_** (comme spécifié par la mémoire maximum du serveur). Si la mémoire libre contiguë est insuffisante pour répondre aux demandes de mémoire de plusieurs pages (plus de 8 Ko) en raison de la fragmentation de la mémoire, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut procéder à une surallocation au lieu de rejeter la demande de mémoire. 

Dès que cette allocation est effectuée, la tâche en arrière-plan du *moniteur de ressource* signale à tous les consommateurs de mémoire de libérer la mémoire allouée et tente de faire passer la valeur *Mémoire totale du serveur (Ko)* en dessous de la spécification *Mémoire du serveur cible (Ko)* . L’utilisation de la mémoire de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut donc dépasser brièvement le paramètre max server memory. Dans ce cas, la valeur du compteur de performances *Mémoire totale du serveur (Ko)* dépasse les paramètres max server memory et *Mémoire du serveur cible (Ko)* .

Ce comportement est généralement observé durant les opérations suivantes : 
-  Requêtes d’index Columnstore volumineuses
-  Constructions ou reconstructions d’index ColumnStore qui utilisent des volumes importants de mémoire pour effectuer les opérations de hachage et de tri
-  Opérations de sauvegarde qui nécessitent des mémoires tampons volumineuses
-  Traçage d’opérations qui doivent stocker des paramètres d’entrée volumineux

<a name="#changes-to-memory-management-starting-with-includesssql11includessssql11-mdmd"></a>
## <a name="changes-to-memory_to_reserve-starting-with-sssql11"></a>Changements apportés à « memory_to_reserve » à compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]
Dans les versions antérieures de SQL Server ([!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]), le Gestionnaire de mémoire de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] réservait une partie de l’espace d’adressage virtuel (VAS) des processus à **l’allocateur multipage**, à **l’allocateur CLR**, aux allocations de mémoire pour les **piles de threads** dans le processus SQL Server et aux **allocations Windows directes (DWA)** . Cette partie de l’espace d’adressage virtuel est également appelée « Mem-To-Leave » ou « pool non-tampon ».

L’espace d’adressage virtuel réservé pour ces allocations est déterminé par l’option de configuration _**memory\_to\_reserve**_. La valeur par défaut utilisée par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est de 256 Mo. Pour remplacer la valeur par défaut, utilisez le paramètre de démarrage [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] *-g*. Pour plus d’informations sur le paramètre de démarrage *-g*, consultez la page [Options de démarrage du service moteur de base de données](../database-engine/configure-windows/database-engine-service-startup-options.md) dans la documentation.

En effet, à compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], le nouvel allocateur de page de « toute taille » gère également les allocations supérieures à 8 Ko, et la valeur *memory_to_reserve* n’inclut pas les allocations de plusieurs pages. À l’exception de ce changement, cette option de configuration est la même.

Le tableau suivant indique si un type spécifique d’allocation de mémoire appartient à la région *memory_to_reserve* de l’espace d’adressage virtuel pour le processus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] :

|Type d’allocation de mémoire| [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../includes/ssKilimanjaro-md.md)]| À compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|
|-------|-------|-------|
|Allocation de page unique|Non|Non, regroupées dans des allocations de pages de « toute taille »|
|Allocation de plusieurs pages|Oui|Non, regroupées dans des allocations de pages de « toute taille »|
|Allocation du CLR|Oui|Oui|
|Mémoire de piles de threads|Oui|Oui|
|Allocations directes de Windows|Oui|Oui|

## <a name="dynamic-memory-management"></a><a name="dynamic-memory-management"></a> Gestion dynamique de la mémoire
Le comportement par défaut de la gestion de la mémoire du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] est d’acquérir autant de mémoire que nécessaire sans provoquer une insuffisance de mémoire sur le système. Pour cela, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] utilise les API de notification de mémoire de Microsoft Windows.

Lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise dynamiquement la mémoire, il interroge régulièrement le système afin de déterminer la mémoire physique disponible. La conservation de cette mémoire libre empêche le système d'exploitation de paginer. S'il y a moins de mémoire, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en libère pour le système d'exploitation. S’il y a plus de mémoire disponible, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut allouer davantage de mémoire. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] n’ajoute de la mémoire que lorsque sa charge de travail en requiert davantage. Un serveur au repos n’augmente pas la taille de son espace d’adressage virtuel.  
  
L’option **[Mémoire maximum du serveur](../database-engine/configure-windows/server-memory-server-configuration-options.md)** contrôle l’allocation de mémoire de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], la mémoire de compilation, tous les caches (notamment le pool de mémoires tampons), les [allocations de mémoire d’exécution des requêtes](#effects-of-min-memory-per-query), la [mémoire du gestionnaire de verrous](#memory-used-by-sql-server-objects-specifications) et la mémoire du CLR <sup>1</sup> (essentiellement les régisseurs de mémoire se trouvant dans **[sys.dm_os_memory_clerks](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)** ). 

<sup>1</sup> La mémoire du CLR est gérée dans le cadre des allocations max_server_memory à compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].

La requête suivante retourne des informations sur la mémoire allouée actuellement :  
  
```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
    large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
    locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
    virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
    virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
    virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
    page_fault_count AS sql_page_fault_count,
    memory_utilization_percentage AS sql_memory_utilization_percentage, 
    process_physical_memory_low AS sql_process_physical_memory_low, 
    process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  
 
<a name="stacksizes"></a> La mémoire pour les piles de threads <sup>1</sup>, le CLR <sup>2</sup>, les fichiers .dll de procédure étendue, les fournisseurs OLE DB référencés par les requêtes distribuées, les objets Automation référencés dans les instructions [!INCLUDE[tsql](../includes/tsql-md.md)] et la mémoire allouée par une DLL non-[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]**ne sont pas** contrôlées par max server memory.

<sup>1</sup> Pour plus d’informations sur les threads de worker par défaut calculés pour un nombre donné d’UC avec affinité dans l’hôte actif, consultez la page [Configurer l’option de configuration du serveur max worker threads](../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) dans la documentation. Les tailles de pile [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sont les suivantes :

|Architecture de SQL Server|Architecture du système d’exploitation|Taille de la pile|  
|--------------------|----------------------|----------------------|
|x86 (32 bits)|x86 (32 bits)|512 Ko|
|x86 (32 bits)|x64 (64 bits)|768 Ko| 
|x64 (64 bits)|x64 (64 bits)|2 048 Ko|
|IA64 (Itanium)|IA64 (Itanium)|4 096 Ko|

<sup>2</sup> La mémoire du CLR est gérée dans le cadre des allocations max_server_memory à compter de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilise l’API de notification de mémoire **QueryMemoryResourceNotification** pour déterminer à quel moment le Gestionnaire de mémoire de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut allouer et libérer de la mémoire.  

Lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] démarre, il calcule la taille de l'espace d'adressage virtuel pour le pool de mémoires tampons d'après plusieurs paramètres, dont la quantité de mémoire physique du système, le nombre de threads serveur et diverses options de démarrage. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] réserve la quantité ainsi calculée de son espace d’adressage virtuel de processus pour le pool de mémoires tampons, mais il acquiert (valide) uniquement la quantité nécessaire de mémoire physique pour la charge actuelle.

L'instance continue alors à acquérir de la mémoire comme l'exige la prise en charge de la charge de travail. Au fur et à mesure que des utilisateurs se connectent et exécutent des requêtes, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] acquiert la mémoire physique supplémentaire à la demande. Une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] continue d’acquérir de la mémoire physique jusqu’à ce qu’elle atteigne sa cible d’allocation de mémoire maximum du serveur, ou jusqu’à ce que le système d’exploitation indique qu’il n’y a plus de mémoire disponible en surplus. Elle libère de la mémoire quand elle en a plus que la valeur de mémoire minimum du serveur paramétrée et si le système d’exploitation indique qu’il y a une insuffisance de mémoire disponible. 

Étant donné que d'autres applications sont démarrées sur l'ordinateur exécutant une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], elles consomment de la mémoire et la quantité de mémoire physique disponible descend en dessous de la cible de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . L'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] règle sa consommation de mémoire. Si une autre application est arrêtée, la mémoire disponible est augmentée, et l’instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] augmente la taille de son allocation de mémoire. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut libérer et acquérir plusieurs mégaoctets de mémoire chaque seconde, ce qui lui permet de s’adapter rapidement aux changements d’allocation de mémoire.

## <a name="effects-of-min-and-max-server-memory"></a>Effets des options de configuration « min server memory » et « max server memory »
Les options de configuration *min server memory* et *max server memory* permettent d’établir les limites supérieure et inférieure de la quantité de mémoire utilisée par le pool de mémoires tampons et d’autres caches du moteur de base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Le pool de mémoires tampons n'obtient pas immédiatement la quantité de mémoire spécifiée dans min server memory. En effet, il commence seulement avec la mémoire nécessaire à l'initialisation. Au fur et à mesure que la charge de travail du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] augmente, celui-ci acquiert la mémoire nécessaire à la prise en charge de ces travaux. Le pool de mémoires tampons ne libère aucune partie de la mémoire acquise avant d'atteindre la valeur spécifiée dans min server memory. Dès lors que la quantité spécifiée dans min server memory est atteinte, le pool de mémoires tampons utilise l'algorithme standard pour acquérir et libérer la mémoire en fonction des besoins. La seule différence réside dans le fait que le pool de mémoires tampons ne diminue jamais son allocation de mémoire en dessous de la valeur spécifiée dans min server memory et n'obtient jamais plus de mémoire que le niveau spécifié dans max server memory.

> [!NOTE]
> En tant que processus,[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] acquiert plus de mémoire qu’indiqué par l’option max server memory. Les composants internes et externes peuvent allouer de la mémoire en dehors du pool de mémoires tampons, qui consomme un supplément de mémoire, mais la mémoire allouée au pool de mémoires tampons représente généralement la plus grande part de mémoire consommée par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

La quantité de mémoire acquise par le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] dépend entièrement de la charge de travail imposée à l’instance. Une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] qui ne traite pas beaucoup de demandes risque de ne jamais atteindre la valeur de min server memory.

Si la valeur spécifiée pour min server memory et max server memory est identique, une fois que la mémoire allouée au [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] a atteint cette valeur, le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] cesse de libérer et d’obtenir de la mémoire de façon dynamique pour le pool de mémoires tampons.

Si une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] fonctionne sur un ordinateur sur lequel d’autres applications sont régulièrement arrêtées ou démarrées, l’allocation et la désallocation de mémoire par l’instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut ralentir le démarrage de ces applications. De même, si [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est une application serveur parmi d’autres exécutées sur un seul ordinateur, l’administrateur système doit éventuellement contrôler la quantité de mémoire allouée à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour ce faire, il peut utiliser les options min server memory et max server memory pour contrôler la quantité de mémoire utilisable par [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Les options **min server memory** et **max server memory** sont spécifiées en mégaoctets. Pour en savoir plus, consultez les [Options de configuration de la mémoire du serveur](../database-engine/configure-windows/server-memory-server-configuration-options.md).

## <a name="memory-used-by-sql-server-objects-specifications"></a>Mémoire utilisée par les spécifications d’objets SQL Server
La liste suivante décrit la quantité estimée de mémoire utilisée par différents objets dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Les quantités indiquées sont des estimations. Elles peuvent varier en fonction de l’environnement et de la manière dont les objets sont créés :

* Verrou (tel qu’il est géré par le Gestionnaire de verrous) : 64 octets + 32 octets par propriétaire   
* Connexion utilisateur : Environ (3 \* taille_paquet_réseau + 94 Ko)    

La **taille des paquets réseau** représente la taille des paquets TDS (Tabular Data Scheme) utilisés pour la communication entre des applications et le moteur de base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La taille par défaut s'élève à 4 Ko ; elle est contrôlée par l'option de configuration Taille du paquet réseau.

Lorsque la fonctionnalité MARS (Multiple Active Result Sets) est activée, la connexion utilisateur est environ (3 + 3 \*nombre_connexions_logiques)\* taille_paquet_réseau + 94 Ko.

## <a name="effects-of-min-memory-per-query"></a>Effets de min memory per query
L’option de configuration *min memory per query* spécifie la quantité minimale de mémoire (en kilo-octets) allouée pour l’exécution d’une requête. Cela est également appelé « allocation de mémoire minimale ». Avant de commencer à s’exécuter, toutes les requêtes doivent attendre jusqu’à ce que la mémoire minimale demandée soit sécurisée, ou jusqu’à ce que la valeur spécifiée dans l’option de configuration du serveur Attente de la requête soit dépassée. Le type d’attente qui est accumulé dans ce scénario est RESOURCE_SEMAPHORE.

> [!IMPORTANT]
> Ne définissez pas l’option de configuration du serveur min memory per query sur une valeur trop élevée, en particulier sur des systèmes très sollicités, car cela peut aboutir à :         
> - Une concurrence accrue pour les ressources mémoire.         
> - Une concurrence réduite en augmentant la quantité de mémoire pour chaque requête individuelle, même si la mémoire nécessaire à l’exécution est inférieure à cette configuration.    
>    
> Pour obtenir des recommandations sur l’utilisation de cette configuration, consultez [Configurer l’option de configuration de serveur min memory per query](../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md#Recommendations).

### <a name="memory-grant-considerations"></a><a name="memory-grant-considerations"></a>Considérations sur l’allocation de mémoire
Pour **l’exécution en mode ligne**, l’allocation de mémoire initiale ne peut être dépassée en aucune circonstance. Si plus de mémoire que l’allocation initiale est nécessaire pour exécuter des opérations de **hachage** ou de **tri**, leur dépassement est transféré sur le disque. Une opération de hachage qui connaît un dépassement est prise en charge par un fichier de travail dans TempDB, tandis qu’une opération de tri avec dépassement est prise en charge par une [table de travail](../relational-databases/query-processing-architecture-guide.md#worktables).   

Un dépassement qui se produit pendant une opération de tri est appelé [avertissement de tri](../relational-databases/event-classes/sort-warnings-event-class.md). Les avertissements de tri indiquent que les opérations de tri ne peuvent pas être effectuées en mémoire. Ceci n’inclut pas les opérations de tri impliquant la création d’index, mais seulement les opérations de tri effectuées dans une requête (comme une clause `ORDER BY` utilisée dans une instruction `SELECT`).

Un dépassement qui se produit pendant une opération de hachage est appelé [avertissement de hachage](../relational-databases/event-classes/hash-warning-event-class.md). Ceci se produit quand une récurrence de hachage ou une cessation de hachage (interruption de hachage) s’est produite pendant une opération de hachage.
-  Une récurrence de hachage se produit lorsque l'entrée de construction n'est pas adaptée à la mémoire disponible, ce qui entraîne une fragmentation de l'entrée en plusieurs parties traitées séparément. Si une de ces partitions n’est toujours pas adaptée à la mémoire disponible, elle est à nouveau fragmentée en sous-partitions, qui sont également traitées séparément. Ce processus de fragmentation se poursuit jusqu’à ce que toutes les partitions soient adaptées à la mémoire disponible ou jusqu’à ce que le niveau maximal de récursivité soit atteint.
-  L'interruption de hachage a lieu lorsqu'une opération de hachage atteint son niveau maximal de récursivité et se décale à un plan auxiliaire pour traiter les données partitionnées restantes. Ces événements peuvent entraîner une baisse des performances de votre serveur.

Pour **l’exécution en mode batch**, l’allocation de mémoire initiale peut augmenter dynamiquement jusqu’à un certain seuil interne par défaut. Ce mécanisme d’allocation de mémoire dynamique est conçu pour permettre l’exécution en mémoire des opérations de **hachage** ou de **tri** qui s’exécutent en mode batch. Si ces opérations ne tiennent toujours pas dans la mémoire, elles sont transférées sur le disque.

Pour plus d’informations sur les modes d’exécution, consultez le [Guide d’architecture de traitement des requêtes](../relational-databases/query-processing-architecture-guide.md#execution-modes).

## <a name="buffer-management"></a>Gestion des tampons
L'objectif principal d'une base de données [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est de stocker et de récupérer les données, l'utilisation intensive d'E/S sur disque est donc une caractéristique centrale du moteur de base de données. Étant donné que les opérations d'E/S sur disque peuvent consommer beaucoup de ressources et durent relativement longtemps, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'attache à rendre ces opérations efficaces. La gestion des tampons joue un rôle essentiel pour parvenir à cette efficacité. Le composant de gestion des tampons comprend deux mécanismes : le **gestionnaire de tampons** qui permet d’accéder et de mettre à jour les pages de la base de données, et le **cache des tampons** (également appelé **pool de tampons**), qui permet de réduire les opérations d’E/S du fichier de la base de données. 

### <a name="how-buffer-management-works"></a>Fonctionnement de la gestion des tampons
Un tampon est une page de 8 Ko en mémoire dont la taille est similaire à une page d’index ou de données. Ainsi, le cache des tampons est divisé en pages de 8 Ko. Le gestionnaire des tampons gère les fonctions de lecture des pages d'index ou de données à partir des fichiers de disque de base de données dans le cache de tampons ainsi que la réécriture sur le disque des pages modifiées. Une page reste dans le cache des tampons jusqu'à ce que le gestionnaire de tampons ait besoin de la zone de mémoire tampon pour lire davantage de données. Les données ne sont réécrites sur le disque que si elles sont modifiées. Les données dans le cache de tampons peuvent être modifiées plusieurs fois avant leur réécriture sur le disque. Pour plus d’informations, consultez [Lecture de pages](../relational-databases/reading-pages.md) et [Écriture de pages](../relational-databases/writing-pages.md).

Lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] démarre, il calcule la taille de l’espace d’adressage virtuel pour le cache des tampons d’après plusieurs paramètres, dont la quantité de mémoire physique du système, le nombre de threads serveur maximum configuré et diverses options de démarrage. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] réserve la quantité ainsi calculée de son espace d’adressage virtuel de processus (appelée cible mémoire) pour le cache des tampons, mais il acquiert (valide) uniquement la quantité de mémoire physique nécessaire pour la charge actuelle. Vous pouvez interroger les colonnes **bpool_commit_target** et **bpool_committed** dans la vue du catalogue [sys.dm_os_sys_info](../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) pour retourner le nombre de pages réservées comme cible mémoire et le nombre de pages actuellement réservées dans le cache des tampons, respectivement.

L’intervalle entre le démarrage de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et le moment où la mémoire tampon obtient sa cible de mémoire s’appelle l’accélération. Au cours de cette opération, les tampons se remplissent de demandes de lecture selon les besoins. Par exemple, une demande de lecture d’une page de 8 Ko unique remplit une page de tampon unique. Cela signifie que l'accélération dépend du nombre et du type des demandes clientes. L’accélération s’effectue par la transformation des demandes de lecture de page unique en demandes de huit pages alignées (ce qui constitue une étendue). Cette opération permet au processus d'accélération de s'achever plus rapidement en particulier sur les ordinateurs possédant beaucoup de mémoire. Pour plus d’informations sur les pages et les étendues, consultez [Guide d’architecture des pages et des étendues](../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

Comme le gestionnaire de tampons consomme l'essentiel de la mémoire dans les processus [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , il collabore avec le gestionnaire de la mémoire afin de permettre aux autres composants d'utiliser ses tampons. Le gestionnaire de tampons interagit essentiellement avec les composants suivants :

* Le gestionnaire de ressources pour contrôler l'utilisation de l'ensemble de la mémoire et, sur les plateformes 32 bits, pour contrôler l'utilisation de l'espace d'adressage.  
* Le gestionnaire de base de données et le système d’exploitation [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (SQLOS) pour les opérations d’E/S de fichier peu importantes.  
* le gestionnaire du journal pour la journalisation préalable.  

### <a name="supported-features"></a>Fonctionnalités prises en charge
Le gestionnaire de tampons prend en charge les fonctionnalités suivantes.

* Le gestionnaire de tampons est compatible avec la technologie **NUMA (Non-Uniform Memory Access)** . Les pages de cache des tampons sont réparties sur les nœuds NUMA matériels, ce qui permet à un thread d'accéder à une page de tampons allouée sur le nœud NUMA local au lieu de la mémoire étrangère. 
* Le gestionnaire de tampons prend en charge **l’ajout de mémoire à chaud**, ce qui permet aux utilisateurs d’ajouter de la mémoire physique sans redémarrer le serveur. 
* Le gestionnaire de tampons prend en charge les **grandes pages** sur les plateformes 64 bits. La taille de la page est spécifique à la version de Windows.

  > [!NOTE]
  > Avant [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], l’activation des grandes pages dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] nécessitait [l’indicateur de trace 834](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  

* Le gestionnaire de tampons fournit les diagnostics supplémentaires exposés par le biais des vues de gestion dynamique. Vous pouvez utiliser ces affichages pour contrôler un ensemble de ressources du système d'exploitation spécifiques à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Par exemple, vous pouvez utiliser la vue [sys.dm_os_buffer_descriptors](../relational-databases/system-dynamic-management-views/sys-dm-os-buffer-descriptors-transact-sql.md) pour surveiller les pages dans le cache des tampons.   

### <a name="disk-io"></a>E/S disque
Le gestionnaire de tampons n'effectue que des lectures et des écritures dans la base de données. Les autres opérations de base de données et de fichier comme les opérations d'ouverture, de fermeture, d'élargissement et de compactage sont prises en charge par les composants du gestionnaire de base de données et du gestionnaire de fichiers. 

Les opérations d'E/S disque effectuées par le gestionnaire de tampons présentent les caractéristiques suivantes :

* Toutes les opérations d'E/S s'effectuent de manière asynchrone, ce qui permet au thread appelant de continuer le traitement durant l'opération d'E/S en arrière-plan.
* Toutes les opérations d'E/S ont lieu dans des threads appelants, sauf si l'option affinity I/O est utilisée. L'option affinity I/O mask lie les E/S disque de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à un sous-ensemble de processeurs spécifié. Dans les environnements de traitement transactionnel en ligne (OLTP) [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] haut de gamme, cette extension permet d'améliorer les performances des threads [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] émettant des E/S.
* Les E/S de pages multiples s'effectuent à l'aide d'E/S par fragmentation-rassemblement, ce qui permet le transfert des données dans des zones contiguës de la mémoire ou hors de celles-ci. Cela signifie que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut remplir ou vider rapidement le cache de tampons tout en évitant les demandes d'E/S physiques multiples. 

#### <a name="long-io-requests"></a>Longues demandes d'E/S  
Le gestionnaire de tampons signale les demandes d'E/S en suspens pendant un délai minimum de 15 secondes. L'administrateur système peut ainsi distinguer entre les problèmes [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et les problèmes au niveau du sous-système d'E/S. Le message d’erreur 833 est rapporté et consigné dans le journal des erreurs [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] comme suit :

`SQL Server has encountered ## occurrence(s) of I/O requests taking longer than 15 seconds to complete on file [##] in database [##] (#). The OS file handle is 0x00000. The offset of the latest long I/O is: 0x00000.` 

Une longue opération d'E/S peut correspondre à une opération de lecture ou d'écriture ; celle-ci ne figure pas dans le message actif. Les messages d'opérations d'E/S longues sont des avertissements pas des erreurs. Ils n’indiquent pas de problèmes avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], mais avec le système d’E/S sous-jacent. Les messages permettent à l'administrateur de détecter la cause des temps de réponse [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] médiocres plus rapidement et de distinguer les problèmes qui ne dépendent pas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. De ce fait, ces problèmes ne demandent aucune intervention, mais il est nécessaire que l'administrateur système analyse les raisons de la lenteur de la demande d'E/S et si ce délai est justifié.

#### <a name="causes-of-long-io-requests"></a>Facteurs de longues demandes d'E/S  
Un message signalant une opération d'E/S longue peut indiquer qu'une E/S est bloquée de manière permanente et ne peut s'achever (E/S perdue) ou qu'elle ne s'est pas encore terminée. Il est impossible d’identifier le type de scénario à partir du message, même si une E/S perdue entraîne souvent un délai d’attente de verrou.

Une opération d'E/S longue indique souvent une charge de travail [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] trop intense pour le sous-système de disque. Un sous-système de disque inapproprié peut se manifester dans les cas suivants :

* plusieurs opérations d'E/S longues dans le journal d'erreurs au cours d'une charge [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] importante ;
* les compteurs de performance indiquent des latences de disque longues, des files d'attente de disque longues ou pas d'inactivité de disque.  

Les opérations d'E/S longues sont aussi parfois causées par un composant dans le chemin d'accès d'E/S (un pilote, un contrôleur, un microprogramme, entre autres) qui retardent continuellement une demande d'E/S antérieure pour traiter des demandes plus récentes dont la position actuelle est plus proche de la tête de disque. La technique courante de traitement prioritaire des demandes en fonction de la proximité de celles-ci de la tête de lecture-écriture est connue sous le nom de « elevator seeking ». Cette technique peut ne pas être compatible avec l'outil Moniteur système Windows (PERFMON.EXE) en raison du traitement rapide de la plupart des E/S. Les opérations d'E/S longues peuvent se compliquer en raison de charges de travail impliquant de grandes quantités d'E/S séquentielle, parmi lesquelles figurent les opérations de sauvegarde et de restauration, les analyses de table, les tris, les créations d'index, les chargements en masse et les réinitialisations de fichiers.

Les opérations d'E/S longues isolées qui ne présentent à priori aucun rapport avec les conditions décrites plus haut sont peut-être causées par un problème de matériel ou de pilote. Le journal d'événements système peut contenir un événement connexe qui permet de diagnostiquer le problème.

### <a name="memory-pressure-detection"></a>Détection de la forte sollicitation de la mémoire
La forte sollicitation de la mémoire est une situation résultant d’un manque de mémoire et peut entraîner les effets suivants :
- E/S supplémentaires (par exemple un thread d’arrière-plan d’écriture différée très actif)
- Taux de recompilation plus élevé
- Exécution plus longue des requêtes (dans le cas où des allocations de mémoire sont en attente)
- Cycles d’UC supplémentaires

Cette situation peut être déclenchée pour des raisons externes ou internes. Les raisons externes peuvent être les suivantes :
- La mémoire physique (RAM) disponible est faible. Ceci fait que le système raccourcit les plages de travail des processus actuellement en cours d’exécution, ce qui peut aboutir à un ralentissement global. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut réduire la cible de validation du pool de mémoires tampons et commencer à réduire les caches internes plus souvent. 
- La mémoire système globale disponible (qui inclut le fichier d’échange système) est faible. Ceci peut entraîner l’échec des allocations de mémoire par le système, car il ne peut pas paginer la mémoire actuellement allouée.
Les raisons internes peuvent être les suivantes :
- En réponse à une forte sollicitation de la mémoire externe, quand le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] définit des limites plus basses d’utilisation de la mémoire.
- Les paramètres mémoire ont été abaissés manuellement via une réduction de la configuration de *max server memory*. 
- Des modifications de la distribution en mémoire de composants internes entre les différents caches.

Le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] implémente une infrastructure dédiée à la détection et à la gestion de la forte sollicitation de la mémoire dans le cadre de sa gestion de la mémoire dynamique. Cette infrastructure inclut la tâche d’arrière-plan appelée **Moniteur de ressource**. La tâche Moniteur de ressource surveille l’état des indicateurs de la mémoire interne et externe. Une fois qu’un de ces indicateurs change d’état, il calcule la notification correspondante et la diffuse. Ces notifications sont des messages internes provenant de chacun des composants du moteur et elles sont stockées dans des mémoires tampons en anneau. 

Deux mémoires tampons en anneau contiennent des informations relatives à la gestion de la mémoire dynamique : 
- La mémoire tampon en anneau Moniteur de ressource, qui assure le suivi de l’activité du Moniteur de ressource, par exemple si une forte sollicitation de la mémoire a été ou non signalée. Cette mémoire tampon en anneau contient des informations d’état dépendant de la condition actuelle de *RESOURCE_MEMPHYSICAL_HIGH*, *RESOURCE_MEMPHYSICAL_LOW*, *RESOURCE_MEMPHYSICAL_STEADY* ou *RESOURCE_MEMVIRTUAL_LOW*.
- La mémoire tampon Gestionnaire d’allocation mémoire, qui contient des enregistrements des notifications de mémoire pour chaque pool de ressources de Resource Governor. Comme une forte sollicitation de la mémoire interne est détectée, une notification de mémoire insuffisante est activée pour les composants qui allouent de la mémoire, de façon à déclencher des actions destinées à équilibrer la mémoire entre les caches. 

Les gestionnaires d’allocation mémoire surveillent la consommation de la demande de mémoire par chaque composant et, en fonction des informations collectées, ils calculent une valeur optimale de mémoire pour chacun de ces composants. Il existe un ensemble de gestionnaires pour chaque pool de ressources du Resource Governor. Ces informations sont diffusées auprès de chacun des composants, qui augmente ou diminue son utilisation en fonction de ce qui lui est indiqué.
Pour plus d’informations sur les gestionnaires d’allocation mémoire, consultez [sys.dm_os_memory_brokers](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-brokers-transact-sql.md). 

### <a name="error-detection"></a>Détection d'erreurs  
Les pages de base de données peuvent faire appel à deux mécanismes facultatifs qui permettent de garantir l'intégrité de la page depuis son écriture sur le disque à sa relecture : les protections de la somme de contrôle et de la page endommagée. Ces mécanismes offrent une méthode indépendante de vérification de l'exactitude du stockage des données ainsi que des composants matériels tels que les contrôleurs, les pilotes, les câbles et même le système d'exploitation. La protection est ajoutée à la page juste avant l'écriture sur le disque, puis elle est vérifiée après sa lecture sur le disque.

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] procède à quatre nouvelles tentatives pour une lecture qui échoue avec une erreur de somme de contrôle, de page endommagée ou d'E/S disque. Si la lecture réussit lors d'une de ces tentatives, un message est écrit dans le journal des erreurs et l'exécution de la commande qui a déclenché la lecture se poursuit. Si les tentatives de lecture échouent, la commande échoue elle aussi avec le message d'erreur 824. 

Le type de protection de page utilisé est un attribut de la base de données qui contient la page. La protection de la somme de contrôle est la protection par défaut pour les bases de données créées dans [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] et les versions ultérieures. Le mécanisme de protection de page est spécifié au moment de la création de la base de données et peut être modifié avec ALTER DATABASE SET. Vous pouvez déterminer le paramètre de protection de page en cours en interrogeant la colonne *page_verify_option* de l’affichage catalogue [sys.databases](../relational-databases/system-catalog-views/sys-databases-transact-sql.md) ou la propriété *IsTornPageDetectionEnabled* de la fonction [DATABASEPROPERTYEX](../t-sql/functions/databasepropertyex-transact-sql.md). 

> [!NOTE]
> En cas de modification du paramètre de protection de page, le nouveau paramètre ne prend pas immédiatement effet dans l'ensemble de la base de données. Par contre, les pages adoptent le niveau de protection en cours de la base de données lors de leur écriture ultérieure. Cela signifie que la base de données peut contenir des pages utilisant différents types de protection. 

#### <a name="torn-page-protection"></a>Protection de page endommagée  
La protection de page endommagée, introduite dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2000, est essentiellement une méthode de détection des pages endommagées causées par les pannes d’alimentation. Par exemple, une panne d'alimentation inattendue peut n'entraîner que l'écriture partielle d'une page sur le disque. Quand la protection contre les pages endommagées est utilisée, un modèle de signature 2 bits spécifique pour chaque secteur de 512 octets d’une page de base de données de 8 kilo-octets (Ko) est stocké dans l’en-tête de la page quand celle-ci est écrite sur le disque. Lorsque la page est ensuite lue à partir du disque, les bits endommagés stockés dans l'en-tête de la page sont comparés aux informations réelles du secteur concerné. Le modèle de signature alterne entre des binaires 01 et 10 à chaque opération d’écriture. Il est donc toujours possible de savoir si seule une partie des secteurs a été écrite sur le disque : si un bit est dans le mauvais état lors de la lecture ultérieure de la page, celle-ci a été écrite de manière incorrecte et une page endommagée est détectée. Ce type de détection fait appel à des ressources minimales ; cependant, elle ne détecte pas toutes les erreurs causées par les pannes de matériel des disques. Pour plus d’informations sur la définition de la détection des pages endommagées, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify).

#### <a name="checksum-protection"></a>Protection de la somme de contrôle  
La protection de la somme de contrôle, introduite dans [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)], fournit une vérification renforcée de l’intégrité des données. Une somme de contrôle est calculée pour les données de chaque page écrite, elle est stockée dans l'en-tête de page. À chaque lecture d'une page contenant une somme de contrôle stockée sur le disque, le moteur de la base de données recalcule la somme de contrôle pour les données de la page et renvoie l'erreur 824 si la nouvelle somme de contrôle n'est pas identique à la somme de contrôle stockée. La protection de la somme de contrôle peut détecter un plus grand nombre d'erreurs que la protection de page endommagée car celle-ci est affectée par chaque octet de la page, elle utilise toutefois peu de ressources. Lorsque la somme de contrôle est activée, les erreurs causées par les pannes d'alimentation et du matériel ou des microprogrammes défectueux sont détectables à chaque lecture d'une page sur le disque par le gestionnaire de tampons. Pour plus d’informations sur la définition de la somme de contrôle, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify).

> [!IMPORTANT]
> Quand une base de données utilisateur ou système est mise à niveau vers [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)] ou une version ultérieure, la valeur de [PAGE_VERIFY](../t-sql/statements/alter-database-transact-sql-set-options.md#page_verify) (NONE ou TORN_PAGE_DETECTION) est conservée. Nous vous recommandons d'utiliser CHECKSUM.
> Même si la valeur TORN_PAGE_DETECTION utilise moins de ressources, elle ne fournit qu'un sous-ensemble limité de la protection offerte par CHECKSUM.

## <a name="understanding-non-uniform-memory-access"></a>Présentation de l'accès NUMA (Non-uniform Memory Access)
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est compatible avec la technologie NUMA (Non-Uniform Memory Access) et fonctionne correctement avec l'accès NUMA matériel sans configuration particulière. À mesure que la vitesse et le nombre de processeurs augmentent, il devient de plus en plus difficile de réduire le temps de réponse de la mémoire requis pour exploiter cette puissance de traitement supplémentaire. Pour contourner ce problème, les fournisseurs de matériel proposent des caches L3 de grande capacité, mais cette solution présente des limites. L’architecture NUMA fournit une solution évolutive à ce problème. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a été conçu pour tirer parti des ordinateurs reposant sur la technologie NUMA sans qu’il soit nécessaire d’apporter des modifications aux applications. Pour plus d’informations, consultez [Procédure : Configurer SQL Serveur pour utiliser Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md).

## <a name="dynamic-partition-of-memory-objects"></a>Partition dynamique d’objets mémoire
Les allocateurs de tas, appelés objets mémoire dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], permettent à [!INCLUDE[ssde_md](../includes/ssde_md.md)] d’allouer de la mémoire à partir du tas. Celles-ci peuvent être suivies à l’aide de la vue de gestion dynamique [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md). CMemThread est un type d’objet mémoire thread-safe qui autorise les allocations de mémoire simultanées à partir de plusieurs threads. Pour un suivi correct, les objets CMemThread s’appuient sur des constructions de synchronisation (un mutex) pour s’assurer qu’un seul thread met à jour des éléments d’information critiques à la fois. 

> [!NOTE]
> Le type d’objet CMemThread est utilisé dans tout la base de code [!INCLUDE[ssde_md](../includes/ssde_md.md)] pour de nombreuses allocations, et peut être partitionné globalement, par nœud ou par UC.   

Toutefois, l’utilisation de mutex peut entraîner une contention si de nombreux threads sont alloués à partir du même objet mémoire d’une manière hautement simultanée. Par conséquent, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a le concept d’objets de mémoire partitionnée (PMO) et chaque partition est représentée par un objet CMemThread unique. Le partitionnement d’un objet mémoire est défini statiquement et ne peut pas être modifié après la création. Comme les modèles d’allocation de mémoire varient largement en fonction des aspects tels que l’utilisation du matériel et de la mémoire, il est impossible de trouver le modèle de partitionnement parfait au préalable. Dans la grande majorité des cas, l’utilisation d’une partition unique suffira, mais dans certains scénarios cela peut entraîner une contention qui peut être évitée uniquement avec un objet mémoire fortement partitionné. Il n’est pas souhaitable de partitionner chaque objet mémoire, car davantage de partitions peuvent entraîner d’autres inefficacités et augmenter la fragmentation de la mémoire.

> [!NOTE]
> Avant [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], l’indicateur de trace 8048 peut être utilisé pour forcer un PMO basé sur des nœuds à devenir un PMO basé sur le processeur. À compter de SP2 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] et [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], ce comportement est dynamique et contrôlé par le moteur.

À compter de SP2 [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] et [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], le [!INCLUDE[ssde_md](../includes/ssde_md.md)] peut détecter de manière dynamique la contention sur un objet CMemThread spécifique et promouvoir l’objet en une implémentation par nœud ou par processeur.  Une fois promu, le PMO reste promu jusqu’au redémarrage du processus de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La contention de CMemThread peut être détectée par la présence des hautes attentes CMEMTHREAD dans la DMV [sys.dm_os_wait_stats](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) et en observant les colonnes de la DMV [sys.dm_os_memory_objects](../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md) *contention_factor*, *partition_type*, *exclusive_allocations_count* et *waiting_tasks_count*.

## <a name="see-also"></a>Voir aussi
[Mémoire du serveur (option de configuration de serveur)](../database-engine/configure-windows/server-memory-server-configuration-options.md)   
[Lecture de pages](../relational-databases/reading-pages.md)   
[Écriture de pages](../relational-databases/writing-pages.md)   
[Procédure : configurer SQL Server pour utiliser Soft-NUMA](../database-engine/configure-windows/soft-numa-sql-server.md)   
[Exigences liées à l’utilisation des tables à mémoire optimisée](../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)   
[Résoudre les problèmes de mémoire insuffisante à l’aide de tables à mémoire optimisée](../relational-databases/in-memory-oltp/resolve-out-of-memory-issues.md)