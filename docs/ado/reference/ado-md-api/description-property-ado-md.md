---
description: Description, propriété (ADO MD)
title: Description, propriété (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Member::Description
- Level::Description
- CubeDef::Description
- Hierarchy::Description
- Description
- Dimension::Description
helpviewer_keywords:
- Description property [ADO MD]
ms.assetid: 6d626d35-0bf3-4f24-9934-ad9c9c91273a
author: rothja
ms.author: jroth
ms.openlocfilehash: 262d83f37157af43a91c508daaa31127247588d6
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88441121"
---
# <a name="description-property-ado-md"></a>Description, propriété (ADO MD)
Retourne une explication textuelle de l’objet actuel.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une **chaîne** et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 Pour les objets [membres](../../../ado/reference/ado-md-api/member-object-ado-md.md) , la **Description** s’applique uniquement aux membres Measure et Formula. **Description** retourne une chaîne vide ("") pour tous les autres types de membres. Pour plus d’informations sur les différents types de membres, consultez la propriété [type](../../../ado/reference/ado-md-api/type-property-ado-md.md) .  
  
 Cette propriété est uniquement prise en charge sur les objets **membres** qui appartiennent à un objet de [niveau](../../../ado/reference/ado-md-api/level-object-ado-md.md) . Une erreur se produit quand cette propriété est référencée à partir d’objets **membres** appartenant à un objet [position](../../../ado/reference/ado-md-api/position-object-ado-md.md) .  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [CubeDef, objet (ADO MD)](../../../ado/reference/ado-md-api/cubedef-object-ado-md.md)  
        [Dimension, objet (ADO MD)](../../../ado/reference/ado-md-api/dimension-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Hierarchy, objet (ADO MD)](../../../ado/reference/ado-md-api/hierarchy-object-ado-md.md)  
        [Level, objet (ADO MD)](../../../ado/reference/ado-md-api/level-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Member, objet (ADO MD)](../../../ado/reference/ado-md-api/member-object-ado-md.md)  
    :::column-end:::
:::row-end:::
