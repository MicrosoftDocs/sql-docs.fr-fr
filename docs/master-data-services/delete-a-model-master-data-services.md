---
description: Supprimer un modèle (Master Data Services)
title: Supprimer un modèle
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting models [Master Data Services]
- models [Master Data Services], deleting models
ms.assetid: f0ad3cc4-aed7-47c8-94bc-2971fe9fe871
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 4e7465c52f6141e0eb78cc01c74d338c688d1c7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338996"
---
# <a name="delete-a-model-master-data-services"></a>Supprimer un modèle (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Supprimez un modèle afin de supprimer le modèle et toutes ses données de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].  
  
> [!NOTE]  
>  Lorsque vous effectuez cette procédure, tous les objets et toutes les données de toutes les versions du modèle sont supprimés définitivement.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-delete-a-model"></a>Pour supprimer un modèle  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la page **Vue du modèle** , dans la barre de menus, pointez sur **Gérer** , puis cliquez sur **Modèles**.  
  
3.  Dans la grille de la page **Gérer les modèles** , sélectionnez la ligne du modèle à supprimer.  
  
4.  Cliquez sur **Supprimer**.  
  
5.  Dans la boîte de dialogue de confirmation, cliquez sur **OK**.  
  
6.  Dans la boîte de dialogue de confirmation supplémentaire, cliquez sur **OK**.  
  
 La colonne **État** de la grille affiche l’état de l’opération sur le modèle. Lorsque vous cliquez sur le bouton **enregistrer le modèle** , l’image de ![mise à jour](../master-data-services/media/mds-model-status-updating.png "Mise à jour") s’affiche, indiquant que le modèle est en mode de mise à jour. Si des erreurs se produisent lors de la création ou de la modification d’un modèle, l’image d' ![erreur](../master-data-services/media/mds-model-status-error.png "Erreur") s’affiche. Dans le cas contraire, l’état est OK et l’image ![OK](../master-data-services/media/mds-model-status-ok.png "Ok") apparaît.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèles &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)   
 [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)  
  
  
