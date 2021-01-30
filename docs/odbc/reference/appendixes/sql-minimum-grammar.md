---
description: Grammaire minimale de SQL
title: Grammaire minimale SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
ms.assetid: 4f36d785-104f-4fec-93be-f201203bc7c7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6d611fb753034a7cde370f87700aa8b8c3db2e11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159079"
---
# <a name="sql-minimum-grammar"></a>Grammaire minimale de SQL
Cette section décrit la syntaxe SQL minimale qu’un pilote ODBC doit prendre en charge. La syntaxe décrite dans cette section est un sous-ensemble de la syntaxe de niveau d’entrée de SQL-92.  
  
 Une application peut utiliser l’une des syntaxes de cette section et être assurée que tout pilote compatible ODBC prendra en charge cette syntaxe. Pour déterminer si des fonctionnalités supplémentaires de SQL-92 ne sont pas prises en charge dans cette section, l’application doit appeler **SQLGetInfo** avec le type d’informations SQL_SQL_CONFORMANCE. Même si le pilote n’est conforme à aucun niveau de conformité SQL-92, une application peut toujours utiliser la syntaxe décrite dans cette section. Si un pilote est conforme à un niveau SQL-92, en revanche, il prend en charge toutes les syntaxes incluses dans ce niveau. Cela comprend la syntaxe de cette section, car la grammaire minimale décrite ici est un sous-ensemble pur du niveau de conformité SQL-92 le plus bas. Une fois que l’application connaît le niveau SQL-92 pris en charge, elle peut déterminer si une fonctionnalité de niveau supérieur est prise en charge (le cas échéant) en appelant **SQLGetInfo** avec le type d’informations individuel correspondant à cette fonctionnalité.  
  
 Les pilotes qui fonctionnent uniquement avec des sources de données en lecture seule peuvent ne pas prendre en charge les parties de la grammaire incluses dans cette section qui traitent des données modifiées. Une application peut déterminer si une source de données est en lecture seule en appelant **SQLGetInfo** avec le type d’informations SQL_DATA_SOURCE_READ_ONLY.  
  
## <a name="statement"></a>.  
 *Create-table-Statement* :: =  
  
 Nom de la *table de base* de CREATE TABLE  
  
 (*type de données de l’identificateur de colonne* [*, identificateur de colonne-type de données*]...)  
  
> [!IMPORTANT]  
>  En tant que *type de données* dans une *instruction CREATE TABLE*, les applications doivent utiliser un type de données de la colonne TYPE_NAME du jeu de résultats retourné par **SQLGetTypeInfo**.  
  
 *Delete-Statement-recherchée* :: =  
  
 SUPPRIMER de *table-Name* [where *Search-condition*]  
  
 *drop-table-Statement* :: =  
  
 DROP TABLE *base-table-Name*  
  
 *Insert-Statement* :: =  
  
 INSERT dans *table-Name* [( *Column-identifier* [, *Column-identifier*]...)]      VALEURS (*Insert-value*[, *Insert-value*]...)  
  
 *select-statement* ::=  
  
 SELECT [ALL &#124; DISTINCT] *Select-List*  
  
 À partir de *table-reference-list*  
  
 [WHERE *recherche-condition*]  
  
 [*commande par clause*]  
  
 *instruction* :: = *Create-table-Statement*  
  
 *Instruction DELETE-Statement-recherchée* &#124;  
  
 &#124; *drop-table-Statement*  
  
 &#124; *instruction INSERT*  
  
 *Instruction select* &#124;  
  
 &#124; *Update-instruction-recherchée*  
  
 *Update-instruction-recherchée*  
  
 METTRE à jour *le nom de la table*  
  
 SET *Column-identifier* = {*expression* &#124; null}  
  
 [, *Column-identifier* = {*expression* &#124; NULL}]...  
  
 [WHERE *recherche-condition*]  
  
 Cette section contient les rubriques suivantes :  
  
-   [Éléments utilisés dans les instructions SQL](../../../odbc/reference/appendixes/elements-used-in-sql-statements.md)  
  
-   [Prise en charge du type de données](../../../odbc/reference/appendixes/data-type-support.md)  
  
-   [Types de données de paramètre](../../../odbc/reference/appendixes/parameter-data-types.md)  
  
-   [Marqueurs de paramètres](../../../odbc/reference/appendixes/parameter-markers.md)
