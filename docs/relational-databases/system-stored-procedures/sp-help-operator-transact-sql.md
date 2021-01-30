---
description: sp_help_operator (Transact-SQL)
title: sp_help_operator (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_operator
- sp_help_operator_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_operator
ms.assetid: caedc43d-44b8-415a-897e-92923f6de3b8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 03346efcec8571ed6bf586a09a6da3631da28dc4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199664"
---
# <a name="sp_help_operator-transact-sql"></a>sp_help_operator (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Fournit des informations sur les opérateurs définis pour le serveur.  
  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_operator  
     { [ @operator_name = ] 'operator_name'   
     | [ @operator_id = ] operator_id }  
```  
  
## <a name="arguments"></a>Arguments  
`[ @operator_name = ] 'operator_name'` Nom de l’opérateur. *operator_name* est de **type sysname**. Si *operator_name* n’est pas spécifié, des informations sur tous les opérateurs sont retournées.  
  
`[ @operator_id = ] operator_id` Numéro d’identification de l’opérateur pour lequel des informations sont demandées. *operator_id* est de **type int**, avec NULL comme valeur par défaut.  
  
> [!NOTE]  
>  *Operator_id* ou *operator_name* doivent être spécifiés, mais ne peuvent pas être spécifiés.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Numéro d'identification de l'opérateur.|  
|**name**|**sysname**|Nom de l’opérateur.|  
|**activé**|**tinyint**|L'opérateur est disponible pour recevoir des notifications :<br /><br /> **1** = Oui<br /><br /> **0** = Non|  
|**email_address**|**nvarchar(100**|Adresse de messagerie de l'opérateur.|  
|**last_email_date**|**int**|Date de la dernière notification envoyée par courrier électronique à l'opérateur.|  
|**last_email_time**|**int**|Heure de la dernière notification envoyée par courrier électronique à l'opérateur.|  
|**pager_address**|**nvarchar(100**|Adresse de radiomessagerie de l'opérateur.|  
|**last_pager_date**|**int**|Date de la dernière notification envoyée par radiomessagerie à l'opérateur.|  
|**last_pager_time**|**int**|Heure de la dernière notification envoyée par radiomessagerie à l'opérateur.|  
|**weekday_pager_start_time**|**int**|Début de la période pendant laquelle l'opérateur peut recevoir des notifications par radiomessagerie pendant la semaine.|  
|**weekday_pager_end_time**|**int**|Fin de la période pendant laquelle l'opérateur peut recevoir des notifications par radiomessagerie pendant la semaine.|  
|**saturday_pager_start_time**|**int**|Début de la période pendant laquelle l'opérateur peut recevoir des notifications par radiomessagerie le samedi.|  
|**saturday_pager_end_time**|**int**|Fin de la période pendant laquelle l'opérateur peut recevoir des notifications par radiomessagerie le samedi.|  
|**sunday_pager_start_time**|**int**|Début de la période pendant laquelle l'opérateur peut recevoir des notifications par radiomessagerie le dimanche.|  
|**sunday_pager_end_time**|**int**|Fin de la période pendant laquelle l'opérateur peut recevoir des notifications par radiomessagerie le dimanche.|  
|**pager_days**|**tinyint**|Masque de bits (**1** = dimanche, **64** = samedi) des jours de la semaine indiquant quand l’opérateur est disponible pour recevoir des notifications par radiomessagerie.|  
|**netsend_address**|**nvarchar(100**|Adresse de l'opérateur pour les notifications envoyées par le réseau|  
|**last_netsend_date**|**int**|Date de la dernière notification envoyée à l'opérateur via le réseau.|  
|**last_netsend_time**|**int**|Heure de la dernière notification envoyée à l'opérateur via le réseau.|  
|**category_name**|**sysname**|Nom de la catégorie à laquelle appartient cet opérateur.|  
  
## <a name="remarks"></a>Notes  
 **sp_help_operator** doit être exécuté à partir de la base de données **msdb** .  
  
## <a name="permissions"></a>Autorisations  
 Par défaut, les membres du rôle serveur fixe **sysadmin** peuvent exécuter cette procédure stockée. Les autres utilisateurs doivent disposer de l'un des rôles de base de données fixes suivants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans la base de données **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Pour en savoir plus sur les autorisations de ces rôles, consultez [Rôles de base de données fixes de l'Agent SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Exemples  
 Cet exemple vous renseigne sur l'opérateur `François Ajenstat`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_operator  
    @operator_name = N'François Ajenstat' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md)   
 [sp_delete_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-operator-transact-sql.md)   
 [sp_update_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-operator-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
