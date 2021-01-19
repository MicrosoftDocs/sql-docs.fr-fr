---
title: Mettre à niveau Master Data Services | Microsoft Docs
description: Découvrez les quatre scénarios de mise à niveau de Microsoft SQL Server Master Data Services. Apprenez-en plus sur les emplacements de fichiers et la résolution des problèmes liés aux mises à niveau.
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 9c3543f3-3eb9-455d-a9bf-f17e9506ad21
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: f5040c96272eb3345b02077faf1302c9ac88a357
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172251"
---
# <a name="upgrade-master-data-services"></a>Mettre à niveau Master Data Services

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Voici les différents scénarios de mise à niveau de Microsoft [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] Master Data Services.  
  
-   [Mise à niveau sans mise à niveau du moteur de base de données](../../database-engine/install-windows/upgrade-master-data-services.md#noengine)  
  
-   [Mise à niveau avec mise à niveau du moteur de base de données](../../database-engine/install-windows/upgrade-master-data-services.md#engine)  
  
-   [Effectuer une mise à niveau dans un scénario basé sur deux ordinateurs](../../database-engine/install-windows/upgrade-master-data-services.md#twocomputer)  
  
-   [Effectuer une mise à niveau par restauration d'une base de données à partir d'une sauvegarde](../../database-engine/install-windows/upgrade-master-data-services.md#restore)  
  
> [!IMPORTANT]  
> -   La mise à niveau de la version [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] CTP1 vers la version CTP2 n'est pas prise en charge.  
> -   Enregistrez votre base de données avant d'effectuer toute mise à niveau.  
> -   La mise à niveau recrée les procédures stockées et met à niveau les tables utilisées par [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]. Les personnalisations appliquées à l'un ou l'autre de ces composants peuvent être perdues.  
> -   Les packages de déploiement de modèle peuvent être utilisés uniquement dans l'édition de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisée pour les créer. Vous ne pouvez pas déployer des packages de déploiement de modèle créés dans [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] sur [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)].  
> -   Après avoir effectué la mise à niveau de Data Quality Services et Master Data Services vers [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], toutes les versions antérieures du complément Master Data Services pour Excel cesseront de fonctionner. Vous pouvez télécharger le complément Master Data Services pour Excel [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] depuis [Complément Master Data Services pour Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md) .  
  
##  <a name="file-location"></a><a name="fileLocation"></a> Emplacement du fichier  
  
-   Dans [!INCLUDE[ss2017](../../includes/sssqlv14-md.md)], les fichiers sont installés par défaut à l’emplacement suivant : *lecteur* :\Program Files\Microsoft SQL Server\140\Master Data Services.  

-   Dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], les fichiers sont installés par défaut à l’emplacement suivant : *lecteur* :\Program Files\Microsoft SQL Server\130\Master Data Services.  
  
-   Dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], par défaut, les fichiers sont installés à l’emplacement suivant : *lecteur*:\Program Files\Microsoft SQL Server\120\Master Data Services.  
  
-   Dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], par défaut, les fichiers sont installés à l’emplacement suivant : *lecteur*:\Program Files\Microsoft SQL Server\110\Master Data Services.  
  
-   Dans [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], par défaut, les fichiers sont installés à l’emplacement suivant : *lecteur*:\Program Files\Microsoft SQL Server\Master Data Services.  
  
##  <a name="upgrade-without-database-engine-upgrade"></a><a name="noengine"></a> Mise à niveau sans mise à niveau du moteur de base de données  
 Dans ce scénario, vous continuez à utiliser [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] pour héberger votre base de données MDS. Cependant, vous devez mettre à niveau le schéma de la base de données MDS, puis créer une application web de la version actuelle de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] pour accéder à la base de données MDS. Après la mise à niveau, la base de données MDS n’est plus accessible par l’application web antérieure.  
  
 Vous pouvez installer la version actuelle de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] et une version antérieure de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] sur le même ordinateur. Les fichiers sont installés à différents emplacements, comme indiqué dans [Emplacement du fichier](#fileLocation).  
  
 **Pour effectuer une mise à niveau sans mise à niveau du moteur de base de données**  
  
1.  Installez [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] et toutes les autres fonctionnalités que vous souhaitez.  
  
    1.  Ouvrez l'Assistant Installation de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)].  
  
    2.  Dans le volet gauche, cliquez sur **Installation**.  
  
    3.  Dans le volet droit, cliquez sur **Nouvelle installation autonome SQL Server ou ajout de fonctionnalités à une installation existante**.  
  
    4.  Dans la page **Sélection des fonctionnalités** , sélectionnez **[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]** , puis toutes les autres fonctionnalités à installer.  
  
    5.  Terminez l'Assistant.  
  
2.  Mettez à niveau le schéma de la base de données MDS.  
  
    1.  Ouvrez la version actuelle de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
        > [!IMPORTANT]  
        >  Pour mettre à niveau le schéma de la base de données MDS, vous devez ouvrir une session avec le compte Administrateur spécifié lors de la création de la base de données MDS. Dans la base de données MDS, dans mdm.tblUser, cet utilisateur à la valeur d' **ID** **1**.  
  
    2.  Dans le volet gauche, cliquez sur **Configuration de la base de données**.  
  
    3.  Dans le volet droit, cliquez sur **Sélectionner une base de données** et spécifiez les informations de votre instance de base de données [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)].  
  
    4.  Cliquez sur **Mettre à niveau la base de données** pour démarrer l' **Assistant Mise à niveau de base de données**. Pour plus d’informations, consultez [Assistant Mise à niveau de base de données &#40;Gestionnaire de configuration Master Data Services&#41;](../../master-data-services/upgrade-database-wizard-master-data-services-configuration-manager.md).  
  
