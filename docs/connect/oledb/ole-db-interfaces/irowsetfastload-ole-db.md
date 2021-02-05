---
title: IRowsetFastLoad (pilote OLE DB) | Microsoft Docs
description: Les contrôles serveur consommateur OLE DB Driver pour SQL Server utilisent l'interface IRowsetFastLoad pour ajouter rapidement des données à une table SQL Server existante.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- IRowsetFastLoad interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8915de0fe28bd8b5aa3e7cfb05c8281d9304237c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183776"
---
# <a name="irowsetfastload-ole-db"></a>IRowsetFastLoad (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L’interface **IRowsetFastLoad** expose la prise en charge des opérations de copie en bloc basées sur mémoire [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les consommateurs OLE DB Driver pour SQL Server utilisent l'interface pour ajouter rapidement des données à une table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] existante.  
  
 Si vous affectez la valeur VARIANT_TRUE à SSPROP_ENABLEFASTLOAD pour une session, vous ne pouvez pas lire les données des ensembles de lignes retournés ultérieurement à partir de cette session. Lorsque SSPROP_ENABLEFASTLOAD a la valeur VARIANT_TRUE, tous les ensembles de lignes créés sur la session sont du type IRowsetFastLoad. Les ensembles de lignes IRowsetFastLoad ne prennent pas en charge la fonctionnalité de récupération (fetch) des ensembles de lignes. Par conséquent, les données issues de ces ensembles de lignes ne peuvent pas être lues.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Méthode|Description|  
|------------|-----------------|  
|[IRowsetFastLoad::Commit &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-commit-ole-db.md)|Marque la fin d'un lot de lignes insérées et écrit les lignes dans la table [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
|[IRowsetFastLoad::InsertRow &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/irowsetfastload-insertrow-ole-db.md)|Ajoute une ligne à l'ensemble de lignes de copie en bloc.|  
  
## <a name="see-also"></a>Voir aussi  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)   
 [Copier des données en bloc avec IRowsetFastLoad &#40;OLE DB&#41;](../../oledb/ole-db-how-to/bulk-copy-data-using-irowsetfastload-ole-db.md)   
 [Envoyer des données BLOB vers SQL SERVER en utilisant IROWSETFASTLOAD et ISEQUENTIALSTREAM &#40;OLE DB&#41;](../../oledb/ole-db-how-to/send-blob-data-to-sql-server-using-irowsetfastload-and-isequentialstream-ole-db.md)  
  
  
