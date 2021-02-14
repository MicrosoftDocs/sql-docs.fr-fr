---
description: Modifier le modèle (Master Data Services)
title: Modifier le modèle
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- models [Master Data Services], changing name
ms.assetid: 399eed32-7c61-4239-9c06-996a65219518
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 9831a53f56d831dbc52c5d7309a365723ca938ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341718"
---
# <a name="edit-model-master-data-services"></a>Modifier le modèle (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez modifier le nom et la description d’un modèle et indiquer la durée de conservation des journaux des transactions.  
  
 Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/transactions-master-data-services.md).  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-change-a-model"></a>Pour modifier un modèle  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Dans la page **Vue du modèle** , dans la barre de menus, pointez sur **Gérer** , puis cliquez sur **Modèles**.  
  
3.  Sur la page **Gérer les modèles** , dans la grille, sélectionnez la ligne correspondant au modèle dont vous souhaitez modifier le nom ou la description.  
  
4.  Cliquez sur **Modifier**.  
  
5.  Dans la zone **Nom** , tapez le nom mis à jour du modèle.  
  
6.  Dans le champ **Description** , tapez la description mise à jour du modèle.  
  
7.  Dans le champ **Nombre de jours de rétention du journal** , sélectionnez l’une des options permettant de conserver les données du journal. La valeur par défaut est **Paramètre système**, ce qui indique que la valeur est héritée des paramètres système dans le [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]. Pour plus d’informations, consultez [Paramètres système &#40;Master Data Services&#41;](../master-data-services/system-settings-master-data-services.md).  
  
     Pour remplacer le paramètre système sans supprimer les données du journal des transactions, sélectionnez **NON**. Pour conserver uniquement les données du journal d’aujourd’hui et tronquer les données du journal pour tous les jours précédents, sélectionnez **Oui** et définissez le champ **jours** sur 0. Pour conserver les données du journal pendant un nombre de jours déterminé, sélectionnez **OUI** et réglez le champ **Jours** sur le nombre de jours souhaité.  
  
8.  Cliquez sur **Enregistrer le modèle**.  
  
 La colonne **État** de la grille affiche l’état de l’opération sur le modèle. Lorsque vous cliquez sur le bouton **enregistrer le modèle** , l’image de ![mise à jour](../master-data-services/media/mds-model-status-updating.png "Mise à jour") s’affiche, indiquant que le modèle est en mode de mise à jour. Si des erreurs se produisent lors de la création ou de la modification d’un modèle, l’image d' ![erreur](../master-data-services/media/mds-model-status-error.png "Erreur") s’affiche. Dans le cas contraire, l’état est OK et l’image ![OK](../master-data-services/media/mds-model-status-ok.png "Ok") apparaît.  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)   
 [Supprimer un modèle &#40;Master Data Services&#41;](../master-data-services/delete-a-model-master-data-services.md)   
 [Modèles &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)  
  
  