3.  Créez une application web.  
  
    1.  Ouvrez la version actuelle de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssMDScfgmgr](../../includes/ssmdscfgmgr-md.md)].  
  
    2.  Dans le volet gauche, cliquez sur **Configuration Web**.  
  
    3.  Dans le volet droit, dans la liste **Site Web** , sélectionnez l'une des options suivantes :  
  
        -   **Site Web par défaut**, puis cliquez sur **Créer une application**.  
  
        -   **Créer un nouveau site**. Lorsque vous créez un site web, une application web est automatiquement créée.  
  
        > [!IMPORTANT]  
        >  Votre application web MDS existante issue d’une version précédente de SQL Server ([!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]) est disponible à la sélection dans la version [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] du Gestionnaire de configuration Master Data Services. Vous ne devez pas sélectionner l'application Web existante, mais plutôt créer une application Web [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] pour MDS. Sinon, vous allez recevoir une erreur lorsque vous tenterez d'associer l'application Web à la base de données MDS mise à niveau, indiquant que la page demandée n'est pas accessible, car les données de configuration de la page sont erronées.  
        >   
        >  Si vous voulez utiliser le même nom (alias) pour l’application web MDS que le nom de votre application web existante ([!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]), vous devez d’abord supprimer l’application web et le pool d’applications associé d’IIS, puis créer une application web avec le même nom en utilisant la version [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] du Gestionnaire de configuration Master Data Services. Pour plus d’informations sur la suppression d’une application web et de pools d’applications d’IIS, consultez [Supprimer une application (IIS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771205(v=ws.10)) et [Supprimer un pool d’applications (IIS)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772406(v=ws.10)).  
  
4.  Associez la nouvelle application Web à la base de données MDS mise à niveau.  
  
    1.  Sous la section **Associer l'application à une base de données** , cliquez sur **Sélectionner**.  
  
    2.  Sélectionnez la base de données MDS.  
  
    3.  Cliquez sur **Appliquer**.  
  
##  <a name="upgrade-with-database-engine-upgrade"></a><a name="engine"></a> Mise à niveau avec mise à niveau du moteur de base de données  
 Dans ce scénario, vous mettez à niveau le moteur de base de données et l’application [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] provenant d’une version antérieure de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ou [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)].  
  
 **Pour effectuer la mise à niveau avec mise à niveau du moteur de base de données**  
  
