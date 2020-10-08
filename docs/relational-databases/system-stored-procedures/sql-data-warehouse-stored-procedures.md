---
description: Procédures stockées SQL Data Warehouse
title: Procédures stockées SQL Data Warehouse | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.service: sql-data-warehouse
ms.subservice: design
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 02e04dfe-d565-4e45-b427-b8e89c958ba3
author: ronortloff
ms.author: rortloff
monikerRange: = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 75186b766ae144719838db981ea9b857876bce23
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91809436"
---
# <a name="sql-data-warehouse-stored-procedures"></a>Procédures stockées SQL Data Warehouse
[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] fournit des procédures intégrées que vous pouvez utiliser pour effectuer des opérations liées aux rôles de base de données. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] comprend les procédures système suivantes :  
  
<a name="AggregateFunctions"></a>[sp_datatype_info_90 &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-datatype-info-90-sql-data-warehouse.md)  
  
 [sp_pdw_add_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md)  
  
 [sp_pdw_database_encryption &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)  
  
 [sp_pdw_database_encryption_regenerate_system_keys &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)  
  
 [sp_pdw_log_user_data_masking &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
 [sp_pdw_remove_network_credentials &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
 [sp_special_columns_100 &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-special-columns-100-sql-data-warehouse.md)  
  
> [!NOTE]  
>  Certaines procédures stockées système supplémentaires sont utilisées uniquement dans une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou via des API clientes et ne sont pas destinées à une utilisation générale du client. Ces procédures sont répertoriées dans [procédures stockées système (Transact-SQL)](./system-stored-procedures-transact-sql.md). Ces procédures sont sujettes à modification et la compatibilité n’est pas garantie. Toutes les procédures de la liste ne sont pas disponibles dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] .  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions stockées système &#40;&#41;Transact-SQL ](~/relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
