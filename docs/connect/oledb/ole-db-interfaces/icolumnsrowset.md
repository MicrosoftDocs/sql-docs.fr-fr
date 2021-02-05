---
title: IColumnsRowset (pilote OLE DB) | Microsoft Docs
description: La colonne DBCOLUMN_BASETABLEINSTANCE dans IColumnsRowset::GetColumnRowset est réservée à l’utilisation par Microsoft dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 040052f04cdee697a616aa7ced6d790601d0c829
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191821"
---
# <a name="icolumnsrowset"></a>IColumnsRowset
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver pour SQL Server ajoute la colonne DBCOLUMN_BASETABLEINSTANCE à IColumnsRowset::GetColumnRowset. Cette colonne retourne DBTYPE_I2 et est réservée pour une utilisation par Microsoft. Les informations de cette colonne sont fournies sous réserve de modifications dans les versions ultérieures.  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md) 
  
  
