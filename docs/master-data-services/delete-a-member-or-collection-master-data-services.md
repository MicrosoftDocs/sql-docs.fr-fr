---
description: Supprimer un membre ou une collection (Master Data Services)
title: Supprimer un membre ou une collection
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- collections [Master Data Services], deleting
- leaf members [Master Data Services], deleting
- deleting members [Master Data Services]
- members [Master Data Services], deleting
- consolidated members [Master Data Services], deleting
ms.assetid: 519130a7-4226-4d71-9124-d2ee0ce7e5bd
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b8810bb155896b4935163bb1143d535faddb1dad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339003"
---
# <a name="delete-a-member-or-collection-master-data-services"></a>Supprimer un membre ou une collection (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], supprimez un membre ou une collection lorsque vous n'en avez plus besoin. Si vous souhaitez supprimer des membres en bloc, utilisez plutôt les tables de mise en lots. Pour plus d’informations, consultez [importer des données à partir de Tables &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md)  
  
> [!NOTE]  
>  Vous ne pouvez pas supprimer un membre s'il est utilisé comme une valeur d'attribut basée sur un domaine pour un autre membre.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Explorateur** .  
  
-   Pour les membres, vous devez au minimum disposer de l’autorisation **Supprimer** sur l’objet modèle feuille à partir duquel vous supprimez un membre.  
  
-   Pour les collections, vous devez avoir au minimum l'autorisation **Mettre à jour** sur l'objet modèle de collection feuille que vous supprimez.  
  
### <a name="to-delete-a-member-or-collection"></a>Pour supprimer un membre ou une collection  
  
1.  Dans la page d'accueil de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , dans la liste **Modèle** , sélectionnez un modèle.  
  
2.  Dans la liste **Version** , sélectionnez une version.  
  
3.  Cliquez sur **Explorateur**.  
  
4.  Pour supprimer les éléments suivants, procédez comme suit :  
  
    -   Pour un membre feuille, dans la barre de menus, pointez sur **Entités** , puis cliquez sur le nom de l'entité qui contient le membre.  
  
    -   Pour un membre consolidé, dans la barre de menus, pointez sur **Hiérarchies** , puis cliquez sur le nom de la hiérarchie qui contient le membre. Cliquez sur le nœud de la hiérarchie qui contient le membre.  
  
    -   Pour une collection, dans la barre de menus, pointez sur **Collections** , puis cliquez sur le nom de l'entité qui contient la collection.  
  
5.  Dans la grille, cliquez sur la ligne correspondant au membre ou à la collection à supprimer.  
  
6.  Cliquez sur **Supprimer un membre**, sur **Supprimer** ou sur **Supprimer une collection**.  
  
7.  Les administrateurs d’entité verront également apparaître une option de menu permettant de purger (supprimer définitivement) tous les membres supprimés de manière réversible dans la version d’entité.  
  
8.  Dans la boîte de dialogue de confirmation, cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Réactiver un membre ou une collection &#40;Master Data Services&#41;](../master-data-services/reactivate-a-member-or-collection-master-data-services.md)   
 [Membres &#40;Master Data Services&#41;](../master-data-services/members-master-data-services.md)   
 [Collections &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)  
  
  
