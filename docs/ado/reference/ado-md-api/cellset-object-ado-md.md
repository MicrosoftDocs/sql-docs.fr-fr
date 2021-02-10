---
description: Cellset, objet (ADO MD)
title: CellSet, objet (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cellset
helpviewer_keywords:
- Cellset object [ADO MD]
ms.assetid: 5e2452c0-cac0-49b2-8099-836c35794d50
author: rothja
ms.author: jroth
ms.openlocfilehash: dc64756d2b2ac679e185c4dff8888352a7865f83
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055724"
---
# <a name="cellset-object-ado-md"></a>Cellset, objet (ADO MD)
Représente les résultats d’une requête multidimensionnelle. Il s’agit d’une collection de cellules sélectionnées à partir de cubes ou d’autres cellules.  
  
## <a name="remarks"></a>Notes  
 Les données d’un ensemble de **cellules** sont récupérées à l’aide d’un accès direct de type tableau. Vous pouvez accéder à un membre spécifique pour obtenir des informations sur ce membre. Par exemple, le code suivant retourne la légende du premier membre de la première position sur le premier axe d’un CellSet nommé `cst` :  
  
```  
cst.Axes(0).Positions(0).Members(0).Caption  
```  
  
 Il n’existe aucune notion de cellule active dans un CellSet. Au lieu de cela, la propriété [Item](./item-property-ado-md-cellset.md) récupère un objet [Cell](./cell-object-ado-md.md) spécifique du CellSet. Les arguments de la propriété de l' **élément** déterminent la cellule qui est récupérée. Vous pouvez spécifier la valeur ordinale unique d’une cellule. Vous pouvez également récupérer des cellules en utilisant leurs numéros de position le long de chaque axe de l’CellSet. Pour plus d’informations sur la récupération des cellules, consultez la propriété [Item](./item-property-ado-md-cellset.md) .  
  
 Avec les collections, les méthodes et les propriétés d’un objet **Cellset** , vous pouvez effectuer les opérations suivantes :  
  
-   Associez une connexion ouverte à un objet **Cellset** en définissant sa propriété [ActiveConnection](./activeconnection-property-ado-md.md) .  
  
-   Exécutez et récupérez les résultats d’une requête multidimensionnelle à l’aide de la méthode [Open](./open-method-ado-md.md) .  
  
-   Récupérez une **cellule** de l' **Cellset** avec la propriété [Item](./item-property-ado-md-cellset.md) .  
  
-   Retourne les objets [Axis](./axis-object-ado-md.md) qui définissent l’ensemble de **cellules** avec la collection [axes](./axes-collection-ado-md.md) .  
  
-   Récupérez des informations sur les dimensions utilisées pour filtrer les données dans l' **Cellset** avec la propriété [FilterAxis](./filteraxis-property-ado-md.md) .  
  
-   Retourne ou spécifie la requête utilisée pour définir l' **Cellset** avec la propriété [source](./source-property-ado-md.md) .  
  
-   Retourne l’état actuel de l’objet **Cellset** (ouvert, fermé, en cours d’exécution ou en cours de connexion) avec la propriété [State](./state-property-ado-md.md) .  
  
-   Fermez un **Cellset** ouvert avec la méthode [Close](./close-method-ado-md.md) .  
  
-   Récupérez les informations spécifiques au fournisseur relatives à l’ensemble de **cellules** avec la collection de [Propriétés](../ado-api/properties-collection-ado.md) ADO standard.  
  
 Cette section contient la rubrique suivante.  
  
-   [Propriétés, méthodes et événements](./cellset-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [CellSet, exemple (VB)](./cellset-example-vb.md)   
 [Axes, collection (ADO MD)](./axes-collection-ado-md.md)   
 [Cell, objet (ADO MD)](./cell-object-ado-md.md)   
 [Connection, objet (ADO)](../ado-api/connection-object-ado.md)   
 [Properties, collection (ADO)](../ado-api/properties-collection-ado.md)