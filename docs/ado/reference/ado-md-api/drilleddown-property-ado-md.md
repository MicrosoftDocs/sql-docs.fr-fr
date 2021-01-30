---
description: DrilledDown, propriété (ADO MD)
title: Propriété DrilledDown (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DrilledDown
- Member::DrilledDown
helpviewer_keywords:
- DrilledDown property [ADO MD]
ms.assetid: bf39dd36-fc7a-4f6e-86c0-fa71430c0d86
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e25d23cfdbf7f33f024203e25814b16d4ec6da4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169862"
---
# <a name="drilleddown-property-ado-md"></a>DrilledDown, propriété (ADO MD)
Indique si les enfants suivent immédiatement le [membre](./member-object-ado-md.md) sur l’axe.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une valeur **booléenne** et est en lecture seule. **DrilledDown** retourne la **valeur true** s’il n’y a aucun membre enfant du membre actuel sur l’axe. **DrilledDown** retourne la **valeur false** si le membre actuel possède un ou plusieurs membres enfants sur l’axe.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **DrilledDown** pour déterminer s’il existe au moins un enfant de ce membre sur l’axe qui suit immédiatement ce membre. Ces informations sont utiles lors de l’affichage du membre.  
  
 Cette propriété est uniquement prise en charge sur les objets [membres](./member-object-ado-md.md) appartenant à un objet [position](./position-object-ado-md.md) . Une erreur se produit quand cette propriété est référencée à partir d’objets **membres** appartenant à un objet de [niveau](./level-object-ado-md.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Member, objet (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ParentSameAsPrev, propriété (ADO MD)](./parentsameasprev-property-ado-md.md)