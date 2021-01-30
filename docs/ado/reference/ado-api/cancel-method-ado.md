---
description: Cancel, méthode (ADO)
title: Cancel, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::Cancel
- _Record::Cancel
- _Connection::Cancel
- Command25::Cancel
- _Stream::Cancel
helpviewer_keywords:
- Cancel method [ADO]
ms.assetid: e0db4e15-6787-41e2-8f13-9e9b524d620a
author: rothja
ms.author: jroth
ms.openlocfilehash: adc282492b4c6d9a21683693154e43f1177bdca0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164743"
---
# <a name="cancel-method-ado"></a>Cancel, méthode (ADO)
Annule l’exécution d’un appel de méthode asynchrone en attente.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Cancel  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez la méthode **Cancel** pour terminer l’exécution d’un appel de méthode asynchrone : autrement dit, une méthode appelée avec l’option **adAsyncConnect**, **adAsyncExecute** ou **adAsyncFetch** .  
  
 Le tableau suivant indique la tâche qui se termine lorsque vous utilisez la méthode **Cancel** sur un type particulier d’objet.  
  
|Si l' *objet* est un|Le dernier appel asynchrone à cette méthode est terminé|  
|----------------------|-------------------------------------------------------------|  
|[Commande](./command-object-ado.md)|[Execute](./execute-method-ado-command.md)|  
|[Connection](./connection-object-ado.md)|[Exécuter](./execute-method-ado-connection.md) ou [ouvrir](./open-method-ado-connection.md)|  
|[Enregistrement](./record-object-ado.md)|[CopyRecord](./copyrecord-method-ado.md), [DeleteRecord](./deleterecord-method-ado.md), [MoveRecord](./moverecord-method-ado.md)ou [Open](./open-method-ado-record.md)|  
|[Recordset](./recordset-object-ado.md)|[Ouvrir](./open-method-ado-recordset.md)|  
|[STREAM](./stream-object-ado.md)|[Ouvrir](./open-method-ado-stream.md)|  
  
## <a name="applies-to"></a>S'applique à  

:::row:::
    :::column:::
        [Command, objet (ADO)](./command-object-ado.md)  
        [Connection, objet (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Record, objet (ADO)](./record-object-ado.md)  
        [Recordset, objet (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Stream, objet (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Voir aussi  
 [Cancel, exemple de méthode (VB)](./cancel-method-example-vb.md)   
 [Cancel, exemple de méthode (VBScript)](../rds-api/cancel-method-example-vbscript.md)   
 [Cancel, exemple de méthode (VC + +)](./cancel-method-example-vc.md)   
 [Cancel, méthode (RDS)](../rds-api/cancel-method-rds.md)   
 [Méthode CancelBatch (ADO)](./cancelbatch-method-ado.md)   
 [CancelUpdate, méthode (ADO)](./cancelupdate-method-ado.md)   
 [CancelUpdate, méthode (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Execute, méthode (commande ADO)](./execute-method-ado-command.md)   
 [Execute, méthode (connexion ADO)](./execute-method-ado-connection.md)   
 [Open, méthode (connexion ADO)](./open-method-ado-connection.md)   
 [Open, méthode (objet Recordset ADO)](./open-method-ado-recordset.md)