---
title: IBCPSession2 (pilote OLE DB) | Microsoft Docs
description: En savoir plus sur l’interface IBCPSession2 dans OLE DB Driver pour SQL Server, qui fournit BCPSetBulkMode, une alternative à IBCPSession::BCPColFmt par colonne.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- IBCPSession2 interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 50cc43cec2e51162a887801bc52dcf34d4df26fa
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88862325"
---
# <a name="ibcpsession2-ole-db"></a>IBCPSession2 (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L'interface IBCPSession2 est une extension à IBCPSession qui fournit une fonction membre qui est une alternative à l'appel de IBCPSession::BCPColFmt pour chaque colonne.  IBCPSession2 hérite de IBCPSession et ajoute une nouvelle méthode : [IBCPSession2::BCPSetBulkMode](../../oledb/ole-db-interfaces/ibcpsession2-bcpsetbulkmode.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)
  
  
