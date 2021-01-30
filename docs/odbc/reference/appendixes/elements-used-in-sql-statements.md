---
description: Éléments utilisés dans les instructions SQL
title: Éléments utilisés dans les instructions SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL statements [ODBC], elements supported
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 85777525-1555-4731-8309-63a464c6b43a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5911de3ce17fc0557b9fad885fa7cdc6ce5024ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194870"
---
# <a name="elements-used-in-sql-statements"></a>Éléments utilisés dans les instructions SQL
Les éléments suivants sont utilisés dans les instructions SQL répertoriées précédemment.  
  
## <a name="element"></a>Élément  
 *base-table-identifier* :: = *nom défini par l’utilisateur*  
  
 *base-table-Name* :: = *base-table-identifier*  
  
 *Boolean-Factor* :: = [not] *booléen-Primary*  
  
 *Boolean-Primary* :: = comparaison *-prédicat* &#124; ( *condition de recherche* )  
  
 *Boolean-term* :: = *Boolean-Factor* [et *Boolean-term*]  
  
 *caractère-String-Literal* :: = ' ' {*caractère*}... ' ' (le *caractère* est n’importe quel caractère dans le jeu de caractères du pilote/de la source de données. Pour inclure un guillemet littéral simple (' ') dans un littéral de chaîne de caractères, utilisez deux guillemets littéraux [' ' ' '].)  
  
 *Column-identifier* :: = *nom défini par l’utilisateur*  
  
 *Column-Name* :: = [*table-Name*.] *identificateur de colonne*  
  
 *Comparison-opérateur* :: = < &#124; > &#124; \<= &#124; > = &#124; = &#124; <>  
  
 *comparaison-prédicat* :: = *expression* Comparison-opérateur expression  
  
 *Data-type* :: = *character-string-type* (*caractère-String-type* est un type de données pour lequel la colonne « data_type » dans le jeu de résultats renvoyé par SQLGetTypeInfo est SQL_CHAR ou SQL_VARCHAR.)  
  
 *digit* :: = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; 9  
  
 *paramètre dynamique* :: = ?  
  
 *expression* :: = terme &#124; expression {+&#124;-} term  
  
 *facteur* :: = [ *+*&#124;*-* ]*principal*  
  
 *Insert-value* :: =  
  
 *paramètre dynamique*  
  
 *Littéral* &#124;  
  
 &#124; NULL  
  
 UTILISATEUR &#124;  
  
 *Letter ::* = *minuscule-case-minuscule &#124; lettre majuscule*  
  
 *Literal* :: = *caractère-String-Literal*  
  
 *minuscules ::* = a &#124; b &#124; c &#124; d &#124; e &#124; f &#124; g &#124; h &#124; i &#124; j &#124; k &#124; l &#124; m &#124; n &#124; o &#124; p &#124; q &#124; r &#124; s &#124; t &#124; u &#124; v &#124; w &#124; x &#124; y &#124; z  
  
 *commande par clause* :: = Order par *sort-Specification* [, *sort-Specification*]...  
  
 *principal* :: = *Column-Name*  
  
 &#124; *paramètre dynamique*  
  
 *Littéral* &#124;  
  
 &#124; ( *expression* )  
  
 *recherche-condition* :: = *Boolean-term* [ou *condition de recherche*]  
  
 *Select-List* :: = \* &#124; *Select-List* [, *Select-Sublist*]...  (*Select-List* ne peut pas contenir de paramètres.)  
  
 *Select-Sublist* :: = *expression*  
  
 *sort-Specification* :: = {*unsigned-Integer &#124; nom-colonne*} [*ASC &#124; DESC*]  
  
 *table-identifier* :: = *nom défini par l’utilisateur*  
  
 *table-Name* :: = *table-identifier*  
  
 *table-Reference* :: = *table-Name*  
  
 *table-reference-list* :: = *table-Reference* [,*table-Reference*]...  
  
 *terme* :: = *facteur* &#124; *terme* { \*&#124;*/* }   
  
 *unsigned-entier* :: = {*digit*}  
  
 *majuscule-* majuscule :: = *A &#124; B &#124; C &#124; D &#124; E &#124; F &#124; G &#124; H &#124; I &#124; J &#124; K &#124; L &#124; M &#124; N &#124; o &#124; P &#124; Q &#124; R &#124; S &#124; T &#124; U &#124; V &#124; W &#124; X &#124; Y &#124; Z*  
  
 *nom défini par l’utilisateur* :: = *lettre*[*chiffre* &#124; *lettre* &#124; *_*]...
