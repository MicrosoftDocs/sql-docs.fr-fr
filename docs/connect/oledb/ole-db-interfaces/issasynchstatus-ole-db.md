---
title: ISSAsynchStatus (pilote OLE DB) | Microsoft Docs
description: Découvrez comment OLE DB Driver pour SQL Server utilise l’interface ISSAsynchStatus pour prendre en charge des opérations asynchrones SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus (OLE DB)
apitype: COM
helpviewer_keywords:
- ISSAsynchStatus interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a4fd0424a169f36f430dcc79f2bc44515335101b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183710"
---
# <a name="issasynchstatus-ole-db"></a>ISSAsynchStatus (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L’interface **ISSAsynchStatus** expose la prise en charge des opérations asynchrones de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Il s’agit d’une interface facultative qui hérite de l’interface OLE DB de base **IDBAsynchStatus**. Outre les méthodes **Abort** et **GetStatus** héritées de **IDBAsynchStatus**, **ISSAsynchStatus** fournit une nouvelle méthode qui permet d'attendre qu'une opération asynchrone se termine ou qu'un délai d'expiration soit dépassé.  
  
|Méthode|Description|  
|------------|-----------------|  
|[ISSAsynchStatus::Abort &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-abort-ole-db.md)|Annule une opération s'exécutant de manière asynchrone.|  
|[ISSAsynchStatus::GetStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-getstatus-ole-db.md)|Retourne l'état d'une opération s'exécutant de manière asynchrone.|  
|[ISSAsynchStatus::WaitForAsynchCompletion &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md)|Attend que l'opération s'exécutant de façon asynchrone se termine ou qu'un délai d'expiration soit dépassé.|  
  
## <a name="remarks"></a>Notes  
 L'implémentation **ISSAsynchStatus** de la méthode **ISSAsynchStatus::GetStatus** est identique à la méthode **IDBAsynchStatus::GetStatus** , à la différence près que si l'initialisation d'un objet source de données est abandonnée, E_UNEXPECTED est retourné au lieu de DB_E_CANCELED (bien que **ISSAsynchStatus::WaitForAsynchCompletion** retourne DB_E_CANCELED). Cela est dû au fait que l’objet source de données ne reste pas dans l’état habituel après une opération d’abandon, de façon que d’autres tentatives d’initialisation puissent avoir lieu.  
  
 Les méthodes suivantes prennent en charge l'utilisation d'une exécution asynchrone dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]:  
  
-   **ICommand::Execute**  
  
-   **IOpenRowset::OpenRowset**  
  
-   **IMultipleResults::GetResult**  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)    
 [Exécution d’opérations asynchrones](../../oledb/features/performing-asynchronous-operations.md)  
  
  
