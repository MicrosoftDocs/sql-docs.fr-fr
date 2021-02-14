---
title: Chevauchement des autorisations d'accès
description: Découvrez comment les autorisations d’appartenance à un groupe et les autorisations affectées aux utilisateurs interagissent avec les onglets modèles et membres de hiérarchie dans Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- users [Master Data Services], resolving permissions
- permissions [Master Data Services], user and group overlaps
- groups [Master Data Services], resolving permissions
ms.assetid: 31c3cf7d-17d4-4474-b6a7-ffcb9fc45b37
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9df096d687fbd0ea2730c328ee3a4d25b062967f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340267"
---
# <a name="overlapping-user-and-group-permissions-master-data-services"></a>Chevauchement des autorisations d'accès (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Les autorisations d'un utilisateur sont basées sur les :  
  
-   autorisations de l'appartenance aux groupes ;  
  
-   autorisations affectées explicitement à l'utilisateur.  
  
 Si un utilisateur appartient à plusieurs groupes, et que ces groupes ont accès à [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], les règles suivantes s’appliquent :  
  
-   **Refuser** remplace toutes les autres autorisations. Si l’autorisation associée à l’objet est **Refuser** dans un groupe, l’autorisation effective est également Refuser.  
  
-   Une autorisation d’accès correspond à l’association de toutes les autorisations effectives d’un groupe. Si l’autorisation associée à l’objet est **Créer** dans un groupe et **Mettre à jour** dans un autre, l’autorisation effective correspond à **Créer** et **Mettre à jour**.  
  
 Ces règles s'appliquent à la fois à l'onglet **Modèles** et à l'onglet **Membres de hiérarchie** . Les autorisations sont résolues pour chaque onglet, puis combinées. Pour plus d’informations, consultez [Mode de détermination des autorisations &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md).  
  
> [!NOTE]  
>  Vous pouvez afficher la résolution du chevauchement des autorisations d'accès dans l'interface utilisateur. Les onglets **Modèles** et **Membres de hiérarchie** ont tous deux une liste déroulante dans laquelle vous pouvez sélectionner **Effectives** pour afficher les autorisations effectives.  
  
## <a name="example-1"></a>Exemple 1  
 ![mds_conc_user_group_ex_1](../master-data-services/media/mds-conc-user-group-ex-1.gif "mds_conc_user_group_ex_1")  
  
 L'utilisateur appartient au Groupe 1 et au Groupe 2.  
  
 L’utilisateur a l’autorisation **Lecture** sur l’entité Product.  
  
 Groupe 1 a l'autorisation **Mise à jour** sur l'entité Product.  
  
 Groupe 2 a l’autorisation **Lecture** sur l’entité Product.  
  
 Résultat : l'autorisation effective de l'utilisateur est **Mise à jour** sur l'entité Product.  
  
## <a name="example-2"></a>Exemple 2  
 ![mds_conc_user_group_ex_2](../master-data-services/media/mds-conc-user-group-ex-2.gif "mds_conc_user_group_ex_2")  
  
 L'utilisateur appartient au Groupe 1 et au Groupe 2.  
  
 L’utilisateur a l’autorisation **Lecture** sur l’entité Product.  
  
 Groupe 1 a l'autorisation **Mise à jour** sur l'entité Product.  
  
 Groupe 2 a l'autorisation **Refuser** sur l'entité Product.  
  
 Résultat : l'autorisation effective de l'utilisateur est **Refuser** sur l'entité Product.  
  
## <a name="example-3"></a>Exemple 3  
 ![mds_conc_user_group_ex_3](../master-data-services/media/mds-conc-user-group-ex-3.gif "mds_conc_user_group_ex_3")  
  
 L'utilisateur appartient au Groupe 1 et au Groupe 2.  
  
 L'utilisateur a l'autorisation **Mise à jour** sur un groupe de membres dans un nœud de la hiérarchie.  
  
 Groupe 1 a l’autorisation **Lecture** sur un groupe de membres dans un nœud de la hiérarchie.  
  
 Groupe 2 a l’autorisation **Lecture** sur un groupe de membres dans un nœud de la hiérarchie.  
  
 Résultat : l'autorisation effective de l'utilisateur est **Mise à jour** sur les membres.  
  
## <a name="see-also"></a>Voir aussi  
 [Mode de détermination des autorisations &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)   
 [Chevauchement des autorisations de modèle et de membre &#40;Master Data Services&#41;](../master-data-services/overlapping-model-and-member-permissions-master-data-services.md)  
  
  
