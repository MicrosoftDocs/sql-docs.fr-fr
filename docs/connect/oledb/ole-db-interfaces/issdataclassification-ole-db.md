---
title: ISSDataClassification | Microsoft Docs
description: Interface ISSDataClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification
apitype: COM
helpviewer_keywords:
- ISSDataClassification interface
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 23c575e31f151156165e09c34baf6c81b44b209b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342486"
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
