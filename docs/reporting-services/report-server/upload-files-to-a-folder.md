---
title: Télécharger des fichiers dans un dossier | Microsoft Docs
description: Découvrez ce qui se passe quand vous chargez différents types de fichiers à partir du système de fichiers et que vous les stockez en tant qu’éléments gérés dans une base de données du serveur de rapports dans Reporting Services.
ms.date: 06/17/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- publishing reports [Reporting Services], uploading files
- reports [Reporting Services], publishing
- uploading reports [Reporting Services]
- uploading files [Reporting Services]
- files [Reporting Services], uploading
- files [Reporting Services]
- folders [Reporting Services], uploading files to
ms.assetid: 2f99a288-d4aa-4c64-b310-e457a2aef2c5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 94de4754811e1cb35c819b9cf8f4398b9e8c8634
ms.sourcegitcommit: f0772f614482e0b3cde3609e178689ce62ca3a19
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84547851"
---
# <a name="upload-files-to-a-folder"></a>Télécharger des fichiers dans un dossier
  Vous pouvez télécharger des fichiers à partir du système de fichiers, puis les stocker comme éléments gérés dans une base de données de serveur de rapports. Ce qui se produit lorsque vous téléchargez un fichier dépend du type du fichier.  
  
-   Le téléchargement d'un fichier .rdl équivaut à la publication d'un rapport.  
  
-   Le téléchargement de n'importe quel autre fichier l'ajoute à la base de données du serveur de rapports en tant qu'objet binaire unique. Ces fichiers sont publiés sur un serveur de rapports en tant que ressource. Les ressources peuvent être n'importe quel type de fichier. Si l'extension de fichier correspond à un type MIME connu, une icône associée à ce type MIME est utilisée pour identifier le type de la ressource. Sinon, une icône de fichier générique indique la présence d'une ressource.  
  
    >[!NOTE]  
    >Vous pouvez télécharger un fichier de source de données de rapports (.rds) pour créer une source de données partagée. Les fichiers .rds sont utilisables uniquement dans le Générateur de rapports. Ils ne peuvent en aucun cas fournir le contenu d'un élément de source de données partagée que vous définissez et gérez via le portail web. Outre le téléchargement, vous pouvez écrire un script qui crée une source de données partagée en se basant sur un fichier .rds.  
  
 La taille de fichier maximale des éléments chargés est de 2 Go et peut être définie à l’aide de la propriété MaxFileSizeMb dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 Sur un plan visuel, les fichiers que vous téléchargez dans une base de données de serveur de rapports s'affichent dans l'arborescence des dossiers avec les icônes suivantes.  
  
  ![Icônes de fichiers chargeables du serveur de rapports](../../reporting-services/report-server/media/upload-files-to-a-folder/report-server-uploadable-file-icons.png)
  
 Lorsque vous téléchargez un fichier, il est toujours placé dans le dossier en cours de sélection. Vous pouvez soit naviguer d'abord vers le dossier où l'élément sera stocké, soit télécharger un fichier puis le déplacer ultérieurement vers son emplacement définitif.  
  
 Pour charger un fichier, utilisez le portail web. Votre possibilité de télécharger des fichiers sur un serveur de rapports dépend des tâches incluses dans votre attribution de rôle. Si vous utilisez la sécurité par défaut, les administrateurs locaux peuvent ajouter des éléments à un serveur de rapports. Si la fonctionnalité Mes rapports est activée, tout utilisateur disposant d'un dossier Mes rapports est autorisé à télécharger des éléments dans ce dossier. Si vous utilisez des attributions de rôle personnalisées, l'attribution de rôle doit inclure les tâches prenant en charge la gestion de dossiers.  
  
|Action à réaliser|Incluez ces tâches|  
|----------------|-------------------------|  
|Télécharger un fichier .rdl dans un dossier|Gérer les rapports|  
|Télécharger un fichier en tant qu'objet binaire|Gestion des ressources|  
|Afficher le contenu d'un dossier|Afficher les ressources, afficher les rapports|  
  
## <a name="see-also"></a>Voir aussi  
 [Le portail web d’un serveur de rapports (Mode natif SSRS)](../../reporting-services/web-portal-ssrs-native-mode.md)  
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md)   
 [Charger un fichier ou un rapport dans le serveur de rapports](../../reporting-services/reports/upload-a-file-or-report-report-manager.md)   
  