---
description: sp_pdw_log_user_data_masking (Azure Synapse Analytics)
title: sp_pdw_log_user_data_masking (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 43c63b42-03cb-4fb5-8362-ec3b7e22a590
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 8c02aa65df689ab147d3c7312d8c1e84a513aa88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196067"
---
# <a name="sp_pdw_log_user_data_masking-azure-synapse-analytics"></a>sp_pdw_log_user_data_masking (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Utilisez **sp_pdw_log_user_data_masking** pour activer le masquage des données utilisateur dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité. Le masquage des données utilisateur affecte les instructions sur toutes les bases de données de l’appliance.  
  
> [!IMPORTANT]  
>  Les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité affectés par **sp_pdw_log_user_data_masking** sont certains [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité. **sp_pdw_log_user_data_masking** n’affecte pas les journaux de transactions de base de données ou les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] journaux d’erreurs.  
  
 **Arrière-plan :** Dans, les journaux d’activité de configuration par défaut [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] contiennent des [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions complètes et peuvent, dans certains cas, inclure des données utilisateur contenues dans des opérations telles que des instructions **Insert**, **Update** et **Select** . En cas de problème sur l’appliance, cela permet l’analyse des conditions qui ont provoqué le problème sans qu’il soit nécessaire de reproduire le problème. Afin d’éviter que les données utilisateur ne soient écrites dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité, les clients peuvent choisir d’activer le masquage des données utilisateur à l’aide de cette procédure stockée. Les instructions sont toujours écrites dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité, mais tous les littéraux dans les instructions qui peuvent contenir des données utilisateur sont masqués ; remplacés par des valeurs constantes prédéfinies.  
  
 Lorsque le chiffrement transparent des données est activé sur l’appliance, le masquage des données utilisateur dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité est automatiquement activé.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_log_user_data_masking [ [ @masking_mode = ] value ] ;  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

#### <a name="parameters"></a>Paramètres  
`[ @masking_mode = ] masking_mode` Détermine si le masquage transparent des données utilisateur du journal de chiffrement des données est activé. *masking_mode* est de **type int** et peut prendre l’une des valeurs suivantes :  
  
-   0 = désactivé, les données utilisateur apparaissent dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
-   1 = activé, les instructions de données utilisateur apparaissent dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité, mais les données utilisateur sont masquées.  
  
-   2 = les instructions contenant des données utilisateur ne sont pas écrites dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
 L’exécution de **sp_pdw_ log_user_data_masking** sans paramètres retourne l’état actuel du masquage des données utilisateur du journal TDE sur l’appareil sous la forme d’un jeu de résultats scalaire.  
  
## <a name="remarks"></a>Notes  
 Le masquage des données utilisateur dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] les journaux d’activité permet le remplacement des littéraux par des valeurs constantes prédéfinies dans des instructions **Select** et DML, car elles peuvent contenir des données utilisateur. La définition de *masking_mode* sur 1 ne masque pas les métadonnées, telles que les noms de colonne ou de table. La définition de *masking_mode* sur 2 supprime les instructions avec des métadonnées, telles que les noms de colonne ou de table.  
  
 Le masquage des données utilisateur dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] les journaux d’activité est implémenté de la manière suivante :  
  
-   TDE et le masquage des données utilisateur dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] les journaux d’activité sont désactivés par défaut. Les instructions ne sont pas automatiquement masquées si le chiffrement de la base de données n’est pas activé sur l’appliance.  
  
-   L’activation de TDE sur l’appliance active automatiquement le masquage des données utilisateur dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
-   La désactivation de TDE n’affecte pas le masquage des données utilisateur dans les [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
-   Vous pouvez activer explicitement le masquage des données utilisateur dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] les journaux d’activité à l’aide de la procédure **sp_pdw_log_user_data_masking** .  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle de base de données fixe **sysadmin** ou l’autorisation **Control Server** .  
  
## <a name="example"></a>Exemple  
 L’exemple suivant active TDE pour enregistrer le masquage des données utilisateur sur l’appliance.  
  
```sql  
EXEC sp_pdw_log_user_data_masking 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_pdw_database_encryption &#40;&#41;Azure Synapse Analytics ](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_database_encryption_regenerate_system_keys &#40;&#41;Azure Synapse Analytics ](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)  
  
  
