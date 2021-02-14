---
description: Ajouter des attributs à un groupe de suivi des modifications (Master Data Services)
title: Ajouter des attributs au groupe de Change Tracking
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- change tracking groups [Master Data Services]
- attributes [Master Data Services], change tracking groups
- change tracking groups [Master Data Services], adding attributes
ms.assetid: e153eb5f-70ca-4c6f-89d8-1f937ed3917d
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 72bf0900fc46c566ae766f4ab7705ccc0bca47de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272920"
---
# <a name="add-attributes-to-a-change-tracking-group-master-data-services"></a>Ajouter des attributs à un groupe de suivi des modifications (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], ajoutez des attributs à un groupe de suivi des modifications lorsque vous souhaitez effectuer le suivi des modifications apportées aux valeurs d'attribut.  
  
> [!NOTE]  
>  Lorsque qu'un attribut est ajouté à un groupe de suivi des modifications et que ses valeurs changent, l'attribut est alors signalé comme modifié dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Créez une règle d'entreprise pour entreprendre une action basée sur la modification.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Les attributs doivent exister pour pouvoir être ajoutés au groupe de suivi des modifications. Pour plus d’informations, consultez [Créer un attribut de texte &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md).  
  
### <a name="to-add-attributes-to-a-change-tracking-group"></a>Pour ajouter des attributs à un groupe de suivi des modifications  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Sur la page **gérer le modèle** , sélectionnez un modèle dans la grille, puis cliquez sur **entités**.  
  
3.  Dans la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez créer un attribut.  
  
4.  Cliquez sur **Attributs**.  
  
5.  Sur la page **Gérer les attributs** , effectuez l’une des opérations suivantes.  
  
    -   Si l’attribut concerne les membres feuille, sélectionnez **Feuille** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les membres consolidés, sélectionnez **Consolidé** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les collections, sélectionnez **Collection** dans la zone de liste **Types de membre** .  
  
6.  Sélectionnez la ligne de l’attribut que vous souhaitez modifier, puis cliquez sur **Modifier**.  
  
7.  Activez la case à cocher **Activer le suivi des modifications** .  
  
8.  Dans la zone **Groupe de suivi des modifications** , tapez un numéro pour le groupe.  
  
9. Cliquez sur **Enregistrer l'attribut**.  
  
     Pour l’attribut modifié, la colonne **Activer le groupe de suivi des modifications** de la grille est remplacée par **Oui (Groupe : numéro de groupe saisi)**.  
  
10. Répétez cette procédure pour tous les attributs que vous souhaitez inclure dans le groupe. Utilisez le même numéro de groupe de suivi des modifications pour chaque attribut dans le groupe.  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   [Initier des actions en fonction de modifications de valeurs d’attribut &#40;Master Data Services&#41;](../master-data-services/initiate-actions-based-on-attribute-value-changes-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Créez un attribut de texte &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)   
 [Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)  
  
  
