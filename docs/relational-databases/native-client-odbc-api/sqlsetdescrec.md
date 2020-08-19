---
description: SQLSetDescRec
title: SQLSetDescRec | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetDescRec function
ms.assetid: 203d02a2-aa09-462b-a489-a2cdd6f6023b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f0bd2a767a3ee99d1ffa1609d87e62b05c5af30
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88420813"
---
# <a name="sqlsetdescrec"></a>SQLSetDescRec
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cette rubrique décrit les fonctionnalités de SQLSetDescRec qui sont spécifiques à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="sqlsetdescrec-and-table-valued-parameters"></a>SQLSetDescRec et paramètres table  
 SQLSetDescRec peut être utilisé pour définir des champs de descripteur pour les paramètres table et les colonnes de paramètre table. Les colonnes de paramètre table sont disponibles uniquement lorsque le champ d'en-tête de descripteur SQL_SOPT_SS_PARAM_FOCUS est défini sur l'ordinal d'un enregistrement pour lequel SQL_DESC_TYPE a la valeur SQL_SS_TABLE. Pour plus d'informations sur SQL_SOPT_SS_PARAM_FOCUS, consultez [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md).  
  
 Le tableau suivant décrit le mappage entre les paramètres et les champs de descripteur.  
  
|Paramètre|Attribut associé pour les types de paramètre non table, y compris les colonnes de paramètres table|Attribut associé pour les paramètres table|  
|---------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------|  
|*Type*|SQL_DESC_TYPE|SQL_SS_TABLE|  
|*Sous-type*|Ignoré|Pour les enregistrements de type SQL_DATETIME ou SQL_INTERVAL, affectez la valeur SQL_DESC_DATETIME_INTERVAL_CODE.|  
|*Longueur*|SQL_DESC_OCTET_LENGTH|Longueur du nom du type de paramètre table. Cela peut être SQL_NTS si le nom de type se termine par une valeur NULL ou zéro si le nom de type de paramètre table n'est pas requis.|  
|*Précision*|SQL_DESC_PRECISION|SQL_DESC_ARRAY_SIZE|  
|*Mise à l’échelle*|SQL_DESC_SCALE|Inutilisé. Ce paramètre doit être nul.|  
|*DataPtr*|SQL_DESC_DATA_PTR dans APD|SQL_CA_SS_TYPE_NAME<br /><br /> Ce paramètre est facultatif pour les appels de procédure stockée et NULL peut être spécifié s'il n'est pas requis. Ce paramètre doit être spécifié pour les instructions SQL qui ne sont pas des appels de procédure.<br /><br /> *DataPtr* sert également de valeur unique que l’application peut utiliser pour identifier ce paramètre table lorsque la liaison de ligne variable est utilisée.|  
|*StringLengthPtr*|SQL_DESC_OCTET_LENGTH_PTR|SQL_DESC_OCTET_LENGTH_PTR<br /><br /> Pour un paramètre table, il s'agit du nombre de lignes à transférer ou SQL_DATA_AT_EXEC. Il s’agit d’un pointeur vers une valeur qui contient le nombre de lignes à transférer avec SQLExecDirect.|  
|*IndicatorPtr*|SQL_DESC_INDICATOR_PTR|SQL_DESC_INDICATOR_PTR|  
  
 Pour plus d’informations sur les paramètres table, consultez [paramètres table &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlsetdescrec-support-for-enhanced-date-and-time-features"></a>Prise en charge de SQLSetDescRec pour les fonctionnalités Date et Heure améliorées  
 Les valeurs autorisées pour les types date/heure sont les suivantes :  
  
| Attribut | *Type* | *Sous-type* | *Longueur* | *Précision* | *Mise à l’échelle* |
| --------- | ------ | --------- | -------- | ----------- | ------- |
|DATETIME|SQL_DATETIME|SQL_CODE_TIMESTAMP|4|3|3|  
|smalldatetime|SQL_SQL_DATETIME|SQL_CODE_TIMESTAMP|8|0|0|  
|Date|SQL_DATETIME|SQL_CODE_DATE|6|0|0|  
|time|SQL_SS_TIME2|0|10|0..7|0..7|  
|datetime2|SQL_DATETIME|SQL_CODE_TIMESTAMP|16|0..7|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|0|20|0..7|0..7|  
  
 Pour plus d’informations, consultez améliorations de la [date et de l’heure &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="sqlsetdescrec-support-for-large-clr-udts"></a>Prise en charge SQLSetDescRec pour les types CLR volumineux définis par l'utilisateur  
 **SQLSetDescRec** prend en charge les types CLR volumineux définis par l’utilisateur (UDT). Pour plus d’informations, consultez [types CLR volumineux définis par l’utilisateur &#40;ODBC&#41;](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLSetDescRec](https://go.microsoft.com/fwlink/?LinkId=80704)   
 [Détails de l’implémentation d’API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
  
