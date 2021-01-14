---
title: Éditions et fonctionnalités prises en charge
titleSuffix: SQL Server 2016
description: Cet article décrit les fonctionnalités prises en charge par les différentes éditions de SQL Server 2016, qui répondent à différents besoins en termes de performances, de runtime et de prix.
ms.custom:
- seo-lt-2019
- sqlfreshmay19
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
- CPSA
ms.assetid: e5186f02-dd91-47d0-8fa4-de3f41c76903
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 80b93a4a55b6d12ee5177230b78447c21fc0cae8
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172531"
---
# <a name="editions-and-supported-features-of-sql-server-2016"></a>Éditions et fonctionnalités prises en charge de SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

Cette rubrique fournit des détails sur les fonctionnalités prises en charge par les éditions de SQL Server.

Pour plus d’informations sur les autres versions, consultez :

* [SQL Server 2019](editions-and-components-of-sql-server-version-15.md).  
* [SQL Server 2017](editions-and-components-of-sql-server-2017.md).  
* [SQL Server 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true).

La configuration requise pour l'installation varie selon vos besoins applicatifs. Les différentes éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'adaptent aux exigences de chaque organisation et de chaque individu en termes de performances, d'exécution et de prix. Les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que vous installez dépendent également de vos exigences spécifiques. Les sections suivantes vous aident à choisir parmi les éditions et les composants disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

La version d’évaluation de SQL Server est disponible pendant une période d’évaluation de 180 jours.  
  
Pour obtenir les notes de publication les plus récentes et des informations sur les nouveautés, consultez les rubriques suivantes :
- [Notes de publication de SQL Server 2017](../sql-server/sql-server-2017-release-notes.md)
- [Notes de publication de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md)
- [Nouveautés de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Nouveautés de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)

### <a name="try-sql-server"></a>Essayez SQL Server !    
    
