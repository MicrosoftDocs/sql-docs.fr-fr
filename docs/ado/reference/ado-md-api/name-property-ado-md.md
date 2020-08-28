---
description: Name, propriété (ADO MD)
title: Name, propriété (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Level::Name
- CubeDef::Name
- Member::Name
- Catalog::Name
- Dimension::Name
- Name
- Axis::Name
- Hierarchy::Name
helpviewer_keywords:
- Name property [ADO MD]
ms.assetid: 4a04380b-51dc-4aaf-8d25-123cdd589641
author: rothja
ms.author: jroth
ms.openlocfilehash: 4dfec86bb631dda661b957e02667cd320c20fec6
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88986240"
---
# <a name="name-property-ado-md"></a>Name, propriété (ADO MD)
Indique le nom d’un objet.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une **chaîne** et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez récupérer la propriété **Name** d’un objet à l’aide d’une référence ordinale, après laquelle vous pouvez faire référence à l’objet directement par son nom. Par exemple, si `cdf.CubeDefs(0).Name` donne « Bobs Video Store », vous pouvez faire référence à ce [CubeDef](./cubedef-object-ado-md.md) en tant que `cdf.CubeDefs("Bobs Video Store")` .  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Axis, objet (ADO MD)](./axis-object-ado-md.md)  
        [Catalog, objet (ADO MD)](./catalog-object-ado-md.md)  
        [CubeDef, objet (ADO MD)](./cubedef-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Dimension, objet (ADO MD)](./dimension-object-ado-md.md)  
        [Hierarchy, objet (ADO MD)](./hierarchy-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Level, objet (ADO MD)](./level-object-ado-md.md)  
        [Member, objet (ADO MD)](./member-object-ado-md.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Exemple de catalogue (VB)](./catalog-example-vb.md)   
 [Propriété Caption (ADO MD)](./caption-property-ado-md.md)   
 [Description, propriété (ADO MD)](./description-property-ado-md.md)   
 [UniqueName, propriété (ADO MD)](./uniquename-property-ado-md.md)