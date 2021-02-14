---
description: Modifier le type du journal des transactions de l’entité (Master Data Services)
title: Modifier le type du journal des transactions de l’entité
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 75250b32-3384-43c2-9b5c-1607cc3aa7b3
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 87fd757f99f8e7dce79bc8f48c30731a876d3cd9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272830"
---
# <a name="change-the-entity-transaction-log-type-master-data-services"></a>Modifier le type du journal des transactions de l’entité (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Vous pouvez modifier le type du journal des transactions d’une entité afin de lui octroyer la valeur d’attribut, de membre ou aucune valeur.  
  
|Type de journal des transactions|Description|  
|--------------------------|-----------------|  
|Attribut|Les journaux de modification d’entité sont enregistrés au niveau des attributs.<br /><br /> Le journal des transactions est enregistré, comme pour [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)].|  
|Membre|Les journaux de modification d’entité sont enregistrés au niveau de la ligne.<br /><br /> Toute modification de l’attribut entraîne une nouvelle révision de la ligne.<br /><br /> Lorsque vous utilisez le type de journal des transactions de ligne, l’entité est stockée en tant que dimension à variation lente Type 4. La vue d’abonnement Type 2 et la vue d’abonnement Type 4 (Historique) sont prises en charge. Pour plus d’informations, consultez [Formats de vue d’abonnement &#40;Master Data Services&#41;](../master-data-services/subscription-view-formats-master-data-services.md).<br /><br /> Offre de meilleures performances.|  
|Aucun|Aucun journal de modification n’est enregistré.<br /><br /> Offre les meilleures performances.|  
  
## <a name="prerequisites"></a>Prérequis  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l’autorisation d’accéder à la zone fonctionnelle Administration de système. Pour plus d’informations, consultez [Autorisations de zone fonctionnelle &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   L'entité doit exister. Pour plus d’informations, consultez [créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md).  
  
 **Pour modifier le type du journal des transactions**  
  
1.  Dans Master Data Manager, cliquez sur **Administration de système**.  
  
2.  Sur la page **Gérer le modèle** , sélectionnez la ligne du modèle contenant l’entité que vous souhaitez modifier, puis cliquez sur **Entités**.  
  
3.  Sur la page **Gérer l’entité** , sélectionnez la ligne de l’entité pour laquelle vous souhaitez effectuer une mise à jour, puis cliquez sur **Modifier**.  
  
4.  Choisissez le type du journal des transactions dans la liste déroulante.  
  
5.  Cliquez sur **Enregistrer**.  
  
  
