---
description: Parent, propriété (ADO MD)
title: Propriété Parent (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parent
- Member::Parent
helpviewer_keywords:
- Parent property [ADO MD]
ms.assetid: 32c278c1-d8e1-4bb7-9ecd-2fbfdffee34b
author: rothja
ms.author: jroth
ms.openlocfilehash: 0674b3455611e8700e8859d4ced5d4052c51135c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050950"
---
# <a name="parent-property-ado-md"></a>Parent, propriété (ADO MD)
Indique le membre qui est le parent du [membre](./member-object-ado-md.md) actuel dans une hiérarchie.  
  
## <a name="return-values"></a>Valeurs de retour  
 Retourne un objet [membre](./member-object-ado-md.md) et est en lecture seule.  
  
## <a name="remarks"></a>Notes  
 Un membre qui se trouve au niveau supérieur d’une hiérarchie (la racine) n’a pas de parent. Cette propriété est prise en charge uniquement sur les objets **membres** appartenant à un objet de [niveau](./level-object-ado-md.md) . Une erreur se produit quand cette propriété est référencée à partir d’objets **membres** appartenant à un objet [position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Member, objet (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Children, propriété (ADO MD)](./children-property-ado-md.md)