---
description: 'SQL à C : Numérique'
title: 'SQL en C : Numeric | Microsoft Docs'
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC], numeric
- numeric data type [ODBC], converting
- converting data from SQL to C types [ODBC], numeric
ms.assetid: 76f8b5d5-4bd0-4dcb-a90a-698340e0d36e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7f18cc2ecff847b2d7d7716e2c302de2b7bb3ff2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203017"
---
# <a name="sql-to-c-numeric"></a>SQL à C : Numérique

Les identificateurs des types de données SQL ODBC numériques sont les suivants :

- SQL_DECIMAL  
- SQL_BIGINT  
- SQL_NUMERIC  
- SQL_REAL  
- SQL_TINYINT  
- SQL_FLOAT  
- SQL_SMALLINT  
- SQL_DOUBLE SQL_INTEGER  

Le tableau suivant répertorie les types de données ODBC C dans lesquels les données SQL numériques peuvent être converties. Pour obtenir une explication des colonnes et des termes du tableau, consultez [conversion de données SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md).  

|Identificateur de type C|Test|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|Longueur en octets < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) >= *BufferLength*|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|Longueur de caractère < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) < *BufferLength*<br /><br /> Nombre de chiffres entiers (par opposition à fractionnaires) >= *BufferLength*|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données en caractères<br /><br /> Longueur des données en caractères<br /><br /> Indéfini|n/a<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_STINYINT<br /><br /> SQL_C_UTINYINT<br /><br /> SQL_C_TINYINT<br /><br /> SQL_C_SBIGINT<br /><br /> SQL_C_UBIGINT<br /><br /> SQL_C_SSHORT<br /><br /> SQL_C_USHORT<br /><br /> SQL_C_SHORT<br /><br /> SQL_C_SLONG<br /><br /> SQL_C_ULONG<br /><br /> SQL_C_LONG<br /><br /> SQL_C_NUMERIC|Données converties sans troncation [a]<br /><br /> Données converties avec troncation de chiffres fractionnaires [a]<br /><br /> La conversion de données entraînerait la perte de chiffres entiers (par opposition aux chiffres fractionnaires) [a]|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Taille du type de données C<br /><br /> Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22003|  
|SQL_C_FLOAT<br /><br /> SQL_C_DOUBLE|Les données se trouvent dans la plage du type de données dans lequel le nombre est converti [a]<br /><br /> Les données se trouvent en dehors de la plage du type de données dans lequel le nombre est converti [a]|Données<br /><br /> Indéfini|Taille du type de données C<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_BIT|Les données sont 0 ou 1 [a]<br /><br /> Les données sont supérieures à 0, inférieures à 2 et ne sont pas égales à 1 [a]<br /><br /> Les données sont inférieures à 0 ou supérieures ou égales à 2 [a]|Données<br /><br /> Données tronquées<br /><br /> Indéfini|1 [b]<br /><br /> 1 [b]<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22003|  
|SQL_C_BINARY|Longueur en octets des données <= *BufferLength*<br /><br /> Longueur en octets des données > *BufferLength*|Données<br /><br /> Indéfini|Longueur des données<br /><br /> Indéfini|n/a<br /><br /> 22003|  
|SQL_C_INTERVAL_MONTH [c] SQL_C_INTERVAL_YEAR [c] SQL_C_INTERVAL_DAY [c] SQL_C_INTERVAL_HOUR [c] SQL_C_INTERVAL_MINUTE [c] SQL_C_INTERVAL_SECOND [c]|Données non tronquées<br /><br /> Fraction de seconde fractionnaire tronquée<br /><br /> Partie entière du nombre tronqué|Données<br /><br /> Données tronquées<br /><br /> Indéfini|Longueur des données en octets<br /><br /> Longueur des données en octets<br /><br /> Indéfini|n/a<br /><br /> 01S07<br /><br /> 22015|  
|SQL_C_INTERVAL_YEAR_TO_MONTH SQL_C_INTERVAL_DAY_TO_HOUR SQL_C_INTERVAL_DAY_TO_MINUTE SQL_C_INTERVAL_DAY_TO_SECOND SQL_C_INTERVAL_HOUR_TO_MINUTE SQL_C_INTERVAL_HOUR_TO_SECOND|Partie entière du nombre tronqué|Indéfini|Indéfini|22015|  
  
 [a] la valeur de *BufferLength* est ignorée pour cette conversion. Le pilote suppose que la taille de **TargetValuePtr* est la taille du type de données C.  
  
 [b] il s’agit de la taille du type de données C correspondant.  
  
 [c] cette conversion est prise en charge uniquement pour les types de données numériques exacts (SQL_DECIMAL, SQL_NUMERIC, SQL_TINYINT, SQL_SMALLINT, SQL_INTEGER et SQL_BIGINT). Elle n’est pas prise en charge pour les types de données numériques approximatifs (SQL_REAL, SQL_FLOAT ou SQL_DOUBLE).  

## <a name="sql_c_numeric-and-sqlsetdescfield"></a>SQL_C_NUMERIC et SQLSetDescField

 La [fonction SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) est requise pour effectuer une liaison manuelle avec des valeurs SQL_C_NUMERIC. (Notez que SQLSetDescField a été ajouté dans ODBC 3,0.) Pour effectuer une liaison manuelle, vous devez d’abord récupérer le handle du descripteur.  

```cpp
if (fCType == SQL_C_NUMERIC) {   
   // special processing required for NUMERIC to get right scale & precision  
   // Modify the fields in the implicit application parameter descriptor  
   SQLHDESC hdesc=NULL;  
  
   // Use SQL_ATTR_APP_ROW_DESC for calls to SQLBindCol()  
   // Use SQL_ATTR_APP_PARAM_DESC for calls to SQLBindParameter()  
   //  
   // retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_ROW_DESC, &hdesc, 0, NULL);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);  
   if (!ODBC_CALL_SUCCESS(retcode)) {  
      printf ("\nSQLGetStmtAttr failed");  
      i = 1;  
      sqlstate[7] = '\0';  
      while (SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, i, sqlstate, &NativeError, wrkbuf, sizeof(wrkbuf), &len) != SQL_NO_DATA) {  
         printf("\niTestCase = %d Failed...Precision = %d, Scale = %d\nNativeError=%d, State=%s, \n  Message=%s",   
            iTestCase, Precision, Scale, NativeError, sqlstate, wrkbuf);  
         i++;  
      }  
      continue;  
   }  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_TYPE, (SQLPOINTER) SQL_C_NUMERIC, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_PRECISION, (SQLPOINTER)num.precision, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_SCALE, (SQLPOINTER)num.scale, 0);  
   if (!ODBC_CALL_SUCCESS(retcode))  
      goto error;  
   retcode = SQLSetDescField(hdesc, iCol, SQL_DESC_DATA_PTR, (SQLPOINTER) &(num), sizeof(num));  
```
