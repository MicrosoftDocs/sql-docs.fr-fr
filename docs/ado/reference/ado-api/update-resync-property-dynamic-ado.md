---
description: Update Resync, propriété dynamique (ADO)
title: Mettre à jour la resynchronisation Property-Dynamic (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Update Resync property [ADO]
ms.assetid: 8a3bb608-66d7-4128-a3ef-84cb0556de0d
author: rothja
ms.author: jroth
ms.openlocfilehash: d92f70c5091ec2468199874c4a79843398bc7833
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170026"
---
# <a name="update-resync-property-dynamic-ado"></a>Update Resync, propriété dynamique (ADO)
Spécifie si la méthode [UpdateBatch](./updatebatch-method.md) est suivie d’une opération de méthode de [resynchronisation](./resync-method.md) implicite et, le cas échéant, de l’étendue de cette opération.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne une ou plusieurs des valeurs [ADCPROP_UPDATERESYNC_ENUM](./adcprop-updateresync-enum.md) .  
  
## <a name="remarks"></a>Notes  
 Les valeurs de ADCPROP_UPDATERESYNC_ENUM peuvent être combinées, à l’exception de adResyncAll qui représente déjà la combinaison du reste des valeurs.  
  
 La constante **adResyncConflicts** stocke les valeurs de resynchronisation comme valeurs sous-jacentes, mais ne remplace pas les modifications en attente.  
  
 **Update Resync** est une propriété dynamique ajoutée à la collection de [Propriétés](./properties-collection-ado.md) de l’objet [Recordset](./recordset-object-ado.md) lorsque la propriété [CursorLocation](./cursorlocation-property-ado.md) a la valeur **adUseClient**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](./recordset-object-ado.md)