> [![Télécharger à partir du Centre d’évaluation](/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) **[Télécharger SQL Server 2016 à partir du Centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**    
> 
> ![Petite machine virtuelle Azure](/analysis-services/analysis-services/media/azure-virtual-machine-small.png) **[Faites tourner une machine virtuelle sur laquelle SQL Server 2016 est déjà installé](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)**   
  
## <a name="ssnoversion-editions"></a>Éditions de[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]  
 Le tableau ci-dessous décrit les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 
  
|Édition de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Définition|  
|---------------------------------------|----------------|  
|Entreprise|Offre Premium, l’édition [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise fournit des fonctions Datacenter avancées et complètes avec des performances ultrarapides, une virtualisation illimitée et des fonctions Business Intelligence de bout en bout qui autorisent un haut niveau de service pour les charges de travail critiques et l’accès de l’utilisateur aux insights de données.|  
|Standard|L’édition[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard permet la gestion des données de base et inclut une base de données Business Intelligence destinée aux services des grandes entreprises comme aux PME, leur permettant d’exécuter les applications et prenant en charge des outils de développement communs locaux et dans le cloud, pour une gestion efficace des bases de données avec des ressources informatiques minimales.|  
|Web|L’édition Web [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] est une option offrant un coût total de possession faible destinée aux hébergeurs web et aux VAP web, fournissant des fonctions scalables, rentables et gérables aux propriétés web à petite ou grande échelle.|  
|Développeur|L'édition[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer permet aux développeurs de créer des applications basées sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il inclut toutes les fonctionnalités de l'édition Enterprise, mais sa licence permet uniquement de l'utiliser comme un système de développement et de test, et non comme un serveur de production. L'édition [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Développeur est la solution idéale pour les personnes qui génèrent [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] et testent des applications.|  
|Éditions Express|L’édition Express est une édition de base comprenant une base de données gratuite, idéale pour découvrir et créer des applications bureautiques et de petites applications serveur pilotées par les données. C'est la solution idéale pour les éditeurs de logiciels, les développeurs et les amateurs de création d'applications clientes. Si vous avez besoin de fonctionnalités de base de données plus évoluées, vous pouvez mettre à niveau de manière transparente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express vers des versions plus sophistiquées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La nouveauté de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est Express LocalDB, une version allégée d’Express qui conserve toutes les fonctions de programmabilité de ce dernier, s’exécute en mode utilisateur, s’installe rapidement sans aucune configuration et n’exige que peu de prérequis.|  
  
## <a name="using-ssnoversion-with-an-internet-server"></a>Utilisation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec un serveur Internet  
 Sur un serveur Internet, comme un serveur exécutant les services Internet (IIS), vous installez généralement les outils clients [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Les outils clients comprennent les composants de connectivité clients utilisés par une application qui se connecte à une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!NOTE]
> Bien que vous puissiez installer une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant les services Internet (IIS), ceci ne se fait en général que pour des sites Web de petite taille qui ne possèdent qu'un seul ordinateur serveur. La plupart des sites Web disposent de leur système IIS de niveau intermédiaire sur un serveur ou sur un cluster de serveurs, et de leurs bases de données sur un serveur distinct ou sur une fédération distincte de serveurs.  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Utilisation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec des applications client/serveur  
 Vous pouvez installer uniquement les composants clients de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant des applications client/serveur qui se connectent directement à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Une installation de composants clients est également un bon choix si vous administrez une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un serveur de base de données ou si vous prévoyez de développer des applications [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 L'option d'outils clients installe les fonctionnalités suivantes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] : composants de compatibilité descendante, [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], composants de connectivité, outils d'administration, kit de développement logiciel et composants de la documentation en ligne de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez  [Installer SQL Server](../database-engine/install-windows/install-sql-server.md).  
  
## <a name="deciding-among-ssnoversion-components"></a>Choix des composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  
 Utilisez la page de sélection de composant de l'Assistant Installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour sélectionner les composants à inclure dans une installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Par défaut, aucune des fonctionnalités incluses dans l'arborescence n'est sélectionnée.  
  
 Utilisez les informations des tableaux suivants pour déterminer le jeu de fonctionnalités qui répond le mieux à vos besoins.  
  
|Composants serveur|Description|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] inclut [!INCLUDE[ssDE](../includes/ssde-md.md)], le service principal de stockage, de traitement et de protection des données, la réplication, la recherche en texte intégral, des outils de gestion de données relationnelles et XML, l’intégration analytique dans la base de données et l’intégration de PolyBase pour accéder à Hadoop et à d’autres sources de données hétérogènes, ainsi que le serveur [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS).|  
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] inclut les outils de création et de gestion d’applications de traitement analytique en ligne (OLAP, OnLine Analytical Processing) et d’exploration de données.|  
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] inclut les composants serveur et clients permettant de créer, de gérer et de déployer des rapports tabulaires, de matrice, graphiques et de forme libre. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est également une plateforme évolutive que vous pouvez utiliser pour développer des applications de création de rapports.|  
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] propose un ensemble d’outils graphiques et d’objets programmables permettant de déplacer, de copier et de transformer les données. Il inclut également le composant [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) pour [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) est la solution [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de gestion des données de référence. MDS peut être configuré pour gérer tout domaine (produits, clients, comptes) et inclut des hiérarchies, une sécurité granulaire, des transactions, la gestion des versions des données et les règles métier, ainsi que [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] , pouvant être utilisé pour gérer les données.|  
|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)]|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)] prend en charge les solutions R évolutives et distribuées sur plusieurs plateformes et avec plusieurs sources de données d’entreprise, notamment Linux, Hadoop et Teradata.|  
  
