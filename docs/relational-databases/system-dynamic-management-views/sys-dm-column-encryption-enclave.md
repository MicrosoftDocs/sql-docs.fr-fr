---
description: sys.dm_column_encryption_enclave (Transact-SQL)
title: sys.dm_column_encryption_enclave (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: system-objects
ms.topic: reference
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 402c5f6703a8639c1fbedc645da54ae821162bcd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200339"
---
# <a name="sysdm_column_encryption_enclave-transact-sql"></a>sys.dm_column_encryption_enclave (Transact-SQL)
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Retourne des compteurs de performances pour l’enclave sécurisée pour Always Encrypted. Pour plus d’informations, consultez [Always Encrypted avec enclaves sécurisées](../security/encryption/always-encrypted-enclaves.md).

Si l’enclave est configurée et a été correctement initialisée après le dernier redémarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la vue contient exactement une ligne. Si l’enclave n’est pas configurée ou n’a pas été initialisée correctement, la vue ne retourne aucune ligne. 

|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|current_enclave_session_count|**int**|Nombre actuel de sessions clientes utilisant l’enclave.|  
|current_column_encryption_key_count|**int**|Nombre de clés de chiffrement de colonne que l’enclave contient actuellement.|  
|current_memory_size_kb|**bigint**|Taille de la mémoire de l’enclave en Ko|  
|total_evicted_session_count|**bigint**|Nombre total de sessions d’enclaves supprimées depuis le dernier redémarrage du serveur.|   
  
## <a name="permissions"></a>Autorisations  
Nécessite l'autorisation `VIEW SERVER STATE`.   
  
## <a name="examples"></a>Exemples  
 
```sql  
SELECT * FROM sys.dm_column_encryption_enclave;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer le type d’enclave pour l’option de configuration de serveur Always Encrypted](../../database-engine/configure-windows/configure-column-encryption-enclave-type.md)
  
  
