---
description: Créer une hiérarchie explicite (Master Data Services)
title: Créer une hiérarchie explicite
ms.custom: ''
ms.date: 04/01/2016
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating explicit hierarchies [Master Data Services]
- explicit hierarchies, creating
ms.assetid: ba768393-6990-4eda-8cb0-d58cb3cfc2e2
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 04cec56f41b83b011e4c9514d078dada7ddbd402
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351913"
---
# <a name="create-an-explicit-hierarchy-master-data-services"></a>Créer une hiérarchie explicite (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], créez une hiérarchie explicite lorsque vous avez besoin d'une hiérarchie déséquilibrée dans laquelle les membres peuvent exister à tous les niveaux. Les hiérarchies explicites contiennent des membres d'une entité unique.  
  
 Après avoir créé une hiérarchie explicite, vous pouvez lui ajouter des membres dans la zone fonctionnelle **Explorateur** .  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   L'entité doit être activée pour les hiérarchies et collections explicites.  
  
### <a name="to-create-an-explicit-hierarchy"></a>Pour créer une hiérarchie explicite  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Sur la page **gérer le modèle** , sélectionnez un modèle dans la grille, puis cliquez sur **entités**.  
  
3.  Dans la grille de la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez créer une hiérarchie explicite.  
  
4.  Cliquez sur **Hiérarchies explicites**.  
  
5.  Dans la page **Gérer la hiérarchie explicite** , cliquez sur **Ajouter**.  
  
6.  Dans la zone **Nom** , tapez un nom pour la hiérarchie.  
  
7.  Décochez éventuellement la case **Hiérarchie obligatoire** pour créer la hiérarchie comme une hiérarchie non obligatoire. Pour plus d’informations sur les types de hiérarchies, consultez [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md).  
  
8.  Cliquez sur **Save**.  
  
## <a name="grid-columns"></a>Colonnes de la grille  
 Pour chaque hiérarchie explicite que vous créez, une ligne comportant sept colonnes est ajoutée à la grille. Ces différentes colonnes sont décrites ci-après.  
  
|Nom|Description|  
|----------|-----------------|  
|Statut|État de l’entité. Lorsque vous cliquez sur **Enregistrer** , l’image ci-après s’affiche pour indiquer que l’entité est en cours de mise à jour.<br /><br /> ![Icône de mise à jour de l’État](../master-data-services/media/mds-statusicon-updating.png "Icône de mise à jour de l’État")<br /><br /> En cas d’erreur lors de la création ou de la modification d’une entité, l’image suivante apparaît.<br /><br /> ![Icône d’état d’erreur](../master-data-services/media/mds-statusicon-error.png "Icône d’état d’erreur")<br /><br /> Si l’état présente la valeur OK, l’image ci-dessous s’affiche.<br /><br /> ![Icône d’état OK](../master-data-services/media/mds-statusicon-ok.png "Icône d’état OK")|  
|Nom|Nom de hiérarchie explicite.|  
|Est obligatoire|Indique si la hiérarchie explicite est ou non obligatoire.|  
|Created By|Nom de l’utilisateur ayant créé la hiérarchie explicite.|  
|Créée le|Date et heure de création de la hiérarchie explicite.|  
|Mise à jour par|Nom de l’utilisateur ayant effectué la dernière mise à jour de la hiérarchie explicite.|  
|Updated On|Date et heure de la dernière mise à jour de la hiérarchie explicite.|  
  
## <a name="next-steps"></a>Étapes suivantes  
  
-   [Créer un membre consolidé &#40;Master Data Services&#41;](../master-data-services/create-a-consolidated-member-master-data-services.md)  
  
  
  
## <a name="see-also"></a>Voir aussi  
 [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Hiérarchies dérivées avec des majuscules &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-with-explicit-caps-master-data-services.md)   
 [Modifier le nom d’une hiérarchie explicite &#40;Master Data Services&#41;](../master-data-services/change-an-explicit-hierarchy-name-master-data-services.md)  
  
  

