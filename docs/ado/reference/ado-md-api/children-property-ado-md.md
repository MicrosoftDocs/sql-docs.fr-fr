---
description: Children, propriété (ADO MD)
title: Propriété Children (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member::Children
- Children
helpviewer_keywords:
- Children property [ADO MD]
ms.assetid: 61d36468-1ccd-467a-9cb5-17d0bfacc766
author: rothja
ms.author: jroth
ms.openlocfilehash: 81cbaddf1db2017384392e20b249b9c1beb3ccd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169916"
---
# <a name="children-property-ado-md"></a>Children, propriété (ADO MD)
Retourne une collection de [membres](./members-collection-ado-md.md) pour laquelle le [membre](./member-object-ado-md.md) actuel est le parent dans la hiérarchie.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne une collection de **membres** et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 La propriété **Children** contient une collection de **membres** pour laquelle le **membre** actuel est le parent hiérarchique. Les objets **membres** de niveau feuille n’ont pas de membres enfants dans la collection **members** . Cette propriété est uniquement prise en charge sur les objets **membres** appartenant à un objet de [niveau](./level-object-ado-md.md) . Une erreur se produit quand cette propriété est référencée à partir d’objets **membres** appartenant à un objet [position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Member, objet (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ChildCount, propriété (ADO MD)](./childcount-property-ado-md.md)