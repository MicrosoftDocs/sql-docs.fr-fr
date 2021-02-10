---
description: UnderlyingValue, propriété
title: Propriété UnderlyingValue | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::GetUnderlyingValue
- Field20::get_UnderlyingValue
- Field20::UnderlyingValue
helpviewer_keywords:
- UnderlyingValue property
ms.assetid: 00a0c8b8-8b63-433f-95b8-020ab05874a0
author: rothja
ms.author: jroth
ms.openlocfilehash: d96f48d796e3eef7b41467ea4f5b5b14bfc66d36
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056374"
---
# <a name="underlyingvalue-property"></a>UnderlyingValue, propriété
Indique la valeur actuelle d’un objet [champ](./field-object.md) dans la base de données.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur de **type Variant** qui indique la valeur du **champ**.  
  
## <a name="remarks"></a>Notes  
 Utilisez la propriété **UnderlyingValue** pour retourner la valeur de champ actuelle de la base de données. La valeur de champ dans la propriété **UnderlyingValue** est la valeur qui est visible pour votre transaction et peut être le résultat d’une mise à jour récente par une autre transaction. Cela peut être différent de la propriété [OriginalValue](./originalvalue-property-ado.md) , qui reflète la valeur retournée à l’origine à l' [objet Recordset](./recordset-object-ado.md).  
  
 Cela est similaire à l’utilisation de la méthode [Resync](./resync-method.md) , mais la propriété **UnderlyingValue** retourne uniquement la valeur d’un champ spécifique de l’enregistrement en cours. Il s’agit de la même valeur que celle utilisée par la méthode [Resync](./resync-method.md) pour remplacer la propriété [value](./value-property-ado.md) .  
  
 Lorsque vous utilisez cette propriété avec la propriété **OriginalValue** , vous pouvez résoudre les conflits qui résultent des mises à jour par lots.  
  
## <a name="record"></a>Enregistrer  
 Pour les objets [Record](./record-object-ado.md) , cette propriété est vide pour les champs ajoutés avant l’appel de la méthode [Update](./update-method.md) .  
  
## <a name="applies-to"></a>S'applique à  
 [Objet Field](./field-object.md)  
  
## <a name="see-also"></a>Voir aussi  
 [OriginalValue et UnderlyingValue, exemples de propriétés (VB)](./originalvalue-and-underlyingvalue-properties-example-vb.md)   
 [OriginalValue et UnderlyingValue, exemples de propriétés (VC + +)](./originalvalue-and-underlyingvalue-properties-example-vc.md)   
 [OriginalValue, propriété (ADO)](./originalvalue-property-ado.md)   
 [Resync, méthode](./resync-method.md)