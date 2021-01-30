---
description: Status, exemple de propriété (objet Field) (VB)
title: Status, exemple de propriété (Field) (VB) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Status property [ADO Field], Visual Basic example
ms.assetid: fdd09b60-39c7-44be-8008-e891a031f80e
author: rothja
ms.author: jroth
ms.openlocfilehash: 69e80f025c29e379495507447ef78af5bcc8868f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166458"
---
# <a name="status-property-example-field-vb"></a>Status, exemple de propriété (objet Field) (VB)
L’exemple suivant ouvre un document à partir d’un dossier en lecture/écriture à l’aide du [fournisseur de publication Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). La propriété [Status](./status-property-ado-field.md) d’un objet [Field](./field-object.md) de l' [enregistrement](./record-object-ado.md) est d’abord définie sur **AdFieldPendingInsert**, puis mise à jour vers **adFieldOK**.  
  
```  
'BeginStatusFieldVB  
  
 ' to integrate this code replace the values in the source string  
  
Sub Main()  
  
   Dim File As ADODB.Record  
   Dim strFile As String  
   Dim Cnxn As ADODB.Connection  
   Dim strCnxn As String  
  
   Set Cnxn = New ADODB.Connection  
   strCnxn = "url=https://MyServer/"  
   Cnxn.Open strCnxn  
  
   Set File = New ADODB.Record  
   strFile = "Folder/FileName"  
   ' Open a read/write document  
   File.Source = strFile  
   File.ActiveConnection = Cnxn  
   File.Mode = adModeReadWrite  
   File.Open  
  
   Debug.Print "Append a couple of fields"  
  
   File.Fields.Append "chektest:fld1", adWChar, 42, adFldUpdatable, "fld1"  
   File.Fields.Append "chektest:fld2", adWChar, 42, adFldUpdatable, "fld2"  
  
   Debug.Print "status for the fields"  
   Debug.Print File.Fields("chektest:fld1").Status 'adfldpendinginsert  
   Debug.Print File.Fields("chektest:fld2").Status 'adfldpendinginsert  
  
    'turn off error-handling to verify field status  
   On Error Resume Next  
  
   File.Fields.Update  
  
   Debug.Print "Update succeeds"  
   Debug.Print File.Fields("chektest:fld1").Status 'adfldpendinginsert + adFieldUnavailable  
   Debug.Print File.Fields("chektest:fld2").Status 'adfldpendinginsert + adFieldUnavailable  
  
    ' resume default error-handling  
   On Error GoTo 0  
  
     ' clean up  
    File.Close  
    Cnxn.Close  
    Set File = Nothing  
    Set Cnxn = Nothing  
  
End Sub  
'EndStatusFieldVB  
```  
  
 L’exemple suivant supprime un **champ** connu d’un **enregistrement** ouvert à partir d’un document. La propriété **Status** sera d’abord définie sur **adFieldOK**, puis sur **adFieldPendingUnknown**.  
  
```  
Attribute VB_Name = "StatusField"  
```  
  
 Le code suivant supprime un **champ** d’un **enregistrement** ouvert sur un document en lecture seule. L' **État** est défini sur **adFieldPendingDelete**. Lors de la [mise à jour](./update-method.md), la suppression échoue et l' **État** est **adFieldPendingDelete** plus **adFieldPermissionDenied**. [CancelUpdate](./cancelupdate-method-ado.md) efface le paramètre d' **État** en attente.  
  
```  
Attribute VB_Name = "StatusField"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Field, objet](./field-object.md)   
 [Record, objet (ADO)](./record-object-ado.md)   
 [Status, propriété (objet Field ADO)](./status-property-ado-field.md)