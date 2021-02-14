---
description: Autorisations des membres de la hiérarchie (Master Data Services)
title: Autorisations des membres de la hiérarchie
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- members [Master Data Services], permissions
- permissions [Master Data Services], members
ms.assetid: b3880eed-1bf6-4f65-ab23-b08c194cc858
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 58c354eae5dadcc34d160b756e936dd82c3844e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347740"
---
# <a name="hierarchy-member-permissions-master-data-services"></a>Autorisations des membres de la hiérarchie (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Les autorisations des membres de la hiérarchie sont optionnelles et doivent être utilisées uniquement lorsque vous souhaitez qu'un utilisateur ait un accès limité à des membres spécifiques. Si vous n'affectez pas d'autorisations sous l'onglet **Membres de hiérarchie** , les autorisations de l'utilisateur sont basées uniquement sur celles affectées sous l'onglet **Modèles** .  
  
 Les autorisations des membres de la hiérarchie sont affectées dans l' [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] interface utilisateur, dans la zone fonctionnelle **autorisations d’accès** sous l’onglet **membres de hiérarchie** . Ces autorisations déterminent les membres auxquels un utilisateur peut accéder dans la zone fonctionnelle **Explorateur** de l’interface utilisateur.  
  
 Sous l'onglet **Membres de hiérarchie** , chaque hiérarchie est représentée comme une arborescence. Lorsque vous affectez une autorisation à un nœud dans l'arborescence, tous les enfants héritent de cette autorisation à moins qu'elle ne soit affectée explicitement à un niveau inférieur.  
  
> [!NOTE]  
>  Lorsque vous affectez une autorisation à un nœud dans une hiérarchie, tous les membres des autres nœuds au même niveau ou à un niveau supérieur sont implicitement refusés.  
  
 Dans l' **Explorateur**, les autorisations des membres sont appliquées partout où le membre est affiché. Par exemple, un membre doté de l’autorisation **Lire** peut lire toutes les entités, hiérarchies et collections auxquelles il appartient.  
  
 Les autorisations des membres de la hiérarchie s'appliquent à la version de modèle à laquelle vous les affectez ainsi qu'à toutes les futures copies de la version. Elles ne s'appliquent pas aux versions antérieures à celle à laquelle vous les affectez.  
  
|Autorisation|Description|  
|----------------|-----------------|  
|**Lire**|Les membres sont affichés.<br /><br /> <br /><br /> Remarque : si vous affectez uniquement l’autorisation **Lire** à **Racine**, les membres sous **Racine** sont en lecture seule. Toutefois, dans les collections et hiérarchies explicites, l’utilisateur peut déplacer des membres vers **Racine** et ajouter de nouveaux membres à **Racine**.|  
|**Créer**|L’autorisation Créer n’est pas disponible dans les autorisations des membres de la hiérarchie.|  
|**Mettre à jour**|Les membres sont affichés et l'utilisateur peut les modifier. L'utilisateur peut également déplacer les membres dans les collections ou hiérarchies explicites auxquelles les membres appartiennent.|  
|**Supprimer**|Les membres sont affichés, et l’utilisateur peut les modifier.|  
|**Deny**|Les membres ne sont pas affichés.|  
  
 Sous l'onglet **Membres de hiérarchie** , les autorisations que vous affectez n'entrent pas immédiatement en vigueur. La fréquence à laquelle les autorisations sont appliquées dépend du paramètre **Intervalle de traitement de la sécurité des membres** dans la table Paramètres système de la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Vous pouvez appliquer immédiatement des autorisations de membre en suivant la procédure décrite dans [Appliquer immédiatement des autorisations de membre &#40;Master Data Services&#41;](../master-data-services/immediately-apply-member-permissions-master-data-services.md).  
  
> [!NOTE]  
>  Vous ne pouvez pas affecter d'autorisations de membre de hiérarchie à des hiérarchies récursives, des hiérarchies dérivées avec un niveau supérieur composé d'une hiérarchie explicite et des hiérarchies dérivées avec des niveaux masqués.  
  
## <a name="possible-overlapping-permissions"></a>Possibilité de chevauchement des autorisations  
 Lorsque vous affectez une autorisation aux membres, vous devrez peut-être résoudre des problèmes de chevauchement des autorisations.  
  
### <a name="when-a-member-belongs-to-multiple-hierarchies"></a>Lorsqu'un membre appartient à plusieurs hiérarchies  
 Plusieurs hiérarchies peuvent contenir le même membre.  
  
-   Si l’autorisation **Mettre à jour** est affectée à un nœud de hiérarchie et que l’autorisation **Lire** est affectée à un autre nœud, les membres du nœud disposent de l’autorisation **Lire**.  
  
-   Si les autorisations **Mettre à jour** et **Créer** sont affectées à un nœud de hiérarchie, et que les autorisations **Mettre à jour** et **Supprimer** sont attribuées à un autre nœud, les membres du nœud peuvent être mis à jour.  
  
-   Si un nœud de hiérarchie est affecté à n’importe quelle combinaison d’autorisations **Create** /  / **Update Update** / **Delete** et qu’un autre nœud reçoit les autorisations **Deny** , l’accès aux membres du nœud est refusé.  
  
## <a name="external-resources"></a>Ressources externes  
 Billet de blog, [Améliorations de sécurité](/archive/blogs/e7/improvements-to-autoplay), sur msdn.com.  
  
## <a name="see-also"></a>Voir aussi  
 [Affecter des autorisations de membre de hiérarchie &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)   
 [Mode de détermination des autorisations &#40;Master Data Services&#41;](../master-data-services/how-permissions-are-determined-master-data-services.md)   
 [Membres &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Hiérarchies &#40;Master Data Services&#41;](../master-data-services/hierarchies-master-data-services.md)   
 [Appliquer immédiatement des autorisations de membre &#40;Master Data Services&#41;](../master-data-services/immediately-apply-member-permissions-master-data-services.md)  
  
