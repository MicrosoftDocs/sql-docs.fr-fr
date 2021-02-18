---
title: Nouveautés de SQL Server 2016
description: Découvrez les nouvelles fonctionnalités de sécurité de SQL Server 2016, les fonctionnalités d’interrogation, l’intégration de Hadoop et du cloud, l’analytique R et bien plus encore.
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
keywords:
- nouveautés dans SQL Server
helpviewer_keywords:
- new features [SQL Server]
- SQL Server, what's new
- SQL Server 2008 what's new
- what's new [SQL Server]
ms.assetid: 6a428023-e3cc-4626-a88a-4c13ccbd7db0
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: c9313877cac9c4e22fdaa4234fc40f0701649934
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271736"
---
# <a name="whats-new-in-sql-server-2016"></a>Nouveautés de SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]    
 Avec SQL Server 2016, vous pouvez créer des applications stratégiques intelligentes à l’aide d’une plateforme de base de données hybride évolutive où tout est intégré, depuis les performances en mémoire et la sécurité avancée jusqu’à l’analytique en base de données. La version SQL Server 2016 ajoute de nouveaux composants de sécurité, des fonctionnalités d’interrogation, l’intégration de Hadoop et du cloud, l’analytique R et plus encore, ainsi que de nombreuses améliorations et de nombreux perfectionnements. 

Cette page offre une synthèse ainsi que des liens vers des informations plus détaillées sur les nouveautés de SQL Server 2016 pour chaque composant SQL Server. 

![SQL Server 2016](../sql-server/media/sql-server-2016.png)

 **Essayez SQL Server aujourd’hui !** 
