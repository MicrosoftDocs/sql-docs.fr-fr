---
description: Éditions et fonctionnalités prises en charge de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Éditions et fonctionnalités prises en charge de SQL Server 2019 | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
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
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 9987384899eeb9087a938d8fd8e8b156ff8e1962
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236267"
---
# <a name="editions-and-supported-features-of-sssql19-md"></a>Éditions et fonctionnalités prises en charge de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx](../includes/applies-to-version/sqlserver.md)]

Cette rubrique fournit des détails sur les fonctionnalités prises en charge par les différentes éditions de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)].

Pour plus d’informations sur les versions antérieures, consultez :

* [SQL Server 2017](editions-and-components-of-sql-server-2017.md)
* [SQL Server 2016](editions-and-components-of-sql-server-2016.md)

La configuration requise pour l'installation varie selon vos besoins applicatifs. Les différentes éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'adaptent aux exigences de chaque organisation et de chaque individu en termes de performances, d'exécution et de prix. Les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que vous installez dépendent également de vos exigences spécifiques. Les sections suivantes vous aident à choisir parmi les éditions et les composants disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

L’édition d’évaluation [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est disponible pendant une période d’essai gratuit de 180 jours.

Pour obtenir les notes de publication les plus récentes et des informations sur les nouveautés, consultez les rubriques suivantes :

* [Notes de publication de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]](../sql-server/sql-server-version-15-release-notes.md)
* [Nouveautés de la version [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2019](../sql-server/what-s-new-in-sql-server-ver15.md)

**Essayez [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ! : [Télécharger [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] à partir du Centre d’évaluation](https://www.microsoft.com//evalcenter/evaluate-sql-server)**

## <a name="ssnoversion-editions"></a>Éditions de[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]

Le tableau ci-dessous décrit les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

|Édition de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Définition|
|---------------------------------------|----------------|
|Entreprise|L’offre Premium, l’édition [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise, fournit des fonctionnalités de centre de données avancées et complètes avec des performances ultrarapides, une virtualisation illimitée<sup>1</sup> et des fonctions décisionnelles de bout en bout qui autorisent un haut niveau de service pour les charges de travail stratégiques et l’accès de l’utilisateur aux insights de données.|
|standard|L’édition [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard permet la gestion des données de base et inclut une base de données décisionnelle destinée aux services des grandes et petites entreprises, leur permettant d’exécuter les applications et prenant en charge des outils de développement communs locaux et dans le cloud, pour une gestion efficace des bases de données avec des ressources informatiques minimales.|
|Web|L’édition Web [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] est une option offrant un coût total de possession faible destinée aux hébergeurs web et aux VAP web, fournissant des fonctions scalables, rentables et gérables aux propriétés web à petite ou grande échelle.|
|Développeur|L'édition[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer permet aux développeurs de créer des applications basées sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il inclut toutes les fonctionnalités de l'édition Enterprise, mais sa licence permet uniquement de l'utiliser comme un système de développement et de test, et non comme un serveur de production. L'édition [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer est la solution idéale pour le développement et le test d'applications.|
|Éditions Express|L’édition Express est une édition de base comprenant une base de données gratuite, idéale pour découvrir et créer des applications bureautiques et de petites applications serveur pilotées par les données. C'est la solution idéale pour les éditeurs de logiciels, les développeurs et les amateurs de création d'applications clientes. Si vous avez besoin de fonctionnalités de base de données plus évoluées, vous pouvez mettre à niveau de manière transparente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express vers des versions plus sophistiquées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB, une version allégée d’Express qui conserve toutes les fonctions de programmabilité de ce dernier, s’exécute en mode utilisateur, s’installe rapidement sans aucune configuration et ne comporte qu’une courte liste de prérequis.|

<sup>1</sup> La virtualisation illimitée est disponible dans Enterprise Edition pour les clients disposant de la [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). Les déploiements doivent respecter le guide des licences. Pour plus d’informations, consultez notre page Tarifs et licences.

## <a name="using-ssnoversion-with-clientserver-applications"></a>Utilisation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec des applications client/serveur

Vous pouvez installer uniquement les composants clients de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant des applications client/serveur qui se connectent directement à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Une installation de composants clients est également un bon choix si vous administrez une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un serveur de base de données ou si vous prévoyez de développer des applications [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

L'option d'outils clients installe les fonctionnalités suivantes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] : composants de compatibilité descendante, [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], composants de connectivité, outils d'administration, kit de développement logiciel et composants de la documentation en ligne de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Installer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/install-sql-server.md).

### <a name="running-with-iis"></a>Exécution avec IIS

Sur un serveur Internet, comme un serveur exécutant les services Internet (IIS), vous installez généralement les outils clients [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Les outils clients comprennent les composants de connectivité clients utilisés par une application qui se connecte à une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

>[!NOTE]
>Bien que vous puissiez installer une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant les services Internet (IIS), ceci ne se fait en général que pour des sites Web de petite taille qui ne possèdent qu'un seul ordinateur serveur. La plupart des sites Web disposent de leur système IIS de niveau intermédiaire sur un serveur ou sur un cluster de serveurs, et de leurs bases de données sur un serveur distinct ou sur une fédération distincte de serveurs.

## <a name="deciding-among-ssnoversion-components"></a>Choix des composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Utilisez la page de sélection de composant de l'Assistant Installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour sélectionner les composants à inclure dans une installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Par défaut, aucune des fonctionnalités incluses dans l'arborescence n'est sélectionnée.

Utilisez les informations des tableaux suivants pour déterminer le jeu de fonctionnalités qui répond le mieux à vos besoins.

|Composants serveur|Description|
|-----------------------|-----------------|
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] inclut [!INCLUDE[ssDE](../includes/ssde-md.md)], le service de base du stockage, du traitement et de la protection des données, la réplication, la recherche en texte intégral, des outils de gestion de données relationnelles et XML, l’intégration analytique bases de données et l’intégration PolyBase pour accéder à Hadoop et à d’autres sources de données hétérogènes et Machine Learning Services pour exécuter des scripts Python et R avec des données relationnelles.|
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] inclut les outils de création et de gestion d’applications de traitement analytique en ligne (OLAP, OnLine Analytical Processing) et d’exploration de données.|
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] inclut les composants serveur et clients permettant de créer, de gérer et de déployer des rapports tabulaires, de matrice, graphiques et de forme libre. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] est également une plateforme évolutive que vous pouvez utiliser pour développer des applications de création de rapports.|
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] propose un ensemble d’outils graphiques et d’objets programmables permettant de déplacer, de copier et de transformer les données. Il inclut également le composant [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) pour [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) est la solution [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de gestion des données de référence. MDS peut être configuré pour gérer tout domaine (produits, clients, comptes) et inclut des hiérarchies, une sécurité granulaire, des transactions, la gestion des versions des données et les règles métier, ainsi que [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] , pouvant être utilisé pour gérer les données.|
|Machine Learning Services (en base de données)|Machine Learning Services (en base de données) prend en charge les solutions Machine Learning distribuées et scalables avec des sources de données d’entreprise. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016, le langage R était pris en charge. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] prend en charge R et Python.|
|Machine Learning Server (autonome)|Machine Learning Server (autonome) prend en charge le déploiement de solutions Machine Learning distribuées et évolutives sur plusieurs plateformes et avec plusieurs sources de données d’entreprise, notamment Linux et Hadoop. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016, le langage R était pris en charge. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] prend en charge R et Python.|

|Outils d'administration|Description|
|----------------------|-----------------|
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) est un environnement intégré permettant d'accéder aux composants de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en vue de les configurer, de les gérer, de les administrer et de les développer. SSMS permet aux développeurs et aux administrateurs de tous niveaux de compétence d'utiliser [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La dernière édition de SSMS met à jour SMO, qui comprend l’API S[QL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md).<br /><br/> Télécharger et installer <br />[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] de [Télécharger [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Gestionnaire de configuration|Le Gestionnaire de configuration[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permet de gérer la configuration de base des services [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , des protocoles serveur et clients et des alias clients.|
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] fournit une interface utilisateur graphique qui permet d’analyser une instance du [!INCLUDE[ssDE](../includes/ssde-md.md)] ou d’ [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|
|Assistant Paramétrage du[!INCLUDE[ssDE](../includes/ssde-md.md)]|L'Assistant Paramétrage du[!INCLUDE[ssDE](../includes/ssde-md.md)] permet de créer des ensembles d'index, des vues indexées et des partitions optimaux.|
|Data Quality Client|Fournit une interface utilisateur graphique très facile et intuitive pour se connecter au serveur DQS et effectue des opérations de nettoyage des données. Il vous permet également de surveiller de façon centralisée différentes activités effectuées pendant l'opération de nettoyage des données.|
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] fournissent un environnement de développement intégré (IDE) permettant de générer des solutions pour les composants Business Intelligence : [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], et [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Anciennement, Business Intelligence Development Studio.)<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] inclut également des « projets de base de données », qui offrent un environnement intégré aux développeurs, afin qu'ils puissent mener à bien leurs travaux de création de bases de données quelle que soit la plateforme [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (sur site et hors site) dans Visual Studio. Les développeurs de bases de données peuvent utiliser l'explorateur de serveurs amélioré de Visual Studio pour créer ou modifier facilement des objets de base de données et des données, ou exécuter des requêtes.|
|Composants de connectivité|Installe des composants permettant la communication entre les clients et les serveurs, et des bibliothèques réseau pour DB-Library, ODBC et OLE DB.|

|Documentation|Description|
|-------------------|-----------------|
|Documentation en ligne[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Documentation de base de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|

**Édition Developer et Evaluation** Pour connaître les fonctionnalités prises en charge par les éditions Developer et Evaluation, consultez les fonctionnalités répertoriées Édition Entreprise [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans les tableaux ci-dessous.

L’édition Developer continue à prendre en charge seulement 1 client pour [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md).

## <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Limites d’échelle

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------|--------:|------:|---:|-------------------------------:|-----:|
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|Limité à moins de 1 socket ou 4 cœurs|
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ou [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|Limité à moins de 1 socket ou 4 cœurs|
|Mémoire maximale du pool de mémoires tampons par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum du système d’exploitation|128 <nobr/>Go|64 <nobr/>Go|1 410 <nobr/>Mo|1 410<nobr/> Mo|
|Mémoire maximale du cache de segments columnstore par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 <nobr/>Go| 16 <nobr/>Go| 352 <nobr/>Mo| 352 <nobr/>Mo|
|Taille maximale des données à mémoire optimisée par base de données dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 <nobr/>Go| 16 <nobr/>Go| 352 <nobr/>Mo| 352 <nobr/>Mo|
|Mémoire maximale utilisée par instance de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Maximum du système d’exploitation|16 <nobr/>Go<sup>2</sup><br /><br /> 64 <nobr/>Go<sup>3</sup>|N/A|N/A|N/A|
|Mémoire maximale utilisée par instance de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d’exploitation|64 <nobr/>Go|64 <nobr/>Go|4 <nobr/>Go|N/A|
|Taille maximale de la base de données relationnelle|524 <nobr/> Po|524 <nobr/> Po|524 <nobr/> Po|10 <nobr/>Go|10 <nobr/>Go|

<sup>1</sup> la licence de base Édition Entreprise avec serveur + licences d’accès client (CAL) (non disponible pour les nouveaux contrats) limitées à un maximum de 20 cœurs par instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il n'existe aucune limite dans le mode de licence Serveur selon le nombre de cœurs. Pour plus d’informations, consultez [Limites de capacité de calcul par Édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).

<sup>2</sup> Tabulaire

<sup>3</sup> MOLAP

## <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> Haute disponibilité SGBDR

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|--------------------------------:|:-----:|
|Support de base du serveur<sup>1</sup>|Oui|Oui|Oui|Oui|Oui|
|Copie des journaux de transaction|Oui|Oui|Oui|Non|Non|
|Mise en miroir de bases de données|Oui|Oui<sup>2</sup>|Oui<sup>3</sup>|Oui<sup>3</sup>|Oui<sup>3</sup>|
|Compression de sauvegarde|Oui|Oui|Non|Non|Non|
|Instantané de base de données|Oui|Oui|Oui|Oui|Oui|
|Instances de cluster de basculement Always On<sup>4</sup>|Oui|Oui|Non|Non|Non|
|Groupes de disponibilité Always On<sup>5</sup>|Oui|Non|Non|Non|Non|
|Groupes de disponibilité de base<sup>6</sup>|Non|Oui|Non|Non|Non|
|Réacheminement automatique de la connexion en lecture/écriture |Oui|Non|Non|Non|Non|
|Restauration en ligne de pages et de fichiers|Oui|Non|Non|Non|Non|
|Créer et reconstruire un index en ligne|Oui|Non|Non|Non|Non|
|Reconstructions d’index en ligne pouvant être reprises|Oui|Non|Non|Non|Non|
|Modification de schéma en ligne|Oui|Non|Non|Non|Non|
|Récupération rapide|Oui|Non|Non|Non|Non|
|Récupération de base de données accélérée|Oui|Oui|Oui|Non|Non|
|Sauvegardes en miroir|Oui|Non|Non|Non|Non|
|Ajout de mémoire et de processeur à chaud|Oui|Non|Non|Non|Non|
|Assistant de récupération de base de données|Oui|Oui|Oui|Oui|Oui|
|Sauvegarde chiffrée|Oui|Oui|Non|Non|Non|
|Sauvegarde hybride vers Windows Azure (sauvegarde vers une URL)|Oui|Oui|Oui|Non|Non|
|Groupe de disponibilité sans cluster <sup>5,6</sup>|Oui|Oui|Non|Non|Non|
|Serveurs de basculement pour la récupération d’urgence<sup>7</sup>|Oui|Oui|Non|Non|Non|
|Serveurs de basculement pour la haute disponibilité<sup>7</sup>|Oui|Oui|Non|Non|Non|
|Serveurs de basculement pour la récupération d’urgence dans Azure<sup>7</sup>|Oui|Oui|Non|Non|Non|

<sup>1</sup> Pour plus d’informations sur l’installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Server Core, consultez [Installer [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md).

<sup>2</sup> Sécurité complète uniquement

<sup>3</sup> Témoin uniquement

<sup>4</sup> Dans l’Édition Entreprise, le nombre de nœuds correspond au maximum du système d’exploitation. L’édition Standard prend en charge deux nœuds.

<sup>5</sup> L’Édition Entreprise fournit le support de 8 réplicas secondaires maximum, y compris 5 réplicas secondaires synchrones.

<sup>6</sup> L’Édition Standard prend en charge des groupes de disponibilité de base. Un groupe de disponibilité de base prend en charge deux réplicas, avec une base de données. Pour plus d’informations sur les groupes de disponibilité de base, consultez [Groupes de disponibilité de base](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).

<sup>7</sup> [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) requise.

## <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> Scalabilité et performances SGBDR

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|------:|:--------:|:------:|:-:|:-------------------------------:|:------:|
|Columnstore<sup>1</sup> <sup>2</sup>|Oui|Oui|Oui|Oui|Oui|
|Fichiers binaires LOB dans les index columnstore cluster|Oui|Oui|Oui|Oui|Oui|
|Reconstruction d’index columnstore non cluster en ligne|Oui|Non|Non|Non|Non|
|Base de données en mémoire : OLTP en mémoire<sup>1</sup>|Oui|Oui|Oui|Oui<sup>3</sup>|Oui|
|Base de données en mémoire : pool de mémoires tampons hybride|Oui|Oui|Non|Non|Non|
|Base de données en mémoire : métadonnée tempdb de mémoire optimisée|Oui|Non|Non|Non|Non|
|Base de données en mémoire : support de la mémoire persistante|Oui|Oui|Oui|Oui|Oui|
|Stretch Database|Oui|Oui|Oui|Oui|Oui|
|Prise en charge de plusieurs instances|50|50|50|50|50|
|Partitionnement des tables et des index|Oui|Oui|Oui|Oui|Oui|
|Compression des données|Oui|Oui|Oui|Oui|Oui|
|Gouverneur de ressources|Oui|Non|Non|Non|Non|
|Parallélisme de tableau partitionné|Oui|Non|Non|Non|Non|
|Plusieurs conteneurs de flux de fichier|Oui|Oui|Oui|Oui|Oui|
|Mémoire de pages de grande taille compatible NUMA et allocation de tableau de tampons|Oui|Non|Non|Non|Non|
|Extension du pool de mémoires tampons|Oui|Oui|Non|Non|Non|
|Gouvernance des ressources d’E/S|Oui|Non|Non|Non|Non|
|Lecture anticipée|Oui|Non|Non|Non|Non|
|Analyse avancée|Oui|Non|Non|Non|Non|
|Durabilité différée|Oui|Oui|Oui|Oui|Oui|
|Base de données intelligente : réglage automatique|Oui|Non|Non|Non|Non|
|Base de données intelligente : mode lot pour le magasin de lignes <sup>1</sup>|Oui|Non|Non|Non|Non|
|Base de données intelligente : commentaires d’allocation de mémoire en mode ligne|Oui|Non|Non|Non|Non|
|Base de données intelligente : nombre approximatif distinct|Oui|Oui|Oui|Oui|Oui|
|Base de données intelligente : compilation différée de variables de tables|Oui|Oui|Oui|Oui|Oui|
|Base de données intelligente : incorporation d’UDF scalaires|Oui|Oui|Oui|Oui|Oui|
|Jointures adaptatives en mode batch|Oui|Non|Non|Non|Non|
|Retour d’allocation de mémoire en mode batch|Oui|Non|Non|Non|Non|
|Exécution entrelacée pour les fonctions table à instructions multiples|Oui|Oui|Oui|Oui|Oui|
|Améliorations de l’insertion en bloc|Oui|Oui|Oui|Oui|Oui|

<sup>1</sup> La taille des données OLTP en mémoire et le cache de segments columnstore sont limités à la quantité de mémoire spécifiée par l’édition dans la section [Limites d’échelle](#Cross-BoxScaleLimits). Le degré de parallélisme (DOP) pour les opérations [en mode batch](../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) est limité à 2 pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Édition standard, et à 1 pour les éditions [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web et Express. Ceci fait référence aux index columnstore créés sur des tables basées sur des disques et des tables à mémoire optimisée.

<sup>2</sup> Pushdown d’agrégats, pushdown de prédicats de chaîne et optimisations SIMD sont des améliorations de l’évolutivité de l’Édition Entreprise [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Index columnstore - Nouveautés](../relational-databases/indexes/columnstore-indexes-what-s-new.md).

<sup>3</sup> Cette fonctionnalité n’est pas incluse dans l’option d’installation LocalDB.

## <a name="rdbms-security"></a><a name="RDBMSS"></a> Sécurité SGBDR

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-----:|:-----------------------:|:-----:|
|Sécurité au niveau des lignes|Oui|Oui|Oui|Oui|Oui|
|Always Encrypted|Oui|Oui|Oui|Oui|Oui|
|Always Encrypted avec enclaves sécurisées|Oui|Oui|Oui|Oui|Oui|
|Masquage dynamique des données|Oui|Oui|Oui|Oui|Oui|
|Audit de serveur|Oui|Oui|Oui|Oui|Oui|
|Audit de base de données|Oui|Oui|Oui|Oui|Oui|
|Chiffrement transparent de base de données|Oui|Oui|Oui|Non|Non|
|Gestion de clés extensible|Oui|Oui|Non|Non|Non|
|Rôles définis par l’utilisateur|Oui|Oui|Oui|Oui|Oui|
|Bases de données autonomes|Oui|Oui|Oui|Oui|Oui|
|Chiffrement des sauvegardes|Oui|Oui|Non|Non|Non|
|Classification et audit des données|Oui|Oui|Oui|Oui|Oui|

## <a name="replication"></a>Réplication<a name="Replication"></a>

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------|:---------:|:-------:|:--:|:--------------------------------:|:------:|
|Abonnés hétérogènes|Oui|Oui|Non|Non|Non|
|Réplication de fusion|Oui|Oui|Oui<sup>1</sup>|Oui<sup>1</sup>|Oui<sup>1</sup>|
|Publication Oracle|Oui|Non|Non|Non|Non|
|Réplication transactionnelle d’égal à égal|Oui|Non|Non|Non|Non|
|Réplication d'instantané|Oui|Oui|Oui<sup>1</sup>|Oui<sup>1</sup>|Oui<sup>1</sup>|
|Suivi des modifications [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Oui|Oui|Oui|Oui|Oui|
|Réplication transactionnelle|Oui|Oui|Oui<sup>1</sup>|Oui<sup>1</sup>|Oui<sup>1</sup>|
|Réplication transactionnelle vers Azure|Oui|Oui|Non|Non|Non|
|Abonnement pouvant être mis à jour à la réplication transactionnelle|Oui|Oui|Non|Non|Non|

<sup>1</sup> Abonné uniquement

## <a name="management-tools"></a><a name="SSMS"></a> Outils d’administration

|Fonctionnalité|Entreprise|standard|Web|Express with Advanced Services|Express|
|-------|:--------:|:------:|:-:|:----------------------------:|:-----:|
|Objets de gestion SQL (SMO)|Oui|Oui|Oui|Oui|Oui|
|API d’évaluation SQL|Oui|Oui|Oui|Oui|Oui|
|Évaluation des vulnérabilités SQL |Oui|Oui|Oui|Oui|Oui|
|Gestionnaire de configuration SQL|Oui|Oui|Oui|Oui|Oui|
|SQL CMD (outil d'invite de commandes)|Oui|Oui|Oui|Oui|Oui|
|Distributed Replay - Outil d’administration|Oui|Oui|Oui|Oui|Non|
|Distribute Replay - Client|Oui|Oui|Oui|Non|Non|
|Distributed Replay - Contrôleur|Oui<sup>1</sup>|Oui<sup>2</sup>|Oui<sup>2</sup>|Non|Non|
|SQL Profiler|Oui|Oui|Non<sup>3</sup>|Non<sup>3</sup>|Non<sup>3</sup>|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Agent|Oui|Oui|Oui|Non|Non|
|Pack d'administration Microsoft System Center Operations Manager|Oui|Oui|Oui|Non|Non|
|Assistant Paramétrage de base de données (DTA)|Oui|Oui<sup>4</sup>|Oui<sup>4</sup>|Non|Non|

<sup>1</sup> Jusqu'à 16 clients

<sup>2</sup> 1 client

<sup>3</sup> Web [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], Express [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], Express [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec outils et Express [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec Advanced Services peuvent être profilés à l’aide des éditions Standard [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et Entreprise [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

<sup>4</sup> Réglage activé uniquement sur les fonctionnalités de l’édition Standard

## <a name="rdbms-manageability"></a><a name="RDBMSM"></a> Simplicité de gestion SGBDR

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Instances utilisateur|Non|Non|Non|Oui|Oui|
|LocalDB|Non|Non|Non|Oui|Non|
|Connexion administrateur dédiée|Oui|Oui|Oui|Oui<sup>1</sup>|Oui<sup>1</sup>|
|Support SysPrep <sup>2</sup>|Oui|Oui|Oui|Oui|Oui|
|Support de scripts PowerShell<sup>3</sup>|Oui|Oui|Oui|Oui|Oui|
|Prise en charge des opérations des composants d’application du niveau Données : extraction, déploiement, mise à niveau, suppression|Oui|Oui|Oui|Oui|Oui|
|Automation de stratégie (vérification selon la planification et sur modification)|Oui|Oui|Oui|Non|Non|
|Collecteur de données de performances|Oui|Oui|Oui|Non|Non|
|Possibilité d’inscription en tant qu’instance managée dans le cadre de la gestion de plusieurs instances|Oui|Oui|Oui|Non|Non|
|Rapports de performances standard|Oui|Oui|Oui|Non|Non|
|Repères de plan et gel de plan relatif|Oui|Oui|Oui|Non|Non|
|Requête directe de vues d'index (à l'aide de l'indicateur NOEXPAND)|Oui|Oui|Oui|Oui|Oui|
|Requête directe SQL Server Analysis Services|Oui|Oui|Non|Non|Oui|
|Maintenance automatique des vues indexées|Oui|Oui|Oui|Non|Non|
|Vues partitionnées distribuées|Oui|Non|Non|Non|Non|
|Opérations d'index parallèles|Oui|Non|Non|Non|Non|
|Utilisation automatique de vues indexées par l'optimiseur de requête|Oui|Non|Non|Non|Non|
|Vérifications de cohérence parallèles|Oui|Non|Non|Non|Non|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Point de contrôle de l'utilitaire|Oui|Non|Non|Non|Non|
|Extension du pool de mémoires tampons|Oui|Oui|Non|Non|Non|
|Instance maître pour le cluster Big Data|Oui|Oui|Non|Non|Non|
|Certification de compatibilité|Oui|Oui|Oui|Oui|Oui|

<sup>1</sup> avec indicateur de trace

<sup>2</sup> Pour plus d'informations, consultez [Considerations pour l’installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l’aide de SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).

<sup>3</sup> Sur Linux, les scripts PowerShell sont pris en charge à partir d’ordinateurs Windows ciblant [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur Linux.

## <a name="development-tools"></a><a name="DevTools"></a> Outils de développement

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Intégration de Microsoft Visual Studio|Oui|Oui|Oui|Oui|Oui|
|Intellisense (Transact-SQL et MDX)|Oui|Oui|Oui|Oui|Oui|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools (SSDT)|Oui|Oui|Oui|Oui|Non|
|Outils de conception, de modification et de débogage MDX|Oui|Oui|Non|Non|Non|

## <a name="programmability"></a><a name="Programmability"></a> Programmability

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Intégration R de base<sup>1</sup>|Oui|Oui|Oui|Oui|Non|
|Intégration R avancée<sup>2</sup>|Oui|Non|Non|Non|Non|
|Intégration de Python de base|Oui|Oui|Oui|Oui|Non|
|Intégration de Python avancée|Oui|Non|Non|Non|Non|
|Machine Learning Server (autonome)|Oui|Non|Non|Non|Non|
|Nœud de calcul PolyBase|Oui|Oui<sup>3</sup>|Oui<sup>3</sup>|Oui<sup>3</sup>|Oui<sup>3</sup> |
|Nœud principal PolyBase<sup>4</sup>|Oui|Oui|Non|Non|Non|
|JSON|Oui|Oui|Oui|Oui|Oui|
|Magasin des requêtes|Oui|Oui|Oui|Oui|Oui|
|Temporal|Oui|Oui|Oui|Oui|Oui|
|Intégration du CLR (Common Language Runtime)|Oui|Oui|Oui|Oui|Oui|
|Intégration du Runtime en langage Java|Oui|Oui|Oui|Oui|Oui|
|Prise en charge XML native|Oui|Oui|Oui|Oui|Oui|
|Indexation XML|Oui|Oui|Oui|Oui|Oui|
|Fonctions MERGE & UPSERT|Oui|Oui|Oui|Oui|Oui|
|prise en charge de FILESTREAM|Oui|Oui|Oui|Oui|Oui|
|FileTable|Oui|Oui|Oui|Oui|Oui|
|Types de données de date et d'heure|Oui|Oui|Oui|Oui|Oui|
|Support d'internationalisation|Oui|Oui|Oui|Oui|Oui|
|Recherche sémantique et en texte intégral|Oui|Oui|Oui|Oui|Non|
|Spécification d'une langue dans une requête|Oui|Oui|Oui|Oui|Non|
|Service Broker (messagerie)|Oui|Oui|Non<sup>5</sup>|Non<sup>5</sup>|Non<sup>5</sup>|
|Transact-SQL, points de terminaison|Oui|Oui|Oui|Non|Non|
|Graph|Oui|Oui|Oui|Oui|Oui|
|Prise en charge d’UTF-8|Oui|Oui|Oui|Oui|Oui|

<sup>1</sup> L’intégration de base est limitée à 2 cœurs et à des jeux de données en mémoire.

<sup>2</sup> L’intégration avancée peut utiliser tous les cœurs disponibles pour le traitement parallèle des jeux de données de toutes tailles soumis aux limites matérielles.

<sup>3</sup> La montée en puissance parallèle (scale out) avec plusieurs nœuds de calcul nécessite un nœud principal.

<sup>4</sup> Avant SQL Server 2019, le nœud principal Polybase exige l’Édition Entreprise.

<sup>5</sup> Client uniquement

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Pour plus d’informations sur les fonctionnalités Integration Services (SSIS) [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Integration Services prises en charge par les Éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

## <a name="master-data-services"></a><a name="MDS"></a> Master Data Services

Pour plus d’informations sur les fonctionnalités [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] et Data Quality Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Master Data Services et Data Quality Services prises en charge par les Éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../master-data-services/master-data-services-and-data-quality-services-features-support.md).

## <a name="data-warehouse"></a><a name="DW"></a> Entrepôt de données

|Fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Génération automatique de la mise en lots et du schéma d'entrepôt de données|Oui|Oui|Non|Non|Non|
|Capture des données modifiées|Oui|Oui|Non|Non|Non|
|Optimisations de requêtes de jointure en étoile|Oui|Non|Non|Non|Non|
|Traitement des requêtes parallèles sur les tables et les index partitionnés|Oui|Non|Non|Non|Non|
|Agrégation globale des traitements|Oui|Non|Non|Non|Non|

## <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services

Pour plus d’informations sur les fonctionnalités Analysis Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les Éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services

Pour plus d’informations sur les fonctionnalités Reporting Services prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Reporting Services prises en charge par les Éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="business-intelligence-clients"></a><a name="BIC"></a> Clients Business Intelligence

Pour plus d’informations sur les fonctionnalités clientes Business Intelligence prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités Analysis Services prises en charge par les Éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md) ou [Fonctionnalités Reporting Services prises en charge par les Éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="spatial-and-location-services"></a><a name="SLS"></a> Services d’emplacement et spatiaux

|Nom de la fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|-------------|:-------:|:------:|:-:|:-------------------------------:|:-----:|
|Index spatiaux|Oui|Oui|Oui|Oui|Oui|
|Types de données planaires et géodésiques|Oui|Oui|Oui|Oui|Oui|
|Bibliothèques spatiales avancées|Oui|Oui|Oui|Oui|Oui|
|Importation/exportation de formats de données spatiales standard|Oui|Oui|Oui|Oui|Oui|

## <a name="additional-database-services"></a><a name="ADS"></a> Services de base de données supplémentaires

|Nom de la fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Assistant Migration|Oui|Oui|Oui|Oui|Oui|
|Messagerie de base de données|Oui|Oui|Oui|Non|Non|

## <a name="other-components"></a><a name="Other"></a> Autres composants

|Nom de la fonctionnalité|Entreprise|standard|Web|Express avec<br/>Advanced Services|Express|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|Non|Non|
|StreamInsight HA|StreamInsight Premium Edition|Non|Non|Non|Non|

## <a name="next-steps"></a>Étapes suivantes

[Spécifications produit pour [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](https://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)

[Installation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/installation-for-sql-server-2016.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
