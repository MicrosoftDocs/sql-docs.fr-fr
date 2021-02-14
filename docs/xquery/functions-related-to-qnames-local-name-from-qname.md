---
title: local-name-from-QName (XQuery) | Microsoft Docs
description: Découvrez comment utiliser la fonction local-name-from-QName () pour retourner la partie locale du nom d’un QName.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:local-name-from-QName function
- local-name-from-QName function
ms.assetid: fafed718-8c3c-403f-93ee-ec51fc157a6e
author: rothja
ms.author: jroth
ms.openlocfilehash: 97852b8479df926a933e34f5a8dddff7eb240e65
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345005"
---
# <a name="functions-related-to-qnames---local-name-from-qname"></a>Fonctions relatives aux QName : local-name-from-QName
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Retourne un XS : NCNAME qui représente la partie locale de QName spécifiée par *$arg*. Le résultat est une séquence vide si *$arg* est la séquence vide.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
fn:local-name-from-QName($arg as xs:QName?) as xs:NCName?  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 QName d'où le nom local doit être extrait.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockées dans différentes colonnes de type **XML** dans la [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données.  
  
 L’exemple suivant utilise la fonction **local-name-from-QName ()** pour récupérer les parties de nom local et d’URI d’espace de noms à partir d’une valeur de type QName. Cet exemple illustre les opérations suivantes :  
  
-   La requête crée une collection de schémas XML.  
  
-   Elle crée ensuite une table possédant une colonne de type xml. Ce type est typé par le biais de la collection de schémas XML.  
  
-   Elle stocke enfin une instance XML servant d'échantillon dans la table. À l’aide de la méthode **query ()** du type de données XML, l’expression de requête est exécutée pour récupérer la partie du nom local de la valeur de type QName de l’instance.  
  
```sql
DROP TABLE T  
go  
DROP XML SCHEMA COLLECTION SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="QNameXSD" >  
      <element name="root" type="QName" nillable="true"/>  
</schema>'  
go  
  
CREATE TABLE T (xmlCol XML(SC))  
go  
-- following OK  
insert into T values ('<root xmlns="QNameXSD" xmlns:a="https://someURI">a:someLocalName</root>')  
 go  
-- Retrieve the local name.   
SELECT xmlCol.query('declare default element namespace "QNameXSD"; local-name-from-QName(/root[1])')  
FROM T  
-- Result = someLocalName  
-- You can retrieve namespace URI part from the QName using the namespace-uri-from-QName() function  
SELECT xmlCol.query('declare default element namespace "QNameXSD"; namespace-uri-from-QName(/root[1])')  
FROM T  
-- Result = https://someURI  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions liées à QNames &#40;XQuery&#41;](./functions-related-to-qnames-expanded-qname.md)  
  
