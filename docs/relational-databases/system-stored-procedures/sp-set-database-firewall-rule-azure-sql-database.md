---
description: sp_set_database_firewall_rule (Azure SQL Database)
title: sp_set_database_firewall_rule
titleSuffix: Azure SQL Database
ms.date: 08/04/2017
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_set_database_firewall_rule
- sp_set_database_firewall_rule_TSQL
- sys.sp_set_database_firewall_rule
- sys.sp_set_database_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_database_firewall_rule
- firewall_rules, setting database rules
ms.assetid: 8f0506b6-a4ac-4e4d-91db-8077c40cb17a
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 5aca09db0b26d15826547c3fb9847614ad5aa722
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184745"
---
# <a name="sp_set_database_firewall_rule-azure-sql-database"></a>sp_set_database_firewall_rule (Azure SQL Database)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Crée ou met à jour les règles de pare-feu au niveau de la base de données pour votre [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] . Les règles de pare-feu de base de données peuvent être configurées pour la base de données **Master** et les bases de données utilisateur sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] . Les règles de pare-feu de base de données sont particulièrement utiles lors de l’utilisation des utilisateurs de base de données Pour plus d’informations, voir [Utilisateurs de base de données autonome - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_set_database_firewall_rule [@name = ] [N]'name'  
, [@start_ip_address =] 'start_ip_address'  
, [@end_ip_address =] 'end_ip_address'
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @name = ] [N]'name'` Nom utilisé pour décrire et distinguer le paramètre de pare-feu au niveau de la base de données. *Name* est de type **nvarchar (128)** sans valeur par défaut. L’identificateur Unicode `N` est facultatif pour [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] . 
  
`[ @start_ip_address = ] 'start_ip_address'` Adresse IP la plus basse dans la plage du paramètre de pare-feu au niveau de la base de données. Les adresses IP supérieures ou égales à celle-ci peuvent essayer de se connecter à l'instance [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus basse possible est `0.0.0.0`. *start_ip_address* est de type **varchar (50)** sans valeur par défaut.  
  
`[ @end_ip_address = ] 'end_ip_address'` Adresse IP la plus élevée dans la plage du paramètre de pare-feu au niveau de la base de données. Les adresses IP inférieures ou égales à celle-ci peuvent essayer de se connecter à l'instance [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus élevée possible est `255.255.255.255`. *end_ip_address* est de type **varchar (50)** sans valeur par défaut.  
  
 Le tableau suivant montre les arguments et les options pris en charge dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] .  
  
> [!NOTE]  
>  Les tentatives de connexion Azure sont autorisées lorsque ce champ et le champ *start_ip_address* est égal à `0.0.0.0` .  
  
## <a name="remarks"></a>Notes  
 Les noms des paramètres de pare-feu au niveau base de données doivent être uniques. Si le nom du paramètre de pare-feu au niveau base de données spécifié pour la procédure stockée existe déjà dans le tableau des paramètres de pare-feu au niveau base de données, les adresses IP de début et de fin sont mises à jour. Sinon, un nouveau paramètre de pare-feu au niveau base de données est créé.  
  
 Lorsque vous ajoutez un paramètre de pare-feu au niveau de la base de données alors que les adresses IP de début et de fin sont égales à `0.0.0.0` , vous activez l’accès à votre base de données sur le [!INCLUDE[ssSDS](../../includes/sssds-md.md)] serveur à partir de n’importe quelle ressource Azure. Spécifiez une valeur pour le paramètre de *nom* qui vous aidera à vous souvenir de la fonction du paramètre de pare-feu.  
  
## <a name="permissions"></a>Autorisations  
 Exige l’autorisation **CONTROL** sur la base de données.  
  
## <a name="examples"></a>Exemples  
 Le code suivant crée un paramètre de pare-feu de niveau base de données nommé `Allow Azure` qui active l'accès à votre base de données à partir de Azure.  
  
```  
-- Enable Azure connections.  
EXECUTE sp_set_database_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';  
  
```  
  
 Le code suivant crée un paramètre de pare-feu au niveau base de données appelé `Example DB Setting 1` uniquement pour l'adresse IP `0.0.0.4`. Ensuite, la `sp_set_database firewall_rule` procédure stockée est de nouveau appelée pour mettre à jour l’adresse IP de fin `0.0.0.6` , dans ce paramètre de pare-feu. Cela permet de créer une plage qui autorise `0.0.0.4` les adresses IP, `0.0.0.5` et `0.0.0.6` à accéder à la base de données.
  
```  
-- Create database-level firewall setting for only IP 0.0.0.4  
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.4';  
  
-- Update database-level firewall setting to create a range of allowed IP addresses
EXECUTE sp_set_database_firewall_rule N'Example DB Setting 1', '0.0.0.4', '0.0.0.6';  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Pare-feu Azure SQL Database](/azure/azure-sql/database/firewall-configure)   
 [Procédure : configurer les paramètres de pare-feu (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)   
 [sp_set_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sp_delete_database_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)   
 [sys.database_firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
  
