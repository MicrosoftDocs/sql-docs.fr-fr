---
description: Nouveautés d’Integration Services dans SQL Server 2016
title: Nouveauté d’Integration Services dans SQL Server 2016 | Microsoft Docs
ms.custom:
- SQL2016_New_Updated
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, what's new
- what's new [Integration Services]
ms.assetid: da6999c7-e5e3-4a59-a284-1da635995af1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 838b01e2fd4f09b787b11f146143fc6c327504b7
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172211"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2016"></a>Nouveautés d’Integration Services dans SQL Server 2016

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



Cette rubrique décrit les fonctionnalités qui ont été ajoutées ou mises à jour dans SQL Server 2016[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Elle mentionne également les fonctionnalités ajoutées ou mises à jour dans le [Feature Pack Azure pour Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md) durant le calendrier SQL Server 2016.  

## <a name="new-for-ssis-in-azure-data-factory"></a>Nouveautés de SSIS dans Azure Data Factory

Avec la préversion publique d’Azure Data Factory version 2 de septembre 2017, vous pouvez désormais effectuer les opérations suivantes :
-   Déployer des packages dans la base de données du catalogue SSIS (SSISDB) sur Azure SQL Database.
-   Exécuter des packages déployés sur Azure sur le runtime d’intégration Azure-SSIS, composant d’Azure Data Factory version 2.

Pour plus d’informations, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md).

Ces nouvelles fonctionnalités nécessitent SQL Server Data Tools (SSDT) version 17.2 ou ultérieure, mais ne nécessitent pas SQL Server 2017 ou SQL Server 2016. Quand vous déployez des packages sur Azure, l’Assistant Déploiement de package met toujours à niveau les packages avec le format de package le plus récent.

## <a name="2016-improvements-by-category"></a>Améliorations de la version 2016 par catégorie  
  
