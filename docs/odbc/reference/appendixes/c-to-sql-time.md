---
description: 'C en SQL : Temps'
title: 'C en SQL : heure | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from C to SQL types [ODBC], time
- time data type [ODBC]
- converting data from c to SQL types [ODBC], time
ms.assetid: a8da43c9-d9a5-45e5-bd9a-1dd633db2ee0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b53505a9f10d6f7039e0811e5f9fda8674092be1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195068"
---
# <a name="c-to-sql-time"></a>C en SQL : Temps
L’identificateur pour le type de données ODBC C Time est :  
  
 SQL_C_TYPE_TIME  
  
 Le tableau suivant présente les types de données SQL ODBC dans lesquels les données de temps C peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longueur d’octet de colonne >= 8<br /><br /> Longueur d’octet de colonne < 8<br /><br /> La valeur des données n’est pas une heure valide|n/a<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longueur des caractères de la colonne >= 8<br /><br /> Longueur des caractères de la colonne < 8<br /><br /> La valeur des données n’est pas une heure valide|n/a<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_TIME|La valeur des données est une heure valide<br /><br /> La valeur des données n’est pas une heure valide|n/a<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|La valeur des données est une heure valide [a]<br /><br /> La valeur des données n’est pas une heure valide|n/a<br /><br /> 22007|  
  
 [a] la partie Date de l’horodateur est définie sur la date actuelle et la partie fractions de seconde de l’horodatage est définie sur zéro.  
  
 Pour plus d’informations sur les valeurs qui sont valides dans une structure SQL_C_TYPE_TIME, consultez [types de données C](../../../odbc/reference/appendixes/c-data-types.md), plus haut dans cette annexe.  
  
 Lorsque les données de temps C sont converties en données SQL de caractères, les données de caractères résultantes sont au format «*hh*:*mm*:*SS*».  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion de données à partir du type de données Time C et suppose que la taille de la mémoire tampon de données est égale à la taille du type de données Time C. La valeur de longueur/indicateur est passée dans l’argument *StrLen_Or_Ind* dans **SQLPutData** et dans la mémoire tampon spécifiée avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**. La mémoire tampon de données est spécifiée avec l’argument *DataPtr* dans **SQLPutData** et l’argument *ParameterValuePtr* dans **SQLBindParameter**.
