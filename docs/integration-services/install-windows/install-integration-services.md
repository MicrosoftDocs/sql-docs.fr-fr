---
title: Installer SQL Server Integration Services | Microsoft Docs
description: Découvrez comment installer Microsoft SQL Server Integration Services (SSIS) et comment obtenir d’autres téléchargements pour SSIS.
ms.custom: ''
ms.date: 09/19/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, installing
- SSIS, installing
- installing Integration Services, about installing Integration Services
- SQL Server Integration Services, installing
- Setup [Integration Services], about installing Integration Services
- installing Integration Services
- Setup [Integration Services]
ms.assetid: bd20fd3a-414b-4581-959d-ebba4ddf5a55
author: chugugrace
ms.author: chugu
ms.openlocfilehash: abb62aec3a712ace96d6db82ea8eb0085d8a9e74
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049273"
---
# <a name="install-integration-services-ssis"></a>Installer Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit un programme d’installation unique pour installer tout ou une partie de ses composants, notamment [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Utilisez-le pour installer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec ou sans d’autres composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un seul ordinateur.

 Cet article met l’accent sur quelques facteurs importants qu’il convient de prendre en compte avant d’installer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Les informations de cet article vous aident à évaluer les options d’installation, de façon à ce que votre sélection aboutisse à une installation réussie.

![Installer Integration Services](install-integration-services/install-integration-services-sql-setup.png)

## <a name="get-ready-to-install-integration-services"></a>Préparer l’installation d’Integration Services

Avant d’installer [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vérifiez les informations suivantes :

- [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)

- [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)

## <a name="install-standalone-or-side-by-side"></a>Installer la version autonome ou côte à côte

Vous pouvez installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans les configurations suivantes :

- Vous pouvez installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un ordinateur dépourvu d’instances antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Vous pouvez installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] côte à côte avec une instance existante de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].

Quand vous effectuez une mise à niveau vers la dernière version de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur un ordinateur où une version antérieure de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est déjà installée, la version actuelle est installée côte à côte avec la version antérieure.

Pour plus d’informations sur la mise à niveau de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Mettre à niveau Integration Services](../../integration-services/install-windows/upgrade-integration-services.md).

## <a name="get-sql-server-with-integration-services"></a>Obtenir SQL Server avec Integration Services

Si vous ne disposez pas de Microsoft SQL Server, téléchargez une édition d’évaluation gratuite ou l’édition développeur gratuite sur [Téléchargements SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads). SSIS n’est pas inclus dans l’édition Express de SQL Server.

## <a name="install-integration-services"></a>Installer Integration Services

 Après avoir pris connaissance de la configuration requise pour l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et vérifié que votre ordinateur répondait à ces critères, vous pouvez installer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].

Si vous utilisez l’Assistant Installation pour installer [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous utilisez une série de pages pour spécifier les composants et les options.

- Dans la page **Sélection des fonctionnalités**, sous **Fonctionnalités partagées**, sélectionnez **Integration Services**.

- Sous **Fonctionnalités de l’instance**, sélectionnez si vous le souhaitez **Services Moteur de base de données** pour héberger la base de données du catalogue SSIS, `SSISDB`, afin de stocker, gérer, exécuter et surveiller des packages SSIS.

- Pour installer des assemblys managés pour la programmation [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], également sous **Fonctionnalités partagées**, sélectionnez **Kit de développement logiciel (SDK) des outils clients**.

> [!NOTE]
> Parmi les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous pouvez sélectionner dans la page **Sélection de composant** de l’Assistant Installation, certains n’installent qu’une partie des composants [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Ces composants sont utiles pour des tâches spécifiques, mais les fonctionnalités de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] s’en trouvent limitées. Par exemple, l'option **Services Moteur de base de données** installe les composants [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] requis pour l'Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour garantir une installation complète de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vous devez sélectionner **Integration Services** dans la page **Sélection de composant** .

### <a name="installing-a-dedicated-server-for-etl-processes"></a>Installer un serveur dédié pour les processus ETL

Pour utiliser un serveur dédié pour les processus d’extraction, de transformation et de chargement (ETL), installez une instance locale du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] quand vous installez [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stocke habituellement les packages dans une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et compte sur l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour la planification de ces packages. Si le serveur ETL ne dispose pas d’une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], vous devez planifier ou exécuter les packages à partir d’un serveur qui a une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cela signifie que les packages ne s’exécutent pas sur le serveur ETL, mais sur le serveur à partir duquel ils sont démarrés. En conséquence, les ressources du serveur ETL dédié ne sont pas utilisées comme prévu. De plus, les ressources d'autres serveurs peuvent être éprouvées par les processus ETL en cours d'exécution

