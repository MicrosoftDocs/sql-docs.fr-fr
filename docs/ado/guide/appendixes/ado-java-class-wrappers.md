---
description: Wrappers de classe Java ADO
title: Wrappers de classe Java ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- class wrappers [ADO]
ms.assetid: 1fc09dc1-9e32-412e-9f43-b8eb8bb483ca
author: rothja
ms.author: jroth
ms.openlocfilehash: 79d5b19f9d59f648305f2c5719401d44a9a00fed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029493"
---
# <a name="ado-java-class-wrappers"></a>Wrappers de classe Java ADO
Ce code déclare une instance du wrapper de la classe [Recordset](../../reference/ado-api/recordset-object-ado.md) ADO et l’initialise, le tout sur la même ligne de code. En outre, il déclare des variables pour chacun des arguments de la méthode [Open](../../reference/ado-api/open-method-ado-recordset.md) , en particulier pour [LockType](../../reference/ado-api/locktype-property-ado.md) et [CursorType](../../reference/ado-api/cursortype-property-ado.md) (car Java ne prend pas en charge les types énumérés). Il ouvre et ferme l’objet **Recordset** . La définition de RS1 sur NULL planifie simplement cette variable à libérer lorsque Java effectue sa mise en version systématique et intermittente des objets inutilisés.  
  
```java
public static void main( String args[])  
{  
   msado15._Recordset   Rs1 = new msado15.Recordset();  
   Variant Source     = new Variant( "SELECT * FROM Authors" );  
   Variant Connect    = new Variant( "DSN=AdoDemo;UID=admin;PWD=;" );  
   int     LockType   = msado15.CursorTypeEnum.adOpenForwardOnly;  
   int     CursorType = msado15.LockTypeEnum.adLockReadOnly;  
   int     Options    = -1;  
  
   Rs1.Open( Source, Connect, LockType,  CursorType, Options );  
   Rs1.Close();  
   Rs1 = null;  
  
   System.out.println( "Success!\n" );  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Utilisation du SDK Microsoft pour Java](./using-the-microsoft-sdk-for-java.md)