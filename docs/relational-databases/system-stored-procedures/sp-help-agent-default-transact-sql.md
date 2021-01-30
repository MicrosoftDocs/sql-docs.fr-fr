---
description: sp_help_agent_default (Transact-SQL)
title: sp_help_agent_default (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_help_agent_default
- sp_help_agent_default_TSQL
helpviewer_keywords:
- sp_help_agent_default
ms.assetid: 7ba55e39-05dd-43c7-b5da-b268ed8426dd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a5c2d74b71dfc55f4654b566e314157895f743bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208969"
---
# <a name="sp_help_agent_default-transact-sql"></a>sp_help_agent_default (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Récupère l'ID de la configuration par défaut du type d'Agent passé en paramètre. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_agent_default [ @profile_id= ] profile_id OUTPUT   
        , [ @agent_type = ] agent_type  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_id = ] _profile_idOUTPUT` ID de la configuration par défaut pour le type d’agent. *profile_id* est de **type int**, sans valeur par défaut. *profile_id* est également un paramètre de sortie et retourne l’ID de la configuration par défaut pour le type d’agent.  
  
`[ @agent_type = ] 'agent_type'` Type d’agent. *agent_type* est de **type int**, sans valeur par défaut, et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**1**|Agent d'instantané.|  
|**2**|Agent de lecture du journal|  
|**3**|Agent de distribution.|  
|**4**|Agent de fusion.|  
|**9**|Agent de lecture de la file d'attente|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_help_agent_default** est utilisé dans tous les types de réplications.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **replmonitor** peuvent exécuter **sp_help_agent_default**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
