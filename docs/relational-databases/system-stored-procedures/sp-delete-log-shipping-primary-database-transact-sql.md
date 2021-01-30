---
description: sp_delete_log_shipping_primary_database (Transact-SQL)
title: sp_delete_log_shipping_primary_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_log_shipping_primary_database
- sp_delete_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_log_shipping_primary_database
ms.assetid: cb1d5d00-2805-4d47-bd04-545232067345
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a42bb060044c35e4d2569e23e24eb8ba9bc46fd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211139"
---
# <a name="sp_delete_log_shipping_primary_database-transact-sql"></a>sp_delete_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette procédure stockée supprime la copie des journaux de transactions de la base de données primaire, y compris les travaux de sauvegarde, ainsi que les historiques locaux et distants. Utilisez cette procédure stockée uniquement après avoir supprimé les bases de données secondaires à l’aide de **sp_delete_log_shipping_primary_secondary**.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_delete_log_shipping_primary_database  
[ @database = ] 'database'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @database = ] 'database'` Nom de la base de données primaire d’envoi de journaux. *Database est de* **type sysname**, sans valeur par défaut et ne peut pas avoir la valeur null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 **sp_delete_log_shipping_primary_database** doit être exécuté à partir de la base de données **Master** sur le serveur principal. Elle effectue les actions suivantes :  
  
1.  Supprime le travail de sauvegarde de la base de données primaire spécifiée.  
  
2.  Supprime l’enregistrement du moniteur local dans **log_shipping_monitor_primary** sur le serveur principal.  
  
3.  Supprime les entrées correspondantes dans **log_shipping_monitor_history_detail** et **log_shipping_monitor_error_detail**.  
  
4.  Si le serveur moniteur est différent du serveur principal, supprime l’enregistrement du moniteur dans **log_shipping_monitor_primary** sur le serveur moniteur.  
  
5.  Supprime les entrées correspondantes dans **log_shipping_monitor_history_detail** et **log_shipping_monitor_error_detail** sur le serveur moniteur.  
  
6.  Supprime l’entrée de **log_shipping_primary_databases** pour cette base de données primaire.  
  
7.  Appelle **sp_delete_log_shipping_alert_job** sur le serveur moniteur.  

## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="examples"></a>Exemples  
 Cet exemple illustre l’utilisation de **sp_delete_log_shipping_primary_database** pour supprimer la base de données primaire **AdventureWorks2012**.  
  
```  
EXEC master.dbo.sp_delete_log_shipping_primary_database @database = N'AdventureWorks2012';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
