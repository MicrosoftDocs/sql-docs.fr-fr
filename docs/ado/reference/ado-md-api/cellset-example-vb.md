---
description: Exemple avec Cellset (VB)
title: CellSet, exemple (VB) | Microsoft Docs
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
- Cellset object [ADO MD], Visual Basic example
ms.assetid: 2666ad1c-b48e-4b2c-b269-5a9f4e4a7810
author: rothja
ms.author: jroth
ms.openlocfilehash: 19e573a2beb425479faeaeabf2f42163300f4e95
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169944"
---
# <a name="cellset-example-vb"></a>Exemple avec Cellset (VB)
Ce projet de Visual Basic montre les bases de l’utilisation de ADO MD pour accéder aux données du cube. Elle affiche des légendes de membre pour les en-têtes de colonne et de ligne, puis affiche les valeurs mises en forme de cellules spécifiques dans l’objet CellSet.  
  
```  
Private Sub cmdCellSettoDebugWindow_Click()  
Dim cat As New ADOMD.Catalog  
Dim cst As New ADOMD.Cellset  
Dim i As Integer  
Dim j As Integer  
Dim strServer As String  
Dim strSource As String  
Dim strColumnHeader As String  
Dim strRowText As String  
  
On Error GoTo Error_cmdCellSettoDebugWindow_Click  
Screen.MousePointer = vbHourglass  
'*-----------------------------------------------------------------------  
'* Set Server to Local Host  
'*-----------------------------------------------------------------------  
    strServer = "LOCALHOST"  
  
'*-----------------------------------------------------------------------  
'* Set MDX query string Source  
'*-----------------------------------------------------------------------  
    strSource = strSource & "SELECT "  
    strSource = strSource & "{[Measures].members} ON COLUMNS,"  
    strSource = strSource & _  
        "NON EMPTY [Store].[Store City].members ON ROWS"  
    strSource = strSource & " FROM Sales"  
  
'*-----------------------------------------------------------------------  
'* Set Active Connection  
'*-----------------------------------------------------------------------  
        cat.ActiveConnection = "Data Source=" & strServer & _  
            ";Provider=msolap;"  
  
'*-----------------------------------------------------------------------  
'* Set Cell Set source to MDX query string  
'*-----------------------------------------------------------------------  
        cst.Source = strSource  
  
'*-----------------------------------------------------------------------  
'* Set Cell Sets active connection to current connection  
'*-----------------------------------------------------------------------  
    Set cst.ActiveConnection = cat.ActiveConnection  
  
'*-----------------------------------------------------------------------  
'* Open Cell Set  
'*-----------------------------------------------------------------------  
    cst.Open  
  
'*-----------------------------------------------------------------------  
'* Allow space for Row Header Text  
'*-----------------------------------------------------------------------  
strColumnHeader = vbTab & vbTab & vbTab & vbTab & vbTab & vbTab  
  
'*-----------------------------------------------------------------------  
'* Loop through Column Headers  
'*-----------------------------------------------------------------------  
       For i = 0 To cst.Axes(0).Positions.Count - 1  
            strColumnHeader = strColumnHeader & _  
                cst.Axes(0).Positions(i).Members(0).Caption & vbTab & _  
                    vbTab & vbTab & vbTab  
       Next  
       Debug.Print vbTab & strColumnHeader & vbCrLf  
  
'*-----------------------------------------------------------------------  
'* Loop through Row Headers and Provide data for each row  
'*-----------------------------------------------------------------------  
        strRowText = ""  
        For j = 0 To cst.Axes(1).Positions.Count - 1  
            strRowText = strRowText & _  
                cst.Axes(1).Positions(j).Members(0).Caption & vbTab & _  
                    vbTab & vbTab & vbTab  
            For k = 0 To cst.Axes(0).Positions.Count - 1  
                strRowText = strRowText & cst(k, j).FormattedValue & _  
                    vbTab & vbTab & vbTab & vbTab  
            Next  
            Debug.Print strRowText & vbCrLf  
            strRowText = ""  
        Next  
  
    Screen.MousePointer = vbDefault  
Exit Sub  
  
Error_cmdCellSettoDebugWindow_Click:  
   Beep  
   Screen.MousePointer = vbDefault  
   MsgBox "The Following Error has occurred:" & vbCrLf & _  
      Err.Description, vbCritical, " Error!"  
   Exit Sub  
End Sub  
```
