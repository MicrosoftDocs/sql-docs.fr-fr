---
title: Considérations relatives à la sécurité XML (SQLXML)
description: En savoir plus sur les considérations importantes relatives à la sécurité lors de la spécification du mode FOR XML AUTO dans SQLXML 4,0.
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- NESTED mode
- client-side XML formatting
- FOR XML clause, security
- server-side XML formatting
- AUTO mode
- security [SQLXML], FOR XML
ms.assetid: facba279-df93-475b-ad43-0043dc5bae03
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8dbfe261078d35579bdc97a82cbef90abff289bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467120"
---
# <a name="for-xml-security-considerations-sqlxml-40"></a>Considérations relatives à la sécurité de FOR XML (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Le mode AUTO de FOR XML génère une hiérarchie XML dans laquelle les noms d'éléments sont mappés aux noms de tables et les noms d'attributs sont mappés aux noms de colonnes. Les informations de colonne et de table de la base de données sont de ce fait exposées. Vous pouvez masquer les informations de la base de données lorsque vous utilisez le mode AUTO (mise en forme côté serveur) en spécifiant des alias de table et de colonne dans la requête. Ces alias sont retournés dans le document XML résultant comme noms d'éléments et noms d'attributs.  
  
 Par exemple, la requête suivante spécifie le mode AUTO. La mise en forme XML est par conséquent réalisée sur le serveur :  
  
```  
SELECT C.FirstName as F,C.LastName as L   
FROM Person.Contact C   
FOR XML AUTO  
```  
  
 Dans le document XML résultant, les alias sont utilisés pour les noms d'éléments et les noms d'attributs :  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <C F="Nancy" L="Fuller" />   
  <CE F="Andrew" L="Peacock" />   
  <C F="Janet" L="Leverling" />   
  ...  
</root>  
```  
  
 Lorsque vous utilisez le mode NESTED (mise en forme côté client), les alias sont retournés uniquement pour les attributs figurant dans le document XML résultant. Les noms des tables de base sont toujours retournés comme noms d'éléments. Par exemple, la requête suivante spécifie le mode NESTED.  
  
```  
SELECT C.FirstName as F,C.LastName as L   
FROM Person.Contact C   
FOR XML AUTO  
```  
  
 Dans le document XML résultant, les noms des tables de base sont retournés comme noms d'éléments et les alias de tables ne sont pas utilisés :  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Person.Contact F="Nancy" L="Fuller" />   
  <Person.Contact F="Andrew" L="Peacock" />   
  <Person.Contact F="Janet" L="Leverling" />   
       ...  
</root>  
```  
  
  
