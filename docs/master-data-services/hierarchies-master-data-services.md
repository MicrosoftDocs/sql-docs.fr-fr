---
title: Hierarchies
description: Une hiérarchie est une arborescence que vous pouvez utiliser pour regrouper des membres similaires et consolider/synthétiser des membres pour la création de rapports et l’analyse dans Master Data Services.
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- hierarchies [Master Data Services]
- hierarchies [Master Data Services], about hierarchies
ms.assetid: 70dbb1fc-ead7-45be-9552-a45e3ccd8d21
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9705dbcd81c263573bbc911a22e4464852fcd441
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272530"
---
# <a name="hierarchies-master-data-services"></a>Hiérarchies (services de données de référence)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], une hiérarchie est une arborescence qui vous permet d'effectuer les opérations suivantes :  
  
-   regrouper des membres semblables à des fins d'organisation ;  
  
-   consolider et synthétiser des membres pour la création de rapports et l'analyse.  
  
## <a name="what-hierarchies-contain"></a>Les hiérarchies contiennent  
 Chaque hiérarchie contient les membres d'une ou de plusieurs entités. Lorsqu'un membre est ajouté, modifié ou supprimé, toutes les hiérarchies sont mises à jour. Cela garantit l'exactitude de vos données dans toutes les hiérarchies. Les hiérarchies permettent aussi de garantir que chaque membre est compté une seule fois.  
  
 Si vous souhaitez créer un regroupement d'un sous-ensemble de membres, envisagez d'utiliser une collection. Pour plus d’informations, consultez [Collections &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md).  
  
## <a name="kinds-of-hierarchies"></a>Types de hiérarchies  
 Vous pouvez créer plusieurs hiérarchies pour afficher et organiser vos membres de différentes façons. Vous pouvez créer :  
  
-   Des hiérarchies déséquilibrées d'une entité unique, appelées hiérarchies explicites. Pour plus d’informations, consultez [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md).  
  
-   Des hiérarchies basées sur le niveau à partir de plusieurs entités selon les relations existantes entre les entités et leurs attributs, appelées hiérarchies dérivées. Pour plus d’informations, consultez [Hiérarchies dérivées &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md).  
  
> [!NOTE]  
>  Tous les membres dans une hiérarchie doivent figurer dans le même modèle.  
  
## <a name="hierarchies-are-not-taxonomies"></a>Les hiérarchies ne sont pas des taxonomies  
 Une hiérarchie est différente d'une taxonomie. Une taxonomie classe des membres selon plusieurs attributs en même temps, alors qu'une hiérarchie les organise selon un attribut à la fois. Une taxonomie peut inclure le même membre plusieurs fois, alors qu'une hiérarchie inclut un membre une seule fois.  
  
 Par exemple, le même vélo peut être inclus deux fois dans une taxonomie : une fois parce qu'il est de couleur rouge et une fois parce que sa taille est égale à 38. Dans une hiérarchie, le vélo n’étant inclus qu’une seule fois, vous devez décider de l’afficher en fonction de sa couleur ou de sa taille.  
  
## <a name="hierarchy-example"></a>Exemple de hiérarchie  
 Dans l'exemple suivant, les membres de produit sont regroupés par membres de sous-catégorie.  
  
 ![Exemple de hiérarchie regroupée par sous-catégorie](../master-data-services/media/mds-conc-hierarchy.gif "Exemple de hiérarchie regroupée par sous-catégorie")  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer une hiérarchie explicite.|[Créer une hiérarchie explicite &#40;Master Data Services&#41;](../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|Créer une hiérarchie dérivée.|[Créer une hiérarchie dérivée &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|Masquer ou supprimer des niveaux dans une hiérarchie dérivée existante.|[Masquer ou supprimer des niveaux dans une hiérarchie dérivée &#40;Master Data Services&#41;](../master-data-services/hide-or-delete-levels-in-a-derived-hierarchy-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)  
  
-   [Hiérarchies dérivées &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
-   [Hiérarchies récursives &#40;Master Data Services&#41;](../master-data-services/recursive-hierarchies-master-data-services.md)  
  
-   [Hiérarchies dérivées avec un niveau supérieur composé d’une hiérarchie explicite &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-with-explicit-caps-master-data-services.md)  
  
-   [Collections &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)  
  
  
