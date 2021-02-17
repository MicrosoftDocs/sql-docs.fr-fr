---
description: Autorisations de feuille (services de données de référence)
title: Autorisations feuille
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attribute groups [Master Data Services], permissions
- members [Master Data Services], leaf member permissions
- permissions [Master Data Services], leaf members
- leaf members [Master Data Services], attribute permissions
- attributes [Master Data Services], leaf member attribute permissions
ms.assetid: bde16e8c-bcd4-4041-8130-55c5450e5f72
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 1924b33891b36d68a7b79c9046e0375b92f1d940
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338220"
---
# <a name="leaf-permissions-master-data-services"></a>Autorisations de feuille (services de données de référence)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Les autorisations de feuille s'appliquent aux valeurs d'attribut pour tous les membres feuille d'une entité.  
  
 Pour les entités sans hiérarchies explicites activées, l'affectation d'une autorisation **Feuille** revient à affecter l'autorisation à l'entité.  
  
 **Remarques :**  
  
-   Les autorisations de feuille s'appliquent à la zone fonctionnelle **Explorateur** de l'interface utilisateur uniquement.  
  
-   Les autorisations attribuées à **Nom** et **Code** ne sont pas appliquées.  
  
|Autorisation|Description|  
|----------------|-----------------|  
|**Lire**|L’utilisateur peut lire les membres feuille et les attributs.|  
|**Créer**|L’utilisateur peut créer des membres feuille et affecter des valeurs d’attribut lors de la création.|  
|**Mettre à jour**|L’utilisateur peut mettre à jour les membres feuille et les attributs.|  
|**Supprimer**|L’utilisateur peut supprimer des membres feuille.|  
|**Deny**|Refusez tout accès aux membres feuille.|  
  
 Vous pouvez aussi combiner les autorisations Read, Create, Update et Delete. Lorsque les autorisations Create, Update et Delete sont attribuées, l’autorisation Read est attribuée automatiquement.  
  
## <a name="attribute-permissions"></a>Autorisations d'attribut  
 Les autorisations d’attribut s’appliquent aux valeurs de l’attribut pour l’entité spécifique. Les utilisateurs avec des autorisations d'attribut uniquement ne peuvent pas ajouter ni supprimer des membres.  
  
|Autorisation|Description|  
|----------------|-----------------|  
|**Lire**|L’utilisateur peut lire des attributs.|  
|**Créer**|L’utilisateur peut attribuer des valeurs lorsqu’il crée des membres.|  
|**Mettre à jour**|L’utilisateur peut mettre à jour des attributs.|  
|**Supprimer**|Aucun effet.|  
|**Deny**|L'attribut n'est pas affiché.<br /><br /> Remarque : vous ne pouvez pas refuser explicitement l’accès aux attributs Name et Code.|  
  
### <a name="example"></a>Exemple  
 Pour l'entité Product, affectez l'autorisation **Mise à jour** à l'attribut Subcategory. Autorisation refuser à tous les autres attributs.  
  
|Nom|Code|Subcategory (Mise à jour)|  
|----------|----------|----------------------------|  
|Mountain-100|BK-M101|{5} VTT|  
|Mountain-100|BK-M201|{5} VTT|  
  
 Dans l' **Explorateur**, vous pouvez mettre à jour toute valeur d'attribut de la colonne Subcategory. Si vous n'avez pas d'autorisation sur un attribut, l'attribut n'est pas affiché.  
  
> [!NOTE]  
>  Dans cet exemple, Subcategory est un attribut basé sur un domaine, basé sur l'entité SubcategoryList. Vous pouvez sélectionner une sous-catégorie différente pour Mountain-100, mais vous ne pouvez pas ajouter ni supprimer des membres dans l'entité SubcategoryList.  
  
## <a name="see-also"></a>Voir aussi  
 [Affecter des autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
    
 [Autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Membres &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)  
  
  
