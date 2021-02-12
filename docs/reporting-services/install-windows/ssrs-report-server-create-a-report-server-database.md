---
title: Créer une base de données du serveur de rapports, Gestionnaire de configuration | Microsoft Docs
description: Le mode natif SQL Server Reporting Services utilise deux bases de données relationnelles SQL Server pour le stockage des objets et des métadonnées du serveur de rapports. Une base de données est utilisée pour le stockage principal et l'autre pour le stockage des données temporaires.
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 9/2/2020
ms.openlocfilehash: 70a92bf37285e9af58dcef2e7a69abfb88b37178
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067094"
---
# <a name="create-a-report-server-database-report-server-configuration-manager"></a>Créer une base de données du serveur de rapports, Gestionnaire de configuration du serveur de rapports  

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Le mode natif SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise deux bases de données relationnelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le stockage des objets et des métadonnées du serveur de rapports. Une base de données est utilisée pour le stockage principal et l'autre pour le stockage des données temporaires. 

Les bases de données sont créées ensemble et liées par le nom. Avec une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par défaut, les bases de données ont pour nom respectif **reportserver** et **reportservertempdb**. Les deux bases de données sont collectivement appelées **base de données de serveur de rapports** ou **catalogue du serveur de rapports**.

::: moniker range="=sql-server-2016"

Le [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]mode SharePoint **SQL Server** inclut une troisième base de données utilisée pour les métadonnées d’alerte des données. Les trois bases de données sont créées pour chaque application de service SSRS. Les noms de base de données par défaut incluent un identificateur global unique (GUID) qui représente l’application de service. 

Voici des exemples de noms des trois bases de données en mode SharePoint :

- ReportingService_90a9f37075544f22953c4a62e4a9f370  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370TempDB  
  
- ReportingService_90a9f37075544f22953c4a62e4a9f370_Alerting  

::: moniker-end
  
> [!IMPORTANT]  
> N'écrivez pas d'applications qui exécutent des requêtes sur la base de données du serveur de rapports. La base de données du serveur de rapports n'est pas un schéma public. La structure des tables peut changer d'une version à la suivante. Si vous écrivez une application qui nécessite un accès à la base de données du serveur de rapports, utilisez toujours les API SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour accéder à la base de données du serveur de rapports.  
>
> Les vues du journal des exécutions constituent des exceptions à cette règle. Pour plus d’informations, consultez [Journal des exécutions du serveur de rapports et vue ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
## <a name="ways-to-create-the-report-server-database"></a>Méthodes pour créer la base de données de serveur de rapports

 ### <a name="native-mode"></a>en mode natif
 Vous pouvez créer la base de données du serveur de rapports en mode natif en procédant comme suit :  
  
- **Automatique**. Utilisez l’Assistant Installation de SQL Server si vous choisissez l’option d’installation de la configuration par défaut. Dans l’Assistant Installation de SQL Server, cette option s’appelle **Installer et configurer** dans la page des **options d’installation du serveur de rapports**. Si vous choisissez l’option **Installer uniquement**, vous devez utiliser le Gestionnaire de configuration du serveur de rapports de SQL Server pour créer la base de données.  
  
- **Manuel**. Utilisez le Gestionnaire de configuration de SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Créez la base de données du serveur de rapports manuellement si vous utilisez un [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] distant pour héberger la base de données. Pour plus d’informations, consultez [Créer une base de données du serveur de rapports en mode natif](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  

::: moniker range="=sql-server-2016"
  
### <a name="sharepoint-mode"></a>Mode SharePoint 
La page des **options d’installation du serveur de rapports** n’offre que l’option **Installer uniquement** pour le mode SharePoint. Cette option installe tous les fichiers SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ainsi que le service partagé SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. L'étape suivante consiste à créer au moins une application de service SSRS de l'une des manières suivantes :  
  
- Accédez à l'Administration centrale dans SharePoint Server pour créer une application de service SSRS. Pour plus d’informations, consultez la section **créer une application de service** dans [Installer le premier serveur de rapports en mode SharePoint](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md#bkmk_create_serrviceapplication).  
  
- Utilisez les applets de commande PowerShell SQL Server [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour créer une application de service et les bases de données du serveur de rapports. Pour plus d’informations, consultez l’exemple pour la création d’applications de service dans la rubrique [Applets de commande PowerShell pour le mode SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  

::: moniker-end
  
## <a name="database-server-version-requirements"></a>Conditions requises pour une version du serveur de bases de données

 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisé pour héberger les bases de données du serveur de rapports. L'instance [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] peut être locale ou distante. Voici les versions prises en charge du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] permettant d'héberger les bases de données du serveur de rapports :  
::: moniker range=">=sql-server-ver15"

- Azure SQL Managed Instance

- SQL Server 2019

::: moniker-end
::: moniker range=">=sql-server-2017"

- SQL Server 2017  
::: moniker-end

- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]  
  
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

Si vous créez la base de données du serveur de rapports sur un ordinateur distant, configurez la connexion de manière à employer un compte d'utilisateur de domaine ou un compte de service pouvant accéder au réseau. Si vous utilisez une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distante, réfléchissez aux informations d'identification que le serveur de rapports devra utiliser pour se connecter à l'instance. Pour plus d’informations, consultez [Configurer une connexion à la base de données du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
> [!IMPORTANT]  
> Le serveur de rapports et l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hébergeant la base de données du serveur de rapports peuvent appartenir à des domaines différents. Dans le cadre d'un déploiement Internet, il est courant d'utiliser un serveur situé derrière un pare-feu. 
>
> Si vous configurez un serveur de rapports de manière à accéder à Internet, utilisez les informations d'identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour vous connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] située derrière le pare-feu. Recourez à IPSEC pour sécuriser la connexion.  
  
## <a name="edition-requirements-for-a-database-server"></a>Conditions requises pour l’édition d’un serveur de bases de données 

 Lorsque vous créez une base de données de serveur de rapports, toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peuvent pas être utilisées pour héberger la base de données. Pour plus d’informations, consultez [Conditions requises pour l’édition du serveur de base de données du serveur de rapports](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md#edition-requirements-for-the-report-server-database) dans [Fonctionnalités Reporting Services prises en charge par les éditions de SQL Server](../reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).  

## <a name="next-steps"></a>Étapes suivantes

Apprenez-en davantage sur le [Gestionnaire de configuration du serveur de rapports](reporting-services-configuration-manager-native-mode.md).  

D’autres questions ? Posez-les sur le [forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).