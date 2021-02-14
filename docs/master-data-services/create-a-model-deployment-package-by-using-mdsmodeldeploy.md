---
title: Créer un package de déploiement de modèle (MDSModelDeploy)
description: Utilisez MDSModelDeploy pour créer un package de déploiement dans Master Data Services. Un package peut contenir des objets de modèle uniquement ou des objets de modèle et des données.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: c2687e39-dc20-494f-a707-2aa29f4c329e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ef35b92932cd4e55b47e2f75a16825f14202a815
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272640"
---
# <a name="create-a-model-deployment-package-by-using-mdsmodeldeploy"></a>Créer un package de déploiement de modèle à l'aide de MDSModelDeploy

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], utilisez l’outil MDSModelDeploy pour créer un package. Selon les commandes spécifiées, le package peut contenir :  
  
-   Des objets de modèle uniquement.  
  
-   Des objets de modèle et des données.  
  
 Si vous souhaitez déployer un package qui contient uniquement des objets de modèle, vous pouvez utiliser l'Assistant Déploiement de modèle dans l'application web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] à la place. Pour plus d’informations, consultez [Créer un package de déploiement de modèle à l’aide de l’Assistant](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md).  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
1.  Les autorisations de base nécessaires pour exécuter l'outil MDSModelDeploy sont les suivantes :  
  
    -   La même autorisation Windows que le gestionnaire de configuration MDS (administrateur Windows)  
  
    -   Autorisation DBA sur la base de données MDS.  
  
2.  Les autorisations nécessaires pour créer le package à l'aide de l'outil MDSModelDeploy sont les suivantes :  
  
    -   Autorisation d'administrateur de modèle MDS sur le modèle de données.  
  
    -   Autorisation de zone fonctionnelle MDS ImportExport.  
  
3.  Les autorisations nécessaires pour déployer le package à l'aide de l'outil MDSModelDeploy sont les suivantes :  
  
    -   Autorisation de fonction Explorateur MDS  
  
    -   Autorisation de fonction Administration de système MDS.  
  
4.  Les autorisations nécessaires pour répertorier les modèles à l'aide de l'outil MDSModelDeploy sont les suivantes :  
  
    -   Autorisation de fonction Explorateur MDS  
  
    -   Autorisation d'administrateur de modèle MDS sur le modèle de données pour voir le modèle dans la liste.  
  
 Un modèle doit exister pour que vous puissiez créer un package. Pour plus d’informations, consultez [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md).  
  
 Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-model-deployment-package-by-using-mdsmodeldeploy"></a>Pour créer un package de déploiement de modèle à l'aide de MDSModelDeploy  
  
1.  Ouvrez une invite de commandes d’administrateur.  
  
2.  Accédez à l'emplacement de MDSModelDeploy.exe.  
  
    -   Si MDS a été installé à l’emplacement par défaut, le fichier est disponible dans *lecteur*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
    -   Si MDS n'a pas été installé dans l'emplacement par défaut, recherchez MDSModelDeploy.exe sur l'ordinateur local.  
  
3.  facultatif. Consultez les options et l'aide.  
  
    -   Pour afficher toutes les options disponibles, tapez `MDSModelDeploy` et appuyez sur Entrée.  
  
    -   Pour afficher l’aide pour une option, tapez la commande suivante, où *OptionName* est le nom de l’option : `MDSModelDeploy help OptionName`.  
  
4.  facultatif. Si vous possédez plusieurs applications Web, déterminez le nom du service que vous allez déployer en entrant cette commande et en appuyant sur ENTRÉE :  
  
    ```  
    MDSModelDeploy listservices  
    ```  
  
     Une liste de valeurs est retournée, par exemple `MDS1, Default Web Site, MDS`. La première valeur de cette liste (dans ce cas, `MDS1`) est nécessaire pour déployer le modèle.  
  
5.  Pour créer un package qui contient des objets de modèle et des données, tapez la commande suivante, où *ModelName*, *VersionName*, *ServiceName* et *PackageName* sont respectivement les noms du modèle, la version, le service et le fichier de sortie .pkg :  
  
    ```  
    MDSModelDeploy createpackage -model ModelName -version VersionName -service ServiceName -package PackageName -includedata  
    ```  
  
     Si vous ne souhaitez pas inclure de données, n’utilisez pas les commutateurs `-version` et `-includedata` .  
  
6.  Appuyez sur Entrée. Quand le package est créé, un message indiquant que l’opération de MDSModelDeploy est terminée s’affiche.  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   [Déployer un package de déploiement de modèle à l'aide de MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Options de déploiement de modèle &#40;Master Data Services&#41;](../master-data-services/model-deployment-options-master-data-services.md)   
 [Déploiement de modèles &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md)  
  
  
