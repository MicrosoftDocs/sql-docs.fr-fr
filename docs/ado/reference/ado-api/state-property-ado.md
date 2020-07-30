---
title: State, propriété (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Command25::State
helpviewer_keywords:
- State property [ADO]
ms.assetid: 0b993bac-2653-40b1-bcbb-5b57b6aae2bf
author: rothja
ms.author: jroth
ms.openlocfilehash: f4fa19bf21764437d3713c434e288b3bb3a3d25d
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87242569"
---
# <a name="state-property-ado"></a>State, propriété (ADO)
Indique pour tous les objets applicables si l’état de l’objet est ouvert ou fermé. Si l’objet exécute une méthode asynchrone, indique si l’état actuel de l’objet est en cours de connexion, en cours d’exécution ou en cours d’extraction.  
  
## <a name="return-value"></a>Valeur de retour  
 Retourne une valeur de **type long** qui peut être une valeur [ObjectStateEnum](../../../ado/reference/ado-api/objectstateenum.md) . La valeur par défaut est **adStateClosed**.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser la propriété **State** pour déterminer l’état actuel d’un objet donné à tout moment.  
  
 La propriété **State** de l’objet peut avoir une combinaison de valeurs. Par exemple, si une instruction est en cours d’exécution, cette propriété aura une valeur combinée de **adStateOpen** et **adStateExecuting**.  
  
 La propriété d' **État** est en lecture seule.  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
        [Connection, objet (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)  
        [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [ConnectionString, ConnectionTimeout et State, exemple de propriétés (VB)](../../../ado/reference/ado-api/connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [ConnectionString, ConnectionTimeout et State, exemple de propriétés (VC + +)](../../../ado/reference/ado-api/connectionstring-connectiontimeout-and-state-properties-example-vc.md)   
