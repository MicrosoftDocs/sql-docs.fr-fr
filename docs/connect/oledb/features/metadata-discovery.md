---
title: Découverte des métadonnées | Microsoft Docs
description: Découverte des métadonnées dans OLE DB Driver pour SQL Server
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 95f3d5aad2705645cc58c1b4a85e40ad9e5b0d26
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86006922"
---
# <a name="metadata-discovery"></a>Découverte des métadonnées
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  L’amélioration de la découverte des métadonnées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] permet aux applications OLE DB Driver pour SQL Server de s’assurer que les métadonnées de colonne ou de paramètre retournées par l’exécution d’une requête sont identiques ou conformes au format des métadonnées que vous avez spécifié avant l’exécution de la requête. Vous recevrez une erreur si les métadonnées retournées après l'exécution de la requête ne sont pas compatibles avec le format des métadonnées que vous avez spécifié avant l'exécution de la requête.  
  
 Dans bcp ainsi que dans les interfaces IBCPSession et IBCPSession2, vous pouvez maintenant spécifier une lecture différée (découverte des métadonnées retardée) pour éviter la découverte de métadonnées pour des opérations de requête. Cela améliore la performance et élimine les échecs de découverte des métadonnées.  
  
 Si vous développez une application avec OLE DB Driver pour SQL Server, mais que vous vous connectez à une version du serveur antérieure à [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], les fonctionnalités de découverte des métadonnées correspondront à la version du serveur.  
  
## <a name="remarks"></a>Notes   
 Les fonctions membres OLE DB suivantes ont été améliorées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] pour fournir une fonctionnalité améliorée de découverte des métadonnées :  
  
-   IColumnsInfo::GetColumnInfo  
  
-   IColumnsRowset::GetColumnsRowset  
  
-   ICommandWithParameters::GetParameterInfo (voir [ICommandWithParameters](../../oledb/ole-db-interfaces/icommandwithparameters.md) pour plus d’informations)  
  
 Vous noterez également une amélioration des performances lors de la spécification du format de métadonnées avec IBCPSession::BCPSetBulkMode  
  
 La découverte améliorée des métadonnées dans OLE DB Driver pour SQL Server est possible grâce à l'ajout de deux procédures stockées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] :  
  
-   sp_describe_first_result_set  
  
-   sp_describe_undeclared_parameters  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
