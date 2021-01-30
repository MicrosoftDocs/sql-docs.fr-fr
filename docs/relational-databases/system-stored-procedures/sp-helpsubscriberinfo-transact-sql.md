---
description: sp_helpsubscriberinfo (Transact-SQL)
title: sp_helpsubscriberinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscriberinfo
- sp_helpsubscriberinfo_TSQL
helpviewer_keywords:
- sp_helpsubscriberinfo
ms.assetid: fbabe1ec-57cf-425c-bae7-af7f5d3198fd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 25eb68ab0e5c825b646c254f494ab11e1b297b60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199121"
---
# <a name="sp_helpsubscriberinfo-transact-sql"></a>sp_helpsubscriberinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Affiche des informations relatives à un Abonné. Cette procédure stockée est exécutée sur n'importe quelle base de données du serveur de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpsubscriberinfo [ [ @subscriber =] 'subscriber']  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec la valeur par défaut **%** , qui retourne toutes les informations.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname** et sa valeur par défaut est le nom du serveur actuel.  
  
> [!NOTE]  
>  l' *éditeur* ne doit pas être spécifié, sauf s’il s’agit d’un serveur de publication Oracle.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**côté**|**sysname**|Nom de l'Abonné.|  
|**type**|**tinyint**|Type d'Abonné :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données **1** = source de données ODBC|  
|**login**|**sysname**|ID de connexion pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**mot de passe**|**sysname**|Mot de passe pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**commit_batch_size**|**int**|Non pris en charge.|  
|**status_batch_size**|**int**|Non pris en charge.|  
|**flush_frequency**|**int**|Non pris en charge.|  
|**frequency_type**|**int**|Fréquence d'exécution de l’Agent de distribution :<br /><br /> **1** = une fois<br /><br /> **2** = à la demande<br /><br /> **4** = tous les jours<br /><br /> **8** = hebdomadaire<br /><br /> **16** = mensuelle<br /><br /> **32** = mensuelle relative<br /><br /> **64** = démarrage automatique<br /><br /> **128** = périodique|  
|**frequency_interval**|**int**|Valeur appliquée à la fréquence définie par *frequency_type*.|  
|**frequency_relative_interval**|**int**|Date de la Agent de distribution utilisée lorsque *frequency_type* a la valeur **32** (valeur mensuelle relative) :<br /><br /> **1** = premier<br /><br /> **2** = seconde<br /><br /> **4** = troisième<br /><br /> **8** = quatrième<br /><br /> **16** = dernier|  
|**frequency_recurrence_factor**|**int**|Facteur de récurrence utilisé par *frequency_type*.|  
|**frequency_subday**|**int**|Fréquence de replanification nécessaire pendant la période définie :<br /><br /> **1** = une fois<br /><br /> **2** = seconde<br /><br /> **4** = minute<br /><br /> **8** = heure|  
|**frequency_subday_interval**|**int**|Intervalle de *frequency_subday*.|  
|**active_start_time_of_day**|**int**|Heure de la journée à laquelle le Agent de distribution est planifié pour la première fois, au format HHMMSS.|  
|**active_end_time_of_day**|**int**|Heure à laquelle l’Agent de distribution cesse d'être planifié, au format HHMMSS.|  
|**active_start_date**|**int**|Date à laquelle le Agent de distribution est planifié pour la première fois, au format AAAAMMJJ.|  
|**active_end_date**|**int**|Date à laquelle le Agent de distribution cesse d’être planifié, au format AAAAMMJJ.|  
|**retryattempts**|**int**|Non pris en charge.|  
|**retrydelay**|**int**|Non pris en charge.|  
|**description**|**nvarchar(255)**|Texte de description de l'Abonné.|  
|**security_mode**|**int**|Mode de sécurité implémenté :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification Windows|  
|**frequency_type2**|**int**|Fréquence d'exécution de l’Agent de fusion :<br /><br /> **1** = une fois<br /><br /> **2** = à la demande<br /><br /> **4** = tous les jours<br /><br /> **8** = hebdomadaire<br /><br /> **16** = mensuelle<br /><br /> **32** = mensuelle relative<br /><br /> **64** = démarrage automatique<br /><br /> **128** = périodique|  
|**frequency_interval2**|**int**|Valeur appliquée à la fréquence définie par *frequency_type*.|  
|**frequency_relative_interval2**|**int**|Date du Agent de fusion utilisé lorsque *frequency_type* a la valeur 32 (mensuel relatif) :<br /><br /> **1** = premier<br /><br /> **2** = seconde<br /><br /> **4** = troisième<br /><br /> **8** = quatrième<br /><br /> **16** = dernier|  
|**frequency_recurrence_factor2**|**int**|Facteur de récurrence utilisé par *frequency_type * *.*|  
|**frequency_subday2**|**int**|Fréquence de replanification nécessaire pendant la période définie :<br /><br /> **1** = une fois<br /><br /> **2** = seconde<br /><br /> **4** = minute<br /><br /> **8** = heure|  
|**frequency_subday_interval2**|**int**|Intervalle de *frequency_subday*.|  
|**active_start_time_of_day2**|**int**|Heure à laquelle l’Agent de fusion est planifié pour la première fois, au format HHMMSS.|  
|**active_end_time_of_day2**|**int**|Heure de la journée à laquelle le Agent de fusion cesse d’être planifié, au format HHMMSS.|  
|**active_start_date2**|**int**|Date à laquelle l’Agent de fusion est planifié pour la première fois, au format AAAAMMJJ.|  
|**active_end_date2**|**int**|Date à laquelle l’Agent de fusion cesse d'être planifié, au format AAAAMMJJ.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpsubscriberinfo** est utilisé dans la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** ou de la liste d’accès à la publication pour la publication peuvent s’exécuter **sp_helpsubscriberinfo**.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_adddistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_changesubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_dropsubscriber &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscriber-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
