---
title: Utilisation de la syntaxe abrégée dans une expression de chemin d’accès | Microsoft Docs
description: Découvrez comment utiliser la syntaxe abrégée dans les expressions de chemin d’accès XQuery.
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
- axis step [XQuery]
- abbreviated syntax [XQuery]
ms.assetid: f83c2e41-5722-47c3-b5b8-bf0f8cbe05d3
author: rothja
ms.author: jroth
ms.openlocfilehash: 959d5498011984fc1f400567517d3b5b07727fcc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339934"
---
# <a name="path-expressions---using-abbreviated-syntax"></a>Expressions de chemin : utilisation de la syntaxe abrégée
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Tous les exemples de [compréhension des expressions de chemin d’accès dans XQuery](../xquery/path-expressions-xquery.md) utilisent une syntaxe unabrégée pour les expressions de chemin d’accès. La syntaxe non abrégée pour une étape d'axe dans une expression de chemin d'accès inclut le nom de l'axe et le test du nœud, séparés par un double signe deux-points et suivis le cas échéant par des qualificatifs d'étape.  
  
 Par exemple :  
  
```  
child::ProductDescription[attribute::ProductModelID=19]  
```  
  
 XQuery prend en charge les abréviations suivantes lors de l'utilisation d'expressions de chemin d'accès :  
  
-   L’axe **enfant** est l’axe par défaut. Par conséquent, l' **enfant ::** AXIS peut être omis dans une étape d’une expression. Par exemple, `/child::ProductDescription/child::Summary` peut s'écrire sous la forme `/ProductDescription/Summary`.  
  
-   Un axe d' **attribut** peut être abrégé comme @ . Par exemple, `/child::ProductDescription[attribute::ProductModelID=10]` peut s'écrire sous la forme `/ProudctDescription[@ProductModelID=10]`.  
  
-   Un **/descendant-or-self :: node ()/** peut être abrégé sous la forme//. Par exemple, `/descendant-or-self::node()/child::act:telephoneNumber` peut s'écrire sous la forme `//act:telephoneNumber`.  
  
     La requête précédente récupère tous les numéros de téléphone stockés dans la colonne AdditionalContactInfo de la table Contact. Le schéma pour AdditionalContactInfo est défini de telle sorte qu’un \<telephoneNumber> élément puisse apparaître n’importe où dans le document. Ainsi, pour retrouver tous les numéros de téléphone, vous devez rechercher tous les nœuds du document. Cette recherche part de la racine du document et se poursuit à travers tous les nœuds descendants.  
  
     La requête suivante permet de récupérer tous les numéros de téléphone pour un client donné d'après ses informations de contact :  
  
    ```  
    SELECT AdditionalContactInfo.query('             
                declare namespace act="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactTypes";             
                declare namespace crm="https://schemas.adventure-works.com/Contact/Record";             
                declare namespace ci="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ContactInfo";             
                /descendant-or-self::node()/child::act:telephoneNumber             
                ') as result             
    FROM Person.Contact             
    WHERE ContactID=1             
    ```  
  
     Si vous remplacez l'expression de chemin d'accès par sa syntaxe abrégée `//act:telephoneNumber`, vous obtenez les mêmes résultats.  
  
-   Le **self :: node ()** d’une étape peut être abrégé en un seul point (.). Toutefois, le point n’est pas équivalent ou interchangeable avec le **self :: node ()**.  
  
     Par exemple, utiliser un point dans la requête suivante revient à représenter une valeur et non un nœud :  
  
    ```  
    ("abc", "cde")[. > "b"]  
    ```  
  
-   Le **parent :: node ()** d’une étape peut être abrégé en point double (..).  
  
  
