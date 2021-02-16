---
description: Mettre à niveau Integration Services
title: Mettre à niveau Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, upgrading
- SSIS, upgrading
- SQL Server Integration Services, upgrading
- upgrading Integration Services
ms.assetid: 04f9863c-ba0b-47c5-af91-f2d41b078a23
author: MikeRayMSFT
ms.author: mikeray
manager: erikre
ms.openlocfilehash: de42bca3c6cbcc2ec459bce77ba31e76b572715b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352055"
---
# <a name="upgrade-integration-services"></a>Mettre à niveau Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Si [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] (ou une version ultérieure) est installé sur votre ordinateur, vous pouvez effectuer une mise à niveau vers [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)].  
  
 Lorsque vous mettez à niveau vers [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] sur un ordinateur sur lequel une de ces versions antérieures d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est installée, [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] est installé côte à côte avec une version antérieure.  
  
 Avec cette installation côte à côte, plusieurs versions de l'utilitaire dtexec sont installés. Pour vérifier que vous exécutez la version appropriée de l’utilitaire, à l’invite de commandes, exécutez l’utilitaire en entrant le chemin complet (\<drive>:\Program Files\Microsoft SQL Server\\<version\>\DTS\Binn). Pour plus d'informations dtexec, consultez [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
> [!NOTE]  
>  Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par défaut, lorsque vous installiez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tous les utilisateurs du groupe Utilisateurs avaient accès au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Lorsque vous installez [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] et après l’installation, les utilisateurs n’ont pas accès au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Ce service est sécurisé par défaut. Une fois [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installé, l'administrateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit exécuter l'outil de configuration DCOM (Dcomcnfg.exe) pour accorder à des utilisateurs spécifiques l'accès au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez [Service Integration Services &#40;Service SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).  
  
## <a name="before-upgrading-integration-services"></a>Avant de procéder à la mise à niveau d'Integration Services  
 Nous vous avons recommandé d'exécuter le Conseiller de mise à niveau avant de procéder à la mise à niveau vers [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]. Le Conseiller de mise à niveau signale des problèmes que vous pouvez rencontrer si vous effectuez une migration de packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] existants vers le nouveau format de package que [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] utilise.  
  
> [!NOTE]
>  La prise en charge de la migration ou de l’exécution de packages DTS (Data Transformation Services) a été suspendue dans SQL Server 2012. Les fonctionnalités DTS suivantes ne sont plus disponibles.  
> 
>  -   Runtime DTS ;  
> -   DTS API ;  
> -   Assistant Migration de package pour la migration de packages DTS vers la prochaine version de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  
> -   Prise en charge de la maintenance des packages DTS dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
> -   Tâche d'exécution de package DTS 2000 ;  
> -   analyse du Conseiller de mise à niveau des packages DTS.  
> 
>  Pour plus d’informations sur les autres fonctionnalités abandonnées, consultez [Fonctionnalités Integration Services abandonnées dans SQL Server 2016](/previous-versions/sql/sql-server-2016/bb500429(v=sql.130)).  
  
## <a name="upgrading-integration-services"></a>mise à niveau d'Integration Services  
 Vous pouvez effectuer la mise à niveau au moyen de l'une des méthodes suivantes :  
  
-   Exécutez le programme d’installation de [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] et sélectionnez l’option **Mise à niveau de SQL Server 2008, SQL Server 2008 R2, [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
-   Exécutez le fichier **setup.exe** à l’invite de commandes et spécifiez l’option **/ACTION=upgrade** . Pour plus d’informations, consultez la section, « Scripts d’installation pour [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]», dans [Installer SQL Server 2016 à partir de l’invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
 Vous ne pouvez pas utiliser la mise à niveau pour exécuter les actions suivantes :  
  
-   Reconfigurer une installation existante de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   Passer d'une version 32 bits à une version 64 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d'une version 64 bits à une version 32 bits.  
  
-   Passer d'une version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à une autre version localisée.  
  
 Lors d’une mise à niveau, vous pouvez mettre à niveau à la fois [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et [!INCLUDE[ssDE](../../includes/ssde-md.md)], mais vous pouvez également choisir de mettre à niveau uniquement [!INCLUDE[ssDE](../../includes/ssde-md.md)]ou uniquement [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Si vous mettez à niveau uniquement [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] (ou une version ultérieure) reste fonctionnel, mais sans les fonctionnalités de [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)]. Si vous mettez uniquement à niveau [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] sera totalement fonctionnel, mais uniquement en mesure de stocker des packages dans le système de fichiers, à moins qu'une instance du [!INCLUDE[ssDECurrent](../../includes/ssdecurrent-md.md)] ne soit disponible sur un autre ordinateur.  
  
## <a name="upgrading-both-integration-services-and-the-database-engine-to-sssql19"></a>Mise à niveau d'Integration Services et du moteur de base de données vers [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]  
 Cette section décrit les conséquences liées à l'exécution d'une mise à niveau qui obéit aux critères suivants :  
  
-   Mise à niveau de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et d’une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] vers [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)].  
  
-   Installation de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et de l’instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur le même ordinateur.  
  
### <a name="what-the-upgrade-process-does"></a>Ce que fait le processus de mise à niveau  
 Le processus de mise à niveau effectue les tâches suivantes :  
  
-   Installe les fichiers [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , ainsi que le service et les outils ([!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]). Lorsque plusieurs instances de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] sont installées sur le même ordinateur, la première mise à niveau d’une instance quelconque vers [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]active l’installation des fichiers, services et outils [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] .  
  
-   Met à niveau l’instance de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssDE](../../includes/ssde-md.md)] vers la version [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] .  
  
-   Déplace les données des tables système [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] ou ultérieures vers les tables système [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , comme suit :  
  
    -   Déplace les packages sans modification de la table système msdb.dbo.sysdtspackages90 vers la table système msdb.dbo.sysssispackages.  
  
        > [!NOTE]  
        >  Bien que les données soient déplacées vers une autre table système, la mise à niveau n'effectue pas la migration des packages vers le nouveau format.  
  
    -   Déplace les métadonnées de dossier de la table système msdb.sysdtsfolders90 vers la table système msdb.sysssisfolders.  
  
    -   Déplace les données du journal de la table système msdb.sysdtslog90 vers la table système msdb.sysssislog.  
  
-   Supprime les tables système msdb.sysdts*90 et les procédures stockées utilisées pour y accéder après avoir déplacé les données vers les nouvelles tables msdb.sysssis\* . Toutefois, la mise à niveau remplace la table sysdtslog90 par une vue qui est également nommée sysdtslog90. Cette nouvelle vue sysdtslog90 présente la nouvelle table système msdb.sysssislog. Cela garantit que les rapports basés sur la table du journal continuent à s'exécuter sans interruption.  
  
-   Pour contrôler l'accès aux packages, crée trois nouveaux rôles de base de données fixes : db_ssisadmin, db_ssisltduser et db_ssisoperator. Les rôles [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de db_dtsadmin, db_dtsltduser et db_dtsoperator ne sont pas supprimés, mais deviennent membres des nouveaux rôles correspondants.  
  
-   Si le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] (autrement dit, l’emplacement du système de fichiers géré par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ) constitue l’emplacement par défaut sous **\SQL Server\90**, **\SQL Server\100**, **\SQL Server\110** ou **\SQL Server\120** , ces packages sont déplacés vers le nouvel emplacement par défaut sous **\SQL Server\130**.  
  
-   Met à jour le fichier de configuration du service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de sorte qu’il pointe vers l’instance mise à niveau de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
### <a name="what-the-upgrade-process-does-not-do"></a>Ce que ne fait pas le processus de mise à niveau  
 Le processus de mise à niveau n'effectue pas les tâches suivantes :  
  
-   **Ne** supprime pas le service [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] ou version ultérieure.  
  
-   N'effectue pas la migration des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] existants vers le nouveau format utilisé par [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] pour les packages. Pour plus d’informations sur la migration de packages, consultez [Mettre à niveau des packages Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
-   Ne déplace pas de packages à partir d'emplacements du système de fichiers, autre que l'emplacement par défaut, qui ont été ajoutés au fichier de configuration du service. Si vous avez déjà modifié le fichier de configuration du service de manière à ajouter des dossiers de système de fichiers supplémentaires, les packages stockés dans ces dossiers ne seront pas déplacés à un nouvel endroit.  
  
-   Dans les étapes du travail de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui appellent directement l’utilitaire **dtexec** (dtexec.exe), ne met pas à jour le chemin du système de fichiers pour l’utilitaire **dtexec** . Vous devez modifier manuellement ces étapes du travail pour mettre à jour le chemin d’accès au système de fichiers en vue de spécifier l’emplacement [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] pour l’utilitaire **dtexec** .  
  
### <a name="what-you-can-do-after-upgrading"></a>Ce que vous pouvez faire après la mise à niveau  
 Une fois la mise à niveau terminée, vous pouvez accomplir les tâches suivantes :  
  
-   Exécuter les travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécutent les packages.  
  
-   Utiliser [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] pour gérer les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stockés dans une instance de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Vous devez modifier le fichier de configuration du service pour ajouter l’instance de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à la liste des emplacements gérés par le service.  
  
    > [!NOTE]  
    >  Les premières versions de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ne peuvent pas se connecter au service [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] .  
  
-   Identifier la version des packages dans la table système msdb.dbo.sysssispackages en vérifiant la valeur de la colonne packageformat. La table possède une colonne packageformat qui identifie la version de chaque package. La valeur 3 indique un package [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] . Tant que vous n'effectuez pas la migration des packages vers le nouveau format, la valeur de la colonne packageformat ne change pas.  
  
-   Vous ne pouvez pas utiliser les outils [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] pour concevoir, exécuter ou gérer des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Les outils [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] incluent les versions correspondantes de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], de l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de l’utilitaire d’exécution de package (dtexecui.exe). La mise à niveau ne supprime pas les outils [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Toutefois, vous ne serez pas en mesure de faire appel à ces outils pour continuer à utiliser les packages [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] ou version ultérieure sur un serveur qui a été mis à niveau.  
  
-   Par défaut, dans le cadre d'une installation mise à niveau, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est configuré de façon à journaliser les événements en rapport avec l'exécution de packages dans le journal d'événements de l'application. Ce paramètre peut générer trop d'entrées de journal d'événements lorsque vous utilisez la fonctionnalité collecteur de données de [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]. Les événements enregistrés incluent l'ID d'événement 12288, « le Package a démarré », et l'ID d'événement 12289, « le Package a fini avec succès ». Pour arrêter l'enregistrement de ces deux événements dans le journal d'événements de l'application, ouvrez le Registre pour y apporter des modifications. Dans le Registre, recherchez le nœud HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS et modifiez la valeur DWORD du paramètre LogPackageExecutionToEventLog en remplaçant 1 par 0.  
  
## <a name="upgrading-only-the-database-engine-to-sssql19"></a>Mise à niveau du seul moteur de base de données vers [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]  
 Cette section décrit les conséquences liées à l'exécution d'une mise à niveau qui obéit aux critères suivants :  
  
-   Mise à niveau d’une instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)]uniquement. Autrement dit, l’instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] est maintenant une instance de [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)], mais l’instance de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et les outils clients proviennent de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
-   L’instance de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se trouve sur un ordinateur, tandis que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et les outils clients se trouvent sur un autre ordinateur.  
  
