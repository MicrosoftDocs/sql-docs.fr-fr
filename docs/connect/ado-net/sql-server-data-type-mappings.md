---
title: Mappages de types de données SQL Server
description: En savoir plus sur le mappage entre les différents systèmes de type pour SQL Server et .NET Framework. Cet article résume la façon dont les systèmes interagissent avec le Fournisseur de données Microsoft SqlClient pour SQL Server.
ms.date: 11/13/2020
ms.assetid: fafdc31a-f435-4cd3-883f-1dfadd971277
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3cf6d932a3f458e971d827c1ca4c493c298c2241
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771362"
---
# <a name="sql-server-data-type-mappings"></a>Mappages de types de données SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

SQL Server et le .NET Framework sont basés sur des systèmes de types différents. Par exemple, la structure <xref:System.Decimal> du .NET Framework a une échelle maximale de 28, tandis que l'échelle maximale des types de données décimal et numérique de SQL Server est de 38. Pour préserver l'intégrité des données lors des opérations de lecture et d'écriture, le <xref:Microsoft.Data.SqlClient.SqlDataReader> expose des méthodes d'accesseur typé spécifiques à SQL Server qui retournent des objets de <xref:System.Data.SqlTypes>, ainsi que des méthodes d'accesseur qui retournent des types .NET Framework. Les types SQL Server et les types .NET Framework sont également représentés par des énumérations dans les classes <xref:System.Data.DbType> et <xref:System.Data.SqlDbType>, que vous pouvez utiliser pour spécifier des types de données <xref:Microsoft.Data.SqlClient.SqlParameter>.

La table suivante indique le type de .NET Framework déduit, les énumérations de <xref:System.Data.DbType> et de <xref:System.Data.SqlDbType>, ainsi que les méthodes d’accesseurs pour <xref:Microsoft.Data.SqlClient.SqlDataReader>.

|Type de moteur de base de données SQL Server|Type .NET Framework|Énumération SqlDbType|Accesseur typé SqlTypes SqlDataReader|Énumération DbType|Accesseur typé DbType SqlDataReader|  
|-------------------------------------|-------------------------|---------------------------|-------------------------------------------|------------------------|-----------------------------------------|  
|bigint|Int64|<xref:System.Data.SqlDbType.BigInt>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlInt64%2A>|<xref:System.Data.DbType.Int64>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetInt64%2A>|  
|binary|Byte[]|<xref:System.Data.SqlDbType.VarBinary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|bit|Boolean|<xref:System.Data.SqlDbType.Bit>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBoolean%2A>|<xref:System.Data.DbType.Boolean>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBoolean%2A>|  
|char|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.Char>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.AnsiStringFixedLength>,<br /><br /> <xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|date <sup>1</sup><br /><br /> (SQL Server 2008 et versions ultérieures)|DateTime|<xref:System.Data.SqlDbType.Date> <sup>1</sup>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDateTime%2A>|<xref:System.Data.DbType.Date> <sup>1</sup>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|DATETIME|DateTime|<xref:System.Data.SqlDbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDateTime%2A>|<xref:System.Data.DbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|datetime2<br /><br /> (SQL Server 2008 et versions ultérieures)|DateTime|<xref:System.Data.SqlDbType.DateTime2>|Aucun|<xref:System.Data.DbType.DateTime2>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|datetimeoffset<br /><br /> (SQL Server 2008 et versions ultérieures)|DateTimeOffset|<xref:System.Data.SqlDbType.DateTimeOffset>|Aucun|<xref:System.Data.DbType.DateTimeOffset>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTimeOffset%2A>|  
|Décimal|Decimal|<xref:System.Data.SqlDbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|FILESTREAM attribute (varbinary(max))|Byte[]|<xref:System.Data.SqlDbType.VarBinary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBytes%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|float|Double|<xref:System.Data.SqlDbType.Float>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDouble%2A>|<xref:System.Data.DbType.Double>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDouble%2A>|  
|image|Byte[]|<xref:System.Data.SqlDbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|int|Int32|<xref:System.Data.SqlDbType.Int>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlInt32%2A>|<xref:System.Data.DbType.Int32>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetInt32%2A>|  
|money|Decimal|<xref:System.Data.SqlDbType.Money>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlMoney%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|NCHAR|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.NChar>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.StringFixedLength>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|ntext|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.NText>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|numeric|Decimal|<xref:System.Data.SqlDbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|NVARCHAR|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.NVarChar>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|real|Unique|<xref:System.Data.SqlDbType.Real>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlSingle%2A>|<xref:System.Data.DbType.Single>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetFloat%2A>|  
|rowversion|Byte[]|<xref:System.Data.SqlDbType.Timestamp>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|smalldatetime|DateTime|<xref:System.Data.SqlDbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDateTime%2A>|<xref:System.Data.DbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|SMALLINT|Int16|<xref:System.Data.SqlDbType.SmallInt>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlInt16%2A>|<xref:System.Data.DbType.Int16>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetInt16%2A>|  
|SMALLMONEY|Decimal|<xref:System.Data.SqlDbType.SmallMoney>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlMoney%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|sql_variant|Objet <sup>2</sup>|<xref:System.Data.SqlDbType.Variant>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlValue%2A> <sup>2</sup>|<xref:System.Data.DbType.Object>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetValue%2A> <sup>2</sup>|  
|text|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.Text>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|time<br /><br /> (SQL Server 2008 et versions ultérieures)|TimeSpan|<xref:System.Data.SqlDbType.Time>|Aucun|<xref:System.Data.DbType.Time>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|timestamp|Byte[]|<xref:System.Data.SqlDbType.Timestamp>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|TINYINT|Byte|<xref:System.Data.SqlDbType.TinyInt>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlByte%2A>|<xref:System.Data.DbType.Byte>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetByte%2A>|  
|UNIQUEIDENTIFIER|Guid|<xref:System.Data.SqlDbType.UniqueIdentifier>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlGuid%2A>|<xref:System.Data.DbType.Guid>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetGuid%2A>|  
|varbinary|Byte[]|<xref:System.Data.SqlDbType.VarBinary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|varchar|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.VarChar>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.AnsiString>, <xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|Xml|Xml|<xref:System.Data.SqlDbType.Xml>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlXml%2A>|<xref:System.Data.DbType.Xml>|Aucun|

<sup>1</sup> Vous ne pouvez pas définir la propriété `DbType` d’un `SqlParameter` sur `SqlDbType.Date`.  
<sup>2</sup> Utilisez un accesseur typé spécifique si vous connaissez le type sous-jacent de `sql_variant`.

## <a name="sql-server-documentation"></a>documentation SQL Server

Pour plus d’informations sur les types de données SQL Server, consultez [Types de données (Transact-SQL)](/sql/t-sql/data-types/data-types-transact-sql).

## <a name="see-also"></a>Voir aussi

- [Types de données SQL Server et ADO.NET](./sql/sql-server-data-types.md)
- [Données binaires et à valeurs élevées SQL Server](./sql/sql-server-binary-large-value-data.md)
- [Configuration des paramètres](configure-parameters.md)
- [Mappages des types de données dans ADO.NET](data-type-mappings-ado-net.md)
