---
description: Éditions et fonctionnalités prises en charge de SQL Server 2019 sur Linux
title: Éditions et fonctionnalités prises en charge de SQL Server 2019 - Linux
ms.date: 01/08/2020
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- default components
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: 398d8d1aaf626fdc65674ee1d5eb959951dcd5e2
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552622"
---
# <a name="editions-and-supported-features-of-sql-server-2019-on-linux"></a>Éditions et fonctionnalités prises en charge de SQL Server 2019 sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article fournit des détails sur les fonctionnalités prises en charge par les différentes éditions de SQL Server 2019 sur Linux. Pour les éditions et pour les fonctionnalités de SQL Server sur Windows prises en charge, consultez [SQL Server 2019 - Windows](../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
La configuration requise pour l'installation varie selon vos besoins applicatifs. Les différentes éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s'adaptent aux exigences de chaque organisation et de chaque individu en termes de performances, d'exécution et de prix. Les composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que vous installez dépendent également de vos exigences spécifiques. Les sections suivantes vous aident à choisir parmi les éditions et les composants disponibles dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

Pour obtenir les notes de publication les plus récentes et des informations sur les nouveautés, consultez les rubriques suivantes :
- [Notes de publication de SQL Server 2019 sur Linux](sql-server-linux-release-notes-2019.md)
- [Nouveautés dans SQL Server 2019 sur Linux](sql-server-linux-whats-new-2019.md)