-   **Facilité de gestion**  
  
    -   Amélioration du déploiement  
  
        -   [Assistant Mise à niveau de SSISDB](#ssisdbupgrwiz)  
  
        -   [Prise en charge de la fonctionnalité AlwaysOn dans le catalogue SSIS](#AlwaysOn)  
  
        -   [Déploiement incrémentiel de packages](#IncrementalDeployment)  
  
        -   [Prise en charge de la fonctionnalité de chiffrement intégral dans le catalogue SSIS](#encrypted)  
  
    -   Amélioration du débogage  
  
        -   [Nouveau rôle de base de données ssis_logreader dans le catalogue SSIS](#LogReader)  
  
        -   [Nouveau niveau de journalisation RuntimeLineage dans le catalogue SSIS](#RuntimeLineage)  
  
        -   [Nouveau niveau de journalisation personnalisé dans le catalogue SSIS](#CustomLogging)  
  
        -   [Noms de colonnes pour les erreurs contenues dans le flux de données](#ErrorColumn)  
  
        -   [Prise en charge étendue des noms de la colonne d’erreur](#getidstring)  
  
        -   [Prise en charge du niveau de journalisation par défaut au niveau du serveur](#ServerLogLevel)  
  
        -   [Nouvelle interface IDTSComponentMetaData130 dans l’API](#CMD130)  
  
    -   Amélioration de la gestion des packages  
  
        -   [Amélioration de l’expérience pour la mise à niveau des projets](#ProjectUpgrade)  
  
        -   [La propriété AutoAdjustBufferSize calcule automatiquement la taille de la mémoire tampon du flux de données](#BufferSize)  
  
        -   [Modèles de flux de contrôle réutilisables](#Templates)  
  
        -   [Nouveaux modèles renommés en tant que parties](#Parts)  
  
-   **Connectivité**  
  
    -   Extension de la connectivité locale  
  
        -   [Prise en charge des sources de données OData v4](#ODatav4)  
  
        -   [Prise en charge explicite des sources de données Excel 2013](#Excel2013)  
  
        -   [Prise en charge du système de fichiers Hadoop (HDFS)](#HDFS)  
  
        -   [Extension de la prise en charge pour Hadoop et HDFS](#more_hadoop)  
  
        -   [La destination du fichier HDFS prend désormais en charge le format de fichier ORC](#hdfsORC)  
  
        -   [Composants ODBC mis à jour pour SQL Server 2016](#odbc2016)  
  
        -   [Prise en charge explicite des sources de données Excel 2016](#Excel2016)  
  
        -   [Publication du connecteur pour SAP BW pour SQL Server 2016](#SAPBW)
        
        -   [Publication de la version 4.0 des connecteurs pour Oracle et Teradata](#oracleteradata)
        
        -   [Publication des connecteurs pour Analytics Platform System (PDW) Appliance Update 5](#pdwau5)
  
    -   Extension de la connectivité au cloud  
  
        -   Connecteurs Azure Storage, et tâches Hive et Pig pour HDInsight - [Publication d’Azure Feature Pack pour SSIS pour SQL Server 2016](#AFP2016)
        
        -   [Prise en charge des ressources Microsoft Dynamics Online publiées dans Service Pack 1](#dynamics)
        
        -   [Prise en charge d’Azure Data Lake Store](#datalakestore)
        
        -   [Prise en charge d’Azure Synapse Analytics](#sqldwupload)
  
-   **Convivialité et productivité**  
  
    -   Amélioration de l’expérience d’installation  
  
        -   [Blocage de la mise à niveau quand SSISDB appartient à un groupe de disponibilité](#Upgrade)  
  
    -   Amélioration de l’expérience de conception  
  
        -   [Le concepteur SSIS crée et gère les packages pour SQL Server 2016, 2014 ou 2012](#OneDesigner)  
  
        -   Multiples améliorations et correctifs de bogues pour les concepteurs.  
  
    -   Amélioration de l’expérience de gestion dans SQL Server Management Studio
  
        -   [Performances améliorées pour les affichages catalogue SSIS](#CatViews)  
  
    -   Autres améliorations  
  
        -   [La transformation du distributeur de données équilibrées fait désormais partie de SSIS](#BDDinbox)  
  
        -   [Les composants de publication du flux de données font désormais partie de SSIS](#ComplexFeedinbox)  
  
        -   [Prise en charge du stockage Blob Azure dans l’Assistant Importation et Exportation SQL Server](#AzureBlob)  
  
        -   [Service et concepteur de capture de données modifiées pour Oracle pour Microsoft SQL Server 2016](#CDCOracle)  
  
        -   [Mise à jour des composants de capture de données modifiées pour SQL Server 2016](#cdc2016)  
  
        -   [Mise à jour de la tâche DDL d’exécution Analysis Services](#ASDDL)  
  
        -   [Les tâches Analysis Services prennent en charge les modèles tabulaires](#ssasrc0)  
  
        -   [Prise en charge des services R intégrés](#builtinR)  
  
        -   [Sortie de validation XML détaillée dans la tâche XML](#ValidateXML)  
  
## <a name="manageability"></a>Simplicité de gestion  

### <a name="better-deployment"></a>Amélioration du déploiement

####  <a name="ssisdb-upgrade-wizard"></a><a name="ssisdbupgrwiz"></a> Assistant Mise à niveau de SSISDB  
 Exécutez l’Assistant Mise à niveau de SSISDB pour mettre à niveau la base de données du catalogue SSIS, SSISDB, quand celle-ci est plus ancienne que la version actuelle de l’instance SQL Server. Cela se produit quand l’une des conditions suivantes est remplie.  
  
-   Vous avez restauré la base de données à partir d’une ancienne version de SQL Server.  
  
-   Vous n’avez pas supprimé la base de données d’un groupe de disponibilité Always On avant la mise à niveau de l’instance SQL Server. Cela empêche la mise à niveau automatique de la base de données. Pour plus d’informations, consultez [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  
  
 Pour plus d’informations, consultez [Catalogue SSIS &#40;SSISDB&#41;](../integration-services/catalog/ssis-catalog.md). 

####  <a name="support-for-always-on-in-the-ssis-catalog"></a><a name="AlwaysOn"></a> Prise en charge de la fonctionnalité AlwaysOn dans le catalogue SSIS  
 La fonctionnalité des groupes de disponibilité AlwaysOn est une solution de haute disponibilité et de récupération d’urgence qui offre une alternative au niveau de l’entreprise à la mise en miroir de bases de données. Un groupe de disponibilité prend en charge un environnement de basculement pour un ensemble discret de bases de données utilisateur, appelées bases de données de disponibilité, qui basculent ensemble. Pour plus d’informations, consultez [Groupes de disponibilité AlwaysOn](../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 Dans SQL Server 2016, SSIS introduit de nouvelles fonctionnalités qui vous permettent d’effectuer facilement un déploiement sur un catalogue SSIS centralisé (par exemple, une base de données utilisateur SSISDB). Pour fournir une haute disponibilité à la base de données SSISDB et son contenu (projets, packages, journaux d’exécution, etc.), vous pouvez ajouter la base de données SSISDB à un groupe de disponibilité AlwaysOn, comme n’importe quelle autre base de données utilisateur. Quand un basculement se produit, le nœud secondaire devient automatiquement le nouveau nœud primaire.  
  
 Pour obtenir une présentation détaillée et des instructions pas à pas concernant l’activation d’AlwaysOn pour SSISDB, consultez [Catalogue SSIS](../integration-services/catalog/ssis-catalog.md).  

####  <a name="incremental-package-deployment"></a><a name="IncrementalDeployment"></a> Déploiement incrémentiel de packages  
La fonctionnalité de déploiement incrémentiel de packages vous permet de déployer un ou plusieurs packages dans un projet existant ou nouveau sans déployer la totalité du projet. Vous pouvez déployer des packages de façon incrémentielle à l’aide des outils suivants.  
  
-   Assistant Déploiement  
  
-   SQL Server Management Studio (qui utilise l’Assistant Déploiement)  
  
-   SQL Server Data Tools (Visual Studio) (qui utilise également l’Assistant Déploiement)  
  
-   Procédures stockées  
  
-   API MOM (Management Object Model)  
  
 Pour plus d’informations, consultez [Déployer des projets et des packages Integration Services (SSIS)](../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  

####  <a name="support-for-always-encrypted-in-the-ssis-catalog"></a><a name="encrypted"></a> Prise en charge de la fonctionnalité Always Encrypted dans le catalogue SSIS  
 SSIS prend déjà en charge la fonctionnalité de chiffrement intégral dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez les billets de blog suivants.  
  
-   [SSIS et le chiffrement intégral](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-always-encrypted/ba-p/388272)  
  
-   [Transformation de recherche et chiffrement intégral](https://techcommunity.microsoft.com/t5/sql-server-integration-services/lookup-transformation-with-always-encrypted/ba-p/388282)  

### <a name="better-debugging"></a>Amélioration du débogage

####  <a name="new-ssis_logreader-database-level-role-in-the-ssis-catalog"></a><a name="LogReader"></a> Nouveau rôle de base de données ssis_logreader dans le catalogue SSIS  
 Dans les versions antérieures du catalogue SSIS, seuls les utilisateurs avec le rôle **ssis_admin** peuvent accéder aux affichages qui contiennent la sortie de journalisation. Il existe maintenant un nouveau rôle de base de données, **ssis_logreader** , qui vous permet d’accorder aux utilisateurs non-administrateurs l’accès aux affichages contenant la sortie de journalisation.  
  
 Il existe également un nouveau rôle **ssis_monitor** . Ce rôle prend en charge AlwaysOn et est destiné à être utilisé de façon interne uniquement par le catalogue SSIS.  

####  <a name="new-runtimelineage-logging-level-in-the-ssis-catalog"></a><a name="RuntimeLineage"></a> Nouveau niveau de journalisation RuntimeLineage dans le catalogue SSIS  
 Le nouveau niveau de journalisation **RuntimeLineage** dans le catalogue SSIS permet de collecter les données nécessaires pour le suivi des informations de lignage dans le flux de données. Vous pouvez analyser ces informations de lignage pour mapper la relation de lignage entre différentes tâches. Les éditeurs de logiciels indépendants et les développeurs peuvent créer des outils de mappage de lignage personnalisés à l’aide de ces informations. 

####  <a name="new-custom-logging-level-in-the-ssis-catalog"></a><a name="CustomLogging"></a> Nouveau niveau de journalisation personnalisé dans le catalogue SSIS  
 Dans les versions antérieures du catalogue SSIS, vous pouvez choisir l’un des quatre niveaux de journalisation intégrés ( **None, Basic, Performance ou Verbose**) quand vous exécutez un package. SQL Server 2016 ajoute le niveau de journalisation **RuntimeLineage**. En outre, vous pouvez désormais créer et enregistrer plusieurs niveaux de journalisation personnalisés dans le catalogue SSIS, et choisir le niveau de journalisation à utiliser chaque fois que vous exécutez un package. Pour chaque niveau de journalisation personnalisé, sélectionnez uniquement les statistiques et les événements à capturer. Vous pouvez éventuellement inclure le contexte de l’événement pour voir les valeurs des variables, les chaînes de connexion et les propriétés de la tâche. Pour plus d’informations, consultez [Activer la journalisation des exécutions de package sur le serveur SSIS](../integration-services/performance/integration-services-ssis-logging.md#server_logging). 

####  <a name="column-names-for-errors-in-the-data-flow"></a><a name="ErrorColumn"></a> Noms de colonnes pour les erreurs contenues dans le flux de données  
 Lorsque vous redirigez les lignes contenant des erreurs vers une sortie d'erreur dans le flux de données, la sortie contient un identificateur numérique pour la colonne dans laquelle l'erreur s'est produite, mais n'affiche pas le nom de la colonne. Il existe désormais plusieurs façons de rechercher ou d’afficher le nom de la colonne dans laquelle l’erreur s’est produite.  
  
-   Quand vous configurez la journalisation, sélectionnez l’événement **DiagnosticEx** . Cet événement consigne un mappage de colonnes de flux de données dans le journal. Vous pouvez alors rechercher le nom de colonne dans ce mappage de colonne à l’aide de l’identificateur de colonne capturé par une sortie d’erreur. Pour plus d’informations, consultez [Gestion des erreurs dans les données](../integration-services/data-flow/error-handling-in-data.md).  
  
-   Dans l’éditeur avancé, vous pouvez voir le nom de colonne de la colonne en amont quand vous affichez les propriétés d’une colonne d’entrée ou de sortie d’un composant de flux de données.  
  
-   Pour afficher les noms des colonnes dans lesquelles l’erreur s’est produite, attachez une Visionneuse de données à une sortie d’erreur.  La Visionneuse de données affiche désormais la description de l’erreur et le nom de la colonne dans laquelle l’erreur s’est produite.  
  
-   Dans le composant Script ou un composant de flux de données personnalisé, appelez la nouvelle méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> de l’interface IDTSComponentMetadata100.  
  
 Pour plus d’informations sur cette amélioration, consultez le billet de blog suivant écrit par le développeur SSIS Bo Fan : [Error Column Improvements for SSIS Data Flow (Améliorations de la colonne d’erreur pour le flux de données SSIS)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253).  
  
> [!NOTE]  
>  (Cette prise en charge a été étendue dans les versions ultérieures. Pour plus d’informations, consultez [Prise en charge étendue des noms de la colonne d’erreur](#getidstring) et [Nouvelle interface IDTSComponentMetaData130 dans l’API](#CMD130).)  

####  <a name="expanded-support-for-error-column-names"></a><a name="getidstring"></a> Prise en charge étendue des noms de la colonne d’erreur  
 L’événement **DiagnosticEx** consigne désormais les informations de colonne pour toutes les colonnes d’entrée et de sortie, pas seulement les colonnes de lignage. Ainsi, nous appelons désormais la sortie un mappage de colonne de pipeline au lieu d’un mappage de lignage de pipeline.  
  
 La méthode GetIdentificationStringByLineageID a été renommée <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A>. Pour plus d’informations, consultez [Noms de colonnes pour les erreurs contenues dans le flux de données](#ErrorColumn).  
  
 Pour plus d’informations sur ce changement et sur l’amélioration de la colonne d’erreur, consultez la mise à jour du billet de blog suivant. [Error Column Improvements for SSIS Data Flow (Updated for CTP3.3) [Améliorations de la colonne d’erreur pour le flux de données SSIS (mis à jour pour CTP3.3)]](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)  
  
> [!NOTE]  
>  (Dans RC0, cette méthode a été déplacée dans la nouvelle interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> . Pour plus d’informations, consultez [Nouvelle interface IDTSComponentMetaData130 dans l’API](#CMD130).)  

####  <a name="support-for-server-wide-default-logging-level"></a><a name="ServerLogLevel"></a> Prise en charge du niveau de journalisation par défaut au niveau du serveur  
 Dans **Propriétés du serveur** de SQL Server, sous la propriété **Niveau de journalisation du serveur** , vous pouvez désormais sélectionner un niveau de journalisation par défaut au niveau du serveur. Vous pouvez choisir l’un des niveaux de journalisation intégrés (Basic, None, Verbose, Performance ou RuntimeLineage), ou choisir un niveau de journalisation personnalisé existant. Le niveau de journalisation sélectionné s’applique à tous les packages déployés dans le catalogue SSIS. Il s'applique également par défaut à une étape de travail de l'Agent SQL qui exécute un package SSIS.  

####  <a name="new-idtscomponentmetadata130-interface-in-the-api"></a><a name="CMD130"></a> Nouvelle interface IDTSComponentMetaData130 dans l’API  
 Le nouveau niveau de journalisation <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> ajoute de nouvelles fonctionnalités à l’interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> existante dans SQL Server 2016, en particulier la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> . (La méthode **GetIdentificationStringByID** est déplacée de l’interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> dans la nouvelle interface.) Il y a également deux nouvelles interfaces, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn130> et <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn130> , qui fournissent la propriété **LineageIdentificationString** . Pour plus d’informations, consultez [Noms de colonnes pour les erreurs contenues dans le flux de données](#ErrorColumn).  

### <a name="better-package-management"></a>Amélioration de la gestion des packages

####  <a name="improved-experience-for-project-upgrade"></a><a name="ProjectUpgrade"></a> Amélioration de l’expérience pour la mise à niveau des projets  
 Durant la mise à niveau des projets SSIS de versions antérieures vers la version actuelle, les gestionnaires de connexions au niveau du projet continuent de fonctionner comme prévu. De plus, la disposition du package et les annotations sont conservées.  

####  <a name="autoadjustbuffersize-property-automatically-calculates-buffer-size-for-data-flow"></a><a name="BufferSize"></a> La propriété AutoAdjustBufferSize calcule automatiquement la taille de la mémoire tampon du flux de données  
 Quand vous définissez la nouvelle propriété **AutoAdjustBufferSize** à la valeur **true**, le moteur de flux de données calcule automatiquement la taille de la mémoire tampon pour le flux de données. Pour plus d’informations, consultez [Data Flow Performance Features](../integration-services/data-flow/data-flow-performance-features.md).  

####  <a name="reusable-control-flow-templates"></a><a name="Templates"></a> Modèles de flux de contrôle réutilisables  
 Enregistrez une tâche de flux de contrôle ou un conteneur fréquemment utilisé dans un fichier de modèle autonome, puis réutilisez-le plusieurs fois dans un ou plusieurs packages d’un projet à l’aide de modèles de flux de contrôle. Cette possibilité de réutilisation facilite la conception et la gestion des packages SSIS. Pour plus d’informations, consultez [Réutiliser un flux de contrôle sur des packages à l’aide de composants de package de flux de contrôle](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

####  <a name="new-templates-renamed-as-parts"></a><a name="Parts"></a> Nouveaux modèles renommés en tant que parties  
 Les nouveaux modèles de flux de contrôle réutilisables dans la version CTP 3.0 ont été renommés en tant que parties de flux de contrôle ou parties de package. Pour plus d’informations sur cette fonctionnalité, consultez [Réutiliser un flux de contrôle sur des packages à l’aide de composants de package de flux de contrôle](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

## <a name="connectivity"></a>Connectivité  

### <a name="expanded-connectivity-on-premises"></a>Extension de la connectivité locale

####  <a name="support-for-odata-v4-data-sources"></a><a name="ODatav4"></a> Prise en charge des sources de données OData v4  
 OData Source et le Gestionnaire de connexions OData prennent désormais en charge les protocoles OData v3 et v4.  
  
-   Pour le protocole OData V3, le composant prend en charge les formats de données JSON et ATOM.  
  
-   Pour le protocole OData V4, le composant prend en charge le format de données JSON.  
  
 Pour plus d'informations, consultez [OData Source](../integration-services/data-flow/odata-source.md).  

####  <a name="explicit-support-for-excel-2013-data-sources"></a><a name="Excel2013"></a> Prise en charge explicite des sources de données Excel 2013  
 Le Gestionnaire de connexions Excel, la source Excel, la destination Excel, ainsi que l’Assistant Importation et Exportation SQL Server fournissent désormais une prise en charge explicite des sources de données Excel 2013. 

####  <a name="support-for-the-hadoop-file-system-hdfs"></a><a name="HDFS"></a> Prise en charge du système de fichiers Hadoop (HDFS)  
 La prise en charge du système de fichiers HDFS contient les gestionnaires de connexions permettant de se connecter aux clusters Hadoop, ainsi que les tâches permettant d’effectuer les opérations HDFS courantes. Pour plus d’informations, consultez [Prise en charge de Hadoop et HDFS dans Integration Services &#40;SSIS&#41;](../integration-services/hadoop-and-hdfs-support-in-integration-services-ssis.md).  

####  <a name="expanded-support-for-hadoop-and-hdfs"></a><a name="more_hadoop"></a> Prise en charge étendue pour Hadoop et HDFS  
  
-   Le gestionnaire de connexions Hadoop prend désormais en charge l’authentification de base et l’authentification Kerberos. Pour plus d’informations, consultez [Hadoop Connection Manager](../integration-services/connection-manager/hadoop-connection-manager.md).  
  
-   La source du fichier HDFS et la destination du fichier HDFS prennent désormais en charge les formats texte et Avro. Pour plus d’informations, consultez  [HDFS File Source](../integration-services/data-flow/hdfs-file-source.md) et  [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  
  
-   La tâche du système de fichiers Hadoop prend désormais en charge l’option CopyWithinHadoop en plus des options CopyToHadoop et CopyFromHadoop. Pour plus d’informations, consultez [Hadoop File System Task](../integration-services/control-flow/hadoop-file-system-task.md).  

####  <a name="hdfs-file-destination-now-supports-orc-file-format"></a><a name="hdfsORC"></a> La destination du fichier HDFS prend désormais en charge le format de fichier ORC  
 La destination du fichier HDFS prend désormais en charge le format de fichier ORC, en plus du format texte et Avro. (La source du fichier HDFS prend en charge uniquement le format texte et Avro.) Pour plus d’informations sur ce composant, consultez [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  

####  <a name="odbc-components-updated-for-sql-server-2016"></a><a name="odbc2016"></a> Composants ODBC mis à jour pour SQL Server 2016  
 Les composants ODBC source et de destination ont été mis à jour pour assurer une compatibilité complète avec SQL Server 2016. Il n’existe aucune nouvelle fonctionnalité, ni aucun changement de comportement.  

####  <a name="explicit-support-for-excel-2016-data-sources"></a><a name="Excel2016"></a> Prise en charge explicite des sources de données Excel 2016  
 Le Gestionnaire de connexions Excel, la source Excel et la destination Excel fournissent désormais une prise en charge explicite des sources de données Excel 2016.  

####  <a name="connector-for-sap-bw-for-sql-server-2016-released"></a><a name="SAPBW"></a> Publication du connecteur pour SAP BW pour SQL Server 2016  
 Microsoft® Connector for SAP BW pour Microsoft SQL Server® 2016 a été publié comme faisant partie du Feature Pack de SQL Server 2016. Pour télécharger les composants du Feature Pack, consultez [Feature Pack Microsoft® SQL Server® 2016](https://www.microsoft.com/download/details.aspx?id=56833).
 
#### <a name="connectors-v40-for-oracle-and-teradata-released"></a><a name="oracleteradata"></a> Publication de la version 4.0 des connecteurs pour Oracle et Teradata
La version 4.0 de Microsoft Connectors pour Oracle et Teradata a été publiée. Pour télécharger les connecteurs, consultez [Microsoft Connectors v4.0 for Oracle and Teradata](https://www.microsoft.com/download/details.aspx?id=52950).

### <a name="connectors-for-analytics-platform-system-pdw-appliance-update-5-released"></a><a name="pdwau5"></a> Publication des connecteurs pour Analytics Platform System (PDW) Appliance Update 5
Les adaptateurs de destination pour le chargement des données dans PDW avec AU5 ont été publiés. Pour télécharger les adaptateurs, consultez [Analytics Platform System Appliance Update 5 Documentation and Client Tools](https://www.microsoft.com/download/details.aspx?id=51610).

### <a name="expanded-connectivity-to-the-cloud"></a>Extension de la connectivité au cloud

####  <a name="azure-feature-pack-for-ssis-released-for-sql-server-2016"></a><a name="AFP2016"></a> Publication d’Azure Feature Pack pour SSIS pour SQL Server 2016  
 Le Feature Pack Azure pour Integration Services a été publié pour SQL Server 2016. Le Feature Pack contient les gestionnaires de connexions permettant de se connecter aux sources de données Azure, ainsi que les tâches permettant d’effectuer les opérations Azure courantes. Pour plus d’informations, consultez [Feature Pack Azure pour Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

#### <a name="support-for-microsoft-dynamics-online-resources-released-in-service-pack-1"></a><a name="dynamics"></a> Prise en charge des ressources Microsoft Dynamics Online publiées dans Service Pack 1

Quand SQL Server 2016 Service Pack 1 est installé, le Gestionnaire de connexions OData et de sources OData prend désormais en charge la connexion aux flux OData de Microsoft Dynamics AX Online et Microsoft Dynamics CRM Online.

#### <a name="support-for-azure-data-lake-store-released"></a><a name="datalakestore"></a> Prise en charge d’Azure Data Lake Store

La dernière version du Feature Pack Azure inclut un gestionnaire de connexions, une source et une destination pour déplacer des données vers et depuis Azure Data Lake Store. Pour plus d’informations, consultez l’article [Le Feature Pack SQL Server Integration Services (SSIS) pour Azure](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

#### <a name="support-for-azure-synapse-analytics-released"></a><a name="sqldwupload"></a> Prise en charge d’Azure Synapse Analytics

La dernière version du Feature Pack Azure comprend la tâche de chargement Azure SQL DW pour renseigner Azure Synapse Analytics avec des données. Pour plus d’informations, consultez l’article [Le Feature Pack SQL Server Integration Services (SSIS) pour Azure](../integration-services/azure-feature-pack-for-integration-services-ssis.md).

## <a name="usability-and-productivity"></a>Convivialité et productivité  
 
### <a name="better-install-experience"></a>Amélioration de l’expérience d’installation

####  <a name="upgrade-blocked-when-ssisdb-belongs-to-an-availability-group"></a><a name="Upgrade"></a> Blocage de la mise à niveau quand SSISDB appartient à un groupe de disponibilité  
 Si la base de données du catalogue SSIS (SSISDB) appartient à un groupe de disponibilité AlwaysOn, vous devez supprimer SSISDB du groupe de disponibilité, mettre à niveau SQL Server, puis rajouter SSISDB au groupe de disponibilité. Pour plus d’informations, consultez [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  

### <a name="better-design-experience"></a>Amélioration de l’expérience de conception

####  <a name="multi-targeting-and-multi-version-support-in-ssis-designer"></a><a name="OneDesigner"></a> Prise en charge du multi-ciblage et de plusieurs versions dans le Concepteur SSIS  
 Vous pouvez désormais utiliser le Concepteur SSIS dans SQL Server Data Tools (SSDT) pour Visual Studio 2015 pour créer, gérer et exécuter les packages qui ciblent SQL Server 2016, SQL Server 2014 ou SQL Server 2012. Pour obtenir SSDT, voir [Télécharger la dernière version de SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md). 

 Dans l’Explorateur de solutions, cliquez avec le bouton droit sur un projet Integration Services, puis sélectionnez **Propriétés** pour ouvrir les pages de propriétés du projet. Sous l’onglet **Général** de **Propriétés de configuration**, sélectionnez la propriété **TargetServerVersion** , puis choisissez SQL Server 2016, SQL Server 2014 ou SQL Server 2012.  
   
 ![Propriété TargetServerVersion dans la boîte de dialogue Propriétés du projet](../integration-services/media/targetserverversion2.png "Propriété TargetServerVersion dans la boîte de dialogue Propriétés du projet")  

> [!IMPORTANT]
> Si vous développez des extensions personnalisées pour SSIS, consultez [Support multi-targeting in your custom components](../integration-services/extending-packages-custom-objects/support-multi-targeting-in-your-custom-components.md) (Prise en charge du multi-ciblage dans vos composants personnalisés) et [Getting your SSIS custom extensions to be supported by the multi-version support of SSDT 2015 for SQL Server 2016](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/)(Préparer vos extensions personnalisées SSIS pour utiliser la prise en charge de plusieurs versions de SSDT 2015 pour SQL Server 2016).  

### <a name="better-management-experience-in-sql-server-management-studio"></a>Amélioration de l’expérience de gestion dans SQL Server Management Studio

####  <a name="improved-performance-for-ssis-catalog-views"></a><a name="CatViews"></a> Performances améliorées pour les affichages catalogue SSIS  
 La plupart des affichages de catalogue SSIS sont maintenant plus performants quand ils sont exécutés par un utilisateur qui n’est pas membre du rôle ssis_admin.  

### <a name="other-enhancements"></a>Autres améliorations

####  <a name="balanced-data-distributor-transformation-is-now-part-of-ssis"></a><a name="BDDinbox"></a> La transformation du distributeur de données équilibrées fait désormais partie de SSIS  
 La transformation du distributeur de données équilibrées, qui nécessitait un téléchargement distinct dans les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], est désormais installée quand vous installez [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Pour plus d’informations, consultez [Balanced Data Distributor Transformation](../integration-services/data-flow/transformations/balanced-data-distributor-transformation.md).  
  
####  <a name="data-feed-publishing-components-are-now-part-of-ssis"></a><a name="ComplexFeedinbox"></a> Les composants de publication du flux de données font désormais partie de SSIS  
 Les composants de publication du flux de données, qui nécessitaient un téléchargement distinct dans les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], sont désormais installés quand vous installez [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Pour plus d’informations, consultez [Data Streaming Destination](../integration-services/data-flow/data-streaming-destination.md).  

####  <a name="support-for-azure-blob-storage-in-the-sql-server-import-and-export-wizard"></a><a name="AzureBlob"></a> Prise en charge du stockage Blob Azure dans l’Assistant Importation et Exportation SQL Server  
 L’Assistant Importation et Exportation SQL Server peut désormais importer des données à partir d’Azure Blob Storage, mais également enregistrer des données à cet emplacement. Pour plus d’informations, consultez [Choisir une source de données &#40;Assistant Importation et Exportation SQL Server&#41;](../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md) et [Choisir une destination &#40;Assistant Importation et Exportation SQL Server&#41;](../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md). 

####  <a name="change-data-capture-designer-and-service-for-oracle-for-microsoft-sql-server-2016-released"></a><a name="CDCOracle"></a> Service et concepteur de capture de données modifiées pour Oracle pour Microsoft SQL Server 2016  
 Le service et le concepteur de capture des changements de données Microsoft® pour Oracle par Attunity pour Microsoft SQL Server® 2016 ont été publiés comme faisant partie du Feature Pack SQL Server 2016.  Ces composants prennent désormais en charge Oracle 12c dans une installation classique. (L’installation multi-locataire n’est pas prise en charge). Pour télécharger les composants du Feature Pack, consultez [Feature Pack Microsoft® SQL Server® 2016](https://www.microsoft.com/download/details.aspx?id=56833).  
  
####  <a name="cdc-components-updated-for-sql-server-2016"></a><a name="cdc2016"></a> Mise à jour des composants de capture de données modifiées pour SQL Server 2016  
 Les composants de capture de données modifiées (CDC), tels que la tâche de contrôle, la source et la transformation de séparateur, ont été mis à jour pour assurer une compatibilité complète avec SQL Server 2016. Il n’existe aucune nouvelle fonctionnalité, ni aucun changement de comportement.  
  
####  <a name="analysis-services-execute-ddl-task-updated"></a><a name="ASDDL"></a> Mise à jour de la tâche DDL d’exécution Analysis Services  
 La tâche DDL d’exécution Analysis Services a été mise à jour pour accepter les commandes du langage TMSL (Tabular Model Scripting Language).

####  <a name="analysis-services-tasks-support-tabular-models"></a><a name="ssasrc0"></a> Les tâches Analysis Services prennent en charge les modèles tabulaires  
 Vous pouvez désormais utiliser toutes les tâches et destinations SSIS qui prennent en charge SQL Server Analysis Services (SSAS) avec des modèles tabulaires SQL Server 2016. Les tâches SSIS ont été mises à jour pour représenter des objets tabulaires au lieu d’objets multidimensionnels. Par exemple, quand vous sélectionnez des objets à traiter, la tâche de traitement Analysis Services détecte automatiquement un modèle tabulaire et affiche une liste d’objets tabulaires au lieu de montrer des groupes et des dimensions de mesures. Désormais, la destination de traitement de partition affiche également des objets tabulaires et prend en charge le Push de données dans une partition.  
  
 La destination de traitement de dimension ne fonctionne pas pour les modèles tabulaires avec le niveau de compatibilité SQL 2016.  La tâche de traitement Analysis Services et la destination de traitement de partition sont tout ce dont vous avez besoin pour le traitement tabulaire. 

####  <a name="support-for-built-in-r-services"></a><a name="builtinR"></a> Prise en charge de R Services intégré  
 SSIS prend déjà en charge les services R intégrés dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Vous pouvez utiliser SSIS non seulement pour extraire les données et charger la sortie de l’analyse, mais également pour créer, exécuter et recycler régulièrement les modèles R. Pour plus d’informations, consultez le billet de blog suivant. [Operationalize your machine learning project using SQL Server 2016 SSIS and R Services (Faire fonctionner votre projet d’apprentissage machine à l’aide de SQL Server 2016 SSIS et des services R)](https://techcommunity.microsoft.com/t5/sql-server-integration-services/operationalize-your-machine-learning-project-using-sql-server/ba-p/388296). 

####  <a name="rich-xml-validation-output-in-the-xml-task"></a><a name="ValidateXML"></a> Sortie de validation XML détaillée dans la tâche XML  
 Validez des documents XML et obtenez une sortie d’erreur détaillée en activant la propriété **ValidationDetails** de la tâche XML. Avant que la propriété **ValidationDetails** ne soit disponible, la validation XML par la tâche XML ne renvoyait qu’un résultat true ou false, sans aucune information sur les erreurs ou leur emplacement. À présent, quand vous définissez **ValidationDetails** sur true, le fichier de sortie contient des informations détaillées sur chaque erreur, notamment le numéro de ligne et la position. Vous pouvez utiliser ces informations pour comprendre, localiser et corriger les erreurs dans les documents XML. Pour plus d’informations, consultez [Validate XML with the XML Task](../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 [!INCLUDE[ssIS](../includes/ssis-md.md)] a introduit la propriété **ValidationDetails** dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Service Pack 2. Cette nouvelle propriété n’a pas été annoncée ou documentée à ce moment-là. La propriété **ValidationDetails** est également disponible dans [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] et dans [!INCLUDE[ssSQL15](../includes/sssql16-md.md)].   

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

## <a name="see-also"></a>Voir aussi  
 [Nouveautés de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)   
 [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../sql-server/editions-and-components-of-sql-server-2016.md)