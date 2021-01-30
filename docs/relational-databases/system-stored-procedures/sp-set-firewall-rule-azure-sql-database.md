---
description: sp_set_firewall_rule (Azure SQL Database)
title: sp_set_firewall_rule (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 07/28/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_set_firewall_rule
- sp_set_firewall_rule_TSQL
- sys.sp_set_firewall_rule
- sys.sp_set_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_set_firewall_rule
- firewall_rules, setting server rules
ms.assetid: a974a561-5382-4039-8499-3a56767bcefe
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: aa47f5578403fa6cfd2f945ebcaa5bff767d967b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184732"
---
# <a name="sp_set_firewall_rule-azure-sql-database"></a>sp_set_firewall_rule (Azure SQL Database)
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  Crée ou met à jour les paramètres de pare-feu au niveau serveur du serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Cette procédure stockée est uniquement disponible dans la base de données Master à la connexion du principal au niveau du serveur ou à l’Azure Active Directory principal.  
  
  
## <a name="syntax"></a>Syntaxe  
  
```
sp_set_firewall_rule [@name =] 'name', 
    [@start_ip_address =] 'start_ip_address', 
    [@end_ip_address =] 'end_ip_address'
[ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
 Le tableau suivant montre les arguments et les options pris en charge dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] .  
  
|Nom|Datatype|Description|  
|----------|--------------|-----------------|  
|[ @name =] 'nom'|**NVARCHAR (128)**|Le nom utilisé pour décrire et distinguer le paramètre de pare-feu au niveau serveur.|  
|[ @start_ip_address =] 'start_ip_address'|**VARCHAR (50)**|Adresse IP la plus basse dans la plage de paramètres de pare-feu au niveau serveur. Les adresses IP supérieures ou égales à celle-ci peuvent essayer de se connecter au serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus basse possible est `0.0.0.0`.|  
|[ @end_ip_address =] 'end_ip_address'|**VARCHAR (50)**|Adresse IP la plus élevée dans la plage de paramètres de pare-feu au niveau serveur. Les adresses IP inférieures ou égales à celle-ci peuvent essayer de se connecter au serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. L"adresse IP la plus élevée possible est `255.255.255.255`.<br /><br /> Remarque : les tentatives de connexion Azure sont autorisées lorsque ce champ et le champ *start_ip_address* est égal à `0.0.0.0` .|  
  
## <a name="remarks"></a>Notes  
 Les noms des paramètres de pare-feu au niveau serveur doivent être uniques. Si le nom du paramètre de pare-feu spécifié pour la procédure stockée existe déjà dans le tableau des paramètres de pare-feu, les adresses IP de début et de fin sont mises à jour. Sinon, un nouveau paramètre de pare-feu au niveau serveur est créé.  
  
 Lorsque vous ajoutez un paramètre de pare-feu de niveau serveur et que les adresses IP de début et de fin sont égales à `0.0.0.0`, vous activez l'accès à votre serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] à partir de Azure. Spécifiez une valeur pour le paramètre de *nom* qui vous aidera à vous souvenir de la fonction du paramètre de pare-feu au niveau du serveur.  
  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Seul le nom de connexion du principal au niveau du serveur créé par le processus de configuration ou un principal Azure Active Directory affecté en tant qu’administrateur peut créer ou modifier des règles de pare-feu au niveau du serveur. L’utilisateur doit être connecté à la base de données Master pour exécuter sp_set_firewall_rule.  
  
## <a name="examples"></a>Exemples  
 Le code suivant crée un paramètre de pare-feu de niveau serveur nommé `Allow Azure` qui active l'accès à partir de Azure. Exécutez la commande suivante dans la base de données Master virtuelle.  
  
```  
-- Enable Azure connections.  
exec sp_set_firewall_rule N'Allow Azure', '0.0.0.0', '0.0.0.0';  
  
```  
  
 Le code suivant crée un paramètre de pare-feu au niveau serveur appelé `Example setting 1` uniquement pour l'adresse IP `0.0.0.2`. Ensuite, la `sp_set_firewall_rule` procédure stockée est de nouveau appelée pour mettre à jour l’adresse IP de fin `0.0.0.4` , dans ce paramètre de pare-feu. Cela permet de créer une plage qui autorise `0.0.0.2` les adresses IP, `0.0.0.3` et `0.0.0.4` à accéder au serveur.  
  
```  
-- Create server-level firewall setting for only IP 0.0.0.2  
exec sp_set_firewall_rule N'Example setting 1', '0.0.0.2', '0.0.0.2';  
  
-- Update server-level firewall setting to create a range of allowed IP addresses
exec sp_set_firewall_rule N'Example setting 1', '0.0.0.2', '0.0.0.4';  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Pare-feu Azure SQL Database](/azure/azure-sql/database/firewall-configure)   
 [Procédure : configurer les paramètres de pare-feu (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)   
 [sys.firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)