---
description: Chevauchement des autorisations de modèle et de membre (Master Data Services)
title: Chevauchement des autorisations de modèle et de membre
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], effective permissions
- permissions [Master Data Services], model and member overlaps
- members [Master Data Services], effective permissions
ms.assetid: 9fd7a555-43bf-4796-a8b6-1ca63a291216
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f3227ef69915e606f514b5ecf36e5f1656db733e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340282"
---
# <a name="overlapping-model-and-member-permissions-master-data-services"></a>Chevauchement des autorisations de modèle et de membre (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Une autorisation attribuée à un membre et une autorisation attribuée à un objet de modèle peuvent se chevaucher. Lorsque des chevauchements se produisent, l'autorisation la plus restrictive entre en vigueur.  
  
 Si un membre a une autorisation qui est différente de celle de son objet de modèle correspondant, les règles suivantes s'appliquent :  
  
-   **Refuser** remplace toutes les autres autorisations.  
  
-   L’autorisation **Admin** au niveau du modèle remplace toutes les autres autorisations ; elle est remplacée par une autorisation de type Tous les accès (CRUD) pour les sous-niveaux.  
  
-   L’autorisation d’accès effective chevauche les autorisations des membres et attributs.  
  
     Par exemple, si les membres possèdent des autorisations **Créer** et **Mettre à jour**, l’autorisation des attributs sera **Mettre à jour**. L’autorisation effective sera **Mettre à jour**.  
  
 L'image suivante montre les autorisations appliquées sur une valeur d'attribut individuelle lorsque les autorisations d'attribut sont différentes des autorisations de membre.  
  
 ![mds_conc_security_member_overlap_table](../master-data-services/media/mds-conc-security-member-overlap-table.gif "mds_conc_security_member_overlap_table")  
  
## <a name="example-1"></a>Exemple 1  
 ![mds_conc_overlap_model_1](../master-data-services/media/mds-conc-overlap-model-1.gif "mds_conc_overlap_model_1")  
  
 Sous l'onglet **Modèles** , l'entité Product a l'autorisation **Mise à jour** attribuée. Tous les attributs dans l'entité héritent de cette autorisation.  
  
 Sous l'onglet **Membres de hiérarchie** , le nœud de sous-catégorie Mountain Bikes dans une hiérarchie dérivée a l'autorisation **Mise à jour** attribuée.  
  
 Résultat : dans **Explorateur**, l'utilisateur a l'autorisation **Mise à jour** sur toutes les valeurs d'attribut de tous les membres qui se trouvent dans le nœud Mountain Bikes. Tous les autres membres et attributs sont masqués.  
  
 ![mds_conc_overlap_model_example_1](../master-data-services/media/mds-conc-overlap-model-example-1.gif "mds_conc_overlap_model_example_1")  
  
## <a name="example-2"></a>Exemple 2  
 ![mds_conc_overlap_model_2](../master-data-services/media/mds-conc-overlap-model-2.gif "mds_conc_overlap_model_2")  
  
 Sous l'onglet **Modèles** , l'attribut Subcategory a l'autorisation **Mise à jour** attribuée.  
  
 Sous l’onglet **Membres de hiérarchie** , le nœud de sous-catégorie Mountain Bikes dans une hiérarchie dérivée a l’autorisation **Lecture** attribuée explicitement.  
  
 Résultat : dans **Explorateur**, l’utilisateur a l’autorisation **Lecture** sur les valeurs d’attribut Subcategory des membres qui se trouvent dans le nœud Mountain Bikes. Tous les autres membres et attributs sont masqués.  
  
 ![mds_conc_overlap_model_example_2](../master-data-services/media/mds-conc-overlap-model-example-2.gif "mds_conc_overlap_model_example_2")  
  
## <a name="example-3"></a>Exemple 3  
 ![mds_conc_overlap_model_3](../master-data-services/media/mds-conc-overlap-model-3.gif "mds_conc_overlap_model_3")  
  
 Sous l’onglet **Modèles** , l’attribut Subcategory se voit attribuer l’autorisation **Lecture** .  
  
 Sous l'onglet **Membres de hiérarchie** , le nœud de sous-catégorie Mountain Bikes dans une hiérarchie dérivée a l'autorisation **Mise à jour** attribuée explicitement.  
  
 Résultat : dans l’ **Explorateur**, l’utilisateur a l’autorisation **Lecture** sur les valeurs d’attribut. Tous les autres membres et attributs sont masqués.  
  
 ![mds_conc_overlap_model_example_2](../master-data-services/media/mds-conc-overlap-model-example-2.gif "mds_conc_overlap_model_example_2")  
  
## <a name="see-also"></a>Voir aussi  
 [Mode de détermination des autorisations &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)   
 [Chevauchement des autorisations d’accès &#40;Master Data Services&#41;](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md)  
  
  
