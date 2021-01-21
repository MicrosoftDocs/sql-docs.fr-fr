---
title: Limites de taille des instantanés et des rapports | Microsoft Docs
description: Apprenez-en davantage sur les limites de taille de rapport quand un rapport est publié sur Report Server, rendu au moment de l’exécution et enregistré dans le système de fichiers.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- large reports
- maximum report size
- size [SQL Server], reports
- report size [Reporting Services]
- reports [Reporting Services], size
- denial of service attacks [Reporting Services]
ms.assetid: 1e3be259-d453-4802-b2f5-6b81ef607edf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5b5e8dbaee71c53f8f9491f8a1abcd15559e4800
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596842"
---
# <a name="report-and-snapshot-size-limits"></a>Limites de taille des instantanés et des rapports
  Les administrateurs qui gèrent un déploiement de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peuvent utiliser les informations figurant dans cette rubrique pour comprendre les limites de taille lorsque le rapport est publié sur un serveur de rapports, rendu lors de l'exécution et enregistré dans le système de fichiers. Cette rubrique fournit également des conseils pratiques expliquant comment mesurer la taille d'une base de données de serveur de rapports, et elle décrit l'incidence de la taille des instantanés sur les performances des serveurs.  
  
## <a name="maximum-size-for-published-reports"></a>Taille maximale des rapports publiés  
 Sur le serveur de rapports, la taille des modèles et des rapports repose sur la taille des fichiers de définition de rapport (.rdl) et de modèle de rapport (.smdl) que vous publiez sur un serveur de rapports. Le serveur de rapports ne limite pas la taille du rapport que vous publiez. Toutefois, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] impose une taille maximale pour les éléments qui sont publiés sur le serveur. La valeur par défaut est 4 mégaoctets (Mo). Si vous téléchargez ou publiez sur un serveur de rapports un fichier dont la taille est supérieure à cette limite, vous recevez une exception HTTP. Dans ce cas, vous pouvez modifier le paramétrage par défaut en augmentant la valeur de l'élément **maxRequestLength** dans le fichier Machine.config.  
  
 Bien qu'un modèle de rapport puisse être très volumineux, la taille des définitions de rapport excède rarement 4 Mo. Le plus souvent, la taille d'un rapport se mesure en kilo-octets (Ko). Cependant, si vous incluez des images incorporées, l'encodage de ces images peut générer des définitions de rapports volumineuses qui dépassent la taille par défaut de 4 Mo.  
  
 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] impose une limite maximale aux fichiers publiés pour minimiser les attaques par déni de service lancées à l’encontre du serveur. L'augmentation de la valeur de la limite supérieure amoindrit une partie de la protection que cette limite fournit. Augmentez cette valeur uniquement si vous êtes certain que l'avantage de cette opération compense tout risque de sécurité éventuel.  
  
 N'oubliez pas que la valeur que vous définissez pour l'élément **maxRequestLength** doit être supérieure à la taille maximale réelle que vous souhaitez appliquer. Vous devez augmenter cette valeur afin de tenir compte de l’augmentation inévitable de la taille des requêtes HTTP qui se produit après que tous les paramètres ont été encapsulés dans une enveloppe SOAP, et l’encodage Base64 a été appliqué à certains paramètres. L'encodage Base64 augmente la taille des données d'origine d'environ 33 %. Par conséquent, la valeur spécifiée pour l'élément **maxRequestLength** doit être environ 33 % supérieure à la taille réelle utilisée par l'élément. Par exemple, si vous spécifiez une valeur de 64 Mo pour **maxRequestLength**, la taille maximale des fichiers de rapport publiés sur le serveur de rapports sera d'environ 48 Mo.  
  
