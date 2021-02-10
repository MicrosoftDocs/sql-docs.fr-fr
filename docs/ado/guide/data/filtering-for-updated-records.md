---
description: Filtrage des enregistrements mis à jour
title: Filtrage des enregistrements mis à jour | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- filtering for updated records [ADO]
ms.assetid: 4a798921-d7bb-47c9-a252-550fd9463ec9
author: rothja
ms.author: jroth
ms.openlocfilehash: c96f0519ac3dc421fe3717bd19c00735223e3375
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033115"
---
# <a name="filtering-for-updated-records"></a>Filtrage des enregistrements mis à jour
Avant d’appeler UpdateBatch, vous pouvez utiliser la propriété de filtre Recordset pour afficher uniquement les enregistrements qui ont été modifiés depuis l’ouverture du recordset ou le dernier appel à UpdateBatch. Pour ce faire, définissez le filtre sur adFilterPendingRecords pour déterminer le nombre d’enregistrements qui seront mis à jour, comme indiqué dans l’exemple de code de la section suivante.  
  
## <a name="remarks"></a>Notes  
 Cet exemple étend l’exemple UpdateBatch précédent en filtrant l’objet Recordset juste avant d’appeler la méthode UpdateBatch, en indiquant à l’utilisateur les enregistrements qui seront modifiés et en lui permettant d’annuler la mise à jour (à l’aide de la méthode CancelBatch).  
  
```  
'BeginFilterPend  
    '...  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
  
    ' Change value of Phone field for first record in Recordset, saving value  
    ' for later restoration.  
    intId = objRs1("ShipperId")  
    sPhone = objRs1("Phone")  
  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Add two new records  
    For i = 0 To 1  
        objRs1.AddNew  
        objRs1(1) = "New Shipper #" & CStr((i + 1))  
        objRs1(2) = "(nnn) 555-" & i & i & i & i  
    Next i  
  
    objRs1.Filter = adFilterPendingRecords  
  
    MsgBox "There are " & objRs1.RecordCount & " records pending.", _  
            , "Filter Pending"  
  
    ' Cancel the updates  
    objRs1.CancelBatch  
'EndFilterPend  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Mode Batch](./batch-mode.md)