---
title: Afficher un aperçu des rapports
description: Découvrez comment afficher un aperçu des rapports dans SQL Server Reporting Services avant de les publier dans un environnement de production.
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/14/2018
ms.openlocfilehash: 45965ac8d40496fb2cd18c576ca6bb1228ce855b
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91988625"
---
# <a name="preview-reports-in-sql-server-reporting-services-ssrs"></a>Afficher un aperçu des rapports dans SQL Server Reporting Services (SSRS)

  Quand vous concevez un rapport [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vous pouvez le visualiser avant de le publier sur un environnement de production. Pour ce faire, vous disposez de plusieurs méthodes : en passant en mode aperçu dans le Concepteur de rapports, en utilisant la fenêtre d'aperçu du Concepteur de rapports et en publiant le rapport sur un serveur de rapports dans un environnement de test.  
  
> [!NOTE]  
> Lorsque vous prévisualisez un rapport, les données de ce rapport sont mises en cache dans un fichier sur l'ordinateur local. Ainsi, lorsque vous prévisualisez une nouvelle fois ce rapport (au moyen des mêmes requête, paramètres et informations d'identification), le Concepteur de rapports récupère l'exemplaire mis en cache au lieu d'exécuter à nouveau la requête. Le fichier de données est enregistré sous le nom de *\<reportname>* .rdl.data dans le même répertoire que le fichier de définition de rapport. Le fichier n'est pas supprimé lorsque vous fermez le Générateur de rapports.  
  
## <a name="preview-mode"></a>Mode Aperçu

 Pour prévisualiser un rapport dans le Concepteur de rapports, cliquez sur ![ssrs_ssdt_preview](../../reporting-services/media/ssrs-ssdt-preview.png "ssrs_ssdt_preview"). Cette opération entraîne l'exécution du rapport localement. Elle utilise les mêmes fonctionnalités de traitement et de rendu de rapport que celles fournies par le serveur de rapports. Le rapport qui est affiché est une image interactive. Vous pouvez sélectionner des paramètres, cliquer sur des liens, afficher l'explorateur de documents, et développer ou réduire des zones masquées du rapport. Vous pouvez aussi exporter le rapport dans n'importe quel format de rendu installé.  
  
## <a name="standalone-preview"></a>Afficheur

 Une autre manière d'afficher l'aperçu d'un rapport consiste à exécuter le projet de rapport dans une configuration de débogage, par exemple pour déboguer les assemblys personnalisés que vous écrivez. Le rapport s’ouvre dans votre navigateur par défaut. Vous pouvez exécuter un projet de trois manières :  
  
- Dans le menu **Déboguer** , en cliquant sur **Démarrer le débogage** .  
  
- En cliquant sur le bouton **Démarrer** de la barre d’outils standard [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] ![ssrs_ssdt_startdebug](../../reporting-services/reports/media/ssrs-ssdt-startdebug.png "ssrs_ssdt_startdebug").  
  
- En appuyant sur **F5**.  
  
 Si vous utilisez une configuration de projet qui crée le rapport mais ne le déploie pas, le rapport spécifié dans la propriété **StartItem** de la configuration actuelle s'ouvre dans une fenêtre d'aperçu distincte. La fenêtre d'aperçu affiche le rapport de la même manière et avec les mêmes fonctionnalités que le mode Aperçu.  
  
> [!NOTE]  
> Avant de déboguer un rapport, vous devez définir un élément de départ. C’est par exemple le cas si vous exécutez le mode débogage et que le navigateur ouvre la page de serveur de rapports principale et pas votre rapport en mode Aperçu. Pour ce faire, dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet de rapport, cliquez sur **Propriétés**, puis sur le nom du rapport à afficher dans la zone **StartItem**.  
  
 Si vous souhaitez afficher l’aperçu d’un rapport particulier, sans qu’il soit l’élément de départ du projet, sélectionnez une configuration qui crée le rapport mais ne le déploie pas (par exemple, la configuration DebugLocal), cliquez avec le bouton droit sur le rapport, puis sélectionnez **Exécuter**. Vous devez choisir une configuration qui ne déploie pas le rapport, sinon il sera publié sur le serveur de rapports au lieu de s'afficher localement dans une fenêtre d'aperçu.  
  
## <a name="publish-to-a-test-server"></a>Publier sur un serveur de test

 Vous pouvez également tester les rapports en les publiant sur un serveur de test, accéder aux rapports et en afficher un aperçu. La publication d'un rapport sur un serveur de test revient, au niveau de la procédure, à publier un rapport sur un serveur de production. Pour plus d’informations sur la publication d’un rapport, consultez [Publication de rapports sur un serveur de rapports](../../reporting-services/reports/publishing-reports-to-a-report-server.md).  
  
## <a name="next-steps"></a>Étapes suivantes

 - [Imprimer des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)
 - [Imprimer un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-a-report-report-builder-and-ssrs.md)
 - [Publier des rapports](/previous-versions/sql/sql-server-2016/ms159615(v=sql.130))
 - [Utilisation d'assemblages personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)