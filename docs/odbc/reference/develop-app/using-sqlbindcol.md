---
description: Utilisation de SQLBindCol
title: Utilisation de SQLBindCol | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- result sets [ODBC], binding columns
- binding columns [ODBC]
- SQLBindCol function [ODBC], using
ms.assetid: 17277ab3-33ad-44d3-a81c-a26b5e338512
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f68aa647f600709dd1a4b989cdab8153775f0aaa
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88421443"
---
# <a name="using-sqlbindcol"></a>Utilisation de SQLBindCol
L’application lie les colonnes en appelant **SQLBindCol**. Cette fonction lie une colonne à la fois. Avec celle-ci, l’application spécifie les éléments suivants :  
  
-   Numéro de la colonne. La colonne 0 est la colonne de signet ; Cette colonne n’est pas incluse dans certains jeux de résultats. Toutes les autres colonnes sont numérotées en commençant par le numéro 1. La liaison d’une colonne avec un numéro supérieur à celui des colonnes dans le jeu de résultats est une erreur ; Cette erreur ne peut pas être détectée tant que le jeu de résultats n’a pas été créé. par conséquent, elle est retournée par **SQLFetch**, et non par **SQLBindCol**.  
  
-   Le type de données C, l’adresse et la longueur en octets de la variable liée à la colonne. La spécification d’un type de données C pour lequel le type de données SQL de la colonne ne peut pas être converti est une erreur. Cette erreur peut ne pas être détectée tant que le jeu de résultats n’a pas été créé. par conséquent, elle est retournée par **SQLFetch**, et non par **SQLBindCol**. Pour obtenir la liste des conversions prises en charge, consultez [conversion de données de SQL en types de données C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) dans l’annexe D : types de données. Pour plus d’informations sur la longueur en octets, consultez [Data buffer length](../../../odbc/reference/develop-app/data-buffer-length.md).  
  
-   Adresse d’une mémoire tampon de longueur/d’indicateur. Le tampon de longueur/d’indicateur est facultatif. Elle est utilisée pour retourner la longueur en octets des données binaires ou de type caractère, ou retourner SQL_NULL_DATA si les données sont NULL. Pour plus d’informations, consultez [utilisation des valeurs de longueur/indicateur](../../../odbc/reference/develop-app/using-length-and-indicator-values.md).  
  
 Lorsque **SQLBindCol** est appelé, le pilote associe ces informations à l’instruction. Quand chaque ligne de données est extraite, elle utilise les informations pour placer les données de chaque colonne dans les variables d’application liées.  
  
 Par exemple, le code suivant lie des variables aux colonnes SalesPerson et CustID. Les données des colonnes sont retournées dans *SalesPerson* et *CustID*. Étant donné que *SalesPerson* est une mémoire tampon de caractères, l’application spécifie sa longueur d’octet (11) afin que le pilote puisse déterminer s’il faut tronquer les données. La longueur d’octet du titre retourné, ou si elle est NULL, est retournée dans *SalesPersonLenOrInd*.  
  
 Comme *CustID* est une variable de type entier et a une longueur fixe, il n’est pas nécessaire de spécifier sa longueur d’octet ; le pilote suppose qu’il s’agit **de sizeof (** SQLUINTEGER **)**. La longueur d’octet des données de l’ID client retournées, ou si elle est NULL, est retournée dans *CustIDInd*. Notez que l’application est intéressée uniquement par le fait que le salaire est NULL, car la longueur de l’octet est toujours **sizeof (** SQLUINTEGER **)**.  
  
```  
SQLCHAR       SalesPerson[11];  
SQLUINTEGER   CustID;  
SQLINTEGER    SalesPersonLenOrInd, CustIDInd;  
SQLRETURN     rc;  
SQLHSTMT      hstmt;  
  
// Bind SalesPerson to the SalesPerson column and CustID to the   
// CustID column.  
SQLBindCol(hstmt, 1, SQL_C_CHAR, SalesPerson, sizeof(SalesPerson),  
            &SalesPersonLenOrInd);  
SQLBindCol(hstmt, 2, SQL_C_ULONG, &CustID, 0, &CustIDInd);  
  
// Execute a statement to get the sales person/customer of all orders.  
SQLExecDirect(hstmt, "SELECT SalesPerson, CustID FROM Orders ORDER BY SalesPerson",  
               SQL_NTS);  
  
// Fetch and print the data. Print "NULL" if the data is NULL. Code to   
// check if rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {  
   if (SalesPersonLenOrInd == SQL_NULL_DATA)   
            printf("NULL                     ");  
   else   
            printf("%10s   ", SalesPerson);  
   if (CustIDInd == SQL_NULL_DATA)   
         printf("NULL\n");  
   else   
            printf("%d\n", CustID);  
}  
  
// Close the cursor.  
SQLCloseCursor(hstmt);  
```  
  
 Le code suivant exécute une instruction **Select** entrée par l’utilisateur et imprime chaque ligne de données dans le jeu de résultats. Étant donné que l’application ne peut pas prédire la forme du jeu de résultats créé par l’instruction **Select** , elle ne peut pas lier des variables codées en dur au jeu de résultats comme dans l’exemple précédent. Au lieu de cela, l’application alloue une mémoire tampon qui contient les données et une mémoire tampon de longueur/indicateur pour chaque colonne de cette ligne. Pour chaque colonne, elle calcule le décalage au début de la mémoire pour la colonne et ajuste ce décalage afin que les tampons de données et de longueur/indicateur pour la colonne commencent sur les limites d’alignement. Il lie ensuite la mémoire en commençant au décalage de la colonne. Du point de vue du pilote, l’adresse de cette mémoire ne peut pas être différenciée de l’adresse d’une variable liée dans l’exemple précédent. Pour plus d’informations sur l’alignement, consultez [alignement](../../../odbc/reference/develop-app/alignment.md).  
  
