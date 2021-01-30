---
description: sp_helpreplicationdboption (Transact-SQL)
title: sp_helpreplicationdboption (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpreplicationdboption_TSQL
- sp_helpreplicationdboption
helpviewer_keywords:
- sp_helpreplicationdboption
ms.assetid: 143ce689-108b-49d7-9892-fd3a86897f38
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 17c47d247e154ed80f0daeb8bacdb4984864fa03
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210803"
---
# <a name="sp_helpreplicationdboption-transact-sql"></a>sp_helpreplicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Indique si les bases de données du serveur de publication sont activées pour la réplication. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication. *Non pris en charge pour les serveurs de publication Oracle.*  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpreplicationdboption [ [ @dbname =] 'dbname' ]  
    [ , [ @type = ] 'type' ]  
    [ , [ @reserved = ] reserved ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @dbname = ] 'dbname'` Nom de la base de données. *dbname* est de **type sysname**, avec la valeur par défaut **%** . Si **%** la valeur est, le jeu de résultats contient toutes les bases de données sur le serveur de publication, dans le cas contraire, seules les informations sur la base de données spécifiée sont retournées. Aucune information n'est retournée sur les bases de données pour lesquelles l'utilisateur ne possède pas les autorisations appropriées, comme décrit ci-dessous.  
  
`[ @type = ] 'type'` Limite le jeu de résultats à contenir uniquement les bases de données sur lesquelles la valeur de *type* d’option de réplication spécifiée a été activée. *type* est de type **sysname** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**édition**|Réplication transactionnelle autorisée.|  
|**publication de fusion**|Réplication de fusion autorisée.|  
|**réplication autorisée** (par défaut)|Réplication autorisée, qu'elle soit transactionnelle ou de fusion.|  
  
`[ @reserved = ] reserved` Spécifie si les informations sur les publications et les abonnements existants sont retournées. la valeur *réservée* est de **bit**, avec 0 comme valeur par défaut. Si la valeur est **1**, le jeu de résultats inclut des informations indiquant si la base de données spécifiée a des publications ou des abonnements existants.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la base de données.|  
|**id**|**int**|Identificateur de base de données.|  
|**transpublish**|**bit**|Si la base de données a été activée pour la publication transactionnelle ou d’instantané ; la valeur **1** signifie que la publication transactionnelle ou d’instantané est activée.|  
|**mergepublish**|**bit**|Si la base de données a été activée pour la publication de fusion ; la valeur **1** signifie que la publication de fusion est activée.|  
|**DBOwner**|**bit**|Si l’utilisateur est membre du rôle de base de données fixe **db_owner** ; où la valeur **1** indique que l’utilisateur est membre de ce rôle.|  
|**dbreadonly**|**bit**|Si la base de données est marquée en lecture seule ; la valeur **1** signifie que la base de données est en lecture seule.|  
|**haspublications**|**bit**|Indique si la base de données contient des publications existantes ; la valeur **1** signifie qu’il existe des publications existantes.|  
|**haspullsubscriptions**|**bit**|Indique si la base de données a des abonnements par extraction existants. la valeur **1** signifie qu’il y a des abonnements par extraction existants.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpreplicationdboption** est utilisé dans les réplications d’instantané, transactionnelles et de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_helpreplicationdboption** pour n’importe quelle base de données. Les membres du rôle de base de données fixe **db_owner** peuvent exécuter **sp_helpreplicationdboption** pour cette base de données.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_replicationdboption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
