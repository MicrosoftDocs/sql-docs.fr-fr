---
description: sp_pdw_database_encryption (SQL Data Warehouse)
title: sp_pdw_database_encryption (SQL Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: f5ccb424-7a95-4557-b774-c69de33c1545
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 6b075ac707296073f6b4a4b606306b82571b4268
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91988792"
---
# <a name="sp_pdw_database_encryption-sql-data-warehouse"></a>sp_pdw_database_encryption (SQL Data Warehouse)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Utilisez **sp_pdw_database_encryption** pour activer le chiffrement transparent des données pour un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] appareil. Lorsque **sp_pdw_database_encryption** défini sur 1, utilisez l’instruction **ALTER DATABASE** pour chiffrer une base de données à l’aide de TDE.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption [ [ @enabled = ] enabled ] ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

#### <a name="parameters"></a>Paramètres  
`[ @enabled = ] enabled` Détermine si le chiffrement transparent des données est activé. l' *option Enabled* est de **type int**et peut prendre l’une des valeurs suivantes :  
  
-   0 - Désactivé  
  
-   1 - Activé  
  
 L’exécution de **sp_pdw_database_encryption** sans paramètres retourne l’état actuel de TDE sur l’appliance sous la forme d’un jeu de résultats scalaire : 0 pour désactivé, ou 1 pour activé.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 Lorsque le TDE est activé à l’aide de **sp_pdw_database_encryption**, la base de données tempdb est supprimée, recréée et chiffrée. Pour cette raison, le TDE ne peut pas être activé sur un appareil tant qu’il existe d’autres sessions actives utilisant tempdb. L’activation ou la désactivation de TDE sur un appareil est une action qui modifie l’état de l’appliance. dans la plupart des cas, il est prévu qu’elle soit exécutée une fois dans la durée de vie de l’appliance et doit être exécutée en l’absence de trafic sur l’appliance.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **sysadmin** ou l’autorisation **Control Server** .  
  
## <a name="example"></a>Exemple  
 L’exemple suivant active TDE sur l’appliance.  
  
```sql  
EXEC sys.sp_pdw_database_encryption 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_pdw_database_encryption_regenerate_system_keys &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)   
 [sp_pdw_log_user_data_masking &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
  
