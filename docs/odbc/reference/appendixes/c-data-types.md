---
description: Type de données C
title: Types de données C | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], C data types
- C data types [ODBC], about C data types
- C data types [ODBC]
- C buffers [ODBC]
ms.assetid: b681d260-3dbb-47df-a616-4910d727add7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 322aa55f007e4ebf1ba3fa2b3093bf750db7e678
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051890"
---
# <a name="c-data-types"></a>Type de données C
Les types de données ODBC C indiquent le type de données des mémoires tampons C utilisées pour stocker les données dans l’application.  
  
 Tous les pilotes doivent prendre en charge tous les types de données C. Cela est nécessaire, car tous les pilotes doivent prendre en charge tous les types C auxquels les types SQL qu’ils prennent en charge peuvent être convertis, et tous les pilotes prennent en charge au moins un caractère de type SQL. Étant donné que le type SQL de caractère peut être converti vers et à partir de tous les types C, tous les pilotes doivent prendre en charge tous les types C.  
  
 Le type de données C est spécifié dans les fonctions **SQLBindCol** et **SQLGetData** avec l’argument *TargetType* et dans la fonction **SQLBindParameter** avec l’argument *ValueType* . Il peut également être spécifié en appelant **SQLSetDescField** pour définir le champ SQL_DESC_CONCISE_TYPE d’un ARD ou APD, ou en appelant **SQLSetDescRec** avec l’argument de *type* (et l’argument de *sous-type* si nécessaire) et l’argument *DESCRIPTORHANDLE* défini sur le handle d’un ARD ou APD.  
  
 Les tableaux suivants répertorient les identificateurs de type valides pour les types de données C. Le tableau répertorie également le type de données ODBC C qui correspond à chaque identificateur et la définition de ce type de données.  
  
|Identificateur de type C|Typedef C ODBC|Type C|  
|-----------------------|--------------------|------------|  
|SQL_C_CHAR|SQLCHAR|unsigned char *|  
|SQL_C_WCHAR|SQLWCHAR|wchar_t *|  
|SQL_C_SSHORT [j]|SQLSMALLINT|short int|  
|SQL_C_USHORT [j]|SQLUSMALLINT|unsigned short int|  
|SQL_C_SLONG [j]|SQLINTEGER|long int|  
|SQL_C_ULONG [j]|SQLUINTEGER|unsigned long int|  
|SQL_C_FLOAT|SQLREAL|float|  
|SQL_C_DOUBLE|SQLDOUBLE, SQLFLOAT|double|  
|SQL_C_BIT|SQLCHAR|unsigned char|  
|SQL_C_STINYINT [j]|SQLSCHAR|signed char|  
|SQL_C_UTINYINT [j]|SQLCHAR|unsigned char|  
|SQL_C_SBIGINT|SQLBIGINT|_int64 [h]|  
|SQL_C_UBIGINT|SQLUBIGINT|_int64 non signé [h]|  
|SQL_C_BINARY|SQLCHAR|unsigned char *|  
|SQL_C_BOOKMARK [i]|Signet|unsigned long int [d]|  
|SQL_C_VARBOOKMARK|SQLCHAR|unsigned char *|  
|Tous les types de données de l’intervalle C|SQL_INTERVAL_STRUCT|Consultez la section structure de l' [intervalle C](../../../odbc/reference/appendixes/c-interval-structure.md) , plus loin dans cette annexe.|  
  
 **Identificateur de type C** SQL_C_TYPE_DATE (C)  
  
 **Typedef C ODBC** SQL_DATE_STRUCT  
  
 **Type C**  
  
```  
struct tagDATE_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;    
} DATE_STRUCT;[a]  
```  
  
 **Identificateur de type C** SQL_C_TYPE_TIME (C)  
  
 **Typedef C ODBC** SQL_TIME_STRUCT  
  
 **Type C**  
  
```  
struct tagTIME_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
} TIME_STRUCT;[a]  
```  
  
 **Identificateur de type C** SQL_C_TYPE_TIMESTAMP (C)  
  
 **Typedef C ODBC** SQL_TIMESTAMP_STRUCT  
  
 **Type C**  
  
```  
struct tagTIMESTAMP_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;[b]   
} TIMESTAMP_STRUCT;[a]  
```  
  
 **Identificateur de type C** SQL_C_NUMERIC  
  
 **Typedef C ODBC** SQL_NUMERIC_STRUCT  
  
 **Type C**  
  
