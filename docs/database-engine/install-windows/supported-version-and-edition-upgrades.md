---
title: Mises à niveau de version et d’édition prises en charge (SQL Server 2016)
description: Décrit les mises à niveau de version et d’édition prises en charge pour SQL Server 2016.
ms.custom: ''
ms.date: 06/27/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server], adding to existing installations
- versions [SQL Server], upgrading
- upgrading SQL Server, upgrades supported
- cross-language support
ms.assetid: 702359c4-6ca9-42a8-860c-a95a802898a1
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 8bf008b28a130c705eabaebf986b28d7da8cd669
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172231"
---
# <a name="supported-version--edition-upgrades-sql-server-2016"></a>Mises à niveau de version et d’édition prises en charge (SQL Server 2016)

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  Vous pouvez effectuer une mise à niveau à partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]et [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Cet article répertorie les chemins de mise à niveau pris en charge à partir de ces versions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et les mises à niveau d’édition prises en charge pour [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
## <a name="pre-upgrade-checklist"></a>Liste de contrôle préalable à la mise à niveau  
  
-   Avant de mettre à niveau une édition de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] vers une autre édition, vérifiez si les fonctionnalités que vous utilisez actuellement sont prises en charge dans l'édition vers laquelle vous vous déplacez.  
  
-   Avant de mettre à niveau [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], activez l'authentification Windows pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent et vérifiez la configuration par défaut : le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être membre du groupe sysadmin [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Pour procéder à une mise à niveau vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], vous devez exécuter un système d'exploitation pris en charge. Pour plus d’informations, consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   La mise à niveau sera bloquée s'il existe un redémarrage en attente.  
  
-   La mise à niveau sera bloquée si le service Windows Installer ne s'exécute pas.  
  
## <a name="unsupported-scenarios"></a>Scénarios non pris en charge  
  
-   Les instances inter-versions de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] ne sont pas prises en charge. Les numéros de version des composants [!INCLUDE[ssDE](../../includes/ssde-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]et [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] doivent être identiques dans une instance de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] est disponible seulement pour les plateformes 64 bits. La mise à niveau interplateforme n'est pas prise en charge. Vous ne pouvez pas mettre à niveau une instance 32 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers une instance native 64 bits à l'aide du programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez toutefois sauvegarder ou détacher des bases de données d'une instance 32 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puis les restaurer ou les attacher sur une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (64 bits) si les bases de données ne sont pas publiées dans la réplication. Vous devez recréer toute connexion et tout autre objet utilisateur dans les bases de données système master, msdb et model.  
  
-   Vous ne pouvez pas ajouter de nouvelles fonctionnalités pendant la mise à niveau de votre instance existante de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Après avoir mis à niveau une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], vous pouvez ajouter des fonctionnalités via le programme d'installation de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]. Pour plus d’informations, consultez [Ajouter des fonctionnalités à une instance de SQL Server 2016 &#40;programme d’installation&#41;](./add-features-to-an-instance-of-sql-server-setup.md).  
 
-   Les clusters de basculement ne sont pas pris en charge en mode WOW.  
  
-   La mise à niveau à partir d'une version d'évaluation précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas prise en charge.

-   Lors de la mise à niveau à partir de RC1 ou de versions précédentes de SQL Server 2016 vers RC3 ou versions ultérieures, PolyBase doit être désinstallé avant la mise à niveau et réinstallé après celle-ci.
  
## <a name="upgrades-from-earlier-versions-to-sssql15-md"></a>Mises à niveau des versions antérieures vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]  
 
SQL Server 2016 prend en charge la mise à niveau à partir des versions suivantes de SQL Server :
 
- [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] SP4 ou ultérieur
- [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] SP3 ou ultérieur
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 ou ultérieur
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ou ultérieur 
 
