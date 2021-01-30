---
description: 'C en SQL : Timestamp'
title: 'C en SQL : horodateur | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from C to SQL types [ODBC], timestamp
- timestamp data type [ODBC]
- converting data from c to SQL types [ODBC], timestamp
ms.assetid: 0e08bfff-68f9-4648-9558-09b57fea08ad
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 54632816fb7013639e1cdb761fbc8df53c461a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158583"
---
# <a name="c-to-sql-timestamp"></a>C en SQL : Timestamp
L’identificateur pour le type de données ODBC C d’horodatage est :  
  
 SQL_C_TYPE_TIMESTAMP  
  
 Le tableau suivant présente les types de données SQL ODBC dans lesquels les données timestamp C peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|Longueur d’octet de colonne >= longueur d’octet de caractère<br /><br /> 19 <= longueur d’octet de colonne < longueur d’octet de caractère<br /><br /> Longueur d’octet de colonne < 19<br /><br /> La valeur des données n’est pas un horodateur valide|n/a<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|Longueur de caractère de colonne >= longueur de caractère des données<br /><br /> 19 <= longueur de caractère de colonne < longueur de caractère des données<br /><br /> Longueur des caractères de la colonne < 19<br /><br /> La valeur des données n’est pas un horodateur valide|n/a<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|Les champs d’heure sont nuls<br /><br /> Les champs d’heure sont non nuls<br /><br /> La valeur des données ne contient pas de date valide|n/a<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIME|Les champs de fractions de seconde sont nuls [a]<br /><br /> Les champs de fractions de seconde sont différents de zéro [a]<br /><br /> La valeur des données ne contient pas d’heure valide|n/a<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|Les champs de fractions de seconde ne sont pas tronqués<br /><br /> Les champs de fractions de seconde sont tronqués<br /><br /> La valeur des données n’est pas un horodateur valide|n/a<br /><br /> 22008<br /><br /> 22007|  
  
 [a] les champs de date de la structure d’horodatage sont ignorés.  
  
 Pour plus d’informations sur les valeurs qui sont valides dans une structure SQL_C_TIMESTAMP, consultez [types de données C](../../../odbc/reference/appendixes/c-data-types.md), plus haut dans cette annexe.  
  
 Lorsque les données timestamp C sont converties en données SQL de type caractère, les données de type caractère obtenues sont au format «*yyyy* - *mm* - *DD* *hh*:*mm*:*SS*[.*f...*]».  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion des données du type de données timestamp C et suppose que la taille de la mémoire tampon de données est égale à la taille du type de données timestamp C. La valeur de longueur/indicateur est passée dans l’argument *StrLen_Or_Ind* dans **SQLPutData** et dans la mémoire tampon spécifiée avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**. La mémoire tampon de données est spécifiée avec l’argument *DataPtr* dans **SQLPutData** et l’argument *ParameterValuePtr* dans **SQLBindParameter**.