- Téléchargez **gratuitement** [**la dernière version de SQL Server !**](https://www.microsoft.com/sql-server/sql-server-downloads).
- Téléchargez la dernière version de [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). 
- Vous avez un compte Azure ? Faites tourner une [machine virtuelle sur laquelle SQL Server 2016 est déjà installé](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview).

## <a name="sql-server-2016-database-engine"></a>Moteur de base de données SQL Server 2016
- Il est à présent possible de configurer plusieurs fichiers de base de données **tempDB** pendant l’installation et la configuration de SQL Server.
- Le nouveau **Magasin des requêtes** stocke le texte des requêtes, les plans d’exécution et les indicateurs de performances dans la base de données, ce qui facilite le monitoring et la résolution des problèmes de performances. Un tableau de bord affiche les requêtes qui ont consommé le plus de temps, de mémoire ou de ressources du processeur.
- Les **tables temporelles** sont des tables d’historique qui enregistrent toutes les modifications de données, avec la date et l’heure auxquelles elles se sont produites.
- La nouvelle **prise en charge de JSON** intégrée à SQL Server gère les importations, les exportations, l’analyse et le stockage JSON.
- Le nouveau moteur d’interrogation **PolyBase** intègre SQL Server avec des données externes dans Hadoop ou le Stockage Blob Azure. Vous pouvez importer et exporter des données, de même qu’exécuter des requêtes.
- La nouvelle fonctionnalité **Stretch Database** permet d’archiver des données, de manière dynamique et en toute sécurité, d’une base de données SQL Server locale vers une base de données SQL Azure dans le cloud. SQL Server interroge automatiquement les données locales et distantes dans les bases de données liées. 
- **OLTP en mémoire :** 
    - prend maintenant en charge les contraintes FOREIGN KEY, UNIQUE et CHECK, les procédures stockées compilées natives OR, NOT, SELECT DISTINCT et OUTER JOIN et les sous-requêtes dans SELECT ;
    - prend en charge des tables d’une taille pouvant atteindre 2 To (à partir de 256 Go) ; 
    - a fait l’objet d’améliorations des index columnstore pour le tri et la prise en charge des groupes de disponibilité AlwaysOn.
- Nouvelles fonctionnalités de sécurité :
    - **Always Encrypted :** lorsqu’elle est activée, seule l’application qui possède la clé de chiffrement peut accéder aux données sensibles chiffrées de la base de données SQL Server 2016. La clé n’est jamais transmise à SQL Server.
    - **Dynamic Data Masking :** s’il est spécifié dans la définition de table, les données masquées sont cachées à la plupart des utilisateurs ; seuls les utilisateurs possédant l’autorisation UNMASK peuvent voir la totalité des données.
    - **Sécurité au niveau des lignes :** l’accès aux données peut être restreint au niveau du moteur de base de données, afin que les utilisateurs ne voient que ce qui les concerne. 

## <a name="sql-server-2016-analysis-services-ssas"></a>SQL Server 2016 Analysis Services (SSAS)
SQL Server 2016 Analysis Services offre une amélioration des performances, de la création, de la gestion, du filtrage, du traitement et plus encore pour les bases de données model tabulaires en fonction du **niveau de compatibilité 1200**.
- **[SQL Server R Services](~/machine-learning/what-s-new-in-sql-server-machine-learning-services.md)** intègre le langage de programmation R, utilisé pour l’analyse statistique, dans SQL Server. 
- Le nouveau **vérificateur de cohérence de base de données (DBCC)** s’exécute en interne pour détecter les problèmes potentiels d’altération des données.
- **Direct Query**, qui interroge des données externes en direct plutôt qu’après les avoir importées, prend désormais en charge d’autres sources de données, notamment SQL Azure, Oracle et Teradata. 
- Il y a beaucoup de nouvelles **fonctions DAX (Data Access Expressions)** .
- Le nouvel espace de noms **[Microsoft.AnalysisServices.Tabular](/dotnet/api/microsoft.analysisservices.tabular)** gère les modèles et les instances en mode tabulaire. 
- [Analysis Services Management Objects (AMO)](/dotnet/api/) a été réusiné de façon à inclure un deuxième assembly, **Microsoft.AnalysisServices.Core.dll**.

Consultez la page [Moteur Analysis Services (SSAS)](/analysis-services/what-s-new-in-analysis-services). 

## <a name="sql-server-2016-integration-services-ssis"></a>SQL Server 2016 Integration Services (SSIS)
- Prise en charge des **groupes de disponibilité AlwaysOn**
- **Déploiement incrémentiel de packages**
- Prise en charge **d’Always Encrypted**
- Nouveau rôle **ssis_logreader** au niveau de la base de données
- Nouveau **niveau de journalisation personnalisée**
- **Noms de colonnes pour les erreurs** contenues dans le flux de données 
- Nouveaux **connecteurs**
- Prise en charge du **système de fichiers Hadoop (HDFS)**

Consultez la page [Integration Services (SSIS)](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md).

## <a name="sql-server-2016-master-data-services-mds"></a>SQL Server 2016 Master Data Services (MDS)
- **Amélioration de la hiérarchie dérivée**, avec prise en charge des hiérarchies récursives et à origines et destinations multiples
- Filtrage **d’attributs par domaines**
- **Synchronisation d’entités** pour partager des données d’entité entre les modèles
- Flux de travail approbation par le biais des **ensembles de modifications**
- **Index personnalisés** pour améliorer les performances des requêtes
- Nouveaux **niveaux d’autorisation** pour améliorer la sécurité
- Expérience de **gestion des règles métier** repensée

Consultez la page [Master Data Services (MDS)](../master-data-services/what-s-new-in-master-data-services-mds.md).

## <a name="sql-server-2016-reporting-services-ssrs"></a>SQL Server 2016 Reporting Services (SSRS)
Microsoft a soigneusement remanié Reporting Services dans cette version. 
- Nouveau **Portail de rapports web** avec fonctionnalité d’indicateur de performance clé
- Nouvel **Éditeur de rapports mobiles**
- **Moteur de génération de rapport repensé** qui prend en charge HTML5 
- Nouveaux **types de graphiques** de compartimentage et en rayons de soleil 

Consultez la page [Reporting Services (SSRS)](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).

## <a name="next-steps"></a>Étapes suivantes   
- [Programme d’installation de SQL Server](../database-engine/install-windows/install-sql-server.md)   
- [Notes de publication de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md) 
- [Feuille de données SQL Server 2016](https://download.microsoft.com/download/C/5/3/C53C3AEF-653C-4598-8721-D522E8AC6A3A/SQL_Server_2016_Everything_Built-In_Datasheet_EN_US.pdf)
- [Fonctionnalités prises en charge par les éditions de SQL Server](./editions-and-components-of-sql-server-2016.md)
- [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
- [Installer SQL Server 2016 avec l’Assistant Installation](../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)
- [Configuration et installation de maintenance](../database-engine/install-windows/install-sql-server-servicing-updates.md)
- [Nouveau module SQL PowerShell](https://blogs.technet.microsoft.com/dataplatforminsider/2016/06/30/sql-powershell-july-2016-update/)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]