---
title: Drop SQL Server table (fournisseur Native Client OLE DB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- tables [OLE DB]
- deleting tables
- SQL Server Native Client OLE DB provider, tables
- removing tables
- dropping tables
ms.assetid: b6d6c4de-43c6-473e-92aa-34ffddd58cbe
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d34ea9bec2ff64f2c7565b3d0980a8bc3aed5df
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87240413"
---
# <a name="dropping-a-sql-server-native-client-table"></a>Suppression d’une table SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client expose la fonction **ITableDefinition ::D roptable** pour supprimer une [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table d’une base de données.  
  
 Les consommateurs spécifient le nom de table en tant que chaîne de caractères Unicode dans le membre*pwszName* de l’union *uName* dans le paramètre *pTableID*. Le membre *eKind* de *pTableID* doit être DBKIND_NAME.  
  
## <a name="see-also"></a>Voir aussi  
 [Tables et index](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
  
