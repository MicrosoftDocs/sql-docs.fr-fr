---
description: sp_cleanup_log_shipping_history (Transact-SQL)
title: sp_cleanup_log_shipping_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cleanup_log_shipping_history_TSQL
- sp_cleanup_log_shipping_history
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cleanup_log_shipping_history
ms.assetid: 96d236a9-1d0e-4f83-a4d3-f825b7381e46
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 456a083257452e180c7fae90b7f5f79a67924011
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206514"
---
# <a name="sp_cleanup_log_shipping_history-transact-sql"></a>sp_cleanup_log_shipping_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette procédure stockée nettoie l'historique localement et sur le serveur moniteur en fonction de la période de rétention.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cleanup_log_shipping_history  
[ @agent_id = ] 'agent_id',  
[ @agent_type = ] 'agent_type'  
```  
  
## <a name="arguments"></a>Arguments  
`[ @agent_id = ] 'agent_id',` ID principal de la sauvegarde ou ID secondaire pour la copie ou la restauration. *agent_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
`[ @agent_type = ] 'agent_type'` Type du travail d’envoi de journaux. 0 = Sauvegarde, 1 = Copie, 2 = Restauration. *agent_type* est de **type tinyint** et ne peut pas avoir la valeur null.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 **sp_cleanup_log_shipping_history** doit être exécuté à partir de la base de données **Master** sur un serveur de copie des journaux de données. Cette procédure stockée nettoie les copies locales et distantes des **log_shipping_monitor_history_detail** et des **log_shipping_monitor_error_detail** en fonction de la période de rétention de l’historique.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
