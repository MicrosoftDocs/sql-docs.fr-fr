---
description: Programmation ADO VBScript
title: Programmation ADO VBScript | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, VBScript
- VBScript [ADO]
ms.assetid: 6aaaf6d0-1376-4473-bea6-b81f2645a9ac
author: rothja
ms.author: jroth
ms.openlocfilehash: 31e6c98d12c6c6f472053634d8b3aea314ccbab2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037869"
---
# <a name="vbscript-ado-programming"></a>Programmation ADO VBScript
## <a name="creating-an-ado-project"></a>Création d’un projet ADO  
 Microsoft Visual Basic, l’édition de script ne prend pas en charge les bibliothèques de types. vous n’avez donc pas besoin de référencer ADO dans votre projet. Par conséquent, aucune fonctionnalité associée, telle que l’exécution de la ligne de commande, n’est prise en charge. En outre, par défaut, les constantes énumérées ADO ne sont pas définies dans VBScript.  
  
 Toutefois, ADO vous fournit deux fichiers Include contenant les définitions suivantes à utiliser avec VBScript :  
  
-   Pour les scripts côté serveur, utilisez Adovbs. Inc, qui est installé dans le dossier c:\Program Files\Common Files\System\ado\ par défaut.  
  
-   Pour les scripts côté client, utilisez Adcvbs. Inc, qui est installé dans le dossier c:\Program Files\Common Files\System\msdac\ par défaut.  
  
 Vous pouvez copier et coller des définitions constantes à partir de ces fichiers dans vos pages ASP, ou, si vous effectuez des scripts côté serveur, copiez le fichier adovbs. Inc dans un dossier de votre site Web et référencez-le à partir de votre page ASP comme suit :  
  
```vb
<!--#include File="adovbs.inc"-->  
```  
  
## <a name="creating-ado-objects-in-vbscript"></a>Création d’objets ADO en VBScript  
 Vous ne pouvez pas utiliser l’instruction **Dim** pour assigner des objets à un type spécifique dans VBScript. En outre, VBScript ne prend pas en charge la **nouvelle** syntaxe utilisée avec l’instruction **Dim** dans Visual Basic pour applications. Vous devez à la place utiliser l’appel de fonction **CreateObject** :  
  
```vb
Dim Rs1  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
```  
  
## <a name="vbscript-examples"></a>Exemples VBScript  
 Le code suivant est un exemple générique de programmation côté serveur VBScript dans un fichier de page (ASP) Active Server :  
  
```vb
<%  @LANGUAGE="VBSCRIPT" %>  
<%  Option Explicit %>  
<!--#include File="adovbs.inc"-->  
<HTML>  
    <BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
  
    <!-- Your ASP Code goes here -->  
<%  
Dim Source  
Dim Connect  
Dim Rs1  
  
Source = "SELECT * FROM Authors"  
Connect = "Provider=sqloledb;Data Source=srv;" & _  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
  
Set Rs1 = Server.CreateObject( "ADODB.Recordset" )  
Rs1.Open Source, Connect, adOpenForwardOnly  
Response.Write("Success!")  
%>  
    </BODY>  
</HTML>  
```  
  
 Des exemples VBScript plus spécifiques sont inclus dans la documentation ADO. Pour plus d’informations, consultez [exemples de code ADO dans Microsoft Visual Basic Scripting Edition](../../reference/ado-api/ado-code-examples-vbscript.md).  
  
## <a name="differences-between-vbscript-and-visual-basic"></a>Différences entre VBScript et Visual Basic  
 L’utilisation d’ADO avec VBScript est semblable à l’utilisation d’ADO avec Visual Basic de nombreuses façons, notamment la façon dont la syntaxe est utilisée. Toutefois, il existe quelques différences importantes :  
  
-   VBScript ne prend en charge que le type de données Variant, qui peut contenir différents types de données. Vous pouvez stocker les données dont vous avez besoin dans un type de données Variant, et les données fonctionneront correctement en raison du cast effectué par VBScript. Il reconnaît le type requis par ADO et convertit en conséquence la valeur de la variante.  
  
-   Vous ne pouvez pas utiliser **on \<label> Error GoTo** dans VBScript.  
  
-   VBScript prend en charge certaines fonctions de Visual Basic intégrées, telles que **MsgBox**, **Date** et **IsNumeric**. Toutefois, étant donné que VBScript est un sous-ensemble de Visual Basic, toutes les fonctions intégrées ne sont pas prises en charge. Par exemple, VBScript ne prend pas en charge la fonction **format** et les fonctions d’e/s de fichier.