1.  **Pour [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] uniquement** : ouvrez **Panneau de configuration** > **Programmes et fonctionnalités** et désinstallez Microsoft [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)][!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)].  
  
2.  Mettez à niveau le moteur de base de données vers [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ou [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)]. Pour plus d’informations, consultez [Choose a Database Engine Upgrade Method](../../database-engine/install-windows/choose-a-database-engine-upgrade-method.md).  
  
3.  Effectuez toutes les étapes présentées dans [Mise à niveau sans mise à niveau du moteur de base de données](#noengine) .  
  
##  <a name="upgrade-in-two-computer-scenario"></a><a name="twocomputer"></a> Effectuer une mise à niveau dans un scénario basé sur deux ordinateurs  
 Dans ce scénario, vous mettez à niveau un système où SQL Server est installé sur deux ordinateurs : un avec [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ou [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)], et l’autre avec une version antérieure de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)].  
  
 Si une version antérieure de [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] est installée, vous continuez à utiliser la version antérieure pour héberger votre base de données MDS sur un ordinateur. Cependant, vous devez mettre à niveau le schéma de la base de données MDS, puis utiliser l’application web [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ou [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)] respectivement, pour accéder à la base de données MDS. La base de données MDS n’est plus accessible par l’application web de la version antérieure.  
  
 **Pour effectuer une mise à niveau dans un scénario basé sur deux ordinateurs**  
  
-   Effectuez toutes les étapes présentées dans [Mise à niveau sans mise à niveau du moteur de base de données](#noengine).  
  
##  <a name="upgrade-with-restoring-a-database-from-backup"></a><a name="restore"></a> Effectuer une mise à niveau par restauration d'une base de données à partir d'une sauvegarde  
 Dans ce scénario, [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ou [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)] est installé avec une version antérieure sur le même ordinateur ou sur deux ordinateurs différents. Une base de données a été sauvegardée sur une version antérieure à la version [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ou [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)] avant la mise à niveau, et la base de données doit être restaurée.  
  
 **Pour effectuer une mise à niveau par restauration d’une base de données à partir d’une sauvegarde**  
  
1.  Installez [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] et toutes les autres fonctionnalités que vous souhaitez.  
  
    1.  Ouvrez l'Assistant Installation de [!INCLUDE[sssnoversion](../../includes/ssnoversion-md.md)].  
  
    2.  Dans le volet gauche, cliquez sur **Installation**.  
  
    3.  Dans le volet droit, cliquez sur **Nouvelle installation autonome SQL Server ou ajout de fonctionnalités à une installation existante**.  
  
    4.  Dans la page **Sélection des fonctionnalités** , sélectionnez **[!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)]** , puis toutes les autres fonctionnalités à installer.  
  
    5.  Terminez l'Assistant.  
  
2.  Restaurez la base de données qui a été sauvegardée.  
  
3.  Procédez à la mise à niveau du schéma de base de données MDS, créez une application Web et associez la nouvelle application Web à la base de données MDS mise à niveau. Pour obtenir des instructions, consultez les étapes 2 à 4 dans [Mise à niveau sans mise à niveau du moteur de base de données](#noengine)  
  
## <a name="troubleshooting"></a>Dépannage  
 **Problème :** quand vous ouvrez l’application web [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], un message d’erreur s’affiche, indiquant que la version du client n’est pas compatible avec la version de la base de données.  
  
 **Solution :** ce problème se produit quand une application web Master Data Manager [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] tente d’accéder à une base de données qui a été mise à niveau vers [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)] Master Data Services. Vous devez utiliser une application web [!INCLUDE[ssSQL16](../../includes/sssqlv14-md.md)] à la place.  
  
 Ce problème peut également se produire si vous n'avez pas arrêté puis redémarré le **pool d'applications MDS** dans IIS lors de la mise à niveau du schéma de la base de données MDS. Redémarrez le **pool d'applications MDS** pour corriger le problème.  
  
## <a name="see-also"></a>Voir aussi  
 [Installer Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)  
  
