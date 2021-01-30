---
description: sp_refresh_log_shipping_monitor (Transact-SQL)
title: sp_refresh_log_shipping_monitor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refresh_log_shipping_monitor
- sp_refresh_log_shipping_monitor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refresh_log_shipping_monitor
ms.assetid: edefb912-31c5-4d99-9aba-06629afd0171
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 16c1fb1de57fb0e1c61410071ec8fde31951f2ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185713"
---
# <a name="sp_refresh_log_shipping_monitor-transact-sql"></a>sp_refresh_log_shipping_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette procédure stockée actualise les tables de moniteurs distants avec les dernières informations provenant d'un serveur principal ou secondaire spécifique pour l'Agent de copie des journaux de transaction. La procédure est appelée sur le serveur principal ou secondaire.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_refresh_log_shipping_monitor  
[ @agent_id = ] 'agent_id',  
[ @agent_type = ] 'agent_type'  
[ @database = ] 'database'  
[ @mode ] n  
```  
  
## <a name="arguments"></a>Arguments  
`[ @agent_id = ] 'agent_id'` ID principal de la sauvegarde ou ID secondaire pour la copie ou la restauration. *agent_id* est de type **uniqueidentifier** et ne peut pas être null.  
  
`[ @agent_type = ] 'agent_type'` Type du travail d’envoi de journaux.  
  
 0 = Sauvegarde.  
  
 1 = Copie.  
  
 2 = Restauration.  
  
 *agent_type* est de **type tinyint** et ne peut pas avoir la valeur null.  
  
`[ @database = ] 'database'` Base de données primaire ou secondaire utilisée par la journalisation par les agents de sauvegarde ou de restauration.  
  
`[ @mode ] n` Spécifie s’il faut actualiser les données d’analyse ou les nettoyer. Le type de données de *m* est tinyint et les valeurs prises en charge sont les suivantes :  
  
 1 = actualisation (Il s'agit de la valeur par défaut.)  
  
 2 = delete  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 **sp_refresh_log_shipping_monitor** actualise les tables **log_shipping_monitor_primary**, **log_shipping_monitor_secondary**, **log_shipping_monitor_history_detail** et **log_shipping_monitor_error_detail** avec les informations de session qui n’ont pas encore été transférées. Vous pouvez ainsi synchroniser le serveur moniteur avec un serveur principal ou secondaire lorsque sa dernière synchronisation remonte à un certain temps et vous pouvez si nécessaire y nettoyer les informations de moniteur.  
  
 **sp_refresh_log_shipping_monitor** doit être exécuté à partir de la base de données **Master** sur le serveur principal ou secondaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de la copie des journaux des transactions &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
