---
description: Count, propriété (ADO)
title: Count, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Collection::Count
helpviewer_keywords:
- Count property [ADO]
ms.assetid: da9ccd1f-d402-41a2-940c-45556fc5340d
author: rothja
ms.author: jroth
ms.openlocfilehash: b478a9f88d33503c5eda98bda11cfe0e0fcc8d6b
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88974520"
---
# <a name="count-property-ado"></a>Count, propriété (ADO)
Indique le nombre d’objets dans une collection.  
  
## <a name="return-value"></a>Valeur renvoyée  
 Retourne une valeur de **type long** .  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **Count** pour déterminer le nombre d’objets dans une collection donnée.  
  
 Étant donné que la numérotation des membres d’une collection commence par zéro, vous devez toujours coder les boucles en commençant par le membre zéro et en terminant par la valeur de la propriété **Count** moins 1. Si vous utilisez Microsoft Visual Basic et souhaitez parcourir les membres d’une collection sans vérifier la propriété Count, utilisez l' **argument** **for each... Commande suivante** .  
  
 Si la propriété **Count** est égale à zéro, la collection ne contient aucun objet.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Axes, collection (ADO MD)](../ado-md-api/axes-collection-ado-md.md)  
        [Columns, collection (ADOX)](../adox-api/columns-collection-adox.md)  
        [CubeDefs, collection (ADO MD)](../ado-md-api/cubedefs-collection-ado-md.md)  
        [Dimensions, collection (ADO MD)](../ado-md-api/dimensions-collection-ado-md.md)  
        [Errors, collection (ADO)](./errors-collection-ado.md)  
        [Fields, collection (ADO)](./fields-collection-ado.md)  
        [Groups, collection (ADOX)](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Hierarchies, collection (ADO MD)](../ado-md-api/hierarchies-collection-ado-md.md)  
        [Indexes, collection (ADOX)](../adox-api/indexes-collection-adox.md)  
        [Keys, collection (ADOX)](../adox-api/keys-collection-adox.md)  
        [Levels, collection (ADO MD)](../ado-md-api/levels-collection-ado-md.md)  
        [Members, collection (ADO MD)](../ado-md-api/members-collection-ado-md.md)  
        [Parameters, collection (ADO)](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Positions, collection (ADO MD)](../ado-md-api/positions-collection-ado-md.md)  
        [Procedures, collection (ADOX)](../adox-api/procedures-collection-adox.md)  
        [Properties, collection (ADO)](./properties-collection-ado.md)  
        [Tables, collection (ADOX)](../adox-api/tables-collection-adox.md)  
        [Users, collection (ADOX)](../adox-api/users-collection-adox.md)  
        [Views, collection (ADOX)](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Count, exemple de propriété (VB)](./count-property-example-vb.md)   
 [Count, exemple de propriété (VC + +)](./count-property-example-vc.md)   
 [Refresh, méthode (ADO)](./refresh-method-ado.md)