---
title: Déployer un package de déploiement de modèle (MDSModelDeploy)
description: Découvrez comment utiliser MDSModelDeploy pour déployer un package dans Master Data Services. Un package peut contenir des objets de modèle uniquement ou des objets de modèle et des données.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: fb2a4df4-5e0d-4b34-818f-383dbde1b15c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ade6b388deb6414a6b2940d5fef74a9f26ef4637
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350209"
---
# <a name="deploy-a-model-deployment-package-by-using-mdsmodeldeploy"></a>Déployer un package de déploiement de modèle à l'aide de MDSModelDeploy

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], utilisez l'outil MDSModelDeploy pour déployer un package qui contient soit :  
  
-   Des objets de modèle uniquement.  
  
-   Des objets de modèle et des données.  
  
 Si vous souhaitez déployer un package qui contient uniquement des objets de modèle, vous pouvez utiliser l'Assistant Déploiement de modèle dans l'application web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] à la place. Pour plus d’informations, consultez [Déployer un package de déploiement de modèle à l’aide de l’Assistant](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md).  
  
> [!IMPORTANT]  
>  Les packages peuvent être déployés uniquement dans l'édition de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dans laquelle ils ont été créés. Cela signifie que les packages créés dans [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] ne peuvent pas être déployés sur [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] ou les versions ultérieures.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** dans l'environnement [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] cible.  
  
-   Un package de déploiement de modèle doit exister. Pour plus d’informations, consultez  [Créer un package de déploiement de modèle à l'aide de MDSModelDeploy](../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
-   Vous devez être administrateur dans l'environnement où vous déployez le modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Si vous mettez à jour un modèle avec des données, la version que vous déployez ne peut pas avoir l’état **Verrouillé** ou **Activé**.  
  
### <a name="to-deploy-a-model-deployment-package"></a>Pour déployer un package de déploiement de modèle  
  
1.  Déterminez si vous déployez un nouveau modèle, un clone d'un modèle, ou si vous mettez à jour un modèle préalablement cloné. Pour plus d’informations, consultez [Options de déploiement de modèle &#40;Master Data Services&#41;](../master-data-services/model-deployment-options-master-data-services.md).  
  
2.  Ouvrez une invite de commandes d’administrateur et accédez à MDSModelDeploy.exe.  
  
    -   Si MDS est installé à l’emplacement par défaut, l’outil est disponible sur *lecteur*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration  
  
    -   Si MDS n'est pas installé dans l'emplacement par défaut, recherchez MDSModelDeploy.exe sur l'ordinateur local.  
  
3.  facultatif. Consultez les options et l'aide.  
  
    -   Pour afficher toutes les options disponibles, tapez `MDSModelDeploy` et appuyez sur Entrée.  
  
    -   Pour afficher l’aide pour une option, tapez la commande suivante, où *OptionName* est le nom de l’option : `MDSModelDeploy help OptionName`.  
  
4.  facultatif. Si vous possédez plusieurs applications Web, déterminez le nom du service que vous allez déployer en entrant cette commande et en appuyant sur ENTRÉE :  
  
    ```  
    MDSModelDeploy listservices  
    ```  
  
     Une liste de valeurs est retournée, par exemple `MDS1, Default Web Site, MDS`. La première valeur de cette liste (dans ce cas, `MDS1`) est nécessaire pour déployer le modèle.  
  
5.  Selon que vous créez un modèle, clonez modèle ou mettez à jour un modèle, à l'invite de commandes, tapez la commande suivante et appuyez sur Entrée.  
  
    -   Pour créer un modèle :  
  
        ```  
        MDSModelDeploy deploynew -package PackageName -model ModelName -service ServiceName  
        ```  
  
    -   Pour créer un clone d'un modèle :  
  
        ```  
        MDSModelDeploy deployclone -package PackageName  
        ```  
  
    -   Pour mettre à jour un de modèle existant et ses données :  
  
        ```  
        MDSModelDeploy deployupdate -package PackageName -version VersionName  
        ```  
  
    > [!IMPORTANT]  
    >  Si vous utilisez l'outil MDSModelDeploy pour mettre à jour un modèle existant et ses données et que le package ne contient pas une entité, un attribut ou un membre existant dans le modèle de destination, MDSModelDeploy ne supprime pas cette entité, cet attribut ou ce membre du modèle.  
  
     Où *PackageName* est le nom du fichier de package (.pkg), *ModelName* est le nom du nouveau modèle, *VersionName* est le nom de la version et *ServiceName* est le nom du service que vous avez retourné à l’étape précédente. Vérifiez que les noms de modèle et de version correspondent aux noms en respectant la casse.  
  
6.  Quand le package est déployé, un message indiquant que l’opération de MDSModelDeploy est terminée s’affiche.  
  
 **Remarques :**  
  
-   Si une vue d’abonnement dans le package a le même nom qu’une vue d’abonnement dans un modèle existant, l’avertissement suivant s’affiche : **La vue d’abonnement du système de déploiement a été renommée** . La vue est alors créée en tant que *modelname.subscriptionviewname*. Si ce nom existe déjà, la vue d'abonnement n'est pas créée.  
  
-   Le processus de déploiement comporte quatre étapes :  
  
    1.  Les objets de modèle sont créés.  
  
    2.  Les règles d'entreprise sont créées.  
  
    3.  Les vues d'abonnement sont créées.  
  
    4.  Les données de référence sont remplies.  
  
-   Lorsque vous créez un modèle nouveau ou cloné, si le processus échoue au cours d'une étape, le modèle est supprimé.  
  
     Lorsque vous mettez à jour un modèle, si le processus échoue au cours des trois premières étapes, il s'arrête ; toutefois, les modifications qui sont déjà effectuées ne sont pas annulées. Si le processus échoue à l'étape 4, les membres qui peuvent être mis à jour sont mis à jour.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Les attributs de fichier et les autorisations d’accès ne sont pas inclus dans les packages de déploiement de modèle. Après avoir déployé un modèle, vous devez les mettre à jour manuellement. Pour plus d'informations, consultez les pages suivantes :  
  
-   [Affecter des autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Déploiement de modèles &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md)  
  
  
