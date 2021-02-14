---
description: Créer un administrateur de modèle (Master Data Services)
title: Créer un administrateur de modèle
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- administrators [Master Data Services], creating
ms.assetid: dae17afc-3b39-490e-b51f-2d8da26d429e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 11aada5002d7f35a66d64040016e9cf6e614fdf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272650"
---
# <a name="create-a-model-administrator-master-data-services"></a>Créer un administrateur de modèle (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez un administrateur de modèle quand vous souhaitez qu’un groupe ou un utilisateur ait toutes les autorisations sur tous les objets dans un ou plusieurs modèles.  
  
> [!TIP]  
>  Pour simplifier l’administration, créez un groupe Windows ou local et configurez-le en tant qu’administrateur de modèle. Vous pouvez ensuite ajouter et supprimer des utilisateurs dans le groupe sans accéder à l'interface utilisateur de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Autorisations d'accès** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-model-administrator"></a>Pour créer un administrateur de modèle  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Autorisations d'accès**.  
  
2.  Dans la page **Utilisateurs** ou **Groupes** , sélectionnez la ligne de l'utilisateur ou du groupe à modifier.  
  
3.  Cliquez sur **Modifier l'utilisateur sélectionné**.  
  
4.  Cliquez sur l'onglet **Modèles** .  
  
5.  Dans la liste **Modèle** , sélectionnez éventuellement un modèle.  
  
6.  Cliquez sur **Modifier**.  
  
7.  Cliquez sur le modèle auquel vous souhaitez affecter une autorisation.  
  
8.  Dans le menu, sélectionnez **Administrateur**.  
  
9. Effectuez les étapes 7 et 8 pour chaque modèle que vous souhaitez que le groupe ou l'utilisateur administre.  
  
10. Cliquez sur **Enregistrer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [Affecter des autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Affecter des autorisations de membre de hiérarchie &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)   
 [Autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Autorisations des membres de la hiérarchie &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)  
  
  