> [!NOTE]  
> Pour mettre à niveau des bases de données sur [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , consultez [Prise en charge de 2005](#SupportFor2005).  
  
Le tableau ci-dessous répertorie les scénarios de mise à niveau pris en charge depuis des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
|Mise à niveau à partir de|Chemin d'accès de mise à niveau pris en charge|  
|------------------|----------------------------|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Small Business|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Workgroup|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] SP4 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Datacenter|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Small Business|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Workgroup|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] SP3 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1 Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Business Intelligence|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP2 Evaluation|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] version d’évaluation <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Enterprise|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Developer|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Standard|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Express |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] Business Intelligence|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/> |  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] version d’évaluation|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] version d’évaluation <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer|  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] version Release Candidate (RC)* |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] Developer |[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise | 

 \* La prise en charge Microsoft de la mise à niveau à partir de la version Release Candidate (RC) s’adresse particulièrement aux clients qui ont participé au programme TAP (Technology Adoption Program). 
   
###  <a name="sssql15-md-support-for-ssversion2005"></a><a name="SupportFor2005"></a> Prise en charge de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] par [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)]  
Cette section aborde la prise en charge de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] pour [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Dans [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], vous pouvez effectuer les opérations suivantes :  
  
-   Attacher une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] (fichiers mdf/ldf) à l'instance [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] du moteur de base de données.  
  
-   Restaurer une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] vers l'instance [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] du moteur de base de données à partir d'une sauvegarde.  
  
-   Sauvegarder un cube [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] et le restaurer sur [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
> [!NOTE]  
> Lorsqu'une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] est mise à niveau vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], le niveau de compatibilité de cette base de données passe de 90 à 100. Dans [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)], les valeurs valides pour le niveau de compatibilité de la base de données sont 100, 110, 120 et 130. [ALTER DATABASE Compatibility Level &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) explique comment la modification du niveau de compatibilité peut affecter les applications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Les scénarios non spécifiés dans la liste ci-dessus ne sont pas pris en charge, notamment :  
  
-   Installation de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] sur le même ordinateur (côte à côte).  
  
-   Utilisation d'une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] comme membre de la topologie de réplication qui implique une instance de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
-   Configuration de la mise en miroir de bases de données entre les instances [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
-   Sauvegarde du journal des transactions avec la copie des journaux de transaction entre les instances [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
-   Configuration des serveurs liés entre les instances [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] et [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
-   Gestion d'une instance de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] à partir de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Attachement d'un cube [!INCLUDE[ssASversion2005](../../includes/ssasversion2005-md.md)] dans [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Connexion à [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] depuis [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Gestion d'un service [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] à partir de [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Management Studio.  
  
-   Prise en charge des composants personnalisés Integration Services tiers de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , par exemple, pour l'exécution et la mise à niveau.  
  
## <a name="sssql15-md-edition-upgrade"></a>[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Mise à niveau d’édition  
Le tableau suivant répertorie les scénarios de mise à niveau d'édition prise en charge dans [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)].  
  
Pour obtenir des instructions détaillées sur la façon d’effectuer une mise à niveau d’édition, consultez [Mettre à niveau vers une autre édition de SQL Server 2016 &#40;programme d’installation&#41;](../../database-engine/install-windows/upgrade-to-a-different-edition-of-sql-server-setup.md).  
  
|Mise à niveau à partir de|Mise à niveau vers|  
|------------------|----------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL et licence principale)**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise |  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation Enterprise**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL ou licence principale) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> La mise à niveau depuis une version d’évaluation (une édition gratuite) vers toutes les éditions payantes est prise en charge pour les installations autonomes, mais pas pour les installations en cluster. Cette limitation ne s’applique pas aux instances autonomes installées sur un cluster de basculement Windows qui est membre d’un groupe de disponibilité.|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL ou licence principale)|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL ou licence principale) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL ou licence principale) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express*|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL ou licence principale) <br/><br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard <br/> <br/> [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Web|  
  
En outre vous pouvez également effectuer une mise à niveau d'édition entre [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL) et [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence principale) :  
  
|Mise à niveau d'édition depuis|Mise à niveau d'édition vers|  
|--------------------------|------------------------|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL)**|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence principale)|  
|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence principale)|[!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise (licence serveur+CAL)|  
  
 \* S’applique également à [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express with Tools et à [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Express with Advanced Services.  
  
 ** La modification de l’édition d’un cluster de basculement [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] est limitée à certains scénarios. Les scénarios suivants ne sont pas pris en charge pour les clusters de basculement [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] :  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Enterprise vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer, Standard ou Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Developer vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard ou Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation.  
  
-   [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Evaluation vers [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] Standard.  
  
## <a name="see-also"></a>Voir aussi  

[Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)     
[Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)     
[Mettre à niveau vers SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)    
  