### <a name="what-you-can-do-after-upgrading"></a>Ce que vous pouvez faire après la mise à niveau  
 Les tables système qui stockent les packages dans l’instance mise à niveau de [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne sont pas les mêmes que celles utilisées dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]. Par conséquent, les versions [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ne peuvent pas découvrir les packages dans les tables système de l’instance mise à niveau de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Comme ces packages ne peuvent pas être découverts, l'utilisation que vous pouvez en faire est limitée :  
  
-   Vous ne pouvez pas utiliser les outils [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]sur d’autres ordinateurs pour charger ou gérer des packages à partir de l’instance mise à niveau de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
    > [!NOTE]  
    >  Bien que les packages de l’instance mise à niveau de [!INCLUDE[ssDE](../../includes/ssde-md.md)] n’aient pas encore été migrés vers le nouveau format de package, ils ne sont pas encore détectables par les outils [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . Par conséquent, les packages ne peuvent pas être utilisés par les outils [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] .  
  
-   Vous ne pouvez pas utiliser [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] sur d’autres ordinateurs pour exécuter les packages stockés dans msdb sur l’instance mise à niveau du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Vous ne pouvez pas utiliser les travaux de l’Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur des ordinateurs [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] pour exécuter les packages [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] stockés dans l’instance mise à niveau de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="external-resources"></a>Ressources externes  
 Entrée de blog [Faire fonctionner vos extensions et applications SSIS personnalisées existantes à Denali](https://techcommunity.microsoft.com/t5/sql-server-integration-services/making-your-existing-custom-ssis-extensions-and-applications/ba-p/387951), sur blogs.msdn.com.  
  
