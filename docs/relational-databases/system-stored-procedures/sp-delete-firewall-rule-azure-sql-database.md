---
description: sp_delete_firewall_rule (Azure SQL Database)
title: sp_delete_firewall_rule
titleSuffix: Azure SQL Database
ms.date: 07/27/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sp_delete_firewall_rule_TSQL
- sp_delete_firewall_rule
- sys.sp_delete_firewall_rule
- sys.sp_delete_firewall_rule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_firewall_rule procedure
ms.assetid: cf93eed1-ba97-4850-9fcc-b9c5a9317908
author: VanMSFT
ms.author: vanto
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 0908e90e8f84c98a3b4b74cc5877cc11d41d6de2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195510"
---
# <a name="sp_delete_firewall_rule-azure-sql-database"></a>sp_delete_firewall_rule (Azure SQL Database)
[!INCLUDE [asdb-asa](../../includes/applies-to-version/asdb-asa.md)]

  Supprime des paramètres de pare-feu de niveau serveur de votre serveur [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Cette procédure stockée est uniquement disponible dans la base de données master à la connexion du principal au niveau du serveur.  

  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_delete_firewall_rule [@name =] 'name' 
[ ; ] 
```  
  
## <a name="arguments"></a>Arguments  
 L'argument de la procédure stockée est le suivant :  
  
 [ @name =] '*nom*'  
 Nom du paramètre de pare-feu de niveau serveur qui sera supprimé. *Name* est de type **nvarchar (128)** sans valeur par défaut.  
  
## <a name="remarks"></a>Notes  
 Dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], les données de connexion exigées pour authentifier une connexion et les règles de pare-feu de niveau serveur sont temporairement mises en cache dans chaque base de données. Ce cache est régulièrement actualisé. Pour forcer une actualisation du cache d’authentification et garantir qu’une base de données a la version la plus récente de la table de connexions, exécutez [DBCC FLUSHAUTHCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-flushauthcache-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Seul le nom de connexion du principal au niveau du serveur créé par le processus de déploiement peut supprimer les règles de pare-feu côté serveur. L’utilisateur doit être connecté à la base de données Master pour exécuter sp_delete_firewall_rule.  
  
## <a name="example"></a>Exemple  
 L'exemple suivant supprime le paramètre de pare-feu de niveau serveur nommé « Example setting 1 ». Exécutez l’instruction dans la base de données Master virtuelle.  
  
```   
EXEC sp_delete_firewall_rule N'Example setting 1';   
```  
  
## <a name="see-also"></a>Voir aussi  
 [Pare-feu Azure SQL Database](/azure/azure-sql/database/firewall-configure)   
 [Procédure : configurer les paramètres de pare-feu (Azure SQL Database)](/azure/azure-sql/database/firewall-configure)   
 [sp_set_firewall_rule &#40;Azure SQL Database&#41;](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)   
 [sys.firewall_rules &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database.md)  
  
