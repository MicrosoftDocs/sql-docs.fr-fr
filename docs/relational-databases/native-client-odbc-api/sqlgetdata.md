---
description: SQLGetData
title: SQLGetData | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetData function
ms.assetid: 204848be-8787-45b4-816f-a60ac9d56fcf
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da7192553da45a8b8971203f4d76fa3bd869afc3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88428251"
---
# <a name="sqlgetdata"></a>SQLGetData
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLGetData** est utilisé pour récupérer les données du jeu de résultats sans les valeurs de colonne de liaison. **SQLGetData** peut être appelé successivement sur la même colonne pour récupérer de grandes quantités de données à partir d’une colonne avec un type de données **Text**, **ntext**ou **image** .  
  
 Il n'y a aucune spécification exigeant qu'une application lie des variables pour extraire des données de jeu de résultats. Les données de n’importe quelle colonne peuvent être récupérées à partir du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client à l’aide de **SQLGetData**.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC Native Client ne prend pas en charge l’utilisation de **SQLGetData** pour récupérer des données dans l’ordre aléatoire des colonnes. Toutes les colonnes indépendantes traitées avec **SQLGetData** doivent avoir des ordinaux de colonne plus élevés que les colonnes liées dans le jeu de résultats. L'application doit traiter les données de la valeur d'ordinal de colonne indépendante la plus basse à la plus élevée. Toute tentative d'extraction de données d'une colonne d'ordinal inférieur provoque une erreur. Si l'application utilise des curseurs côté serveur pour signaler les lignes de jeu de résultats, l'application peut réextraire la ligne actuelle, puis extraire la valeur d'une colonne. Si une instruction est exécutée sur le curseur par défaut en lecture seule et avant uniquement, vous devez exécuter à nouveau l’instruction pour sauvegarder **SQLGetData**.  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pilote ODBC native client signale avec précision la longueur des données **Text**, **ntext**et **image** extraites à l’aide de **SQLGetData**. L’application peut utiliser le retour du paramètre *StrLen_or_IndPtr* pour récupérer rapidement des données de type long.  
  
> [!NOTE]  
>  Pour les types de valeur élevée, *StrLen_or_IndPtr* retourne SQL_NO_TOTAL en cas de troncation des données.  
  
## <a name="sqlgetdata-support-for-enhanced-date-and-time-features"></a>Prise en charge de SQLGetData pour les fonctionnalités Date et Heure améliorées  
 Les valeurs de colonne de résultats de types date/heure sont converties comme décrit dans [conversions de SQL en C](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-sql-to-c.md).  
  
 Pour plus d’informations, consultez améliorations de la [date et de l’heure &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlgetdata-support-for-large-clr-udts"></a>Prise en charge SQLGetData pour les types CLR volumineux définis par l'utilisateur  
 **SQLGetData** prend en charge les grands types CLR définis par l’utilisateur (UDT). Pour plus d’informations, consultez [types CLR volumineux définis par l’utilisateur &#40;ODBC&#41;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="example"></a> Exemple  
  
```  
SQLHDBC     hDbc = NULL;  
SQLHSTMT    hStmt = NULL;  
long        lEmpID;  
PBYTE       pPicture;  
SQLINTEGER  pIndicators[2];  
  
// Get an environment, connection, and so on.  
...  
  
// Get a statement handle and execute a command.  
SQLAllocHandle(SQL_HANDLE_STMT, hDbc, &hStmt);  
  
if (SQLExecDirect(hStmt,  
    (SQLCHAR*) "SELECT EmployeeID, Photo FROM Employees",  
    SQL_NTS) == SQL_ERROR)  
    {  
    // Handle error and return.  
    }  
  
// Retrieve data from row set.  
SQLBindCol(hStmt, 1, SQL_C_LONG, (SQLPOINTER) &lEmpID, sizeof(long),  
    &pIndicators[0]);  
  
while (SQLFetch(hStmt) == SQL_SUCCESS)  
    {  
    cout << "EmployeeID: " << lEmpID << "\n";  
  
    // Call SQLGetData to determine the amount of data that's waiting.  
    if (SQLGetData(hStmt, 2, SQL_C_BINARY, pPicture, 0, &pIndicators[1])  
        == SQL_SUCCESS_WITH_INFO)  
        {  
        cout << "Photo size: " pIndicators[1] << "\n\n";  
  
        // Get all the data at once.  
        pPicture = new BYTE[pIndicators[1]];  
        if (SQLGetData(hStmt, 2, SQL_C_DEFAULT, pPicture,  
            pIndicators[1], &pIndicators[1]) != SQL_SUCCESS)  
            {  
            // Handle error and continue.  
            }  
  
        delete [] pPicture;  
        }  
    else  
        {  
        // Handle error on attempt to get data length.  
        }  
    }  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonction SQLGetData](https://go.microsoft.com/fwlink/?LinkId=59350)   
 [Détails de l’implémentation d’API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
