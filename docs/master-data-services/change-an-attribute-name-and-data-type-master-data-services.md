---
description: Modifier le nom d’un attribut et un type de données (Master Data Services)
title: Modifier le nom d’un attribut et un type de données
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attributes [Master Data Services], changing name
ms.assetid: d348f238-f59d-41c7-ad20-3ccd55bfd9e5
author: lrtoyou1223
ms.author: lle
manager: erikre
ms.openlocfilehash: d139a8c1bfc46c838d3720eb906faa33bfec2d09
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272850"
---
# <a name="change-an-attribute-name-and-data-type-master-data-services"></a>Modifier le nom d’un attribut et un type de données (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez modifier le nom d'un attribut.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-change-an-attribute-name-and-type"></a>Pour modifier le nom d’un attribut et un type  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Sur la page **gérer le modèle** , sélectionnez un modèle dans la grille, puis cliquez sur **entités**.  
  
3.  Dans la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez créer un attribut.  
  
4.  Cliquez sur **Attributs**.  
  
5.  Sur la page **Gérer les attributs** , effectuez l’une des opérations suivantes.  
  
    -   Si l’attribut concerne les membres feuille, sélectionnez **Feuille** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les membres consolidés, sélectionnez **Consolidé** dans la zone de liste **Types de membre** .  
  
    -   Si l’attribut concerne les collections, sélectionnez **Collection** dans la zone de liste **Types de membre** .  
  
6.  Sélectionnez la ligne de l’attribut que vous souhaitez modifier, puis cliquez sur **Modifier**.  
  
7.  Dans la zone **Nom** , entrez le nom mis à jour de l'attribut. Pour obtenir la liste des mots qui ne doivent pas être utilisés comme noms d’attribut, consultez la section [Mots réservés &#40;Master Data Services&#41;](../master-data-services/reserved-words-master-data-services.md).  
  
8.  Dans la liste **Type d’attribut** , sélectionnez un autre type.  
  
9. Cliquez sur **Enregistrer**.  
  
## <a name="see-also"></a>Voir aussi  
 [Créez un attribut de texte &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)   
 [Supprimer un attribut &#40;Master Data Services&#41;](../master-data-services/delete-an-attribute-master-data-services.md)   
 [Attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
