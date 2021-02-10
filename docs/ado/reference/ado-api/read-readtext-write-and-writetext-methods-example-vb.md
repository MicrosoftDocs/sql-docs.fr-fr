---
description: Read, ReadText, Write et WriteText, exemples de méthodes (VB)
title: Read, ReadText, Write et WriteText, exemples de méthodes (VB) | Microsoft Docs
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
- ReadText method [ADO], Visual Basic example
- Write method [ADO], Visual Basic example
- Read method [ADO], Visual Basic example
- WriteText method [ADO], Visual Basic example
ms.assetid: 699b73f7-04f9-4d46-94b2-6cb12be6de56
author: rothja
ms.author: jroth
ms.openlocfilehash: 6be85fef5e69e879034536c8785eccfd72c9f331
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040759"
---
# <a name="read-readtext-write-and-writetext-methods-example-vb"></a>Read, ReadText, Write et WriteText, exemples de méthodes (VB)
Cet exemple montre comment lire le contenu d’une zone de texte dans un [flux](./stream-object-ado.md) de texte et un **flux** binaire. Les autres propriétés et méthodes indiquées incluent la [position](./position-property-ado.md), la [taille](./size-property-ado-parameter.md), le [jeu](./charset-property-ado.md)de caractères et [SetEOS](./seteos-method.md).  
  
```  
'BeginReadVB  
Private Sub cmdRead_Click()  
    On Error GoTo ErrorHandler  
  
    'Declare variables  
    Dim objStream As Stream  
    Dim varA As Variant  
    Dim bytA() As Byte  
    Dim i As Integer  
    Dim strBytes As String  
  
    'Instantiate and Open Stream  
    Set objStream = New Stream  
    objStream.Open  
  
    'Write the text content of a textbox to the stream  
    If Text1.Text = "" Then  
        Err.Raise 1, , "The text field is blank."  
    End If  
    objStream.WriteText Text1.Text  
  
    'Display the text contents and size of the stream  
    objStream.Position = 0  
    Debug.Print "Default text:"  
    Debug.Print objStream.ReadText  
    Debug.Print objStream.Size  
  
    'Switch character set and display  
    objStream.Position = 0  
    objStream.Charset = "Windows-1252"  
    Debug.Print "New Charset text:"  
    Debug.Print objStream.ReadText  
    Debug.Print objStream.Size  
  
    'Switch to a binary stream and display  
    objStream.Position = 0  
    objStream.Type = adTypeBinary  
    Debug.Print "Binary:"  
    Debug.Print objStream.Read  
    Debug.Print objStream.Size  
  
    'Load an array of bytes with the text box text  
    ReDim bytA(Len(Text1.Text))  
    For i = 1 To Len(Text1.Text)  
        bytA(i - 1) = CByte(Asc(Mid(Text1.Text, i, 1)))  
    Next  
  
    'Write the buffer to the binary stream and display  
    objStream.Position = 0  
    objStream.Write bytA()  
    objStream.SetEOS  
    objStream.Position = 0  
    Debug.Print "Binary after Write:"  
    Debug.Print objStream.Read  
    Debug.Print objStream.Size  
  
    'Switch back to a text stream and display  
    Debug.Print "Translated back:"  
    objStream.Position = 0  
    objStream.Type = adTypeText  
    Debug.Print objStream.ReadText  
    Debug.Print objStream.Size  
  
    ' clean up  
    objStream.Close  
    Set objStream = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not objStream Is Nothing Then  
        If objStream.State = adStateOpen Then objStream.Close  
    End If  
    Set objStream = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndReadVB  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CharSet, propriété (ADO)](./charset-property-ado.md)   
 [Position, propriété (ADO)](./position-property-ado.md)   
 [Read, méthode](./read-method.md)   
 [ReadText, méthode](./readtext-method.md)   
 [SetEos, méthode](./seteos-method.md)   
 [Size, propriété (objet Stream ADO)](./size-property-ado-stream.md)   
 [Stream, objet (ADO)](./stream-object-ado.md)   
 [Write, méthode](./write-method.md)   
 [WriteText, méthode](./writetext-method.md)