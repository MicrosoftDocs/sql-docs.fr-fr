---
title: Concevoir des rapports à l’aide du Concepteur de rapports | Microsoft Docs
description: Découvrez comment utiliser le Concepteur de rapports dans SQL Server Reporting Services pour créer des rapports paginés complets et des solutions de création de rapports.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- Report Designer [Reporting Services], report creation
ms.assetid: 3a26dccc-6ad6-48f5-a882-f96c6c0dd405
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 500ecc4e4b0dae80faa06f8d152b99423bb45251
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597173"
---
# <a name="design-reporting-services-paginated-reports-with-report-designer-ssrs"></a>Concevoir des rapports paginés Reporting Services à l’aide du Concepteur de rapports (SSRS)

Utilisez le Concepteur de rapports pour créer des rapports et des solutions de création de rapports complets et paginés de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Le Concepteur de rapports fournit une interface graphique dans laquelle vous pouvez définir les sources de données, les datasets et les requêtes, les positions de mise en page des rapports pour les régions de données et les champs, ainsi que des fonctionnalités interactives telles que les paramètres et les jeux de rapports qui fonctionnent ensemble.  

Le Concepteur de rapports est une fonctionnalité de  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], un environnement Microsoft Visual Studio pour la création de solutions décisionnelles. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] n’est pas fourni avec SQL Server. Téléchargez [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md). 
  
## <a name="benefits-of-report-projects"></a>Avantages des projets de rapports  
Les projets de rapports fonctionnent comme des conteneurs de définitions de rapports et de ressources. Utilisez les projets pour :  
  
-   organiser les rapports et les éléments associés dans un conteneur,  
  
-   tester localement des solutions de rapport qui incluent des rapports et des éléments liés,  
  
-   déployer des éléments associés ensemble, utiliser les propriétés du projet et la gestion de la configuration pour déployer dans plusieurs environnements,  
  
-   conserver un ensemble de copies principales pour les rapports et les éléments connexes. Après le déploiement, les rapports publiés peuvent être accidentellement modifiés.  
  
 Utilisez les informations de cette rubrique pour concevoir des rapports paginés et des éléments associés pour un projet de rapport dans une solution de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] . Pour plus d’informations sur les solutions et les projets multiples dans les outils de données SQL Server, consultez [Reporting Services dans les outils de données SQL Server](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).  

  
