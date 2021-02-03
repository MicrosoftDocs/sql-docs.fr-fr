---
description: Applications de la couche Données
title: Applications de la couche Données | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- designing DACs
- How to [DAC]
- data-tier application [SQL Server], designing
- wizard [DAC]
ms.assetid: a04a2aba-d07a-4423-ab8a-0a31658f6317
author: stevestein
ms.author: sstein
ms.openlocfilehash: 562769cc1f9ce3e895b59811a61026747c0a3724
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251092"
---
# <a name="data-tier-applications"></a>Applications de la couche Données
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Une application de la couche Données (DAC) est une entité de gestion de base de données logique qui définit tous les objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], notamment les tables, les vues et les objets d’instance, y compris les connexions, qui sont associés à une base de données utilisateur. Une application DAC est une unité autonome de déploiement de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui permet aux développeurs et aux administrateurs de base de données de la couche Données d'empaqueter les objets [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans un artefact portable appelé « package DAC », ou encore DACPAC.  
  
 Un BACPAC est un artefact connexe qui encapsule le schéma de la base de données, ainsi que les données stockées dans la base de données.  
  
## <a name="benefits-of-data-tier-applications"></a>Avantages des applications de la couche Données  
 Le cycle de vie de la plupart des applications de base de données implique le partage et l'échange par les administrateurs de bases de données et les développeurs de scripts et de notes d'intégration ad hoc pour les opérations de mise à jour et de maintenance d'application. Bien que cela soit acceptable pour un petit nombre de bases de données, cela devient rapidement ingérable dès lors que les bases de données se développent et que leur nombre, leur taille et leur complexité augmentent.  
  
 Une application DAC correspond à un outil de productivité et de gestion du cycle de vie de base de données qui permet au développement de base de données déclaratif de simplifier le déploiement et la gestion. Un développeur peut créer une base de données dans un projet SQL Server Data Tools, puis créer la base de données dans un DACPAC pour la transférer à un administrateur de base de données. L'administrateur peut alors déployer la DAC à l'aide de SQL Server Management Studio dans une instance de test ou de production de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou de [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Sinon, l'administrateur peut utiliser le DACPAC pour mettre à niveau une base de données déjà déployée à l'aide de SQL Server Management Studio. Pour terminer le cycle de vie, l'administrateur peut extraire la base de données dans un DACPAC et la remettre à un développeur afin de refléter les réglages de test ou de production, ou de permettre d'autres modifications de conception de base de données en réponse à des modifications de l'application.  
  
 L'avantage d'un déploiement piloté par une DAC par rapport à celui piloté par script est que l'outil aide l'administrateur à identifier et à valider les comportements émanant de bases de données sources et cibles différentes. Lors de mises à niveau, l'outil avertit l'administrateur si la mise à niveau peut entraîner la perte de données ; il fournit également un plan de mise à niveau. L'administrateur peut évaluer le plan, puis utiliser l'outil pour poursuivre la mise à niveau.  
  
 Les DAC prennent également en charge le contrôle de version pour aider le développeur et l’administrateur à conserver et à gérer la traçabilité de la base de pendant tout son cycle de vie.  
  
## <a name="dac-concepts"></a>Concepts DAC  
 Une DAC simplifie le développement, le déploiement et la gestion des éléments de la couche Données qui prennent en charge une application :  
  
-   Une application de la couche Données (DAC) est une entité de gestion de base de données logique qui définit tous les objets SQL Server, tels que les tables, les vues et les objets d’instance, qui sont associés à une base de données utilisateur. Il s'agit d'une unité autonome d'un déploiement de base de données SQL Server qui permet aux développeurs de la couche Données et aux administrateurs de bases de données d'empaqueter des objets SQL Server dans un artefact portable appelé « package DAC », ou fichier .dacpac.  
  
-   Pour qu’une base de données SQL Server soit traitée comme une DAC, elle doit être inscrite, soit explicitement par une opération utilisateur, soit implicitement par l’une des opérations DAC. Lorsqu'une base de données est inscrite, la version de la DAC et d'autres propriétés sont enregistrées dans le cadre des métadonnées de la base de données. Inversement, l'inscription d'une base de données peut également être annulée et les propriétés de la DAC supprimées.  
  
-   En général, les outils DAC sont capables de lire des fichiers DACPAC générés par les outils DAC de versions précédentes de SQL Server et peuvent également déployer des fichiers DACPAC dans des versions précédentes de SQL Server. Toutefois, les outils DAC de versions antérieures ne peuvent pas lire les fichiers DACPAC générés par les outils DAC de versions ultérieures. Plus précisément :  
  
    -   Les opérations DAC sont apparues dans SQL Server 2008 R2. Outre les bases de données SQL Server 2008 R2, les outils prennent en charge la génération de fichiers DACPAC provenant de bases de données SQL Server 2008, SQL Server 2005 et SQL Server 2000.  
  
    -   En plus des bases de données SQL 2016, les outils fournis avec SQL Server 2016 peuvent lire des fichiers DACPAC générés par les outils DAC livrés avec SQL Server 2008 R2 ou SQL Server 2012. Cela inclut les bases de données SQL Server 2014, 2012, 2008 R2, 2008 et 2005, mais **pas** SQL Server 2000.  
  
    -   Les outils DAC de SQL Server 2008 R2 ne peuvent pas lire les fichiers DACPAC générés par les outils de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
-   Un DACPAC est un fichier Windows avec une extension .dacpac. Le fichier prend en charge un format ouvert constitué de plusieurs sections XML représentant des détails de l'origine de DACPAC, des objets de la base de données, ainsi que d'autres caractéristiques. Un utilisateur expérimenté peut décompresser le fichier à l'aide de l'utilitaire DacUnpack.exe fourni avec le produit afin d'examiner chaque section plus en détail.  
  
-   L'utilisateur doit être membre du rôle **dbmanager** ou disposer d'autorisations **CREATE DATABASE** afin de créer une base de données, ce qui inclut la création d’une base de données en déployant un package DAC. L'utilisateur doit être membre du rôle **dbmanager**, ou bénéficier d'autorisations **DROP DATABASE** pour pouvoir supprimer une base de données.  
  
## <a name="dac-tools"></a>Outils DAC  
 Un DACPAC peut être utilisé de façon transparente avec plusieurs outils fournis avec [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Ces outils répondent aux conditions des différents utilisateurs recourant à un DACPAC comme unité d'interopérabilité.  
  
-   Développeurs d’applications :  
  
    -   Possibilité d’utiliser un projet de base de données d’outils de données SQL Server pour concevoir une base de données. Une génération réussie de ce projet entraîne la création d'un DACPAC contenu dans un fichier .dacpac.  
  
    -   Possibilité d’importer un DACPAC dans un projet de base de données et de poursuivre la conception de la base de données.  
  
        Les outils de données SQL Server prennent également en charge une base de données locale pour le développement d'applications de base de données non connecté, côté client. Le développeur peut prendre un instantané de cette base de données locale afin de créer un DACPAC contenu dans un fichier .dacpac.  
  
    -   Indépendamment, le développeur peut publier un projet de base de données directement dans une base de données, sans même générer un DACPAC. L'opération de publication suit le même comportement que l'opération de déploiement d'autres outils.  
  
-   Administrateurs de bases de données :  
  
    -   Possibilité d’utiliser SQL Server Management Studio pour extraire un DACPAC d’une base de données existante et d’exécuter également d’autres opérations DAC.  
  
    -   De plus, le DBA d’un [!INCLUDE[ssSDS](../../includes/sssds-md.md)] peut utiliser le portail Azure pour les opérations DAC.  
  
-   Éditeurs de logiciels :  
  
    -   Les services d'hébergement et d'autres produits de gestion des données pour SQL Server peuvent utiliser l'API DACFx pour les opérations DAC.  
  
-   Administrateurs informatiques :  
  
    -   Les intégrateurs et les administrateurs de systèmes informatiques peuvent utiliser l'outil de ligne de commande SqlPackage.exe pour les opérations DAC.  
  
## <a name="dac-operations"></a>Opérations DAC  
 Une DAC prend en charge les opérations suivantes :  
  
-   **EXTRACT** : l’utilisateur peut extraire une base de données dans un DACPAC.  
  
-   **DEPLOY** : l’utilisateur peut déployer un DACPAC sur un serveur hôte. Quand le déploiement est effectué à partir d’un outil de gestion comme SQL Server Management Studio ou du portail Azure, la base de données obtenue sur le serveur hôte est implicitement inscrite en tant qu’application de la couche Données.  
  
-   **REGISTER** : l’utilisateur peut inscrire une base de données en tant qu’application de la couche Données.  
  
-   **UNREGISTER** : il est possible d’annuler l’inscription d’une base de données déjà inscrite comme DAC.  
  
-   **UPGRADE** : une base de données peut être mise à niveau à l’aide d’un DACPAC. La mise à niveau est prise en charge même sur les bases de données qui ne sont pas déjà inscrites en tant qu'applications de la couche Données, mais qui le sont implicitement suite à une mise à niveau.  
  
## <a name="bacpac"></a>BACPAC  
 Un BACPAC est un fichier Windows avec une extension .bacpac qui encapsule le schéma et les données d’une base de données. L’utilisation principale d’un BACPAC consiste à déplacer une base de données d’un serveur vers un autre, ou de [migrer une base de données d’un serveur local vers le cloud](/azure/azure-sql/database/migrate-to-database-from-sql-server), et à archiver une base de données existante dans un format ouvert.  
 Tout comme le DACPAC, le format de fichier BACPAC est ouvert ; le contenu de schéma du BACPAC est identique à celui du DACPAC. Les données d’un BACPAC sont stockées au format JSON.  
  
 Les DACPAC et BACPAC sont similaires, mais ils conviennent à des scénarios différents. Le but d'un DACPAC est de capturer et de déployer le schéma, y compris de mettre à niveau une base de données existante. Un DACPAC sert essentiellement à déployer un schéma bien défini dans les environnements de développement, de test, puis de production. Il permet également d’effectuer l’opération inverse, c’est-à-dire capturer le schéma de l’environnement de production, puis l’appliquer aux environnements de test et de développement.  
  
 Un BACPAC, en revanche, consiste principalement à capturer le schéma et les données prenant en charge deux opérations majeures :  
  
-   **EXPORT** : l’utilisateur peut exporter le schéma et les données d’une base de données vers un BACPAC.  
  
-   **IMPORT** : l’utilisateur peut importer le schéma et les données dans une nouvelle base de données du serveur hôte.  
  
 Ces deux fonctionnalités sont prises en charge par les outils de gestion de bases de données : SQL Server Management Studio, le portail Azure et l’API DACFx.  
  
## <a name="permissions"></a>Autorisations  
 Vous devez être membre du rôle **dbmanager** ou disposer d'autorisations **CREATE DATABASE** pour pouvoir créer une base de données, notamment en déployant un package DAC. Vous devez être membre du rôle **dbmanager** ou disposer d'autorisations **DROP DATABASE** pour pouvoir supprimer une base de données.  
  
## <a name="data-tier-application-tasks"></a>Tâches de l'application de la couche Données  
  
|Tâche|Lien de rubrique|  
|----------------------|-----------|  
|Explique comment utiliser un fichier de package DAC pour créer une nouvelle instance de la DAC.|[Déployer une application de la couche Données](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)|  
|Explique comment utiliser un nouveau fichier de package DAC pour mettre à niveau une instance vers une nouvelle version de la DAC.|[Mettre à niveau une application de la couche Données](../../relational-databases/data-tier-applications/upgrade-a-data-tier-application.md)|  
|Explique comment supprimer une instance DAC. Vous pouvez également choisir de détacher ou de supprimer la base de données associée, ou de laisser la base de données intacte.|[Supprimer une application de la couche Données](../../relational-databases/data-tier-applications/delete-a-data-tier-application.md)|  
|Explique comment afficher l'état des DAC actuellement déployées à l'aide de l'utilitaire SQL Server.|[Analyser les applications de la couche Données](../../relational-databases/data-tier-applications/monitor-data-tier-applications.md)|  
|Explique comment créer un fichier .bacpac contenant une archive des données et des métadonnées dans une DAC.|[Exporter une application de la couche Données](../../relational-databases/data-tier-applications/export-a-data-tier-application.md)|  
|Explique comment utiliser un fichier d'archive DAC (.bacpac) pour effectuer une restauration logique d'une DAC ou pour migrer la DAC vers une autre instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou [!INCLUDE[ssSDS](../../includes/sssds-md.md)].|[Importer un fichier BACPAC pour créer une nouvelle base de données utilisateur](../../relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database.md)|  
|Indique comment importer un fichier BACPAC pour créer une nouvelle base de données utilisateur dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|[Extraire une DAC d'une base de données](../../relational-databases/data-tier-applications/extract-a-dac-from-a-database.md)|  
|Explique comment promouvoir une base de données existante en tant qu'instance DAC. Une définition de DAC est créée et stockée dans les bases de données système.|[Inscrire une base de données en tant que DAC](../../relational-databases/data-tier-applications/register-a-database-as-a-dac.md)|  
|Explique comment vérifier le contenu d'un package DAC et les actions associées à la mise à niveau d'une DAC avant d'utiliser le package dans un système de production.|[Valider un package DAC](../../relational-databases/data-tier-applications/validate-a-dac-package.md)|  
|Explique comment placer le contenu d'un package DAC dans un dossier où un administrateur de base de données peut vérifier ce que fait la DAC avant de la déployer dans un serveur de production.|[Décompresser un package DAC](../../relational-databases/data-tier-applications/unpack-a-dac-package.md)|  
|Explique comment utiliser un Assistant pour déployer une base de données existante. L'Assistant utilise les DAC pour exécuter le déploiement.|[Déployer une base de données à l’aide d’une DAC](../../relational-databases/data-tier-applications/deploy-a-database-by-using-a-dac.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en charge DAC pour les objets et versions SQL Server](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))  
  
