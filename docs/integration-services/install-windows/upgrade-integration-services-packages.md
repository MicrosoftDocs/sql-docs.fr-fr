---
description: Mettre à niveau des packages Integration Services
title: Mettre à niveau les packages Integration Services | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, migrating
- migrating packages [Integration Services]
ms.assetid: 68dbdf81-032c-4a73-99f6-41420e053980
author: MikeRayMSFT
ms.author: mikeray
manager: erikre
ms.openlocfilehash: 35b7d58a37e5f343380af6ef26e1f8e9bdb11102
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835858"
---
# <a name="upgrade-integration-services-packages"></a>Mettre à niveau des packages Integration Services

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Lorsque vous mettez à niveau une instance de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] vers la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vos packages [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] existants ne sont pas automatiquement mis à niveau vers le format de package utilisé par la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Vous devez choisir une méthode de mise à niveau et mettre à niveau vos packages manuellement.  
  
 Pour plus d’informations sur la mise à niveau des packages lorsque vous convertissez un projet en modèle de déploiement de projet, consultez [Déployer des projets et des packages Integration Services Server (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).
  
## <a name="selecting-an-upgrade-method"></a>Choix d'une méthode de mise à niveau  
 Vous pouvez utiliser différentes méthodes pour mettre à niveau des packages [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] . Pour certaines d'entre elles, la mise à niveau n'est que temporaire. Pour d'autres, elle est définitive. Le tableau suivant décrit chacune de ces méthodes et indique si la mise à niveau est temporaire ou définitive.  
  
> [!NOTE]  
>  Lorsque vous exécutez un package [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à l’aide de l’utilitaire **dtexec** (dtexec.exe) qui est installé avec la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la mise à niveau de package temporaire augmente la durée d’exécution. Le taux d'accroissement de temps d'exécution de package varie selon la taille du package. Pour éviter une augmentation pendant la durée d'exécution, il est recommandé d'effectuer la mise à niveau du package avant de l'exécuter.  
  
|Méthode de mise à niveau|Type de mise à niveau|  
|--------------------|---------------------|  
|Utilisez l’utilitaire **dtexec** (dtexec.exe) installé avec la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter un package [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] .<br /><br /> Pour plus d'informations, consultez [Utilitaire dtexec](../../integration-services/packages/dtexec-utility.md).|La mise à niveau de packages est temporaire.<br /><br /> Les modifications ne peuvent pas être enregistrées.|  
|Ouvrir un fichier de package [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|La mise à niveau de packages est définitive si vous enregistrez le package, sinon, elle est temporaire.|  
|Ajouter un package [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] à un projet existant dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|La mise à niveau de packages est permanente.|  
|Ouvrez un fichier de projet [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] ou ultérieur dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], puis utiliser l’Assistant Mise à niveau de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] pour mettre à niveau plusieurs packages dans le projet.<br /><br /> Pour plus d’informations, consultez [Mettre à niveau des packages Integration Services à l’aide de l’Assistant Mise à niveau de packages SSIS](../../integration-services/install-windows/upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard.md) et [Aide sur l’Assistant Mise à niveau de packages SSIS via la touche F1](../../integration-services/ssis-package-upgrade-wizard-f1-help.md).|La mise à niveau de packages est permanente.|  
|Utilisez l’utilitaire <xref:Microsoft.SqlServer.Dts.Runtime.Application.Upgrade%2A> pour mettre à niveau un ou plusieurs packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|La mise à niveau de packages est permanente.|  
  
## <a name="custom-applications-and-custom-components"></a>Applications et composants personnalisés  
 [!INCLUDE[ssISversion2005](../../includes/ssisversion2005-md.md)] ne fonctionnent pas avec la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Vous pouvez utiliser la version actuelle des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour exécuter et gérer des packages qui incluent des composants personnalisés [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)][!INCLUDE[ssIS](../../includes/ssis-md.md)] . Nous avons ajouté quatre règles de redirection de liaison aux fichiers suivants pour faciliter la redirection des assemblys du runtime de la version 10.0.0.0 ( [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]), 11.0.0.0 ( [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) ou 12.0.0.0 ( [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) vers la version 15.0.0.0 ( [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]).  
  
-   DTExec.exe.config  
  
-   dtshost.exe.config  
  
-   DTSWizard.exe.config  
  
-   DTUtil.exe.config  
  
-   DTExecUI.exe.config  
  
 Pour utiliser [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] pour concevoir des packages incluant des composants personnalisés [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], vous devez modifier le fichier devenv.exe.config situé dans *\<drive>* :\Program Files\Microsoft Visual Studio 10.0\Common7\IDE.  
  
 Pour utiliser ces packages avec les applications clientes générées avec le runtime pour [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)], incluez les règles de redirection dans la section de configuration du fichier *.exe .config de l'exécutable. Les règles redirigent les assemblys du runtime vers la version 15.0.0.0 ([!INCLUDE[ssSQL19](../../includes/sssql19-md.md)]). Pour plus d’informations sur la redirection des versions d’assemblys, consultez [Élément \<assemblyBinding> pour \<runtime>](/dotnet/framework/configure-apps/file-schema/runtime/assemblybinding-element-for-runtime).  
  
### <a name="locating-the-assemblies"></a>Recherche d'assemblys  
 Dans [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)], les assemblys [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ont été mis à niveau vers le .NET 4.0. Il existe un Global Assembly Cache distinct pour .NET 4, situé dans *\<drive>* :\Windows\Microsoft.NET\assembly. Vous trouverez tous les assemblys [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sous ce chemin d'accès, en général dans le dossier GAC_MSIL.  
  
 Comme dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les principaux fichiers .dll d’extensibilité [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se trouvent également dans *\<drive>* :\Program files\Microsoft SQL Server\130\SDK\Assemblies.  
  
## <a name="understanding-sql-server-package-upgrade-results"></a>Présentation des résultats de mise à niveau de packages SQL Server  
 Au cours de la mise à niveau de packages, la plupart des composants et fonctionnalités des packages [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] sont convertis de façon transparente en leurs équivalents dans la version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il existe toutefois certains composants et fonctionnalités pour lesquels aucune mise à niveau ne sera effectuée ou pour lesquels vous devez connaître les résultats. Le tableau suivant identifie ces composants et fonctionnalités.  
  
> [!NOTE]  
>  Pour identifier les packages concernés par les points répertoriés dans le tableau, exécutez le Conseiller de mise à niveau.  
  
|Composant ou fonctionnalité|Résultats de la mise à niveau|  
|--------------------------|---------------------|  
|Chaînes de connexion|Pour les packages [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]ou [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , les noms de certains fournisseurs ont changé et requièrent des valeurs différentes dans les chaînes de connexion. Pour mettre à jour les chaînes de connexion, utilisez l'une des procédures suivantes :<br /><br /> Utilisez l'Assistant Mise à niveau de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] pour mettre à niveau le package et sélectionnez l'option **Mettre à jour les chaînes de connexion pour l'utilisation des nouveaux noms de fournisseurs** .<br /><br /> Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], sur la page Général de la boîte de dialogue Options, sélectionnez l'option **Mettre à jour les chaînes de connexion pour l'utilisation des nouveaux noms de fournisseurs** . Pour plus d’informations sur cette option, consultez la page Général.<br /><br /> Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package et modifiez manuellement le texte de la propriété ConnectionString.<br /><br /> Remarque : vous ne pouvez pas appliquer les procédures ci-dessus pour mettre à jour une chaîne de connexion lorsque celle-ci est stockée dans un fichier de configuration ou dans un fichier de source de données, ou lorsqu’une expression définit la propriété **ConnectionString** . Pour mettre à jour la chaîne de connexion dans ces cas-là, vous devez mettre à jour le fichier ou l'expression manuellement.<br /><br /> Pour plus d’informations sur les sources de données disponibles, consultez [Sources de données](../../integration-services/connection-manager/data-sources.md).|  
  
### <a name="scripts-that-depend-on-adodbdll"></a>Scripts qui dépendent d'ADODB.dll  
 Les scripts de la tâche de script et du composant Script qui référencent explicitement ADODB.dll risquent de ne pas pouvoir être mis à niveau ou de ne pas pouvoir s'exécuter sur les ordinateurs qui ne disposent pas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . Pour permettre la mise à niveau des scripts de la tâche de script et du composant Script, il est recommandé de supprimer la dépendance sur ADODB.dll.  Ado.Net est l'alternative recommandée pour le code managé, à l'instar des scripts VB et C#.  
  