##  <a name="shared-data-sources"></a><a name="bkmk_SharedDataSources"></a> Sources de données partagées  
 Utilisez [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] pour définir et déployer des sources de données partagées pour une solution de création de rapports. Les sources de données partagées peuvent être déployées indépendamment des autres éléments dans un projet à l'aide de les propriétés de **OverwriteDataSources** et de **TargetDataSourceFolder** . Pour plus d’informations, consultez [Définir des propriétés de déploiement &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
 Dans le Concepteur de rapports, vous travaillez dans le volet des données de rapport et dans l'explorateur de solutions pour définir les sources de données utilisées dans un rapport. Pour plus d'informations, consultez [Report Data Pane](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_ReportDataPane). Vous ne pouvez pas utiliser [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] pour ouvrir les sources de données publiées sur un serveur de rapports ou un site SharePoint, mais non inclus dans la solution de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] . Pour cette fonctionnalité, utilisez [Environnement de création du Générateur de rapports &#40;SSRS&#41;](../../reporting-services/tools/report-builder-authoring-environment-ssrs.md).  
  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] est un outil client. Vous pouvez tester votre solution de création de rapports localement sur votre ordinateur, la déployer dans un environnement de test pour tester la solution de serveur, puis la déployer dans un environnement de production. Après le déploiement, vérifiez que les extensions de traitement des sources de données et les informations d'identification de la source de données sont configurées pour l'environnement de serveur de rapports. Vous pouvez utiliser le gestionnaire de configuration pour gérer les propriétés de différents déploiements. Pour plus d’informations, consultez [Reporting Services dans les outils de données SQL Server &#40;SSDT&#41;](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).  
  
 Pour plus d’informations, consultez [Créer des chaînes de connexion de données - Générateur de rapports et SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
   
##  <a name="shared-datasets"></a><a name="bkmk_SharedDatasets"></a> Datasets partagés  
 Utilisez [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] pour définir et déployer des datasets partagées pour une solution de création de rapports. Les datasets partagés peuvent être déployés indépendamment des autres éléments dans un projet à l'aide de les propriétés de **OverwriteDatasets** et de **TargetDatasetFolder** . Pour plus d’informations, consultez [Définir des propriétés de déploiement &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
 Dans le Concepteur de rapports, vous travaillez dans le volet des données de rapport et dans l'explorateur de solutions pour définir les datasets partagés utilisés dans un rapport. Pour plus d'informations, consultez [Report Data Pane](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_ReportDataPane). Vous ne pouvez pas utiliser [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] pour ouvrir des datasets publiés directement à partir d'un serveur de rapports ou d'un site SharePoint. Pour cette fonctionnalité, utilisez [Environnement de création du Générateur de rapports &#40;SSRS&#41;](../../reporting-services/tools/report-builder-authoring-environment-ssrs.md) en mode dataset partagé.  
  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] est un outil client. Vous pouvez utiliser les concepteurs de requêtes pour créer et tester vos résultats de requête localement dans l'aperçu. Après le déploiement, vous pouvez gérer les datasets partagés indépendamment des sources de données partagées et des rapports dont ils dépendent. Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md), [Outils de conception de requête &#40;SSRS&#41;](../../reporting-services/report-data/query-design-tools-ssrs.md) et [Gérer des datasets partagés](../../reporting-services/report-data/manage-shared-datasets.md).  
  
##  <a name="paginated-reports"></a><a name="bkmk_Reports"></a> Rapports paginés  
Les rapports paginés sont des fichiers qui sont stockés dans un projet de rapport. Les rapports peuvent être utilisés comme rapports autonomes, sous-rapports ou cibles pour les actions d'extraction des rapports principaux. Les rapports peuvent être déployés indépendamment des autres éléments dans un projet à l'aide de **TargetReportFolder** et d'autres propriétés. Pour plus d’informations, consultez [Définir des propriétés de déploiement &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
> [!NOTE]  
>  Si vous publiez sur un serveur de rapports en mode SharePoint, certaines fonctionnalités de solution de rapport ne peuvent pas être testées dans le projet du Concepteur de rapports. Les références aux rapports, aux sous-rapports et aux rapports d'extraction doivent utiliser des URL complètes qui peuvent être testées uniquement lorsque vous déployez le projet de rapport. Pour plus d’informations, consultez [Exemples d’URL pour les éléments de rapport publiés sur un serveur de rapports en mode SharePoint &#40;SSRS&#41;](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md).  
  
 Vous pouvez ajouter des rapports à un projet comme suit :  
  
-   **Ajoutez un nouveau projet de rapport.** Par défaut, un rapport vide s'ouvre dans le Concepteur de rapports. Pour plus d’informations, consultez [Ajouter un nouveau rapport ou un rapport existant à un projet de rapport &#40;SSRS&#41;](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md).  
  
-   **Ajoutez un nouveau projet d’Assistant de rapport.** Vous créez un rapport pas-à-pas et êtes guidé. L'Assistant Rapport résume la définition des données et la conception des rapports à une série d'étapes aboutissant à un rapport parachevé. Vous pouvez ajouter des styles pour personnaliser l'assistant pour votre propre organisation. Pour plus d’informations, consultez [Ajouter un nouveau rapport ou un rapport existant à un projet de rapport &#40;SSRS&#41;](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md).  
  
-   **Ajoutez un nouvel élément de type Rapport.** Un rapport vide s'ouvre dans le Concepteur de rapports.  
  
-   **Ajoutez un élément existant.** Une définition de rapport existante (.rdl) s'ouvre dans le Concepteur de rapports. Ouvrir un rapport ou un projet d'une version antérieure de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peut automatiquement mettre à niveau le projet vers la version actuelle et le rapport vers le schéma actuel. Pour plus d'informations, consultez [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md).  
  
-   **Importez un rapport [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access.** Importez tous les rapports à partir d’une base de données Access (.mdb, .accdb) ou du fichier de projet (.adp). Le Concepteur de rapports convertit chaque rapport dans un fichier de base de données ou de projet au format RDL, puis l'enregistre dans le projet de rapport. Les fonctionnalités d'un rapport Access ne sont pas toutes transférables dans un fichier de définition de rapport (.rdl). Pour plus d’informations, consultez [Importer des rapports à partir de Microsoft Access &#40;Reporting Services&#41;](../reports/reporting-services-reports-ssrs.md) et [Fonctionnalités des états Access prises en charge &#40;SSRS&#41;](../reports/reporting-services-reports-ssrs.md).  
  
    > [!NOTE]  
    >  Microsoft Access 2002 (ou version ultérieure) doit être installé sur le même ordinateur que le Concepteur de rapports pour pouvoir utiliser la fonctionnalité d'importation. La source de données des états Access doit être disponible lorsque les états sont importés.  
  
-   **Travaillez directement dans RDL.** Quand vous créez un rapport dans le Concepteur de rapports, il est enregistré au format XML en tant que fichier RDL (Report Definition Language). Vous pouvez modifier ce fichier dans le Concepteur de rapports, un éditeur de texte ou tout outil dans lequel il est possible de modifier du code XML.  
  
     Lorsque vous modifiez la source de définition de rapport dans le Concepteur de rapports, vous travaillez dans le schéma RDL actuel pour la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de laquelle vous avez installé les outils de développement. Lorsque vous générez un projet, la version du schéma peut changer selon les propriétés de votre déploiement. Pour plus d’informations, consultez [Déploiement et prise en charge des versions dans les outils de données SQL Server &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
     Modifier directement le langage RDL peut entraîner des problèmes d'exécution du rapport ou des problèmes de publication sur le serveur de rapports. Comme pour tout fichier XML, assurez-vous que les caractères spécifiques XML utilisés dans les éléments sont correctement encodés. Lorsque vous publiez un rapport, le serveur de rapports utilise ce schéma pour valider le code XML contenu dans le fichier RDL.  
  
     Pour inclure les éléments qui ne font pas partie du schéma RDL, placez-les dans l'élément personnalisé. L'élément personnalisé peut être lu par les extensions de rendu personnalisées, mais est ignoré par les extensions de rendu fournies avec [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Par exemple, vous pouvez utiliser l'élément personnalisé pour stocker des commentaires dans le rapport.  
  
     Pour plus d’informations, consultez [Langage de définition de rapport &#40;SSRS, Report Definition Language&#41;](../../reporting-services/reports/report-definition-language-ssrs.md).  
  
##  <a name="report-parts"></a><a name="bkmk_ReportParts"></a> Parties de rapports  
 Dans le Concepteur de rapports, une fois que vous avez créé des tables, graphiques et autres éléments de rapport paginé dans un projet, vous pouvez les publier comme des *parties de rapport* sur un serveur de rapports ou sur le site SharePoint intégré avec un serveur de rapports afin que vous et d’autres personnes puissiez les réutiliser dans d’autres rapports. Pour plus d’informations, consultez [Parties de rapports dans le Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/report-design/report-parts-in-report-designer-ssrs.md).  
  
 Les parties de rapports peuvent être déployées indépendamment des autres éléments dans un projet à l'aide de **TargetReportPartFolder** et d'autres propriétés. Pour plus d’informations, consultez [Définir des propriétés de déploiement &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
##  <a name="resources"></a><a name="bkmk_Resources"></a> Ressources  
 Vous pouvez ajouter à votre projet des fichiers liés à votre rapport mais pas traités par le serveur de rapports. Par exemple, vous pouvez ajouter des images pour des illustrations ou des fichiers de formes ESRI pour des données spatiales. Pour plus d’informations, consultez [Ressources](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md#bkmk_Resources).  
 
##  <a name="paginated-report-layout"></a><a name="bkmk_ReportLayout"></a> Mise en page de rapport paginé  
 Pour créer une mise en page de rapport, faites glisser des éléments de rapport et des régions de données de la boîte à outils vers l'aire de conception, puis réorganisez-les pour créer la mise en page du rapport. Faites glisser les champs du dataset vers les éléments de l'aire de conception pour ajouter des données au rapport. Pour organiser des données en groupes dans une région de données de tableau matriciel, faites glisser les champs du dataset vers le volet de regroupement. Étant donné que les outils de création de rapport sont essentiellement le moyen de créer des définitions de rapport, l'approche de création de rapports est assez similaire entre le Générateur et le Concepteur de rapports.  
   
##  <a name="preview-a-paginated-report"></a><a name="bkmk_Preview"></a> Aperçu d’un rapport paginé  
 Pour vérifier les données du rapport et la conception de la mise en page, utilisez **Aperçu** . Quand vous affichez l'aperçu d'un rapport, le processeur de rapports valide la syntaxe de schéma et d'expression de définition de rapport et répertorie les problèmes dans la fenêtre [Output](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_Output) .  
  
> [!NOTE]  
>  Lorsque vous prévisualisez un rapport, les données de ce rapport sont mises en cache dans un fichier sur l'ordinateur local. Ainsi, lorsque vous prévisualisez une nouvelle fois ce rapport (au moyen des mêmes requête, paramètres et informations d'identification), le Concepteur de rapports récupère l'exemplaire mis en cache au lieu d'exécuter à nouveau la requête. Le fichier de données est enregistré sous le nom de *\<reportname>* .rdl.data dans le même répertoire que le fichier de définition de rapport. Le fichier n'est pas supprimé lorsque vous fermez le Générateur de rapports.  
  
 Vous pouvez afficher un aperçu d'un rapport comme suit :  
  
-   **Vue aperçu.** Cliquez sur l'onglet **Aperçu** . Le rapport est exécuté localement à l'aide des mêmes fonctionnalités de traitement et de rendu de rapport que celles fournies par le serveur de rapports. Le rapport qui est affiché est une image interactive. Vous pouvez sélectionner des paramètres, cliquer sur des liens, afficher l'explorateur de documents, et développer ou réduire des zones masquées du rapport. Vous pouvez aussi exporter le rapport dans n'importe quel format de rendu installé.  
  
-   **Aperçu autonome.** Exécutez le rapport local dans un navigateur. En utilisant une configuration de débogage, vous pouvez également utiliser ce mode pour déboguer les assemblys personnalisés que vous écrivez. Il existe trois manières de gérer un projet en mode débogage :  
  
    -   Dans le menu **Déboguer** , cliquez sur **Démarrer le débogage**.  
  
    -   Dans la barre d'outils standard [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ,cliquez sur le bouton **Démarrer** .  
  
    -   Appuyez sur F5.  
  
     Si vous utilisez une configuration de projet qui crée le rapport mais ne le déploie pas, le rapport spécifié dans la propriété **StartItem** de la configuration actuelle s'ouvre dans une fenêtre d'aperçu distincte.  
  
    > [!NOTE]  
    >  Pour utiliser le mode débogage, vous devez définir un élément de départ. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet de rapport, puis sur **Propriétés**, et sélectionnez le nom du rapport à afficher dans **StartItem**.  
  
     Si vous souhaitez afficher l’aperçu d’un rapport particulier, sans qu’il soit l’élément de départ du projet, sélectionnez une configuration qui crée le rapport mais ne le déploie pas (par exemple, la configuration DebugLocal), cliquez avec le bouton droit sur le rapport, puis sélectionnez **Exécuter**. Vous devez choisir une configuration qui ne déploie pas le rapport, sinon il sera publié sur le serveur de rapports au lieu de s'afficher localement dans une fenêtre d'aperçu.  
  
-   **Aperçu avant impression.**  
  
     Lorsque vous affichez pour la première fois un rapport en mode Aperçu ou dans la fenêtre d'aperçu, le rapport prévisualisé ressemble à un rapport créé par l'extension de rendu HTML. L'aperçu n'est pas en HTML, mais la présentation et la pagination du rapport sont similaires à un affichage HTML.  
  
     Vous pouvez changer la vue pour afficher le rapport tel qu'il sera imprimé en activant le mode d'aperçu avant impression. Cliquez sur le bouton **Aperçu avant impression** dans la barre d'outils d'aperçu. Le rapport s'affiche comme sur une véritable page. Ce que vous voyez ressemble au résultat que produisent les extensions de rendu Image et PDF. L'aperçu avant impression n'est pas une image ni un fichier PDF, mais la disposition et la pagination du rapport sont similaires à celles de ces formats. Vous pouvez choisir la taille de l'image de rapport, par exemple, définir la largeur de la page.  
  
     L'aperçu avant impression vous aide à identifier plusieurs des problèmes de rendu que vous pouvez rencontrer si vous deviez imprimer le rapport. Les problèmes de rendu courants comprennent :  
  
    -   les pages vierges supplémentaires dues à un rapport trop large pour tenir sur le format de papier spécifié pour le rapport,  
  
    -   les pages vierges supplémentaires dues à un rapport contenant une matrice qui s'étend dynamiquement avec pour conséquence un dépassement de la largeur du papier spécifié,  
  
    -   les sauts de page entre les groupes ne fonctionnant pas de la façon souhaitée,  
  
    -   les en-têtes et pieds de page ne s'affichant pas comme prévu,  
  
    -   la mise en page de rapport nécessitant une modification pour une meilleure lecture dans un format imprimé.  
   
##  <a name="save-and-deploy-paginated-reports"></a><a name="bkmk_SaveandDeploy"></a> Enregistrer et déployer des rapports paginés  
 Dans le Concepteur de rapports, vous pouvez enregistrer des rapports et d'autres fichiers projet localement, ou les déployer vers un serveur de rapports ou un site SharePoint. Les sources de données partagées, les datasets partagés, les rapports, les ressources de rapport et les parties de rapports peuvent être déployés indépendamment ou définis selon les propriétés de déploiement du projet que vous configurez. Pour plus d'informations, consultez [Configuration and Deployment Properties](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md#bkmk_ConfigurationandDeploymentProperties).  
  
 Dans le Concepteur de rapports, il est important de comprendre que vous concevez un rapport à l'aide du schéma de définition de rapport pris en charge par la version actuelle de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Lorsque vous définissez les propriétés de déploiement du projet pour un serveur de rapports ou un site SharePoint spécifique et enregistrez ensuite le rapport, le générateur de rapports enregistre la définition de rapport dans le répertoire de build se trouvant dans le schéma correspondant à la version sur le serveur de rapports cible. Pour créer des rapports qui peuvent être publiés sur un serveur de rapports de bas niveau, le Concepteur de rapports supprime les éléments de rapport qui n'existent pas dans le schéma cible. Cela se produit automatiquement et sans invite. Dans ce cas, la définition de rapport d'origine est conservée dans le dossier du projet. La définition de rapport modifiée qui est déployée est dans le dossier de génération.  
  
> [!NOTE]  
>  Pour déboguer les expressions et les erreurs de déploiement, vous devez afficher la définition de rapport dans le dossier de génération. N'utilisez pas **Afficher la source**. **Afficher la source** affiche la source de définition de rapport à partir de le dossier du projet.  
  
 Pour plus d’informations, consultez [Déploiement et prise en charge des versions dans les outils de données SQL Server &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
### <a name="save-a-report-locally"></a>Enregistrer un rapport localement  
 Lorsque vous travaillez sur un rapport ou sur d'autres éléments de projet dans le Concepteur de rapports, les fichiers sont enregistrés dans votre ordinateur local ou dans un partage sur un autre ordinateur auquel vous avez accès.  
  
 Si vous utilisez le logiciel de contrôle de code source, vous pouvez vérifier vos rapports dans le serveur de contrôle de code source lorsque vous enregistrez le rapport. Pour plus d'informations, consultez [Source Control](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_SourceControl).  
  
### <a name="deploy-or-publish-paginated-reports"></a>Déployer ou publier des rapports paginés  
 A partir de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], vous pouvez déployer des rapports ou d'autres éléments de projet vers plusieurs versions de serveurs de rapports de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Utilisez les configurations de projet pour contrôler la mise à niveau des définitions de rapport aux versions de schéma compatibles avec les serveurs de rapports cibles. Les propriétés contrôlées par les configurations de projet incluent le serveur de rapports cible, le dossier où le processus de génération stocke temporairement les définitions de rapport pour l'aperçu et le déploiement, et les niveaux d'erreur. Pour plus d’informations, consultez [Propriétés de configuration et de déploiement](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md#bkmk_ConfigurationandDeploymentProperties) et [Définir des propriétés de déploiement &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md).  
  
### <a name="export-a-paginated-report-to-a-different-file-format"></a>Exporter un rapport paginé vers un autre format de fichier  
 Les rapports peuvent être exportés dans divers formats ; ceux-ci affectent le fonctionnement des fonctionnalités interactives et la mise en page des rapports. Pour plus d’informations sur les considérations relatives à la conception pour plusieurs formats de sortie, consultez [Exporter des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md).  
   
##  <a name="report-validation-and-error-levels"></a><a name="bkmk_ReportValidationandErrorLevels"></a> Validation de rapport et niveaux d’erreurs  
 Les rapports sont validés avant l'aperçu et lors du déploiement. Plusieurs problèmes de génération peuvent se produire lorsque les rapports sont créés. Les rapports peuvent contenir des chaînes, telles que des expressions ou requêtes, qui sont incompatibles avec la version de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] spécifiée par la configuration de projet, par exemple.  
  
 Utilisez la propriété ErrorLevel pour gérer les avertissements de génération et erreurs. La propriété ErrorLevel peut contenir une valeur comprise entre 0 et 4. La valeur détermine les problèmes de génération qui sont signalés comme erreurs et ceux qui sont signalés comme avertissements. La valeur par défaut est 2. Les avertissements et les erreurs sont écrits dans la fenêtre [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)][Sortie](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md#bkmk_Output) .  
  
 Les problèmes avec des niveaux de gravité inférieurs ou égaux à la valeur de ErrorLevel sont signalés comme erreurs ; sinon, ils sont signalés comme avertissements.  
  
 Le tableau suivant répertorie les niveaux d'erreur.  
  
|Niveau d'erreur|Description|  
|-----------------|-----------------|  
|0|Problèmes de génération les plus sévères et inévitables qui empêchent l'aperçu et le déploiement de rapports.|  
|1|Problèmes de génération sévères qui modifient la mise en page de rapport radicalement.|  
|2|Problèmes de génération moins sévères qui modifient la mise en page de rapport de façon significative.|  
|3|Problèmes de génération mineurs qui modifient si peu la mise en page de rapport qu'ils peuvent passer inaperçus.|  
|4|Utilisé uniquement pour la publication d'avertissements.|  
  
 Quand vous essayez d’afficher l’aperçu d’un rapport qui contient des éléments de rapport nouveaux dans [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]ou de déployer ce rapport, ces éléments de rapport peuvent être supprimés du rapport. Par défaut, la propriété ErrorLevel de la configuration a la valeur 2, ce qui provoquerait l’échec de la génération du rapport si la carte était supprimée. Toutefois, si vous remplacez la valeur de la propriété ErrorLevel par 0 ou 1, la carte est supprimée, un avertissement émis, et le processus de génération continue.  

## <a name="next-steps"></a>Étapes suivantes

[Télécharger SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)  
[Reporting Services dans les outils de données SQL Server](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md)   
[Outils de création de requêtes](../../reporting-services/report-data/query-design-tools-ssrs.md)   
[Prise en charge du déploiement et de la version dans les outils de données de serveur SQL](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)