---
title: Support FILESTREAM | Microsoft Docs
description: SQL Server prend en charge la fonctionnalité FILESTREAM améliorée, qui vous permet de stocker et d’accéder à des valeurs binaires élevées, via SQL Server ou le système de fichiers.
ms.custom: ''
ms.date: 09/13/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- FILESTREAM [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server [FILESTREAM support]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1fcc048d51186289d13cbe8918b5328f8c604f8
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766137"
---
# <a name="filestream-support-in-ole-db-driver-for-sql-server"></a>Support FILESTREAM dans OLE DB Driver pour SQL Server
[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

À compter de [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], OLE DB Driver pour SQL Server prend en charge la fonctionnalité FILESTREAM améliorée. Pour obtenir des exemples, consultez [Filestream et OLE DB](../../oledb/ole-db-how-to/filestream/filestream-and-ole-db.md).  

FILESTREAM permet de stocker et d'accéder à de grandes valeurs binaires, soit par le biais de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], soit par accès direct au système de fichiers Windows. Une grande valeur binaire est une valeur supérieure à 2 gigaoctets (Go). Pour plus d'informations sur la prise en charge FILESTREAM améliorée, consultez [FILESTREAM &#40;SQL Server&#41;](../../../relational-databases/blob/filestream-sql-server.md).  
  
Lorsqu’une connexion de base de données est ouverte, **\@\@TEXTSIZE** est défini avec la valeur -1 (« illimité ») par défaut.  
  
Il est également possible d'accéder et de mettre à jour des colonnes FILESTREAM à l'aide d'API de système de fichiers Windows.  
  
Pour plus d’informations, consultez [Accéder à des données FILESTREAM avec OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
## <a name="querying-for-filestream-columns"></a>Interrogation de colonnes FILESTREAM  
Les ensembles de lignes de schéma dans OLE DB ne signalent pas si une colonne est une colonne FILESTREAM. ITableDefinition dans OLE DB ne peut pas être utilisé pour créer une colonne FILESTREAM.    
  
Pour créer des colonnes FILESTREAM ou détecter les colonnes existantes qui sont des colonnes FILESTREAM, vous pouvez utiliser la colonne **is_filestream** de l’affichage catalogue [sys.columns](../../../relational-databases/system-catalog-views/sys-columns-transact-sql.md).  
  
Par exemple :  
  
```sql  
-- Create a table with a FILESTREAM column.  
CREATE TABLE Bob_01 (GuidCol1 uniqueidentifier ROWGUIDCOL NOT NULL UNIQUE DEFAULT NEWID(), IntCol2 int, varbinaryCol3 varbinary(max) FILESTREAM);  
  
-- Find FILESTREAM columns.  
SELECT name FROM sys.columns WHERE is_filestream=1;  
  
-- Determine whether a column is a FILESTREAM column.  
SELECT is_filestream FROM sys.columns WHERE name = 'varbinaryCol3' AND object_id IN (SELECT object_id FROM sys.tables WHERE name='Bob_01');  
```  
  
## <a name="down-level-compatibility"></a>Compatibilité de bas niveau  
Si votre client a été compilé à l’aide d’OLE DB Driver pour SQL Server et que l’application se connecte à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] ou ultérieur), le comportement **varbinary(max)** sera compatible avec le comportement introduit par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client dans [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Autrement dit, la taille maximale des données retournées est limitée à 2 Go. Pour les valeurs de résultat supérieures à 2 Go, une troncation se produit et un avertissement « Troncation à droite de la chaîne de données » est retourné. 
  
Lorsque la compatibilité de type de données est définie à 80, le comportement client est cohérent avec le comportement client de bas niveau.  
  
Pour les clients qui utilisent SQLOLEDB ou d’autres fournisseurs publiés avant [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], **varbinary(max)** est mappé à l’image.  
  
## <a name="comments"></a>Commentaires
- Pour envoyer et recevoir des valeurs **varbinary(max)** supérieures à 2 Go, une application utilise **DBTYPE_IUNKNOWN** dans les liaisons de résultat et de paramètre. Pour les paramètres, le fournisseur doit appeler IUnknown::QueryInterface pour ISequentialStream et pour les résultats qui retournent ISequentialStream.  

-  Pour OLE DB, la vérification en rapport avec les valeurs ISequentialStream sera levée. Lorsque *wType* est **DBTYPE_IUNKNOWN** dans la struct **DBBINDING**, la vérification de la longueur peut être désactivée en omettant **DBPART_LENGTH** de *dwPart* ou en définissant la longueur des données (au niveau du décalage *obLength* dans le tampon de données) sur ~0. Dans ce cas, le fournisseur ne vérifiera pas la longueur de la valeur et demandera et retournera toutes les données disponibles par le biais du flux de données. Cette modification sera appliquée à tous les types d'objets volumineux (LOB) et XML, mais uniquement en cas de connexion à des serveurs [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] (ou ultérieurs). Cela offrira une plus grande souplesse aux développeurs, tout en maintenant la cohérence et la compatibilité descendante des applications existantes et des serveurs de niveau inférieur.  Cette modification affecte toutes les interfaces qui transfèrent des données, principalement IRowset::GetData, ICommand::Execute et IRowsetFastLoad::InsertRow.
 

## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