### <a name="configuring-ssis-event-logging"></a>Configurer la journalisation des événements SSIS

Par défaut, dans une nouvelle installation, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est configuré de façon à ne pas journaliser les événements en rapport avec l'exécution de packages dans le journal des événements des applications. Ce paramètre limite le nombre d'entrées de journal des événements lorsque vous utilisez la fonctionnalité du collecteur de données de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Les événements qui ne sont pas consignés sont l’ID d’événement 12288, « Le package a démarré » et l’ID d’événement 12289, « Le package s’est terminé avec succès ». Pour enregistrer ces événements dans le journal des événements des applications, ouvrez le Registre pour y apporter des modifications. Dans le Registre, recherchez le nœud HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS et modifiez la valeur DWORD du paramètre LogPackageExecutionToEventLog en remplaçant 0 par 1.

## <a name="install-additional-components-for-integration-services"></a>Installer des composants supplémentaires pour Integration Services

Pour une installation complète de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], sélectionnez les composants dont vous avez besoin dans la liste suivante :

- **Integration Services (SSIS)** . Installer SSIS avec l’Assistant Installation de SQL Server. La sélection de SSIS installe les éléments suivants :

  - Prise en charge du catalogue SSIS sur le moteur de base de données SQL Server

  - Éventuellement, la [fonctionnalité Scale Out](../scale-out/walkthrough-set-up-integration-services-scale-out.md), qui se compose d’un Master et de Workers.

  - Les composants SSIS 32 bits et 64 bits

  - L’installation de SSIS n’installe **PAS** les outils nécessaires pour concevoir et développer des packages SSIS.

- **Moteur de base de données SQL Server**. Installer le moteur de base de données avec l’Assistant Installation de SQL Server. La sélection du moteur de base de données vous permet de créer et d’héberger la base de données du catalogue SSIS, `SSISDB`, afin de stocker, gérer, exécuter et surveiller des packages SSIS.

- **SQL Server Data Tools (SSDT)** . Pour télécharger et installer SSDT, consultez [Télécharger SQL Server Data Tools (SSDT)](../../ssdt/download-sql-server-data-tools-ssdt.md). L’installation de SSDT vous permet de concevoir et de déployer des packages SSIS. SSDT installe les éléments suivants :

  - Les outils de conception et de développement de package SSIS, notamment le concepteur SSIS

  - Les composants SSIS 32 bits uniquement

  - Une version limitée de Visual Studio (si aucune édition de Visual Studio n’est déjà installée).

  - Visual Studio Tools for Applications (VSTA), l’éditeur de script utilisé par le composant de script et la tâche de script SSIS

  - Les Assistants SSIS, notamment l’Assistant Déploiement et l’Assistant Mise à niveau de packages

  - L’Assistant Importation et Exportation SQL Server

- **SQL Server Data Tools (SSDT)** . Nous avons supprimé le programme d’installation autonome de SSDT pour Visual Studio 2019. Pour Visual Studio 2019, vous pouvez désormais obtenir l’extension du concepteur SSIS à partir de la [Place de marché Visual Studio](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects&ssr=false#overview).

- **Integration Services Feature Pack pour Azure**. Pour télécharger et installer le Feature Pack, consultez [Feature Pack Microsoft SQL Server Integration Services pour Azure](../azure-feature-pack-for-integration-services-ssis.md). L’installation du Feature Pack permet à vos packages de se connecter aux services de stockage et d’analytique dans le cloud Azure, notamment aux services suivants :

  - Stockage Blob Azure.

  - Azure HDInsight

  - Azure Data Lake Store

  - Azure Synapse Analytics

  - Azure Data Lake Storage (Gen2).

- **Composants supplémentaires facultatifs**. Vous pouvez éventuellement télécharger des composants tiers supplémentaires à partir du SQL Server Feature Package.

  - Microsoft® Connector pour SAP BW pour Microsoft SQL Server®. Pour obtenir ces composants, consultez [Microsoft® SQL Server® 2017 Feature Pack](https://www.microsoft.com/download/details.aspx?id=55992).

  - Microsoft Connectors pour Oracle et Teradata par Attunity. Pour obtenir ces composants, consultez [Connecteurs Attunity](../attunity-connectors.md).

## <a name="next-steps"></a>Étapes suivantes

- [Installation de plusieurs versions Integration Services côte à côte](installing-integration-services-versions-side-by-side.md)

- [Nouveautés d’Integration Services dans SQL Server 2016](../what-s-new-in-integration-services-in-sql-server-2016.md)

- [Nouveautés d’Integration Services dans SQL Server 2017](../what-s-new-in-integration-services-in-sql-server-2017.md)
