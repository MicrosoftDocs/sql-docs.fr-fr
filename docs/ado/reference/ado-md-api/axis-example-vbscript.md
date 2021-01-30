---
description: Exemple avec Axis (VBScript)
title: AXIS, exemple (VBScript) | Microsoft Docs
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
- ADO MD code examples, VBScript
ms.assetid: b4647211-2566-4657-ae7b-3dd761457d7b
author: rothja
ms.author: jroth
ms.openlocfilehash: 27f41879cb69560357cbb32b70d0c1d7fbb985b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166182"
---
# <a name="axis-example-vbscript"></a>Exemple avec Axis (VBScript)
Cette page Active Server affiche les données OLAP d’une chaîne de requête MDX et écrit le CellSet résultant dans une structure de table HTML.  
  
```  
<%@ Language=VBScript %>  
<%  
'************************************************************************  
'**_ Active Server Page displays OLAP data from default  
'_*_ MDX Query string and writes resulting cell set to HTML table  
'_*_ structure.  
'_***********************************************************************  
Response.Buffer=True  
Response.Expires=0  
%>  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Visual Studio 6.0">  
</HEAD>  
<BODY bgcolor=Ivory>  
<FONT FACE=Verdana>  
  
<%  
  
Dim cat,cst,i,j,strSource,csw,intDC0,intDC1,intPC0,intPC1  
  
'************************************************************************  
'**_ Set Connection Objects for Multidimensional Catalog and Cellset  
'_***********************************************************************  
Set cat = Server.CreateObject("ADOMD.Catalog")  
Set cst = Server.CreateObject("ADOMD.CellSet")  
  
'************************************************************************  
'**_ Use default settings of a known OLAP Server  
'_*_ for Server Name for Connection Set Server Name Session Object  
'_*_ to default value  
'_***********************************************************************  
'**_ Must set OLAPServerName to OLAP Server that is  
'_*_ present on network  
'_***********************************************************************  
   OLAPServerName = "Please set to present OLAP Server"  
   cat.ActiveConnection = "Data Source=" & OLAPServerName & _  
      ";Initial Catalog=FoodMart;Provider=msolap;"  
  
'************************************************************************  
'**_ Use default MDX Query string of a known query that works  
'_*_ with default server Set MDXQuery Session Object to default value  
'_***********************************************************************  
   strSource = strSource & "SELECT "  
   strSource = strSource & "{[Measures].members} ON COLUMNS,"  
   strSource = strSource & _  
      "NON EMPTY [Store].[Store City].members ON ROWS"  
   strSource = strSource & " FROM Sales"  
  
'************************************************************************  
'**_ Set Cell Set Source property to strSource to be passed on cell set '_*_ open method  
'_***********************************************************************  
    cst.Source = strSource  
  
'************************************************************************  
'**_ Set Cell Sets Active connection to use the current Catalogs Active   
'_*_ connection  
'_***********************************************************************  
Set cst.ActiveConnection = cat.ActiveConnection  
  
'************************************************************************  
'**_ Using Open method, Open cell set  
'_***********************************************************************  
cst.Open  
  
'************************************************************************  
'**_ Set Dimension Counts minus 1 for Both Axes to intDC0, intDC1  
'_*_ Set Position Counts minus 1 for Both Axes to intPC0, intPC1  
'_***********************************************************************  
intDC0 = cst.Axes(0).DimensionCount-1  
intDC1 = cst.Axes(1).DimensionCount-1  
  
intPC0 = cst.Axes(0).Positions.Count - 1  
intPC1 = cst.Axes(1).Positions.Count - 1  
  
'************************************************************************  
'**_ Create HTML Table structure to hold MDX Query return Record set  
'_***********************************************************************  
      Response.Write "<Table width=100% border=1>"  
  
'************************************************************************  
'**_ Loop to create Column header  
'_***********************************************************************  
      For h=0 to intDC0  
         Response.Write "<TR>"  
  
'************************************************************************  
'**_ Loop to create spaces in front of Column headers  
'_*_ to align with Row header  
'_***********************************************************************  
         For c=0 to intDC1  
            Response.Write "<TD></TD>"  
         Next  
  
'************************************************************************  
'**_ Iterate through Axes(0) Positions writing member captions to table   
'_*_ header  
'_***********************************************************************  
         For i = 0 To intPC0  
            Response.Write "<TH>"  
            Response.Write "<FONT size=-2>"  
            Response.Write cst.Axes(0).Positions(i).Members(h).Caption  
            Response.Write "</FONT>"  
            Response.Write "</TH>"  
         Next  
         Response.Write "</TR>"  
      Next  
'************************************************************************  
'**_ Use Array values for row header formatting to provide  
'_*_ spaces under beginning row header titles  
'_***********************************************************************  
      For j = 0 To intPC1  
         Response.Write "<TR>"  
         For h=0 to intDC1  
            Response.Write "<TD><B>"  
            Response.Write "<FONT size=-2>"  
            Response.Write cst.Axes(1).Positions(j).Members(h).Caption  
            Response.Write "</FONT>"  
            Response.Write "</B></TD>"  
         Next  
         For k = 0 To intPC0  
            Response.Write "<TD align=right bgcolor="  
            Response.Write csw  
            Response.Write ">"  
            Response.Write "<FONT size=-2>"  
            Response.Write cst(k, j).FormattedValue  
            Response.Write "</FONT>"  
            Response.Write "</TD>"  
         Next  
         Response.Write "</TR>"  
      Next  
      Response.Write "</Table>"  
  
%>  
</FONT>  
</BODY>  
</HTML>  
```
