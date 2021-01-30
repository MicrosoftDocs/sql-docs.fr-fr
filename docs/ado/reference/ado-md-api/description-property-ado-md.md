---
description: Description, propriété (ADO MD)
title: Description, propriété (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
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
ms.openlocfilehash: 7339bcafe8df6678ec5cd9a0bef43dd92dd6edbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169891"
---
# <a name="description-property-ado-md"></a>Description, propriété (ADO MD)
Retourne une explication textuelle de l’objet actuel.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une **chaîne** et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 Pour les objets [membres](./member-object-ado-md.md) , la **Description** s’applique uniquement aux membres Measure et Formula. **Description** retourne une chaîne vide ("") pour tous les autres types de membres. Pour plus d’informations sur les différents types de membres, consultez la propriété [type](./type-property-ado-md.md) .  
  
 Cette propriété est uniquement prise en charge sur les objets **membres** qui appartiennent à un objet de [niveau](./level-object-ado-md.md) . Une erreur se produit quand cette propriété est référencée à partir d’objets **membres** appartenant à un objet [position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [CubeDef, objet (ADO MD)](./cubedef-object-ado-md.md)  
        [Dimension, objet (ADO MD)](./dimension-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Hierarchy, objet (ADO MD)](./hierarchy-object-ado-md.md)  
        [Level, objet (ADO MD)](./level-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Member, objet (ADO MD)](./member-object-ado-md.md)  
    :::column-end:::
:::row-end:::