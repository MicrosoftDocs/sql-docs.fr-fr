---
description: sys_firewall_rules (Azure SQL Database)
title: sys. firewall_rules (Azure SQL Database) | Microsoft Docs
ms.date: 03/26/2019
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.firewall_rules
- firewall_rules
- sys.firewall_rules_TSQL
- firewall_rules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- firewall_rules
- sys.firewall_rules
ms.assetid: 140d2cd8-9aa1-4cc5-870d-e1dbc873b3fe
author: CarlRabeler
ms.author: carlrab
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: 71ce10d470e552041dc2282aa27c8af404302df1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88469911"
---
# <a name="sysfirewall_rules-azure-sql-database"></a>sys_firewall_rules (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Retourne des informations sur les paramètres de pare-feu au niveau du serveur associés à votre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
 La vue `sys.firewall_rules` contient les colonnes suivantes :  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|id|**TIERS**|Identificateur du paramètre de pare-feu au niveau serveur.|  
|name|**NVARCHAR (128)**|Le nom que vous avez choisi pour décrire et distinguer le paramètre de pare-feu au niveau serveur.|  
|start_ip_address|**VARCHAR (45)**|Adresse IP la plus basse dans la plage de paramètres de pare-feu au niveau serveur. Les adresses IP supérieures ou égales à celle-ci peuvent essayer de se connecter au serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus basse possible est `0.0.0.0`.|  
|end_ip_address|**VARCHAR (45)**|Adresse IP la plus élevée dans la plage de paramètres de pare-feu au niveau serveur. Les adresses IP inférieures ou égales à celle-ci peuvent essayer de se connecter au serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus élevée possible est `255.255.255.255`.<br /><br /> Remarque : les tentatives de connexion Azure sont autorisées lorsque ce champ et le champ **start_ip_address** est égal à `0.0.0.0` .|  
|create_date|**Date/heure**|Date et heure UTC auxquelles le paramètre de pare-feu au niveau serveur a été créé.<br /><br /> Remarque : l’heure UTC est un acronyme de temps universel coordonné.|  
|modify_date|**Date/heure**|Date et heure UTC auxquelles le paramètre de pare-feu au niveau serveur a été modifié la dernière fois.|  
  
## <a name="remarks"></a>Notes

 Pour renvoyer des informations sur les paramètres de pare-feu au niveau de la base de données associés à votre Microsoft Azure SQL Database, utilisez [sys. database_firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md).  
  
## <a name="permissions"></a>Autorisations

 L’accès en lecture seule à cette vue est disponible pour tous les utilisateurs autorisés à se connecter à la base de données **Master** .  
  
## <a name="see-also"></a>Voir aussi

[sp_set_firewall_rule &#40;Base de données Azure SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)  
[sp_delete_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database.md)   
[sp_set_database_firewall_rule &#40;Base de données Azure SQL&#41;](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)  
[sp_delete_database_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database.md)  
[sys. database_firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database.md)  
[Configurer un pare-feu Windows pour l’accès Moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)     
[Configurer un pare-feu pour l’accès FILESTREAM](../../relational-databases/blob/configure-a-firewall-for-filestream-access.md)  
[Configurer un pare-feu pour accéder au serveur de rapports](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md) 