|Outils d'administration|Description|  
|----------------------|-----------------|  
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] est un environnement intégré permettant d'accéder aux composants de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]en vue de les configurer, de les gérer, de les administrer et de les développer. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] permet aux développeurs et aux administrateurs de tous niveaux de compétence d'utiliser [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].<br /><br /> Télécharger et installer <br />                [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] à partir de  [Télécharger SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Gestionnaire de configuration|Le Gestionnaire de configuration[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permet de gérer la configuration de base des services [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , des protocoles serveur et clients et des alias clients.|  
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] fournit une interface utilisateur graphique qui permet d’analyser une instance du [!INCLUDE[ssDE](../includes/ssde-md.md)] ou d’ [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|  
|Assistant Paramétrage du[!INCLUDE[ssDE](../includes/ssde-md.md)]|L'Assistant Paramétrage du[!INCLUDE[ssDE](../includes/ssde-md.md)] permet de créer des ensembles d'index, des vues indexées et des partitions optimaux.|  
|Data Quality Client|Fournit une interface utilisateur graphique très facile et intuitive pour se connecter au serveur DQS et effectue des opérations de nettoyage des données. Il vous permet également de surveiller de façon centralisée différentes activités effectuées pendant l'opération de nettoyage des données.|  
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] fournissent un environnement de développement intégré (IDE) permettant de générer des solutions pour les composants Business Intelligence : [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], et [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Anciennement, Business Intelligence Development Studio.)<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] inclut également des « projets de base de données », qui offrent un environnement intégré aux développeurs, afin qu'ils puissent mener à bien leurs travaux de création de bases de données quelle que soit la plateforme [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (sur site et hors site) dans Visual Studio. Les développeurs de bases de données peuvent utiliser l'explorateur de serveurs amélioré de Visual Studio pour créer ou modifier facilement des objets de base de données et des données, ou exécuter des requêtes.|  
|Composants de connectivité|Installe des composants permettant la communication entre les clients et les serveurs, et des bibliothèques réseau pour DB-Library, ODBC et OLE DB.|  
  
|Documentation|Description|  
|-------------------|-----------------|  
|Documentation en ligne[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Documentation de base de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].| 

**Éditions Developer et Evaluation**  
Pour connaître les fonctionnalités prises en charge par les éditions Developer et Evaluation, consultez les fonctionnalités répertoriées pour SQL Server Entreprise Edition dans les tableaux ci-dessous.


L’édition Developer continue à prendre en charge seulement 1 client pour [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Scale Limits  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|Limité à moins de 1 socket ou 4 cœurs| 
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ou [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|Limité à moins de 1 socket ou 4 cœurs|  
|Mémoire maximale du pool de mémoires tampons par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum du système d’exploitation|128 Go|64 Go|1410 Mo|1410 Mo|
|Mémoire maximale du cache de segments columnstore par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go<sup>2</sup>| 16 Go<sup>2</sup>| 352 Mo<sup>2</sup>| 352 Mo<sup>2</sup>|  
|Taille maximale des données à mémoire optimisée par base de données dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go<sup>2</sup>| 16 Go<sup>2</sup>| 352 Mo<sup>2</sup>| 352 Mo<sup>2</sup>|  
|Mémoire maximale utilisée par instance de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Maximum du système d’exploitation|Tabulaire : 16 Go<br /><br /> MOLAP : 64 Go|N/A|N/A|N/A|  
|Mémoire maximale utilisée par instance de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d’exploitation|64 Go|64 Go|4 Go|N/A|
|Taille maximale de la base de données relationnelle|524 Po|524 Po|524 Po|10 Go|10 Go|  
  
<sup>1</sup> L’édition Enterprise avec serveur + licences d’accès client (CAL) (non disponibles pour les nouveaux contrats) est limitée à un maximum de 20 cœurs par instance SQL Server. Il n'existe aucune limite dans le mode de licence Serveur selon le nombre de cœurs. Pour plus d’informations, voir [Compute Capacity Limits by Edition of SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
<sup>2</sup> S’applique à [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] SP1 dans le cadre de la création d’une surface de programmabilité commune (CPSA, Common Programmability Surface Area) entre différentes éditions. 

##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> RDBMS High Availability  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Prise en charge de Server Core <sup>1</sup>|Oui|Oui|Oui|Oui|Oui|  
|Copie des journaux de transaction|Oui|Oui|Oui|Non|Non|  
|Mise en miroir de bases de données|Oui|Oui<br /><br /> Sécurité complète uniquement|Témoin uniquement|Témoin uniquement|Témoin uniquement| 
|Compression de sauvegarde|Oui|Oui|Non|Non|Non| 
|Instantané de base de données|Oui|Oui <sup>3</sup>|Oui <sup>3</sup>|Oui <sup>3</sup>|Oui <sup>3</sup>|
|Instances de cluster de basculement Always On|Oui<br /><br /> 16|Oui<br /><br /> Prise en charge de 2 nœuds|Non|Non|Non|  
|Groupes de disponibilité Always On|Oui<br /><br /> Jusqu’à 8 réplicas secondaires, notamment 2 réplicas secondaires synchrones|Non|Non|Non|Non|
|Groupes de disponibilité de base <sup>2</sup>|Non|Oui<br /><br /> Prise en charge de 2 nœuds|Non|Non|Non|
|Restauration en ligne de pages et de fichiers|Oui|Non|Non|Non|Non|
|Indexation en ligne|Oui|Non|Non|Non|Non|
|Modification de schéma en ligne|Oui|Non|Non|Non|Non|
|Récupération rapide|Oui|Non|Non|Non|Non|
|Sauvegardes en miroir|Oui|Non|Non|Non|Non|
|Ajout de mémoire et de processeur à chaud|Oui|Non|Non|Non|Non|
|Assistant de récupération de base de données|Oui|Oui|Oui|Oui|Oui|
|Sauvegarde chiffrée|Oui|Oui|Non|Non|Non|
|Sauvegarde hybride vers Azure (sauvegarde vers une URL)|Oui|Oui|Non|Non|Non|  
  
 <sup>1</sup> Pour plus d’informations sur l’installation de SQL Server sur Server Core, consultez [Installer SQL Server sur Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md). 

<sup>2</sup> Pour plus d’informations sur les groupes de disponibilité de base, consultez [Groupes de disponibilité de base](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).  

<sup>3</sup> S’applique à [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 2016 SP1 dans le cadre de la création d’une surface de programmabilité commune entre différentes éditions. 
  
##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> RDBMS Scalability and Performance  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Columnstore <sup>1</sup>|Oui|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui<sup>2</sup>|Oui<sup>2</sup>|  
|OLTP en mémoire <sup>1</sup>|Oui|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui <sup>2</sup>, <sup>3</sup>|Oui <sup>2</sup>|
|Stretch Database|Oui|Oui|Oui|Oui|Oui|
|Mémoire principale persistante|Oui|Oui|Oui|Oui|Oui|
|Prise en charge de plusieurs instances|50|50|50|50|50|
|Partitionnement des tables et des index|Oui|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui <sup>2</sup>|  
|Compression des données|Oui|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui <sup>2</sup>|
|gouverneur de ressources|Oui|Non|Non|Non|Non|  
|Parallélisme de tables partitionnées|Oui|Non|Non|Non|Non|
|Plusieurs conteneurs Filestream|Oui|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui <sup>2</sup>|Oui <sup>2</sup>|
|Mémoire de pages de grande taille compatible NUMA et allocation de tableau de mémoires tampons|Oui|Non <sup>4</sup>|Non|Non|Non|
|Buffer Pool Extension|Oui|Oui|Non|Non|Non|
|Gouvernance des ressources d’E/S|Oui|Non|Non|Non|Non|
|Lecture anticipée|Oui|Non|Non|Non|Non|
|Analyse avancée|Oui|Non|Non|Non|Non|
|Durabilité différée|Oui|Oui|Oui|Oui|Oui|

<sup>1</sup> La taille des données OLTP en mémoire et le cache de segments columnstore sont limités à la quantité de mémoire spécifiée par l’édition dans la section Limites d’échelle. Le Nombre maximal de degrés de parallélisme est limité. Le nombre de degrés de parallélisme maximal pour la création d’un index est limité à 2 pour l’Édition Standard et à 1 pour les éditions Web et Express. Ceci fait référence aux index columnstore créés sur des tables basées sur des disques et des tables à mémoire optimisée.

<sup>2</sup> S’applique à [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] SP1 dans le cadre de la création d’une surface de programmabilité commune entre différentes éditions. Pushdown d’agrégats, pushdown de prédicats de chaîne et optimisations SIMD sont des améliorations de l’évolutivité de l’Édition Entreprise [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Index columnstore - Nouveautés](../relational-databases/indexes/columnstore-indexes-what-s-new.md). 

<sup>3</sup> Cette fonctionnalité n’est pas incluse dans l’option d’installation LocalDB.

<sup>4</sup> La licence Standard Edition et SQL Server + CAL peut limiter le nombre de processeurs utilisables par SQL Server Standard, mais SQL Server Standard est compatible NUMA. 
##  <a name="rdbms-security"></a><a name="RDBMSS"></a> RDBMS Security  
  
|Fonctionnalité|Entreprise|Standard|Web|Express|Express with Advanced Services|  
|-------------|----------------|--------------|---------|-------------|------------------------------------| 
|Sécurité au niveau des lignes|Oui|Oui|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup>|  
|Always Encrypted|Oui|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup>| 
|Masquage dynamique des données|Oui|Oui|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup>|   
|Audit de serveur|Oui|Oui|Oui|Oui|Oui| 
|Audit de base de données|Oui|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup>|Oui <sup>1</sup>| 
|Chiffrement transparent de base de données|Oui|Non|Non|Non|Non|   
|Gestion de clés extensible|Oui|Non|Non|Non|Non| 
|Rôles définis par l’utilisateur|Oui|Oui|Oui|Oui|Oui| 
|Bases de données autonomes|Oui|Oui|Oui|Oui|Oui| 
|Chiffrement des sauvegardes|Oui|Oui|Non|Non|Non|  

<sup>1</sup> S’applique à [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] SP1 dans le cadre de la création d’une surface de programmabilité commune entre différentes éditions.      

##  <a name="replication"></a>Réplication<a name="Replication"></a>  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Abonnés hétérogènes|Oui|Oui|Non|Non|Non|  
|Réplication de fusion|Oui|Oui|Oui (abonné uniquement)|Oui (abonné uniquement)|Oui (abonné uniquement)|   
|Publication Oracle|Oui|Non|Non|Non|Non| 
|Réplication transactionnelle d’égal à égal|Oui|Non|Non|Non|Non|   
|Réplication d'instantané|Oui|Oui|Oui (abonné uniquement)|Oui (abonné uniquement)|Oui (abonné uniquement)|   
|Suivi des modifications SQL Server|Oui|Oui|Oui|Oui|Oui| 
|Réplication transactionnelle|Oui|Oui|Oui (abonné uniquement)|Oui (abonné uniquement)|Oui (abonné uniquement)|   
|Réplication transactionnelle vers Azure|Oui|Oui|Non|Non|Non|   
|Abonnement pouvant être mis à jour à la réplication transactionnelle|Oui|Oui|Non|Non|Non|  
  
##  <a name="management-tools"></a><a name="SSMS"></a> Management Tools  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Objets de gestion SQL (SMO)|Oui|Oui|Oui|Oui|Oui|  
|Gestionnaire de configuration SQL|Oui|Oui|Oui|Oui|Oui|   
|SQL CMD (outil d'invite de commandes)|Oui|Oui|Oui|Oui|Oui|      
|Distributed Replay - Outil d’administration|Oui|Oui|Oui|Oui|Non|  
|Distribute Replay - Client|Oui|Oui|Oui|Non|Non|  
|Distributed Replay - Contrôleur|Oui (jusqu’à 16 clients)|Oui (1 client)|Oui (1 client)|Non|Non|   
|SQL Profiler|Oui|Oui|Non <sup>1</sup>|Non <sup>1</sup>|Non <sup>1</sup>|  
|SQL Server Agent|Oui|Oui|Oui|Non|Non| 
|Pack d'administration Microsoft System Center Operations Manager|Oui|Oui|Oui|Non|Non|  
|Assistant Paramétrage de base de données (DTA)|Oui|Oui <sup>2</sup>|Oui <sup>2</sup>|Non|Non|      
  
 <sup>1</sup> SQL Server Web, SQL Server Express, SQL Server Express with Tools et SQL Server Express with Advanced Services peuvent être profilés à l’aide de SQL Server Standard et SQL Server Entreprise.  
  
 <sup>2</sup> Paramétrage activé uniquement sur les fonctionnalités de l’édition Standard  
  
##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> RDBMS Manageability  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Instances utilisateur|Non|Non|Non|Oui|Oui| 
|LocalDB|Non|Non|Non|Oui|Non| 
|Connexion administrateur dédiée|Oui|Oui|Oui|Oui avec indicateur de trace|Oui avec indicateur de trace|   
|Prise en charge de scripts PowerShell|Oui|Oui|Oui|Oui|Oui| 
|Prise en charge de SysPrep <sup>1</sup>|Oui|Oui|Oui|Oui|Oui| 
|Prise en charge des opérations des composants d’application du niveau Données : extraction, déploiement, mise à niveau, suppression|Oui|Oui|Oui|Oui|Oui| 
|Automation de stratégie (vérification selon la planification et sur modification)|Oui|Oui|Oui|Non|Non|   
|Collecteur de données de performances|Oui|Oui|Oui|Non|Non| 
|Possibilité d’inscription en tant qu’instance managée dans le cadre de la gestion de plusieurs instances|Oui|Oui|Oui|Non|Non|   
|Rapports de performances standard|Oui|Oui|Oui|Non|Non| 
|Repères de plan et gel de plan relatif|Oui|Oui|Oui|Non|Non|   
|Requête directe de vues d'index (à l'aide de l'indicateur NOEXPAND)|Oui|Oui|Oui|Oui|Oui| 
|Maintenance automatique des vues indexées|Oui|Oui|Oui|Non|Non| 
|Vues partitionnées distribuées|Oui|Non|Non|Non|Non| 
|Opérations d'index parallèles|Oui|Non|Non|Non|Non|  
|Utilisation automatique de vues indexées par l'optimiseur de requête|Oui|Non|Non|Non|Non| 
|Vérifications de cohérence parallèles|Oui|Non|Non|Non|Non| 
|Point de contrôle de l’utilitaire SQL Server|Oui|Non|Non|Non|Non|    
|Extension du pool de mémoires tampons|Oui|Oui|Non|Non|Non| 
  
 <sup>1</sup> Pour plus d’informations, consultez [Considérations relatives à l’installation de SQL Server à l’aide de SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
 
<sup>2</sup> S’applique à [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] SP1 dans le cadre de la création d’une surface de programmabilité commune (CPSA, Common Programmability Surface Area) entre différentes éditions.      
  
##  <a name="development-tools"></a><a name="DevTools"></a> Development Tools  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Intégration de Microsoft Visual Studio|Oui|Oui|Oui|Oui|Oui| 
|Intellisense (Transact-SQL et MDX)|Oui|Oui|Oui|Oui|Oui| 
|SQL Server Data Tools (SSDT)|Oui|Oui|Oui|Oui|Non|    
|Outils de conception, de modification et de débogage MDX|Oui|Oui|Non|Non|Non|   
  
##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Intégration R de base|Oui|Oui|Oui|Oui|Non|   
|Intégration R avancée|Oui|Non|Non|Non|Non| 
|R Server (autonome)|Oui|Non|Non|Non|Non|   
|Nœud de calcul PolyBase|Oui|Oui <sup>1</sup>|Oui <sup>1</sup>, <sup>2</sup>|Oui <sup>1</sup>, <sup>2</sup>|Oui <sup>1</sup>, <sup>2</sup>| 
|Nœud principal PolyBase|Oui|Non|Non|Non|Non| 
|JSON|Oui|Oui|Oui|Oui|Oui|   
|Magasin des requêtes|Oui|Oui|Oui|Oui|Oui|   
|Temporal|Oui|Oui|Oui|Oui|Oui|   
|Intégration du CLR (Common Language Runtime)|Oui|Oui|Oui|Oui|Oui|   
|Prise en charge XML native|Oui|Oui|Oui|Oui|Oui| 
|Indexation XML|Oui|Oui|Oui|Oui|Oui| 
|Fonctions MERGE & UPSERT|Oui|Oui|Oui|Oui|Oui|   
|prise en charge de FILESTREAM|Oui|Oui|Oui|Oui|Oui| 
|FileTable|Oui|Oui|Oui|Oui|Oui| 
|Types de données de date et d'heure|Oui|Oui|Oui|Oui|Oui|  
|Support d'internationalisation|Oui|Oui|Oui|Oui|Oui| 
|Recherche sémantique et en texte intégral|Oui|Oui|Oui|Oui|Non| 
|Spécification d'une langue dans une requête|Oui|Oui|Oui|Oui|Non|   
|Service Broker (messagerie)|Oui|Oui|Non (client uniquement)|Non (client uniquement)|Non (client uniquement)|   
|Transact-SQL, points de terminaison|Oui|Oui|Oui|Non|Non| 

<sup>1</sup> Le scale-out avec plusieurs nœuds de calcul nécessite un nœud principal.

<sup>2</sup> S’applique à [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] SP1 dans le cadre de la création d’une surface de programmabilité commune (CPSA, Common Programmability Surface Area) entre différentes éditions.     
  
## <a name="integration-services"></a><a name="IS"></a> Integration Services

Pour plus d’informations sur les fonctionnalités Integration Services (SSIS) prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Integration Services prises en charge par les éditions de SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="master-data-services"></a><a name="MDS"></a> Master Data Services  
 Pour plus d’informations sur les fonctionnalités [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] et Data Quality Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Master Data Services et Data Quality Services prises en charge par les éditions de SQL Server](../master-data-services/master-data-services-and-data-quality-services-features-support.md). 

  
##  <a name="data-warehouse"></a><a name="DW"></a> Data Warehouse  
  
|Fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Création de cubes sans une base de données|Oui|Oui|Non|Non|Non |   
|Génération automatique de la mise en lots et du schéma d’entrepôt de données|Oui|Oui|Non|Non|Non| 
|Capture des données modifiées|Oui|Oui <sup>1</sup>|Non|Non|Non| 
|Optimisations de requêtes de jointure en étoile|Oui|Non|Non|Non|Non| 
|Configuration en lecture seule évolutive d'Analysis Services|Oui|Non|Non|Non|Non| 
|Traitement des requêtes parallèles sur les tables et les index partitionnés|Oui|Non|Non|Non|Non|   
|Agrégation globale des traitements|Oui|Non|Non|Non|Non| 

<sup>1</sup> S’applique à [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] SP1 dans le cadre de la création d’une surface de programmabilité commune entre différentes éditions.     

##  <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services  
  
Pour plus d’informations sur les fonctionnalités Analysis Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016). 
  
##  <a name="bi-semantic-model-multi-dimensional"></a><a name="BIMD"></a> BI Semantic Model (Multi Dimensional)  
  
Pour plus d’informations sur les fonctionnalités Analysis Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
   
##  <a name="bi-semantic-model-tabular"></a><a name="BIT"></a> BI Semantic Model (Tabular)  
  
Pour plus d’informations sur les fonctionnalités Analysis Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="power-pivot-for-sharepoint"></a><a name="PPSP"></a> Power Pivot for SharePoint  
  
Pour plus d’informations sur les fonctionnalités Power Pivot pour SharePoint prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="data-mining"></a><a name="DM"></a> Data Mining  
  
Pour plus d’informations sur les fonctionnalités d’exploration de données prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services  
  
Pour plus d’informations sur les fonctionnalités Reporting Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

##  <a name="business-intelligence-clients"></a><a name="BIC"></a> Clients Business Intelligence  

Pour plus d’informations sur les fonctionnalités de client Business Intelligence prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les éditions de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016) ou [Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Spatial and Location Services  
  
|Nom de la fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Index spatiaux|Oui|Oui|Oui|Oui|Oui|   
|Types de données planaires et géodésiques|Oui|Oui|Oui|Oui|Oui| 
|Bibliothèques spatiales avancées|Oui|Oui|Oui|Oui|Oui|   
|Importation/exportation de formats de données spatiales standard|Oui|Oui|Oui|Oui|Oui|   
  
##  <a name="additional-database-services"></a><a name="ADS"></a> Additional Database Services  
  
|Nom de la fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Assistant Migration|Oui|Oui|Oui|Oui|Oui|   
|Messagerie de base de données|Oui|Oui|Oui|Non|Non| 
  
##  <a name="other-components"></a><a name="Other"></a> Other Components  
  
|Nom de la fonctionnalité|Entreprise|Standard|Web|Express with Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------|  
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|Non|Non| 
|StreamInsight HA|StreamInsight Premium Edition|Non|Non|Non|Non|   
  
> [![Télécharger SSMS](/analysis-services/analysis-services/media/download.png)](../ssms/download-sql-server-management-studio-ssms.md) **[Télécharger la dernière version de SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)**      
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server](../database-engine/install-windows/install-sql-server.md)  
 