```  
// This application allocates a buffer at run time. For each column, this   
// buffer contains memory for the column's data and length/indicator.   
// For example:  
//      column 1         column 2      column 3      column 4  
// <------------><---------------><-----><------------>  
//      db1   li1   db2   li2   db3   li3   db4   li4  
//      |      |      |      |      |      |      |         |  
//      _____V_____V________V_______V___V___V______V_____V_  
// |__________|__|_____________|__|___|__|__________|__|  
//  
// dbn = data buffer for column n  
// lin = length/indicator buffer for column n  
  
// Define a macro to increase the size of a buffer so that it is a   
// multiple of the alignment size. Thus, if a buffer starts on an   
// alignment boundary, it will end just before the next alignment   
// boundary. In this example, an alignment size of 4 is used because   
// this is the size of the largest data type used in the application's   
// buffer--the size of an SDWORD and of the largest default C data type   
// are both 4. If a larger data type (such as _int64) was used, it would   
// be necessary to align for that size.  
#define ALIGNSIZE 4  
#define ALIGNBUF(Length) Length % ALIGNSIZE ? \  
                  Length + ALIGNSIZE - (Length % ALIGNSIZE) : Length  
  
SQLCHAR        SelectStmt[100];  
SQLSMALLINT    NumCols, *CTypeArray, i;  
SQLINTEGER *   ColLenArray, *OffsetArray, SQLType, *DataPtr;  
SQLRETURN      rc;   
SQLHSTMT       hstmt;  
  
// Get a SELECT statement from the user and execute it.  
GetSelectStmt(SelectStmt, 100);  
SQLExecDirect(hstmt, SelectStmt, SQL_NTS);  
  
// Determine the number of result set columns. Allocate arrays to hold   
// the C type, byte length, and buffer offset to the data.  
SQLNumResultCols(hstmt, &NumCols);  
CTypeArray = (SQLSMALLINT *) malloc(NumCols * sizeof(SQLSMALLINT));  
ColLenArray = (SQLINTEGER *) malloc(NumCols * sizeof(SQLINTEGER));  
OffsetArray = (SQLINTEGER *) malloc(NumCols * sizeof(SQLINTEGER));  
  
OffsetArray[0] = 0;  
for (i = 0; i < NumCols; i++) {  
   // Determine the column's SQL type. GetDefaultCType contains a switch   
   // statement that returns the default C type for each SQL type.  
   SQLColAttribute(hstmt, ((SQLUSMALLINT) i) + 1, SQL_DESC_TYPE, NULL, 0, NULL, (SQLPOINTER) &SQLType);  
   CTypeArray[i] = GetDefaultCType(SQLType);  
  
   // Determine the column's byte length. Calculate the offset in the   
   // buffer to the data as the offset to the previous column, plus the   
   // byte length of the previous column, plus the byte length of the   
   // previous column's length/indicator buffer. Note that the byte   
   // length of the column and the length/indicator buffer are increased   
   // so that, assuming they start on an alignment boundary, they will  
   // end on the byte before the next alignment boundary. Although this   
   // might leave some holes in the buffer, it is a relatively   
   // inexpensive way to guarantee alignment.  
   SQLColAttribute(hstmt, ((SQLUSMALLINT) i)+1, SQL_DESC_OCTET_LENGTH, NULL, 0, NULL, &ColLenArray[i]);  
   ColLenArray[i] = ALIGNBUF(ColLenArray[i]);  
   if (i)  
      OffsetArray[i] = OffsetArray[i-1]+ColLenArray[i-1]+ALIGNBUF(sizeof(SQLINTEGER));  
}  
  
// Allocate the data buffer. The size of the buffer is equal to the   
// offset to the data buffer for the final column, plus the byte length   
// of the data buffer and length/indicator buffer for the last column.  
void *DataPtr = malloc(OffsetArray[NumCols - 1] +  
               ColLenArray[NumCols - 1] + ALIGNBUF(sizeof(SQLINTEGER)));  
  
// For each column, bind the address in the buffer at the start of the   
// memory allocated for that column's data and the address at the start   
// of the memory allocated for that column's length/indicator buffer.  
for (i = 0; i < NumCols; i++)  
   SQLBindCol(hstmt,  
            ((SQLUSMALLINT) i) + 1,  
            CTypeArray[i],  
            (SQLPOINTER)((SQLCHAR *)DataPtr + OffsetArray[i]),  
            ColLenArray[i],  
            (SQLINTEGER *)((SQLCHAR *)DataPtr + OffsetArray[i] + ColLenArray[i]));  
  
// Retrieve and print each row. PrintData accepts a pointer to the data,   
// its C type, and its byte length/indicator. It contains a switch   
// statement that casts and prints the data according to its type. Code   
// to check if rc equals SQL_ERROR or SQL_SUCCESS_WITH_INFO not shown.  
while ((rc = SQLFetch(hstmt)) != SQL_NO_DATA) {  
   for (i = 0; i < NumCols; i++) {  
      PrintData((SQLCHAR *)DataPtr[OffsetArray[i]], CTypeArray[i],  
               (SQLINTEGER *)((SQLCHAR *)DataPtr[OffsetArray[i] + ColLenArray[i]]));  
   }  
}  
  
// Close the cursor.  
SQLCloseCursor(hstmt);  
```