```  
struct tagSQL_NUMERIC_STRUCT {  
   SQLCHAR precision;  
   SQLSCHAR scale;  
   SQLCHAR sign[g];  
   SQLCHAR val[SQL_MAX_NUMERIC_LEN];[e], [f]   
} SQL_NUMERIC_STRUCT;  
```  
  
 **Identificateur de type C** SQL_C_GUID  
  
 **Typedef C ODBC** SQLGUID  
  
 **Type C**  
  
```  
struct tagSQLGUID {  
   DWORD Data1;  
   WORD Data2;  
   WORD Data3;  
   BYTE Data4[8];  
} SQLGUID;[k]  
```  
  
 [a] les valeurs des champs année, mois, jour, heure, minute et seconde des types de données DateTime C doivent être conformes aux contraintes du calendrier grégorien. (Voir [les contraintes du calendrier grégorien](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md) plus loin dans cette annexe.)  
  
 [b] la valeur du champ de fraction est le nombre de milliardièmes de seconde et est comprise entre 0 et 999 999 999 (1 inférieur à 1 milliard). Par exemple, la valeur du champ de fraction pour une demi-seconde est 500 millions, pour un millième de seconde (une milliseconde) est 1 million, pour un millionième de seconde (une microseconde) est 1 000 et pour un milliardième de seconde (une nanoseconde) est 1.  
  
 [c] dans ODBC 2. *x*, les types de données de date, d’heure et d’horodatage C sont SQL_C_DATE, SQL_C_TIME et SQL_C_TIMESTAMP.  
  
 [d] les applications ODBC 3 *. x* doivent utiliser SQL_C_VARBOOKMARK, pas SQL_C_BOOKMARK. Quand une application ODBC 3 *. x* fonctionne avec ODBC 2. *x* , le gestionnaire de pilotes ODBC 3 *. x* mappe SQL_C_VARBOOKMARK à SQL_C_BOOKMARK.  
  
 [e] un nombre est stocké dans le champ *Val* de la structure SQL_NUMERIC_STRUCT sous la forme d’un entier mis à l’échelle, en mode Little endian (l’octet le plus à gauche étant l’octet le moins significatif). Par exemple, le nombre 10,001 de base 10, avec une échelle de 4, est mis à l’échelle sur un entier de 100010. Étant donné qu’il s’agit de 186AA au format hexadécimal, la valeur de SQL_NUMERIC_STRUCT serait «AA 86 01 00 00... 00», avec le nombre d’octets défini par le **#define** SQL_MAX_NUMERIC_LEN.  
  
 Pour plus d’informations sur **SQL_NUMERIC_STRUCT**, consultez [Comment : récupérer des données numériques avec des SQL_NUMERIC_STRUCT](retrieve-numeric-data-sql-numeric-struct-kb222831.md).  
  
 [f] les champs de précision et d’échelle de l’SQL_C_NUMERIC type de données areused pour l’entrée d’une application et pour la sortie du pilote vers l’application. Lorsque le pilote écrit une valeur numérique dans le SQL_NUMERIC_STRUCT, il utilise sa propre valeur par défaut propre au pilote en tant que valeur du champ de *précision* , et il utilise la valeur dans le champ SQL_DESC_SCALE du descripteur d’application (valeur par défaut 0) pour le champ de mise à l' *échelle* . Une application peut fournir ses propres valeurs pour la précision et l’échelle en définissant les champs SQL_DESC_PRECISION et SQL_DESC_SCALE du descripteur d’application.  
  
 [g] le champ signe est 1 si positif, 0 si négatif.  
  
 [h] _int64 peut ne pas être fourni par certains compilateurs.  
  
 [i] _SQL_C_BOOKMARK est déconseillé dans ODBC 3 *. x*.  
  
 [j] _SQL_C_SHORT, SQL_C_LONG et SQL_C_TINYINT ont été remplacés dans ODBC par des types signés et non signés : SQL_C_SSHORT et SQL_C_USHORT, SQL_C_SLONG et SQL_C_ULONG, et SQL_C_STINYINT et SQL_C_UTINYINT. Pilote ODBC 3 *. x* qui doit fonctionner avec ODBC 2. *x* les applications doivent prendre en charge SQL_C_SHORT, SQL_C_LONG et SQL_C_TINYINT, car lorsqu’elles sont appelées, le gestionnaire de pilotes les transmet au pilote.  
  
 [k] SQL_C_GUID peuvent être converties uniquement en SQL_CHAR ou SQL_WCHAR.  
  
 Cette section contient la rubrique suivante.  
  
-   [Structures d’entiers 64 bits](../../../odbc/reference/appendixes/64-bit-integer-structures.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)
