---
title: Requêtes XQuery gestion des données relationnelles | Microsoft Docs
description: 'Découvrez comment lier des données relationnelles non XML à XML à l’aide des extensions XQuery SQL : Column () et SQL : variable ().'
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- relational data [XQuery]
- XQuery, relational data
ms.assetid: 9812b71a-52ec-48a0-92f3-016a93660229
author: rothja
ms.author: jroth
ms.openlocfilehash: 6ac32119090ba7973ad628c35f6b5b994eb03561
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172351"
---
# <a name="xqueries-handling-relational-data"></a>Requêtes XQuery pour la gestion des données relationnelles
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Vous spécifiez XQuery sur une variable ou une colonne de type **XML** à l’aide de l’une des [méthodes de type de données XML](../t-sql/xml/xml-data-type-methods.md). Il peut s’agir des **requêtes ()**, **value ()**, **exist ()** ou **Modify ()**. La requête XQuery est exécutée par rapport à l'instance XML identifiée dans la requête qui génère le document XML.  
  
 Le document XML généré par l'exécution d'une requête XQuery peut comprendre des valeurs extraites à partir d'autres colonnes d'ensemble de lignes ou variables Transact-SQL. Pour lier des données relationnelles non-XML au document XML obtenu, SQL Server fournit les pseudo-fonctions suivantes comme extensions XQuery :  
  
-   fonction **SQL : Column ()**  
  
-   fonction **SQL : variable ()**  
  
 Vous pouvez utiliser ces extensions XQuery lors de la spécification d’une requête XQuery dans la méthode **query ()** du type de données **XML** . Par conséquent, la méthode **query ()** peut produire du code XML qui combine des données de types de données XML et non-**XML** .  
  
 Vous pouvez également utiliser ces fonctions lorsque vous utilisez les méthodes de type de données **XML** **Modify ()**, **value ()**, **query ()** et **exist ()** pour exposer une valeur relationnelle dans du code XML.  
  
 Pour plus d’informations, consultez [SQL : Column (), fonction (XQuery)](../xquery/xquery-extension-functions-sql-column.md) et [fonction SQL : variable () (XQuery)](../xquery/xquery-extension-functions-sql-variable.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Construction XML &#40;XQuery&#41;](../xquery/xml-construction-xquery.md)  
  
  
