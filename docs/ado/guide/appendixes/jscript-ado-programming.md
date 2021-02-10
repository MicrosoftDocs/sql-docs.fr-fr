---
description: Programmation ADO JScript
title: Programmation ADO JScript | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- JScript
helpviewer_keywords:
- JScript programming in ADO
- ADO, JScript programming
ms.assetid: 62273658-0fe7-4aac-b4d8-f725e6baf043
author: rothja
ms.author: jroth
ms.openlocfilehash: 15a58e69810e3df5221d4c869e14b20112a181c9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029413"
---
# <a name="jscript-ado-programming"></a>Programmation ADO JScript
## <a name="creating-an-ado-project"></a>Création d’un projet ADO  
 Microsoft JScript ne prend pas en charge les bibliothèques de types. vous n’avez donc pas besoin de référencer ADO dans votre projet. Par conséquent, aucune fonctionnalité associée, telle que l’exécution de la ligne de commande, n’est prise en charge. En outre, par défaut, les constantes énumérées ADO ne sont pas définies en JScript.  
  
 Toutefois, ADO vous fournit deux fichiers Include contenant les définitions suivantes à utiliser avec JScript :  
  
-   Pour les scripts côté serveur, utilisez Adojavas. Inc, qui est installé dans les dossiers de la bibliothèque ADO.  
  
-   Pour les scripts côté client, utilisez Adcjavas. Inc, qui est installé dans les dossiers de la bibliothèque ADO.  
  
 Vous pouvez copier et coller des définitions constantes à partir de ces fichiers dans vos pages ASP, ou, si vous effectuez des scripts côté serveur, copier le fichier Adojavas. Inc dans un dossier sur votre site Web et le référencer à partir de votre page ASP comme suit :  
  
```javascript
<!--#include File="adojavas.inc"-->  
```  
  
## <a name="creating-ado-objects-in-jscript"></a>Création d’objets ADO en JScript  
 Vous devez à la place utiliser l’appel de fonction **CreateObject** :  
  
```javascript
var Rs1;  
Rs1 = Server.CreateObject("ADODB.Recordset");  
```  
  
## <a name="jscript-example"></a>Exemple JScript  
 Le code suivant est un exemple générique de programmation côté serveur JScript dans un fichier de page Active Server (ASP) qui ouvre un objet **Recordset** :  
  
```javascript
<%  @LANGUAGE="JScript" %>  
<!--#include File="adojavas.inc"-->  
<HTML>  
<BODY BGCOLOR="White" topmargin="10" leftmargin="10">  
<%  
var Source = "SELECT * FROM Authors";  
var Connect =  "Provider=sqloledb;Data Source=srv;" +  
    "Initial Catalog=Pubs;Integrated Security=SSPI;"  
var Rs1 = Server.CreateObject( "ADODB.Recordset.2.5" );  
Rs1.Open(Source,Connect,adOpenForwardOnly);  
Response.Write("Success!");  
%>  
</BODY>  
</HTML>  
```
