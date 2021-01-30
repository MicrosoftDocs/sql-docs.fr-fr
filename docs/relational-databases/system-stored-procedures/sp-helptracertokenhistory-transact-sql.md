---
description: sp_helptracertokenhistory (Transact-SQL)
title: sp_helptracertokenhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helptracertokenhistory_TSQL
- sp_helptracertokenhistory
helpviewer_keywords:
- sp_helptracertokenhistory
ms.assetid: 96910d1c-be76-43eb-9c93-4477e6761749
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f701a1681281bfb63cf6c39ba42869ac83df204e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192873"
---
# <a name="sp_helptracertokenhistory-transact-sql"></a>sp_helptracertokenhistory (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Retourne des informations de latence détaillées pour des jetons de suivi donnés, avec une ligne retournée par abonné. Cette procédure stockée est exécutée sur la base de données de publication du serveur de publication ou sur la base de données de distribution du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helptracertokenhistory [ @publication = ] 'publication'   
        , [ @tracer_id = ] tracer_id  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication dans laquelle le jeton de suivi a été inséré. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @tracer_id = ] tracer_id` ID du jeton de suivi dans la table [MStracer_tokens &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) pour laquelle les informations d’historique sont retournées. *tracer_id* est de **type int**, sans valeur par défaut.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
> [!NOTE]
>  Ce paramètre ne doit être spécifié que pour les serveurs de publication non- [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données de publication. *publisher_db* est de **type sysname**, avec NULL comme valeur par défaut. Ce paramètre est ignoré si la procédure stockée est exécutée sur le serveur de publication.  
  
## <a name="result-set"></a>Jeu de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**distributor_latency**|**bigint**|Nombre de secondes s'écoulant entre la validation de l'enregistrement du jeton de suivi sur le serveur de publication et la validation de l'enregistrement sur le serveur de distribution.|  
|**côté**|**sysname**|Nom de l'Abonné qui a reçu le jeton de suivi.|  
|**subscriber_db**|**sysname**|Nom de la base de données d'abonnement dans laquelle l'enregistrement du jeton de suivi a été inséré.|  
|**subscriber_latency**|**bigint**|Nombre de secondes s'écoulant entre la validation de l'enregistrement du jeton de suivi sur le serveur de distribution et la validation de l'enregistrement sur l'Abonné.|  
|**overall_latency**|**bigint**|Nombre de secondes s'écoulant entre la validation de l'enregistrement du jeton de suivi sur le serveur de publication et la validation de l'enregistrement du jeton sur l'Abonné.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helptracertokenhistory** est utilisé dans la réplication transactionnelle.  
  
 Exécutez [sp_helptracertokens &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) pour obtenir la liste des jetons de suivi pour la publication.  
  
 Si le jeu de résultats contient la valeur NULL, les statistiques de latence ne peuvent pas être calculées. Cela est dû au fait que le jeton de suivi n'a pas été reçu sur le serveur de distribution ou sur l'un des Abonnés.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_tracertokens](../../relational-databases/replication/codesnippet/tsql/sp-helptracertokenhistor_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** dans la base de données de publication, ou **db_owner** des rôles de base de données fixe ou **replmonitor** dans la base de données de distribution peuvent exécuter **sp_helptracertokenhistory**.  
  
## <a name="see-also"></a>Voir aussi  
 [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [sp_deletetracertokenhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md)  
  
  
