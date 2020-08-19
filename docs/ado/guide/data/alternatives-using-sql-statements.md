---
description: 'Alternatives : Utilisation d’instructions SQL'
title: 'Alternatives : utilisation des instructions SQL | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- SQL statements [ADO]
- editing data [ADO], sql statements
- ADO, SQL statements
ms.assetid: 8b528b23-063d-45ea-8dea-6a90d4060b20
author: rothja
ms.author: jroth
ms.openlocfilehash: f71afb691aa170910e93f73ac539e1b69a691bca
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88453741"
---
# <a name="alternatives-using-sql-statements"></a>Alternatives : Utilisation d’instructions SQL
ADO permet également d’utiliser des commandes comme alternatives à ses propriétés et méthodes intégrées pour la modification des données. Selon votre fournisseur, toutes les opérations mentionnées dans cette section peuvent également être accomplies en passant des commandes à votre source de données. Par exemple, les instructions SQL UPDATE peuvent être utilisées pour modifier des données sans utiliser la propriété **value** d’un **champ**. Les instructions SQL INSERT peuvent être utilisées pour ajouter de nouveaux enregistrements à une source de données, plutôt que dans la méthode ADO **AddNew**. Pour plus d’informations sur SQL ou le langage de manipulation de données de votre fournisseur, consultez la documentation de votre source de données.  
  
 Par exemple, vous pouvez passer une chaîne SQL contenant une instruction DELETE à une base de données, comme le montre le code suivant :  
  
```  
'BeginSQLDelete  
strSQL = "DELETE FROM Shippers WHERE ShipperID = " & intId  
objConn1.Execute strSQL, , adCmdText + adExecuteNoRecords  
'EndSQLDelete  
```
