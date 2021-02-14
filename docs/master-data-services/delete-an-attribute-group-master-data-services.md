---
description: Supprimer un groupe d'attributs (Master Data Services)
title: Supprimer un groupe d'attributs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting attribute groups [Master Data Services]
- attribute groups [Master Data Services], deleting
ms.assetid: f915e89b-629d-4725-aea6-a7f051978244
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c1331d00c0c39cbe3a82f4372aaed38a315dc3c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338982"
---
# <a name="delete-an-attribute-group-master-data-services"></a>Supprimer un groupe d'attributs (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], supprimez un groupe d'attributs lorsque vous n'avez plus besoin de l'onglet à afficher dans la zone fonctionnelle **Explorateur** de [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
-   **Remarque** Lorsque les groupes d'attributs existent, les attributs qui n'appartiennent pas à un groupe d'attributs ne sont pas affichés dans l' **Explorateur**. Lorsqu'aucun groupe d'attributs n'existe, tous les attributs sont affichés.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-delete-an-attribute-group"></a>Pour supprimer un groupe d'attributs  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Sur la page **gérer le modèle** , sélectionnez un modèle dans la grille, puis cliquez sur **entités**.  
  
3.  Dans la grille de la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez modifier un groupe d’attributs.  
  
4.  Cliquez sur **Groupes d’attributs**.  
  
5.  Dans la page **Gérer les groupes d’attributs** , sélectionnez le type de membre dans la liste déroulante **Types de membres** pour développer **Feuille**, **Consolidé** ou **Collection**, selon le type de groupe que vous voulez supprimer.  
  
6.  Cliquez sur le groupe d'attributs à supprimer.  
  
7.  Cliquez sur **Supprimer**.  
  
8.  Dans la boîte de dialogue de confirmation, cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Groupes d’attributs &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)   
 [Créer un groupe d’attributs &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md)  
  
  
