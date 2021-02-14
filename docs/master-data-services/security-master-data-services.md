---
title: Sécurité
description: En savoir plus sur la sécurité dans Master Data Services, notamment les types d’utilisateurs, la définition de la sécurité, la sécurité dans le complément pour Excel et les tâches associées.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 56bc41ea-de28-4184-aa7e-99111ae55af5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 97209bd6d906a73f6199f06a09e14e1ab4d0ac22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352996"
---
# <a name="security-master-data-services"></a>Sécurité (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Utilisez la sécurité dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]pour vous assurer que les utilisateurs ont accès aux données de référence spécifiques nécessaires pour réaliser leur travail, et les empêcher d'accéder aux données qui ne doivent pas leur être accessibles.  
  
 Vous pouvez également utiliser la sécurité pour attribuer à un utilisateur le rôle d'administrateur d'un modèle et d'une zone fonctionnelle spécifiques (par exemple, pour lui permettre de créer des versions du modèle Customer ou pour lui donner la possibilité de définir des autorisations de sécurité).  
  
 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] est basée sur des utilisateurs et des groupes de domaine local ou Active Directory. La sécurité MDS vous permet d'utiliser un niveau de détail granulaire pour déterminer les données auxquelles un utilisateur peut accéder. En raison de sa granularité, la sécurité peut rapidement se compliquer et vous devez être prudent lorsque vous utilisez le chevauchement des utilisateurs et des groupes. Pour plus d’informations, consultez [Chevauchement des autorisations d’accès &#40;Master Data Services&#41;](../master-data-services/overlapping-user-and-group-permissions-master-data-services.md).  
  
 Vous pouvez attribuer un accès de sécurité dans la zone fonctionnelle **Autorisations d'accès** de l'application Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] ou à l'aide du service Web.  
  
## <a name="types-of-users"></a>Types d'utilisateurs  
 Il existe deux types d'utilisateurs dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]:  
  
-   ceux qui accèdent aux données dans la zone fonctionnelle **Explorateur** ;  
  
-   ceux qui ont la possibilité d'effectuer des tâches d'administration dans des zones autres que la zone **Explorateur**. Ces utilisateurs sont appelés [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
## <a name="how-to-set-security"></a>Comment définir la sécurité  
 Pour accorder à un utilisateur ou un groupe l'autorisation d'accéder à des données ou des fonctionnalités dans MDS, vous devez attribuer les droits suivants :  
  
-   [Accès à la zone fonctionnelle](../master-data-services/functional-area-permissions-master-data-services.md), qui détermine laquelle des cinq zones fonctionnelles de l'interface utilisateur est accessible à l'utilisateur.  
  
-   [Autorisations d’objet de modèle](../master-data-services/model-object-permissions-master-data-services.md), qui déterminent les attributs auxquels un utilisateur peut accéder, ainsi que le type d’accès (Lire, Créer et Mettre à jour) dont dispose l’utilisateur sur ces attributs. L’utilisateur peut également affecter des autorisations d’administrateur explicites au niveau du modèle.  
  
-   (Facultatif) [Autorisations des membres de la hiérarchie](../master-data-services/hierarchy-member-permissions-master-data-services.md), qui déterminent les membres auxquels un utilisateur peut accéder, ainsi que le type d’accès (Lire, Mettre à jour et Supprimer) dont l’utilisateur dispose sur ces membres.  
  
 Lorsque vous affectez des autorisations à des attributs et des membres, ces autorisations se croisent et des règles déterminent leur ordre de priorité. Pour plus d’informations, consultez [Mode de détermination des autorisations &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md).  
  
## <a name="security-in-the-add-in-for-excel"></a>Sécurité du complément pour Excel  
 La sécurité définie dans l'application Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] s'applique également à [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)]. Les utilisateurs peuvent uniquement afficher et utiliser les données pour lesquelles ils ont l'autorisation nécessaire. Les administrateurs peuvent effectuer les tâches d'administration.  
  
 La seule mise en garde est que toute la sécurité affectée dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] ne s'applique pas à Excel tant qu'un intervalle de 20 minutes ne s'est pas écoulé. L'intervalle est défini par le paramètre *MdsMaximumUserInformationCacheInterval* du fichier web.config. Pour modifier l'intervalle, vous pouvez modifier le paramètre et redémarrer IIS.  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créer un utilisateur disposant d'une autorisation totale sur un modèle.|[Créer un administrateur de modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-administrator-master-data-services.md)|  
|Ajouter un groupe Active Directory à [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]; il s'agit de la première étape pour accorder à un groupe l'autorisation d'accéder aux données dans l'application Web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Ajouter un groupe &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)|  
|Accorder une autorisation d'accès à une zone fonctionnelle de l'application Web [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Affecter des autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/assign-functional-area-permissions-master-data-services.md)|  
|Accorder une autorisation d'accès aux valeurs d'attribut en accordant une autorisation d'accès aux objets de modèle.|[Affecter des autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)|  
|Accorder une autorisation d'accès aux valeurs de membre en accordant une autorisation d'accès aux nœuds de la hiérarchie.|[Affecter des autorisations de membre de hiérarchie &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [Utilisateurs et groupes &#40;Master Data Services&#41;](../master-data-services/users-and-groups-master-data-services.md)   
 [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md)   
 [Autorisations d’objet de modèle &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Autorisations des membres de la hiérarchie &#40;Master Data Services&#41;](../master-data-services/hierarchy-member-permissions-master-data-services.md)   
 [Mode de détermination des autorisations &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)  
  
  