## <a name="report-size-in-memory"></a>Taille de rapport en mémoire  
 Lorsque vous exécutez un rapport, la taille du rapport est égale à la quantité de données qui est retournée dans le rapport plus la taille du flux de sortie. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n'impose pas de limite maximale sur la taille d'un rapport rendu. La mémoire système détermine la limite supérieure en matière de taille (par défaut, un serveur de rapports utilise toute la mémoire configurée disponible lors du rendu d'un rapport), mais vous pouvez spécifier des paramètres de configuration pour définir des seuils de mémoire et des stratégies de gestion de mémoire. Pour plus d’informations, consultez [Configurer la mémoire disponible pour les applications du serveur de rapports](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
 La taille d'un rapport peut varier considérablement selon la quantité de données retournées et le format de rendu que vous utilisez pour le rapport. Un rapport paramétré peut être plus ou moins volumineux en fonction de l'incidence des valeurs des paramètres sur les résultats des requêtes. Le format de sortie de rapport que vous choisissez affecte la taille des rapports comme suit :  
  
-   Le format HTML traite le rapport une page à la fois. Dans la mesure où le rapport est traité en unités de petite taille, la mémoire requise pour traiter des blocs spécifiques est moindre.  
  
-   Les formats PDF, Excel, TIFF, XML et CSV traitent tout le rapport en mémoire avant de l'afficher aux yeux de l'utilisateur.  
  
 Pour mesurer la taille d'un rapport rendu, vous pouvez afficher le journal d'exécution du rapport. Pour plus d’informations, consultez [Journal des exécutions du serveur de rapports et vue ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
 Pour calculer la taille d'un rapport rendu sur disque, exportez-le vers le système de fichiers puis enregistrez-le (le fichier enregistré inclut des informations de mise en forme des données et du rapport).  
  
 La seule limite physique sur la taille du rapport s'applique lors du rendu au format Excel. Les feuilles de calcul ne peuvent pas contenir plus de 65 536 lignes ou 256 colonnes. D'autres formats de rendu ne sont pas soumis à ces limites, par conséquent, la taille est limitée uniquement par la quantité de ressources sur votre serveur.  
  
> [!NOTE]  
>  Le traitement et le rendu d'un rapport sont opérés en mémoire. Si vous possédez des rapports volumineux ou un grand nombre d'utilisateurs, veillez à réaliser une planification de capacité pour vous assurer que vos utilisateurs sont satisfaits des performances de votre déploiement de serveur de rapports. Pour plus d'informations sur les outils et instructions disponibles, consultez les publications suivantes sur MSDN : [Planning for Scalability and Performance with Reporting Services](/previous-versions/sql/sql-server-2005/administrator/cc966418(v=technet.10)) et [Using Visual Studio 2005 to Perform Load Testing on a SQL Server 2005 Reporting Services Report Server](/previous-versions/sql/sql-server-2005/administrator/aa964139(v=sql.90))(en anglais).  
  
## <a name="measuring-snapshot-storage"></a>Mesure du stockage des instantanés  
 La taille d'un instantané est directement proportionnelle à la quantité de données dans le rapport. Les instantanés sont généralement bien plus volumineux que les autres éléments stockés sur un serveur de rapports. Leur taille oscille généralement entre quelques mégaoctets et des dizaines de mégaoctets. Si vos rapports sont extrêmement volumineux, il est probable que les instantanés le seront encore plus. En fonction de la fréquence d'utilisation des instantanés et de la configuration de l'historique des rapports, la quantité d'espace disque nécessaire par la base de données du serveur de rapports peut augmenter rapidement en peu de temps.  
  
 Par défaut, les bases de données **reportserver** et **reportservertempdb** sont configurées pour permettre une croissance automatique. La taille d'une base de données peut augmenter automatiquement, mais elle ne peut en aucun cas diminuer automatiquement. Si la base de données **reportserver** dispose d'une capacité excédentaire car vous avez supprimé des instantanés, vous devez la réduire manuellement pour récupérer de l'espace sur le disque. De même, si la taille de la base de données **reportservertempdb** a augmenté pour gérer un volume anormalement élevé de rapports interactifs, l'allocation de l'espace disque restera à ce paramètre tant que vous ne l'aurez pas diminué.  
  
 Pour mesurer la taille des bases de données du serveur de rapports, vous pouvez exécuter les commandes [!INCLUDE[tsql](../../includes/tsql-md.md)] suivantes. Si vous calculez régulièrement la taille totale des bases de données, vous pourrez évaluer avec justesse comment allouer au fur et à mesure de l'espace à la base de données du serveur de rapports. Les instructions suivantes mesurent la quantité d'espace actuellement utilisée (les instructions partent du principe que vous utilisez les noms de base de données par défaut) :  
  
```  
USE ReportServer  
EXEC sp_spaceused  
```  
  
## <a name="snapshot-size-and-report-server-performance"></a>Taille des instantanés et performances des serveurs de rapports  
 La taille des instantanés a une incidence sur les performances du serveur lorsque le rapport est traité puis rendu. Ce sont les opérations de rendu qui affectent le plus les performances du serveur. Par conséquent, si vous disposez d'un instantané volumineux, un délai interviendra lorsque les utilisateurs demanderont le rapport. En fonction du nombre d'utilisateurs, des délais sont à prévoir lorsque la taille d'un instantané dépasse 100 mégaoctets.  
  
 Pour minimiser les retards de performances en raison d'instantanés volumineuses, vous pouvez :  
  
-   déployer le serveur de rapports et le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sur des ordinateurs distincts ;  
  
-   ajouter davantage de mémoire système ;  
  
-   consulter le document « Planning for Scalability and Performance with Reporting Services » (en anglais) sur le site Web MSDN pour des recommandations concernant la configuration d'un serveur de rapports pour l'entreprise.  
  
 La quantité d'instantanés qui est stockée dans une base de données de serveur de rapports n'est pas, en soi, un facteur de performances. Vous pouvez stocker un grand nombre d'instantanés sans aucune incidence sur les performances. Vous pouvez conserver les instantanés indéfiniment. Sachez cependant que l'historique de rapport est configurable. Si un administrateur de serveur de rapports abaisse la limite de l'historique de rapport, vous risquez de perdre des rapports que vous comptiez garder. Si vous supprimez le rapport, tout l'historique est également supprimé.  
  
## <a name="see-also"></a>Voir aussi  
 [Définir les propriétés de traitement d’un rapport](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Base de données du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
 [Traiter les rapports volumineux](../../reporting-services/report-server/process-large-reports.md)  
  
