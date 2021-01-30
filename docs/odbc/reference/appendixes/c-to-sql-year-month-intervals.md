---
description: 'C en SQL : Intervalles d’années-mois'
title: 'C en SQL : intervalles de Year-Month | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], year-month intervals
- intervals [ODBC], converting
- year-month intervals [ODBC]
- data conversions from C to SQL types [ODBC], year-month intervals
ms.assetid: a0eb7b55-9db0-4375-9210-bddec4593880
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0748585beeae18a0b159cf131a67b4b5ea3cdd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158576"
---
# <a name="c-to-sql-year-month-intervals"></a>C en SQL : Intervalles d’années-mois
Les identificateurs pour les types de données ODBC C de l’intervalle année-mois sont :  
  
 SQL_C_INTERVAL_MONTH SQL_C_INTERVAL_YEAR SQL_C_INTERVAL_YEAR_TO_MONTH  
  
 Le tableau suivant présente les types de données SQL ODBC dans lesquels les données de l’intervalle d’une année-mois peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR [a]<br /><br /> SQL_VARCHAR [a]<br /><br /> SQL_LONGVARCHAR [a]|Longueur d’octet de colonne >= longueur d’octet de caractère<br /><br /> Longueur d’octet de colonne < longueur d’octet de caractère [a]<br /><br /> La valeur des données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22001<br /><br /> 22015|  
|SQL_WCHAR [a]<br /><br /> SQL_WVARCHAR [a]<br /><br /> SQL_WLONGVARCHAR [a]|Longueur de caractère de colonne >= longueur de caractère des données<br /><br /> Longueur des caractères de la colonne < de la longueur des données [a]<br /><br /> La valeur des données n’est pas un littéral d’intervalle valide|n/a<br /><br /> 22001<br /><br /> 22015|  
|SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b]<br /><br /> SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b]<br /><br /> SQL_NUMERIC [b]<br /><br /> SQL_DECIMAL [b]|La conversion d’un intervalle à champ unique n’a pas abouti à la troncation de chiffres entiers<br /><br /> La conversion a entraîné la troncation de chiffres entiers|n/a<br /><br /> 22003|  
|SQL_INTERVAL_MONTH<br /><br /> SQL_INTERVAL_YEAR<br /><br /> SQL_INTERVAL_YEAR_TO_MONTH|La valeur des données a été convertie sans troncation des champs<br /><br /> Un ou plusieurs champs de la valeur de données ont été tronqués pendant la conversion|n/a<br /><br /> 22015|  
  
 [a] tous les types de données d’intervalle C peuvent être convertis en type de données caractère.  
  
 [b] si le champ type de la structure interval est tel que l’intervalle est un champ unique (SQL_YEAR ou SQL_MONTH), le type d’intervalle C peut être converti en n’importe quel nombre exact (SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER, SQL_BIGINT, SQL_DECIMAL ou SQL_NUMERIC).  
  
 La conversion par défaut d’un type d’intervalle C est le type SQL de l’intervalle année-mois correspondant.  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion de données à partir du type de données Interval C et suppose que la taille de la mémoire tampon de données est égale à la taille du type de données Interval C. La valeur de longueur/indicateur est passée dans l’argument *StrLen_Or_Ind* dans **SQLPutData** et dans la mémoire tampon spécifiée avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**. La mémoire tampon de données est spécifiée avec l’argument *DataPtr* dans **SQLPutData** et l’argument *ParameterValuePtr* dans **SQLBindParameter**.
