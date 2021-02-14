---
title: Fonction true (XQuery) | Microsoft Docs
description: En savoir plus sur la fonction XQuery true () qui retourne la valeur booléenne true.
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:true function
- true function
ms.assetid: 318e370d-0444-4812-afe4-307df7ef9f3b
author: rothja
ms.author: jroth
ms.openlocfilehash: 779e3fa093d851e99e2869053f155fb70570f67c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340498"
---
# <a name="boolean-constructor-functions---true-xquery"></a>Fonctions constructeurs booléennes : true (XQuery)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Retourne la valeur xs:boolean True. Ceci équivaut à `xs:boolean("1")`.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
fn:true() as xs:boolean  
```  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la base de données AdventureWorks.  
  
### <a name="a-using-the-true-xquery-boolean-function"></a>R. Utilisation de la fonction booléenne XQuery true()  
 L’exemple suivant interroge une variable **XML** non typée. L’expression dans la méthode **value ()** retourne la valeur booléenne **true ()** si « AAA » est la valeur de l’attribut. La méthode **value ()** du type de données **XML** convertit la valeur booléenne en un bit et la retourne.  
  
```  
DECLARE @x XML  
SET @x= '<ROOT><elem attr="aaa">bbb</elem></ROOT>'  
select @x.value(' if ( (/ROOT/elem/@attr)[1] eq "aaa" ) then fn:true() else fn:false() ', 'bit')  
go  
-- result = 1  
```  
  
 Dans l’exemple suivant, la requête est spécifiée par rapport à une colonne **XML** typée. L' `if` expression vérifie la valeur booléenne typée de l' `ROOT` élément <> et retourne le XML construit, en conséquence. Cet exemple illustre les opérations suivantes :  
  
-   Crée une collection de schémas XML qui définit le <`ROOT`> élément du type xs : Boolean.  
  
-   Crée une table avec une colonne **XML** typée à l’aide de la collection de schémas XML.  
  
-   Enregistrement de l'instance XML dans la colonne et interrogation de l'instance.  
  
```  
-- Drop table if exist  
--DROP TABLE T  
--go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="ROOT" type="boolean" nillable="true"/>  
</schema>'  
go  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<ROOT xmlns="QNameXSD">true</ROOT>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare namespace a="QNameXSD";   
   if (/a:ROOT[1] eq true()) then  
       <result>Found boolean true</result>  
   else  
       <result>Found boolean false</result>')  
  
FROM T  
-- result = <result>Found boolean true</result>  
-- Clean up  
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions de constructeur booléen &#40;XQuery&#41;](./xquery-functions-against-the-xml-data-type.md)  
  
