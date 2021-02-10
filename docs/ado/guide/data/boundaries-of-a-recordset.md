---
description: Limites d’un recordset
title: Limites d’un jeu d’enregistrements | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- EOF property [ADO], boundaries of a Recordset
- Recordset object [ADO], boundaries of a Recordset
- BOF property [ADO], boundaries of a Recordset
ms.assetid: c0dd4a0f-478d-4c5e-b5d5-7535f211d064
author: rothja
ms.author: jroth
ms.openlocfilehash: f737ad11af65045a1d923f427c7487e4b751aaa8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028014"
---
# <a name="boundaries-of-a-recordset"></a>Limites d’un recordset
**Recordset** prend en charge les propriétés **BOF** et **EOF** pour détourer le début et la fin, respectivement, du jeu de données. Vous pouvez considérer **BOF** et **EOF** comme des enregistrements « fantômes » placés au début et à la fin de l’ensemble d' **enregistrements**. En comptant **BOF** et **EOF**, notre exemple de **jeu d’enregistrements** se présenterait comme suit :  
  
|ProductID|ProductName|UnitPrice|  
|---------------|-----------------|---------------|  
|Business|||  
|7|Poires séchées Organic d’oncle Bob|30,0000|  
|14|Tofu|23,2500|  
|28|Rssle choucroute|45,6000|  
|51|Manjimup pommes séchées|53,0000|  
|74|Longlife Tofu|10,0000|  
|EOF|||  
  
 Lorsqu’un curseur se déplace au-delà du dernier enregistrement, **EOF** a la valeur **true**; dans le cas contraire, sa valeur est **false**. De même, lorsque le curseur se déplace avant le premier enregistrement, **BOF** a la valeur **true**; dans le cas contraire, sa valeur est **false**. Ces propriétés sont couramment utilisées pour énumérer des enregistrements dans le DataSet, comme illustré dans le fragment de code JScript suivant.  
  
```  
while (objRecordset.EOF != true)   
{  
   // Work on the current record.  
   ...  
   // Advance the cursor forward to the next record.  
   objRecordset.MoveNext();  
}  
or  
while (objRecordset.BOF != true)   
{  
   // Work on the current record.  
   ...  
   // Move the cursor to the previous record.  
   objRecordset.MovePrevious();  
}  
```  
  
 Si **BOF** et **EOF** ont tous deux la **valeur true**, l’objet **Recordset** est vide. Les deux propriétés ont la **valeur false** pour un objet **Recordset** non vide qui vient d’être ouvert. Vous pouvez utiliser les propriétés **BOF** et **EOF** ensemble pour déterminer si un objet **Recordset** est vide ou non, comme le montre le fragment de code JScript suivant.  
  
```  
if (objRecordset.EOF == true && objRecordset.BOF == true)  
{  
   WScript.Echo("we got an empty dataset.");  
}  
else  
{  
   WScript.Echo("we got a full dataset.");  
}  
```  
  
 Ce schéma fonctionne pour tous les types de curseurs et est indépendant des fournisseurs sous-jacents. Si vous tentez de déterminer le vidage d’un objet **Recordset** en vérifiant si sa valeur de propriété **RecordCount** est égale à zéro (0) ou non, vous devez prendre des précautions pour utiliser un curseur et un fournisseur appropriés qui prennent en charge le retour du nombre d’enregistrements dans le résultat.  
  
 Si vous supprimez le dernier enregistrement restant dans l’objet **Recordset** , le curseur reste dans un état indéterminé. Les propriétés **BOF** et **EOF** peuvent conserver la **valeur false** jusqu’à ce que vous tentiez de repositionner l’enregistrement actif, en fonction du fournisseur. Pour plus d’informations, consultez [Suppression d’enregistrements à l’aide de la méthode Delete](./deleting-records-using-the-delete-method.md).