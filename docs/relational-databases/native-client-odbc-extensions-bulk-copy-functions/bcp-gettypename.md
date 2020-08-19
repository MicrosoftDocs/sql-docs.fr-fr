---
description: bcp_gettypename
title: bcp_gettypename | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_gettypename
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_gettypename function
ms.assetid: 65f036d1-f60e-4b8a-97b3-76fccf0dfed4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8956677e62c3f4a824e704c0905c7970cf9e913
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88448569"
---
# <a name="bcp_gettypename"></a>bcp_gettypename
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Retourne le nom de type de SQL pour un jeton de type BCP spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RETCODE bcp_gettypename (  
        INT token,  
        DBBOOL fIsMaxType);  
```  
  
## <a name="arguments"></a>Arguments  
 *token*  
 Valeur indiquant un jeton de type BCP.  
  
 *case*  
 Indique si le jeton demandé possède le type max.  
  
## <a name="returns"></a>Retours  
 Chaîne contenant le nom de type SQL qui correspond au type BCP. Si un type BCP non valide est spécifié, une chaîne vide est retournée.  
  
## <a name="remarks"></a>Notes  
 Les jetons de type de BCP sont définis dans le fichier d'en-tête sqlncli.h et la bibliothèque sqlncli11.lib.  
  
 Le tableau suivant spécifie les types BCP possibles, s'ils sont ou pas de type max et la sortie attendue.  
  
|Nom du type BCP|MaxType|Output|  
|-------------------|-------------|------------|  
|**SQLDECIMAL**|Vous pouvez soit utiliser|**decimal**|  
|**SQLNUMERIC**|Vous pouvez soit utiliser|**numeric**|  
|**SQLINT1**|Vous pouvez soit utiliser|**tinyint**|  
|**SQLINT2**|Vous pouvez soit utiliser|**smallint**|  
|**SQLINT4**|Vous pouvez soit utiliser|**int**|  
|**SQLMONEY**|Vous pouvez soit utiliser|**money**|  
|**SQLFLT8**|Vous pouvez soit utiliser|**float**|  
|**SQLDATETIME**|Vous pouvez soit utiliser|**datetime**|  
|**SQLBITN**|Vous pouvez soit utiliser|**bit-null**|  
|**SQLBIT**|Vous pouvez soit utiliser|**bit**|  
|**SQLBIGCHAR**|Non|**char**|  
|**SQLCHARACTER**|Non|**char**|  
|**SQLBIGVARCHAR**|Non|**varchar**|  
|**SQLVARCHAR**|Non|**varchar**|  
|**SQLTEXT**|Vous pouvez soit utiliser|**text**|  
|**SQLBIGBINARY**|Non|**binary**|  
|**SQLBINARY**|Non|**Binaire**|  
|**SQLBIGVARBINARY**|Non|**Varbinary**|  
|**SQLVARBINARY**|Non|**Varbinary**|  
|**SQLIMAGE**|Vous pouvez soit utiliser|**Image**|  
|**SQLINTN**|Vous pouvez soit utiliser|**int-null**|  
|**SQLDATETIMN**|Vous pouvez soit utiliser|**datetime-null**|  
|**SQLMONEYN**|Vous pouvez soit utiliser|**money-null**|  
|**SQLFLTN**|Vous pouvez soit utiliser|**float-null**|  
|**SQLAOPSUM**|Vous pouvez soit utiliser|**Sum**|  
|**SQLAOPAVG**|Vous pouvez soit utiliser|**Avg**|  
|**SQLAOPCNT**|Vous pouvez soit utiliser|**Count**|  
|**SQLAOPMIN**|Vous pouvez soit utiliser|**Min**|  
|**SQLAOPMAX**|Vous pouvez soit utiliser|**Max**|  
|**SQLDATETIM4**|Vous pouvez soit utiliser|**smalldatetime**|  
|**SQLMONEY4**|Vous pouvez soit utiliser|**Smallmoney**|  
|**SQLFLT4**|Vous pouvez soit utiliser|**Non**|  
|**SQLUNIQUEID**|Vous pouvez soit utiliser|**uniqueidentifier**|  
|**SQLNCHAR**|Non|**NCHAR**|  
|**SQLNVARCHAR**|Non|**Nvarchar**|  
|**SQLNTEXT**|Vous pouvez soit utiliser|**Text**|  
|**SQLVARIANT**|Vous pouvez soit utiliser|**sql_variant**|  
|**SQLINT8**|Vous pouvez soit utiliser|**Comportant**|  
|**SQLCHARACTER**|Oui|**varchar(max)**|  
|**SQLBIGCHAR**|Oui|**varchar(max)**|  
|**SQLBIGVARCHAR**|Oui|**varchar(max)**|  
|**SQLVARCHAR**|Oui|**varchar(max)**|  
|**SQLBINARY**|Oui|**varbinary(max)**|  
|**SQLBIGBINARY**|Oui|**varbinary(max)**|  
|**SQLBIGVARBINARY**|Oui|**varbinary(max)**|  
|**SQLVARBINARY**|Oui|**varbinary(max)**|  
|**SQLNCHAR**|Oui|**nvarchar(max)**|  
|**SQLNVARCHAR**|Oui|**nvarchar(max)**|  
|**SQLXML**|Oui|**Xml**|  
|**SQLUDT**|Vous pouvez soit utiliser|**Assorti**|  
  
## <a name="bcp_gettypename-support-for-enhanced-date-and-time-features"></a>Prise en charge des fonctionnalités de date et heure améliorées par bcp_gettypename  
 Les valeurs de paramètre de jeton pour les types date/time sont décrites dans la colonne « type dans sqlncli. h » de la table dans les [modifications de copie en bloc pour les types de date et d’heure améliorés &#40;OLE DB et ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md). La valeur retournée est dans la ligne correspondante de la colonne « Type de stockage de fichier ».  
  
 Pour plus d’informations, consultez améliorations de la [date et de l’heure &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
