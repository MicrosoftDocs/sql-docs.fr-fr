---
title: Mappage de type de données dans les ensembles de lignes et les paramètres (fournisseur Native Client OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- DBTYPE_SQLVARIANT data type
- SQL Server Native Client OLE DB provider, data types
- rowsets [OLE DB], data type mapping
- data types [OLE DB]
- GetColumnInfo function
- parameters [OLE DB]
- SSPROP_ALLOWNATIVEVARIANT property
- GetParameterInfo function
- OLE DB, data types
ms.assetid: 3d831ff8-3b79-4698-b2c1-2b5dd2f8235c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a436ee66b42f59994ba902b75d84ba94c29b5008
ms.sourcegitcommit: 08f331b6a5fe72d68ef1b2eccc5d16cb80c6ee39
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86976595"
---
# <a name="data-type-mapping-in-rowsets-and-parameters"></a>Mappage de type de données dans les ensembles de lignes et les paramètres
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Dans les ensembles de lignes et en tant que valeurs de paramètre, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client représente des données à l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aide des types de données OLE DB définis suivants, indiqués dans les fonctions **IColumnsInfo :: GetColumnInfo** et **ICommandWithParameters :: GetParameterInfo**.  
  
|Type de données SQL Server|Type de données OLE DB|  
|--------------------------|----------------------|  
|**bigint**|DBTYPE_I8|  
|**binary**|DBTYPE_BYTES|  
|**bit**|DBTYPE_BOOL|  
|**char**|DBTYPE_STR|  
|**datetime**|DBTYPE_DBTIMESTAMP|  
|**datetime2**|DBTYPE_DBTIME2|  
|**decimal**|DBTYPE_NUMERIC|  
|**float**|DBTYPE_R8|  
|**image**|DBTYPE_BYTES|  
|**int**|DBTYPE_I4|  
|**money**|DBTYPE_CY|  
|**nchar**|DBTYPE_WSTR|  
|**ntext**|DBTYPE_WSTR|  
|**numeric**|DBTYPE_NUMERIC|  
|**nvarchar**|DBTYPE_WSTR|  
|**real**|DBTYPE_R4|  
|**smalldatetime**|DBTYPE_DBTIMESTAMP|  
|**smallint**|DBTYPE_I2|  
|**smallmoney**|DBTYPE_CY|  
|**sql_variant**|DBTYPE_VARIANT, DBTYPE_SQLVARIANT|  
|**sysname**|DBTYPE_WSTR|  
|**text**|DBTYPE_STR|  
|**timestamp**|DBTYPE_BYTES|  
|**tinyint**|DBTYPE_UI1|  
|**UDT**|DBTYPE_UDT|  
|**uniqueidentifier**|DBTYPE_GUID|  
|**varbinary**|DBTYPE_BYTES|  
|**varchar**|DBTYPE_STR|  
|**XML**|DBTYPE_XML|  
  
 Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur OLE DB Native Client prend en charge les conversions de données demandées par le consommateur, comme indiqué dans l’illustration.  
  
 Les objets **sql_variant** peuvent contenir des données de n’importe quel type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sauf text, ntext, image, varchar(max), nvarchar(max), varbinary(max), xml, timestamp et les types CLR (Common Language Runtime) du Microsoft .NET Framework définis par l’utilisateur. sql_variant ne peut pas être le type de données de base sous-jacent d'une instance de données sql_variant. Par exemple, la colonne peut contenir des valeurs **smallint** pour certaines lignes, des valeurs **float** pour d’autres lignes et des valeurs **char**/**nchar** dans le reste.  
  
> [!NOTE]  
>  Le type de données **sql_variant** est similaire au type de données Variant dans Microsoft Visual Basic® et à DBTYPE_VARIANT, DBTYPE_SQLVARIANT dans OLE DB.  
  
 Quand des données **sql_variant** sont extraites en tant que DBTYPE_VARIANT, elles sont placées dans une structure VARIANT dans la mémoire tampon. Cependant, les sous-types dans la structure VARIANT peuvent ne pas être mappés aux sous-types définis dans le type de données **sql_variant**. Les données **sql_variant** doivent ensuite être extraites en tant que DBTYPE_SQLVARIANT pour que tous les sous-types correspondent.  
  
## <a name="dbtype_sqlvariant-data-type"></a>Type de données DBTYPE_SQLVARIANT  
 Pour prendre en charge le type de données **sql_variant** , le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur Native Client OLE DB expose un type de données spécifique au fournisseur appelé DBTYPE_SQLVARIANT. Quand des données **sql_variant** sont extraites en tant que DBTYPE_SQLVARIANT, elles sont stockées dans une structure SSVARIANT spécifique au fournisseur. La structure SSVARIANT contient tous les sous-types qui correspondent aux sous-types du type de données **sql_variant**.  
  
 La propriété de session SSPROP_ALLOWNATIVEVARIANT doit également avoir la valeur TRUE.  
  
## <a name="provider-specific-property-ssprop_allownativevariant"></a>Propriété SSPROP_ALLOWNATIVEVARIANT spécifique au fournisseur  
 Pour extraire des données, vous pouvez spécifier explicitement le type de données à retourner pour une colonne ou un paramètre. **IColumnsInfo** permet également d’obtenir les informations sur les colonnes et d’utiliser ces informations pour effectuer la liaison. Quand **IColumnsInfo** est utilisé pour obtenir des informations sur les colonnes en vue d’effectuer une liaison, si la propriété de session SSPROP_ALLOWNATIVEVARIANT a la valeur FALSE (valeur par défaut), DBTYPE_VARIANT est retourné pour les colonnes **sql_variant**. Si la propriété SSPROP_ALLOWNATIVEVARIANT a la valeur FALSE, DBTYPE_SQLVARIANT n'est pas pris en charge. Si la propriété SSPROP_ALLOWNATIVEVARIANT a la valeur TRUE, le type de colonne est retourné en tant que DBTYPE_SQLVARIANT, auquel cas la mémoire tampon contiendra la structure SSVARIANT. Pour extraire des données **sql_variant** en tant que DBTYPE_SQLVARIANT, la propriété de session SSPROP_ALLOWNATIVEVARIANT doit être définie sur TRUE.  
  
 La propriété SSPROP_ALLOWNATIVEVARIANT est une propriété de session et fait partie du jeu de propriétés DBPROPSET_SQLSERVERSESSION spécifique au fournisseur.  
  
 DBTYPE_VARIANT s'applique à tous les autres fournisseurs OLE DB.  
  
## <a name="ssprop_allownativevariant"></a>SSPROP_ALLOWNATIVEVARIANT  
 SSPROP_ALLOWNATIVEVARIANT est une propriété de session et fait partie du jeu de propriétés DBPROPSET_SQLSERVERSESSION.  
  
|||  
|-|-|  
|SSPROP_ALLOWNATIVEVARIANT|Tapez : VT_BOOL<br /><br /> Lecture/écriture : lecture/écriture<br /><br /> Valeur par défaut : VARIANT_FALSE<br /><br /> Description : détermine si les données sont extraites en tant que DBTYPE_VARIANT ou DBTYPE_SQLVARIANT.<br /><br /> VARIANT_TRUE : le type de colonne est retourné en tant que DBTYPE_SQLVARIANT, auquel cas la mémoire tampon contient la structure SSVARIANT.<br /><br /> VARIANT_FALSE : le type de colonne est retourné en tant que DBTYPE_VARIANT et la mémoire tampon a la structure VARIANT.|  
  
## <a name="see-also"></a>Voir aussi  
 [Types de données &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
  
