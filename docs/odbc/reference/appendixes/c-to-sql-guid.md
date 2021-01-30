---
description: 'C en SQL : GUID'
title: 'C en SQL : GUID | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], guid
- data conversions from C to SQL types [ODBC], guid
- GUID data type [ODBC]
ms.assetid: 9168b0b6-a828-4fef-b8cd-bdf439776f23
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 22b47fa414244d777ec686f146834f28adff4868
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210018"
---
# <a name="c-to-sql-guid"></a>C en SQL : GUID
L’identificateur pour le type de données GUID ODBC C est le suivant :  
  
 SQL_C_GUID  
  
 Le tableau suivant présente les types de données SQL ODBC dans lesquels les données GUID C peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md).  
  
|Identificateur de type SQL|Test|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR|Longueur d’octet de colonne >= 36|n/a|  
|SQL_VARCHAR|Longueur d’octet de colonne < 36|22001|  
|SQL_LONGVARCHAR|La valeur des données n’est pas un GUID valide|22018|  
|SQL_WCHAR|Longueur des caractères de la colonne >= 36|n/a|  
|SQL_WVARCHAR|Longueur des caractères de la colonne < 36|22001|  
|SQL_WLONGVARCHAR|La valeur des données n’est pas un GUID valide|22018|  
|SQL_GUID|Aucun [a]|n/a|  
  
 [a] toutes les valeurs hexadécimales sont valides en tant que GUID.  
  
 Le pilote ignore la valeur de longueur/indicateur lors de la conversion de données à partir du type de données GUID C et suppose que la taille de la mémoire tampon de données est la taille du type de données GUID C. La valeur de longueur/indicateur est passée dans l’argument *StrLen_Or_Ind* dans **SQLPutData** et dans la mémoire tampon spécifiée avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**. La mémoire tampon de données est spécifiée avec l’argument *DataPtr* dans **SQLPutData** et l’argument *ParameterValuePtr* dans **SQLBindParameter**.
