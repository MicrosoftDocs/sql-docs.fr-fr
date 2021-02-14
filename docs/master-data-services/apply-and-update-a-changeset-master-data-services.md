---
description: Appliquer et mettre à jour un ensemble de modifications (Master Data Services)
title: Appliquer et mettre à jour un ensemble de modifications
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 3a6a3cf2-1e77-43d3-a64a-855ae51258e7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ad71934e2eca7e7db651859123eb6562565b4bcc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339595"
---
# <a name="apply-and-update-a-changeset-master-data-services"></a>Appliquer et mettre à jour un ensemble de modifications (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Un ensemble de modifications regroupe des modifications en attente, qui portent sur les données de référence. Vous pouvez appliquer cet ensemble en local, afin d’afficher, d’ajouter, de mettre à jour et de supprimer les modifications en attente dans cet ensemble.  
  
## <a name="prerequisites"></a>Prérequis  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Explorateur** . Pour plus d’informations, consultez [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Vous devez disposer au minimum d’un accès en mise à jour à l’entité ou à l’un de ses attributs.  
  
-   Vous pouvez uniquement afficher l’ensemble de modifications qui vous appartient, ou celui qui est soumis à des fins d’approbation lorsque vous administrez l’entité.  
  
-   Vous pouvez uniquement modifier l’ensemble de modifications qui vous appartient, lorsque le statut de cet ensemble est Ouvert ou Rejeté.  
  
## <a name="to-apply-and-update-a-changeset"></a>Pour appliquer et mettre à jour un ensemble de modifications  
  
1.  Sur la page d’accueil [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , sélectionnez le modèle et la version, puis cliquez sur **Explorateur**.  
  
2.  Dans le menu **Entités** , cliquez sur une entité.  
  
3.  Dans le volet droit, sélectionnez **Ensembles de modifications** , puis double-cliquez sur l’ensemble de modifications à afficher ou modifier.  
  
4.  Cliquez sur **Appliquer**.  
  
     Les modifications en attente sont appliquées au membre d’entité dans la grille. Les modifications en attente sont mises en surbrillance.  
  
     La création, la suppression et la mise à jour des membres entraîne l’application des modifications à l’ensemble de modifications.  
  
5.  Pour rétablir les modifications en attente, accédez au volet **Ensembles de modifications** , cliquez avec le bouton droit dans la grille, puis sélectionnez **Rétablir**.  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Valider ou envoyer un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/commit-or-submit-a-changeset-master-data-services.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/create-a-changeset-master-data-services.md)   
 [Approuver ou rejeter un ensemble de modifications &#40;Master Data Services&#41;](../master-data-services/approve-or-reject-a-changeset-master-data-services.md)  
  
  
