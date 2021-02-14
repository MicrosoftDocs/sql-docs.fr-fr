---
title: Attributs basés sur un domaine
description: En savoir plus sur les attributs basés sur un domaine dans Master Data Services, qui ont des valeurs remplies à partir d’une autre entité. Les utilisateurs doivent choisir une valeur dans une liste.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- domain-based attributes [Master Data Services], about domain-based attributes
- domain-based attributes [Master Data Services]
- attributes [Master Data Services], domain-based attributes
ms.assetid: df6f33ff-97f6-466c-af74-9780b2247473
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 8362789d3de498a2dba4ed83885ba1abb9dd09c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355308"
---
# <a name="domain-based-attributes-master-data-services"></a>Attributs basés sur un domaine (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], un attribut basé sur un domaine est un attribut dont les valeurs sont remplies par les membres d'une autre entité. Vous pouvez considérer un attribut basé sur un domaine sous la forme d'une liste contrainte ; les attributs basés sur un domaine empêchent les utilisateurs d'entrer des valeurs d'attribut qui sont valides. Pour sélectionner une valeur d'attribut, l'utilisateur doit choisir dans une liste.  
  
## <a name="domain-based-attribute-example"></a>Exemple d'attribut basé sur un domaine  
 Dans l'image suivante, l'entité Product a un attribut basé sur un domaine appelé Subcategory. L'attribut Subcategory est rempli par les valeurs de l'entité Subcategory.  
  
 L'entité Subcategory a un attribut basé sur un domaine appelé Category. L'attribut Category est rempli par les valeurs de l'entité Category.  
  
 ![Attributs basés sur un domaine à une entité](../master-data-services/media/mds-conc-domain-based-attribute-conceptual.gif "Attributs basés sur un domaine à une entité")  
  
## <a name="use-same-entity-for-multiple-domain-based-attributes"></a>Utiliser des attributs basés sur un domaine dans la même entité  
 Vous pouvez utiliser la même entité en tant qu'attribut basé sur un domaine de plusieurs entités. Par exemple, vous pouvez créer une entité appelée YesNoIndicator avec les membres : Yes, No et Maybe. Vous pouvez créer un attribut basé sur un domaine nommé InStock et utiliser l'entité YesNoIndicator comme source. Vous pouvez également créer un autre attribut basé sur un domaine nommé Approved et utiliser l'entité YesNoIndicator comme source. Lorsque vous voulez que les utilisateurs choisissent dans une liste des membres de l'entité YesNoIndicator, vous pouvez utiliser l'entité comme attribut basé sur un domaine.  
  
## <a name="domain-based-attributes-form-derived-hierarchies"></a>Les attributs basés sur un domaine forment les hiérarchies dérivées  
 Les relations d'attributs basés sur un domaine sont à la base des hiérarchies dérivées. Pour plus d’informations, consultez [Hiérarchies dérivées &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md).  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer un attribut basé sur un domaine qui provient d'une entité existante.|[Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)|  
|Créez une entité.|[Créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Hiérarchies dérivées &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)  
  
-   [Attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
-   [Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
  
