---
title: Fonctions XQuery sur le type de données XML | Microsoft Docs
description: En savoir plus sur les fonctions XQuery prises en charge pour une utilisation sur le type de données XML.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- XQuery, functions
- xml data type [SQL Server], XQuery
- functions [SQL Server], XQuery
ms.assetid: 8df0877d-a03f-4ca9-b84e-908c4bb42b5e
author: rothja
ms.author: jroth
ms.openlocfilehash: 82f3eae7098b3ac5e86355aed1e13bdd5c137015
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352815"
---
# <a name="xquery-functions-against-the-xml-data-type"></a>Fonctions XQuery impliquant le type de données xml
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Cette rubrique et ses sous-rubriques décrivent les fonctions que vous pouvez utiliser lors de la spécification de XQuery par rapport au type de données **XML** . Pour les spécifications du W3C, consultez [http://www.w3.org/TR/2004/WD-xpath-functions-20040723](https://go.microsoft.com/fwlink/?LinkId=4873) .  
  
 Les fonctions XQuery appartiennent à l' http://www.w3.org/2004/07/xpath-functions espace de noms. Les spécifications W3C utilisent le préfixe d'espace de noms « fn: » pour décrire ces fonctions. Vous n'avez pas besoin de spécifier ce préfixe explicitement lorsque vous utilisez les fonctions. Par conséquent, et dans un souci de lisibilité, les préfixes d'espace de noms ne sont généralement pas utilisés dans cette documentation.  
  
 Le tableau suivant répertorie les fonctions XQuery prises en charge par rapport au type de données **XML**.  
  
|Category|Nom de fonction|  
|--------------|-------------------|  
|[Fonctions sur des valeurs numériques]()|[plafond](../xquery/numeric-values-functions-ceiling.md)|  
||[floor](../xquery/numeric-values-functions-floor.md)|  
||[Round](../xquery/numeric-values-functions-round.md)|  
|[Fonctions XQuery sur des valeurs de chaîne]()|[concat](../xquery/functions-on-string-values-concat.md)|  
||[contains](../xquery/functions-on-string-values-contains.md)|  
||[substring](../xquery/functions-on-string-values-substring.md)|  
||[Fonction en minuscules &#40;XQuery&#41;](../xquery/functions-on-string-values-lower-case.md)|  
||[chaîne-longueur](../xquery/functions-on-string-values-string-length.md)|  
||[Fonction majuscule &#40;XQuery&#41;](../xquery/functions-on-string-values-upper-case.md)|  
|Fonctions sur les valeurs booléennes|[not](../xquery/functions-on-boolean-values-not-function.md)|  
|[Fonctions sur les nœuds]()|[number](../xquery/functions-on-nodes-number.md)|  
||[Fonction local-name (XQuery)](../xquery/functions-on-nodes-local-name.md)|  
||[Fonction namespace-uri (XQuery)](../xquery/functions-on-nodes-namespace-uri.md)|  
|[Fonctions de contexte]()|[last](../xquery/context-functions-last-xquery.md)|  
||[position](../xquery/context-functions-position-xquery.md)|  
|[Fonctions sur les séquences]()|[empty](../xquery/functions-on-sequences-empty.md)|  
||[distinct-values](../xquery/functions-on-sequences-distinct-values.md)|  
||[Fonction id (XQuery)](../xquery/functions-on-sequences-id.md)|  
|[Fonctions d’agrégation &#40;XQuery&#41;]()|[count](../xquery/aggregate-functions-count.md)|  
||[AVG](../xquery/aggregate-functions-avg.md)|  
||[min](../xquery/aggregate-functions-min.md)|  
||[max](../xquery/aggregate-functions-max.md)|  
||[sum](../xquery/aggregate-functions-sum.md)|  
|[Fonctions de constructeur &#40;XQuery&#41;](../xquery/constructor-functions-xquery.md)|[Fonctions constructeur](../xquery/constructor-functions-xquery.md)|  
|[Fonctions d'accesseurs de données](../xquery/data-accessor-functions.md)|[string](../xquery/data-accessor-functions-string-xquery.md)|  
||[data](../xquery/data-accessor-functions-data-xquery.md)|  
|[Fonctions de constructeur booléen &#40;XQuery&#41;]()|[Fonction true (XQuery)](../xquery/boolean-constructor-functions-true-xquery.md)|  
||[Fonction false (XQuery)](../xquery/boolean-constructor-functions-false-xquery.md)|  
|[Fonctions liées à QNames &#40;XQuery&#41;](./functions-related-to-qnames-expanded-qname.md)|[expanded-QName (XQuery)](../xquery/functions-related-to-qnames-expanded-qname.md)|  
||[local-name-from-QName (XQuery)](../xquery/functions-related-to-qnames-local-name-from-qname.md)|  
||[namespace-uri-from-QName (XQuery)](../xquery/functions-related-to-qnames-namespace-uri-from-qname.md)|  
|[Fonctions d'extension SQL Server XQuery](./xquery-extension-functions-sql-column.md)|[fonction SQL : Column () (XQuery)](../xquery/xquery-extension-functions-sql-column.md)|  
||[Fonction sql:variable() (XQuery)](../xquery/xquery-extension-functions-sql-variable.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [méthodes de type de données xml](../t-sql/xml/xml-data-type-methods.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
