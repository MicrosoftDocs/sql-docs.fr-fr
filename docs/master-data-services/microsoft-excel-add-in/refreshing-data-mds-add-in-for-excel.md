---
description: Actualisation des données (Complément MDS pour Excel)
title: Actualisation des données
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 58dbe99a-288d-4f1c-9cd5-704d6836c945
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e0661048822933b8b49274d7addc40cec130e289
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343189"
---
# <a name="refreshing-data-mds-add-in-for-excel"></a>Actualisation des données (Complément MDS pour Excel)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans le [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], actualisez les données lorsque vous souhaitez obtenir les dernières informations du référentiel MDS sans ouvrir une nouvelle feuille de calcul. Vous pouvez actualiser toutes les cellules ou uniquement une sélection de cellules. Cela peut être utile lorsque vous avez inséré des colonnes avec des formules personnalisées ou d'autres données non managées par MDS et que vous souhaitez les conserver.  
  
## <a name="when-you-can-refresh-mds-managed-data"></a>Actualisation des données managées MDS  
 Vous pouvez actualiser les données managées MDS dans une feuille de calcul active si la feuille de calcul contient déjà des données managées MDS. Si vous avez modifié les valeurs d'attribut ou ajouté des membres à la feuille de calcul, vous devez publier vos modifications avant de pouvoir actualiser.  
  
## <a name="refreshing-a-selection"></a>Actualisation d'une sélection  
 Vous avez la possibilité d'actualiser toutes les cellules ou uniquement les cellules sélectionnées. Les cellules sélectionnées doivent être contigües. Si un ensemble de cellules contiguës est sélectionné, toutes les cellules managées MDS appartenant à cet ensemble seront mises à jour pour afficher les valeurs actuellement stockées sur le serveur. Les lignes et colonnes non modifiées qui ne sont pas managées par MDS ne sont pas affectées.  
  
## <a name="what-happens-when-you-refresh-mds-managed-data"></a>Que se produit-il lorsque vous actualisez les données managées MDS ?  
 Lorsque vous actualisez les données dans le [!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], ce qui arrive aux données gérées par MDS dans la feuille dépend de ce qui a changé dans le référentiel MDS depuis le dernier chargement des données.  
  
-   Si de nouveaux membres ont été ajoutés au référentiel, ils sont ajoutés à la fin de la feuille de calcul et sont mis en surbrillance en vert.  
  
-   Si des membres ont été supprimés du référentiel, ils sont supprimés de la feuille de calcul.  
  
-   Si une valeur d'attribut a changé dans le référentiel MDS, la valeur dans la feuille de calcul est mise à jour avec la valeur du référentiel MDS. La couleur de cellule ne change pas.  
  
> [!WARNING]
>  -   Dans la feuille de calcul active, si des données non managées MDS existent dans des lignes sous des données managées MDS, les données non managées peuvent être remplacées. Cela se produit lorsque vous actualisez la feuille et de nouvelles lignes de données managées MDS sont ajoutées, se chevauchant avec des données non managées.  
> -   Lorsque vous actualisez, les commentaires sur les cellules managées MDS sont supprimés.  
  
## <a name="how-to-refresh-mds-managed-data"></a>Actualiser des données managées MDS  
 Dans le groupe **Se connecter et charger** du ruban, le bouton **Actualiser** propose deux options, **Actualiser tout** et **Actualiser la sélection**. L’action par défaut du bouton du ruban est **Actualiser tout**. Pour actualiser la feuille entière avec les valeurs du serveur, cliquez sur le bouton **Actualiser** ou choisissez l’option **Actualiser tout** . Pour actualiser uniquement certaines cellules d’une feuille, sélectionnez les cellules (contiguës) et choisissez l’option **Actualiser la sélection** .  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Créez une connexion à une base de données [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] .|[Se connecter à un référentiel MDS &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/connect-to-an-mds-repository-mds-add-in-for-excel.md)|  
|Chargez des données MDS dans Excel.|[Export Data to Excel from Master Data Services](../../master-data-services/microsoft-excel-add-in/export-data-to-excel-from-master-data-services.md)|  
  
## <a name="related-content"></a>Contenu associé  
  
-   [Connexions &#40;Complément MDS pour Excel#41;](../../master-data-services/microsoft-excel-add-in/connections-mds-add-in-for-excel.md)  
  
-   [Vue d’ensemble : exportation de données vers Excel &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-exporting-data-to-excel-mds-add-in-for-excel.md)  
  
-   [Complément Master Data Services pour Microsoft Excel](../../master-data-services/microsoft-excel-add-in/master-data-services-add-in-for-microsoft-excel.md)  
  
  
