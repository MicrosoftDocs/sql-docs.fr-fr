---
description: Réorganiser des colonnes (Complément MDS pour Excel)
title: Réorganiser des colonnes
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: ac00462e-c0f7-4b8d-86f2-d9eda2598a15
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 93d59286f93f890540f39a6e974dee4a2b5ecf31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355351"
---
# <a name="reorder-columns-mds-add-in-for-excel"></a>Réorganiser des colonnes (Complément MDS pour Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], vous pouvez réorganiser les colonnes en filtrant la liste avant le chargement.  
  
 Lorsque vous réorganisez les attributs dans la boîte de dialogue **Filtre** , les données sont chargées dans Excel dans le nouvel ordre. Toutefois, lors du prochain filtrage des données d'attribut, l'ordre rétabli sera celui de la conception d'origine. Pour modifier définitivement l'ordre, un administrateur doit modifier l'ordre dans la zone **Administration de système** de Master Data Manager. Pour plus d’informations, voir [Change the Order of Attributes](../../master-data-services/change-the-order-of-attributes.md).  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder à la zone fonctionnelle **Explorateur** .  
  
### <a name="to-reorder-mds-managed-columns"></a>Pour réorganiser les colonnes managées MDS  
  
1.  Ouvrez Excel et accédez à l'onglet **Données de référence** , puis connectez-vous à un référentiel MDS. Pour plus d’informations, consultez [Se connecter à un référentiel MDS &#40;complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md).  
  
2.  Dans le volet **Explorateur de données de référence** , sélectionnez un modèle et une version. La liste des entités est remplie.  
  
    -   Si le volet **Explorateur de données de référence** n'est pas visible, dans le groupe **Se connecter et charger** , cliquez sur **Afficher l'Explorateur**.  
  
    -   Si le volet **Explorateur de données de référence** est désactivé, cela signifie que la feuille de calcul existante contient déjà des données managées MDS. Pour activer le volet, ouvrez une nouvelle feuille de calcul.  
  
3.  Dans le volet **Explorateur de données de référence** , cliquez sur une entité.  
  
4.  Dans le groupe **Se connecter et charger** , cliquez sur **Filtrer**.  
  
5.  Dans la boîte de dialogue **Filtrer** , dans la section **Colonnes** , dans la liste d'attributs, cliquez sur l'attribut à déplacer.  
  
6.  À droite de la liste, cliquez sur la flèche **Haut** ou **Bas** pour déplacer l'attribut à gauche et à droite dans la feuille de calcul.  
  
7.  Répétez l'étape 7 pour chaque attribut jusqu'à ce que l'ordre de haut en bas représente l'ordre de gauche à droite que vous souhaitez dans la feuille de calcul.  
  
8.  Cliquez sur **Charger les données**. La feuille est renseignée avec les données managées MDS et les colonnes sont affichées dans l'ordre que vous avez spécifié.  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble : exportation de données vers Excel &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
  
