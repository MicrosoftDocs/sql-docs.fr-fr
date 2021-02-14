---
description: Modifier une entité (Master Data Services)
title: Modifier une entité
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- entities [Master Data Services], changing name
ms.assetid: 6a5b9f14-6dfc-49d7-a771-e96461d4feae
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 55cc510ecfb781d1d4366d9df928223139116fde
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341744"
---
# <a name="edit-an-entity-master-data-services"></a>Modifier une entité (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], vous pouvez modifier une entité.  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Administration de système** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-edit-an-entity"></a>Pour modifier une entité  
  
1.  Dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], cliquez sur **Administration de système**.  
  
2.  Sur la page **gérer le modèle** , sélectionnez un modèle dans la grille, puis cliquez sur **entités**.  
  
3.  Dans la grille de la page **Gérer l’entité** , sélectionnez la ligne de l’entité à modifier, puis cliquez sur **Modifier**.  
  
4.  Dans la zone **Nom** , tapez le nom mis à jour de l’entité.  
  
5.  Dans le champ **Description** , tapez la description mise à jour de l’entité.  
  
6.  Dans le champ **Nom des tables intermédiaires** , tapez un nom mis à jour pour la table de mise en lots.  
  
7.  Pour le champ **Type du journal des transactions** , choisissez le type mis à jour du journal des transactions dans la liste déroulante.  
  
     Pour plus d’informations, consultez [Modifier le type du journal des transactions de l’entité &#40;Master Data Services&#41;](../master-data-services/change-the-entity-transaction-log-type-master-data-services.md).  
  
8.  Cochez ou décochez la case **Créer automatiquement des valeurs de code** .  
  
     Pour plus d’informations, consultez [Création automatique de code &#40;Master Data Services&#41;](../master-data-services/automatic-code-creation-master-data-services.md).  
  
9. Cochez la case **Autoriser la compression des données** . La compression de ligne est activée par défaut.  
  
     Pour plus d’informations, consultez [compression des données](../relational-databases/data-compression/data-compression.md) .  
  
## <a name="status"></a>Statut  
 La colonne d’état de la grille affiche l’état de l’opération sur l’entité. Lorsque vous cliquez sur **Enregistrer l’entité**, l’image ci-après s’affiche pour indiquer que l’entité est en cours de mise à jour.  
  
 ![Icône de mise à jour de l’État](../master-data-services/media/mds-statusicon-updating.png "Icône de mise à jour de l’État")  
  
 En cas d’erreur lors de la création ou de la modification d’une entité, l’image suivante apparaît.  
  
 ![Icône d’état d’erreur](../master-data-services/media/mds-statusicon-error.png "Icône d’état d’erreur")  
  
 Si l’état présente la valeur OK, l’image suivante apparaît.  
  
 ![Icône d’état OK](../master-data-services/media/mds-statusicon-ok.png "Icône d’état OK")  
  
## <a name="see-also"></a>Voir aussi  
 [Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)   
 [Supprimer une entité &#40;Master Data Services&#41;](../master-data-services/delete-an-entity-master-data-services.md)   
 [Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)  
  
  
