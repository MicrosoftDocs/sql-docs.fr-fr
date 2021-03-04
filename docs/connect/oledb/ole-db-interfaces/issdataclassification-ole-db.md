---
title: ISSDataClassification | Microsoft Docs
description: Interface ISSDataClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification
apitype: COM
helpviewer_keywords:
- ISSDataClassification interface
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: e799a27babfe8e7b920b7dcb1c1d8a9be8f4fb27
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837346"
---
# <a name="issdataclassification"></a>ISSDataClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L’interface **ISSDataClassification** fournit les fonctionnalités permettant de récupérer les données de classification de sensibilité de l’ensemble de lignes actif, telles qu’elles sont reçues de SQL Server.
  

## <a name="methods"></a>Méthodes

|Méthode|Description|  
|------------|-----------------|  
|[ISSDataClassification::GetSensitivityClassification](../../oledb/ole-db-interfaces/issdataclassification-getsensitivityclassification-ole-db.md)|Retourne un pointeur vers une structure SENSITIVITYCLASSIFICATION qui contient des informations de classification de sensibilité.|  

## <a name="see-also"></a>Voir aussi  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)   
 [Ensembles de lignes](../ole-db-rowsets/rowsets.md)   
 [Utilisation de la classification des données](../features/using-data-classification.md)