Pour obtenir la liste des fonctionnalités de SQL Server qui ne sont pas disponibles sur Linux, consultez [Fonctionnalités et services non pris en charge](#Unsupported).

### <a name="try-sql-server"></a>Essayez SQL Server !    
    
[Télécharger SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)

## <a name="ssnoversion-editions"></a>Éditions de[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]  
 Le tableau ci-dessous décrit les éditions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 
  
|Édition de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Définition|  
|---------------------------------------|----------------|  
|Entreprise|Offre Premium, l’édition [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise fournit des fonctionnalités de centre de données haut de gamme complètes, avec des performances ultrarapides qui autorisent des niveaux de service élevés pour les charges de travail critiques.|  
|standard|L’édition Standard [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] permet la gestion des données de base destinée aux services des grandes entreprises comme aux PME, leur permettant d’exécuter les applications et prenant en charge des outils de développement communs locaux et dans le cloud, pour une gestion efficace des bases de données avec des ressources informatiques minimales.|  
|Web|L'édition Web[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] est une option offrant un coût total de possession faible destinée aux hébergeurs Web et aux VAP Web, fournissant des fonctions évolutives, rentables et gérables aux propriétés Web à petite ou grande échelle.|  
|Développeur|L'édition[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer permet aux développeurs de créer des applications basées sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Il inclut toutes les fonctionnalités de l'édition Enterprise, mais sa licence permet uniquement de l'utiliser comme un système de développement et de test, et non comme un serveur de production. L'édition [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer est la solution idéale pour le développement et le test d'applications.|  
|Express Edition|L’édition Express est une édition de base comprenant une base de données gratuite, idéale pour découvrir et créer des applications bureautiques et de petites applications serveur pilotées par les données. C'est la solution idéale pour les éditeurs de logiciels, les développeurs et les amateurs de création d'applications clientes. Si vous avez besoin de fonctionnalités de base de données plus évoluées, vous pouvez mettre à niveau de manière transparente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express vers des versions plus sophistiquées de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Utilisation de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec des applications client/serveur  

Vous pouvez installer uniquement les composants clients de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un ordinateur exécutant des applications client/serveur qui se connectent directement à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Une installation de composants clients est également un bon choix si vous administrez une instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un serveur de base de données ou si vous prévoyez de développer des applications [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="ssnoversion-components"></a>Composants [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  

SQL Server 2019 sur Linux prend en charge le moteur de base de données SQL Server. Le tableau ci-dessous décrit les fonctionnalités du moteur de base de données.   
  
|Composants serveur|Description|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] inclut le [!INCLUDE[ssDE](../includes/ssde-md.md)], le service principal de stockage, de traitement et de protection des données, la réplication, la recherche en texte intégral, les outils d’administration de données relationnelles et XML et l’intégration de l’analyse de base de données.|  

**Éditions Developer, Enterprise Core et Evaluation**  
Pour connaître les fonctionnalités prises en charge par les éditions Developer, Enterprise Core et Evaluation, consultez les fonctionnalités répertoriées pour l’édition SQL Server Entreprise dans les tableaux suivants.

L’édition Developer continue à prendre en charge seulement un client pour [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Limites d’échelle  
  
|Fonctionnalité|Entreprise|Standard|Web|Express| 
|-------------|----------------|--------------|---------|------------------------|
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs| 
|Capacité maximale de calcul utilisée par une instance unique - [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ou [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Maximum du système d'exploitation|Limité à moins de 4 sockets ou 24 cœurs|Limité à moins de 4 sockets ou 16 cœurs|Limité à moins de 1 socket ou 4 cœurs|
|Mémoire maximale du pool de mémoires tampons par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Maximum du système d’exploitation|128 Go|64 Go|1410 Mo|
|Mémoire maximale du cache de segments columnstore par instance du [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go| 16 Go| 352 Mo|  
|Taille maximale des données à mémoire optimisée par base de données dans le [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Mémoire illimitée| 32 Go| 16 Go| 352 Mo|
|Taille maximale de la base de données relationnelle|524 Po|524 Po|524 Po|10 Go|  
  
<sup>1</sup> L’édition Enterprise avec serveur + licences d’accès client (CAL) (non disponibles pour les nouveaux contrats) est limitée à un maximum de 20 cœurs par instance SQL Server. Il n'existe aucune limite dans le mode de licence Serveur selon le nombre de cœurs. Pour plus d’informations, voir [Limites de capacité de calcul par édition de SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
 
##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> Haute disponibilité SGBDR  
  
|Fonctionnalité|Entreprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------|  
|Copie des journaux de transaction|Oui|Oui|Oui|Non|  
|Compression de sauvegarde|Oui|Oui|Non|Non| 
|Instantané de base de données|Oui|Oui|Non|Non|
|Instance de cluster de basculement Always On<sup>1</sup>|Oui|Oui|Non|Non| 
|Groupes de disponibilité Always On<sup>2</sup>|Oui|Non|Non|Non|
|Groupes de disponibilité de base<sup>3</sup>|Non|Oui|Non|Non|
|Groupe de disponibilité à validation de réplica minimale|Oui|Oui|Non|Non|
|Groupe de disponibilité sans cluster|Oui|Oui|Non|Non|
|Restauration en ligne de pages et de fichiers|Oui|Non|Non|Non|
|Indexation en ligne|Oui|Non|Non|Non|
|Reconstructions d’index en ligne pouvant être reprises|Oui|Non|Non|Non|
|Modification de schéma en ligne|Oui|Non|Non|Non|
|Récupération rapide|Oui|Non|Non|Non|
|Sauvegardes en miroir|Oui|Non|Non|Non|
|Ajout de mémoire et de processeur à chaud|Oui|Non|Non|Non|
|Sauvegarde chiffrée|Oui|Oui|Non|Non|
|Sauvegarde hybride vers Azure (sauvegarde vers une URL)|Oui|Oui|Non|Non|
  
<sup>1</sup> Dans l’édition Entreprise, le nombre de nœuds correspond au maximum du système d’exploitation. L’édition Standard prend en charge deux nœuds. 

<sup>2</sup> L’édition Entreprise fournit la prise en charge de 8 réplicas secondaires maximum, y compris 2 réplicas secondaires synchrones. 

<sup>3</sup> L’édition Standard prend en charge les groupes de disponibilité de base. Un groupe de disponibilité de base prend en charge deux réplicas, avec une base de données. Pour plus d’informations sur les groupes de disponibilité de base, consultez [Groupes de disponibilité de base](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).    

##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> Scalabilité et performances SGBDR  
  
|Fonctionnalité|Entreprise|Standard|Web|Express|  
|-------------|----------------|--------------|---------|------------------------| 
|Columnstore <sup>1</sup>|Oui|Oui|Oui|Oui|  
|Fichiers binaires LOB dans les index columnstore cluster|Oui|Oui|Oui|Oui|  
|Reconstruction d’index columnstore non cluster en ligne|Oui|Non|Non|Non|
|OLTP en mémoire <sup>1</sup>|Oui|Oui|Oui|Oui|
|Mémoire principale persistante|Oui|Oui|Oui|Oui|
|Partitionnement des tables et des index|Oui|Oui|Oui|Oui|  
|Compression des données|Oui|Oui|Oui|Oui|
|gouverneur de ressources|Oui|Non|Non|Non|  
|Parallélisme de tables partitionnées|Oui|Non|Non|Non|
|Mémoire de pages de grande taille compatible NUMA et allocation de tableau de tampons|Oui|Non|Non|Non|
|Gouvernance des ressources d'E/S|Oui|Non|Non|Non|  
|Durabilité différée|Oui|Oui|Oui|Oui|
|Paramétrage automatique|Oui|Non|Non|Non|
|Jointures adaptatives en mode batch|Oui|Non|Non|Non|
|Retour d’allocation de mémoire en mode batch|Oui|Non|Non|Non|
|Exécution entrelacée pour les fonctions table à instructions multiples|Oui|Oui|Oui|Oui|
|Améliorations de l’insertion en bloc|Oui|Oui|Oui|Oui|


<sup>1</sup> La taille des données OLTP en mémoire et le cache de segments columnstore sont limités à la quantité de mémoire spécifiée par l’édition dans la section Limites d’échelle. Le Nombre maximal de degrés de parallélisme est limité. Le nombre de degrés de parallélisme maximal pour la création d’un index est limité à 2 pour l’édition Standard et à 1 pour les éditions Web et Express. Ceci fait référence aux index columnstore créés sur des tables basées sur des disques et des tables à mémoire optimisée.

##  <a name="rdbms-security"></a><a name="RDBMSS"></a> Sécurité SGBDR  
  
|Fonctionnalité|Entreprise|Standard|Web|Express|
|-------------|----------------|--------------|---------|------------------------------------| 
|Sécurité au niveau des lignes|Oui|Oui|Oui|Oui|  
|Always Encrypted|Oui|Oui|Oui|Oui| 
|Masquage dynamique des données|Oui|Oui|Oui|Oui|   
|Audit de base|Oui|Oui|Oui|Oui| 
|Audit de granularité fine|Oui|Oui|Oui|Oui| 
|Chiffrement transparent de base de données|Oui|Oui|Non|Non|   
|Rôles définis par l’utilisateur|Oui|Oui|Oui|Oui| 
|Bases de données autonomes|Oui|Oui|Oui|Oui| 
|Chiffrement des sauvegardes|Oui|Oui|Non|Non|  

##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> Simplicité de gestion SGBDR  
  
|Fonctionnalité|Entreprise|Standard|Web|Express|   
|-------------|----------------|--------------|---------|------------------------|  
|Connexion administrateur dédiée|Oui|Oui|Oui|Oui avec indicateur de trace|   
|Prise en charge de scripts PowerShell|Oui|Oui|Oui|Oui| 
|Prise en charge des opérations des composants d’application du niveau Données : extraction, déploiement, mise à niveau, suppression|Oui|Oui|Oui|Oui| 
|Automation de stratégie (vérification selon la planification et sur modification)|Oui|Oui|Oui|Non|  
|Collecteur de données de performances|Oui|Oui|Oui|Non|
|Rapports de performances standard|Oui|Oui|Oui|Non|
|Repères de plan et gel de plan relatif|Oui|Oui|Oui|Non| 
|Requête directe de vues d'index (à l'aide de l'indicateur NOEXPAND)|Oui|Oui|Oui|Oui| 
|Maintenance automatique des vues indexées|Oui|Oui|Oui|Non|
|Vues partitionnées distribuées|Oui|Non|Non|Non| 
|Opérations d'index parallèles|Oui|Non|Non|Non|  
|Utilisation automatique de vues indexées par l'optimiseur de requête|Oui|Non|Non|Non| 
|Vérifications de cohérence parallèles|Oui|Non|Non|Non| 
|Point de contrôle de l’utilitaire SQL Server|Oui|Non|Non|Non|    

##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Fonctionnalité|Entreprise|Standard|Web|Express 
|-------------|----------------|--------------|---------|------------------------|  
|JSON|Oui|Oui|Oui|Oui|   
|Magasin des requêtes|Oui|Oui|Oui|Oui|   
|Temporal|Oui|Oui|Oui|Oui|   
|Prise en charge XML native|Oui|Oui|Oui|Oui| 
|Indexation XML|Oui|Oui|Oui|Oui| 
|Fonctions MERGE & UPSERT|Oui|Oui|Oui|Oui|   
|Types de données de date et d'heure|Oui|Oui|Oui|Oui|  
|Support d'internationalisation|Oui|Oui|Oui|Oui| 
|Recherche sémantique et en texte intégral|Oui|Oui|Oui|Oui|
|Spécification d'une langue dans une requête|Oui|Oui|Oui|Oui|
|Service Broker (messagerie)|Oui|Oui|Non (client uniquement)|Non (client uniquement)|
|Transact-SQL, points de terminaison|Oui|Oui|Oui|Non|
|Graph|Oui|Oui|Oui|Oui|  


<sup>1</sup> Le scale-out avec plusieurs nœuds de calcul nécessite un nœud principal.

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Pour plus d’informations sur les fonctionnalités Integration Services (SSIS) prises en charge par les éditions de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consultez [Fonctionnalités d’Integration Services prises en charge par les éditions de SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Services d’emplacement et spatiaux  
  
|Nom de la fonctionnalité|Entreprise|Standard|Web|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Index spatiaux|Oui|Oui|Oui|Oui|   
|Types de données planaires et géodésiques|Oui|Oui|Oui|Oui| 
|Bibliothèques spatiales avancées|Oui|Oui|Oui|Oui|   
|Importation/exportation de formats de données spatiales standard|Oui|Oui|Oui|Oui|   

## <a name="unsupported-features--services"></a><a name="Unsupported"></a> Fonctionnalités et services non pris en charge

Les fonctionnalités et services suivants ne sont pas disponibles pour SQL Server 2019 sur Linux. Le support de ces fonctionnalités sera de plus en plus activé dans le temps.

| Domaine | Fonctionnalité ou service non pris en charge |
|-----|-----|
| **Moteur de base de données** | Réplication de fusion |
| &nbsp; | Base de données pour Stretch |
| &nbsp; | Requête distribuée avec connexions tierces |
| &nbsp; | Serveurs liés à des sources de données autres que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  |
| &nbsp; | Procédures stockées étendues système (XP_CMDSHELL, etc.) |
| &nbsp; | FileTable, FILESTREAM |
| &nbsp; | Assemblys CLR avec l’ensemble d’autorisations EXTERNAL_ACCESS ou UNSAFE |
| &nbsp; | Buffer Pool Extension |
| &nbsp; | Sauvegarde sur URL – Objet blob de pages<sup>2</sup> |
| **SQL Server Agent** |  Sous-systèmes : CmdExec, PowerShell, lecture de la file d’attente, SSIS, SSAS, SSRS |
| &nbsp; | Alertes |
| &nbsp; | Sauvegarde managée |
| **Haute disponibilité** | Mise en miroir de bases de données  |
| **Sécurité** | Gestion de clés extensible |
| &nbsp; | Authentification AD pour les serveurs liés | 
| &nbsp; | Authentification AD pour les groupes de disponibilité | 
| **Services** | SQL Server Browser |
| &nbsp; | SQL Server R services<sup>1</sup> |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Master Data Services |

<sup>1</sup> SQL Server R est pris en charge dans SQL Server, mais SQL Server R services en tant que package distinct n’est pas pris en charge.

<sup>2</sup> La sauvegarde sur URL est prise en charge pour les objets blob de blocs, à l’aide de la [Signature d’accès partagé](../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS).

## <a name="next-steps"></a>Étapes suivantes
 [Fonctionnalités prises en charge par les éditions de SQL Server 2017 - Linux](sql-server-linux-editions-and-components-2017.md)  
 [Fonctionnalités prises en charge par les éditions de SQL Server 2019 – Windows](../sql-server/editions-and-components-of-sql-server-version-15.md)  
 [Fonctionnalités prises en charge par les éditions de SQL Server 2017 – Windows](../sql-server/editions-and-components-of-sql-server-2017.md)  
 [Fonctionnalités prises en charge par les éditions de SQL Server 2016 – Windows](../sql-server/editions-and-components-of-sql-server-2016.md)  
 [Installation de SQL Server 2016](../database-engine/install-windows/install-sql-server.md)  
 [Spécifications de produit pour SQL Server](../sql-server/index.yml)
