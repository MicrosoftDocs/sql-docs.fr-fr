---
description: sp_help_targetserver (Transact-SQL)
title: sp_help_targetserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_targetserver_TSQL
- sp_help_targetserver
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_targetserver
ms.assetid: f841d3bd-901a-4980-ad0b-1c6eeba3f717
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6441641d84e534c91fa0dc887c32a426af5bf669
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205128"
---
# <a name="sp_help_targetserver-transact-sql"></a>sp_help_targetserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Répertorie tous les serveurs cibles.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_targetserver   
     [ [ @server_name = ] 'server_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @server_name = ] 'server_name'` Nom du serveur pour lequel des informations doivent être retournées. *SERVER_NAME* est de type **nvarchar (30)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si *SERVER_NAME* n’est pas spécifié, **sp_help_targetserver** retourne ce jeu de résultats.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Numéro d’identification du serveur.|  
|**server_name**|**nvarchar(30)**|Nom du serveur.|  
|**location**|**nvarchar(200)**|Emplacement du serveur spécifié.|  
|**time_zone_adjustment**|**int**|Définition du fuseau horaire, en heures, par rapport à l'heure de Greenwich (GMT).|  
|**enlist_date**|**datetime**|Date d'inscription du serveur spécifié.|  
|**last_poll_date**|**datetime**|Date à laquelle le serveur a été interrogé pour la dernière fois pour des travaux.|  
|**statut**|**int**|État du serveur spécifié.|  
|**unread_instructions**|**int**|Indique si le serveur a des instructions non lues. Si toutes les lignes ont été téléchargées, cette colonne a la **valeur 0**.|  
|**local_time**|**datetime**|Heure et date locales sur le serveur cible, qui sont fonction de l'heure locale du serveur cible lors de la dernière interrogation du serveur maître.|  
|**enlisted_by_nt_user**|**nvarchar(100**|Utilisateur Microsoft Windows ayant inscrit le serveur cible.|  
|**poll_interval**|**int**|Fréquence (en secondes) à laquelle le serveur cible interroge le service SQLServerAgent principal afin de télécharger des travaux et charger des états de travaux.|  
  
## <a name="permissions"></a>Autorisations  
 Pour exécuter cette procédure stockée, l'utilisateur doit être membre du rôle de serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-information-for-all-registered-target-servers"></a>R. Affichage d'informations pour tous les serveurs cibles enregistrés  
 L'exemple suivant produit une liste d'informations pour tous les serveurs cibles enregistrés.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetserver ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-target-server"></a>B. Affichage d'informations pour un serveur cible spécifique  
 L'exemple suivant produit une liste d'informations pour le serveur cible `SEATTLE2`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetserver N'SEATTLE2' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_add_targetservergroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-targetservergroup-transact-sql.md)   
 [sp_delete_targetserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-targetserver-transact-sql.md)   
 [sp_delete_targetservergroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-targetservergroup-transact-sql.md)   
 [sp_update_targetservergroup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-targetservergroup-transact-sql.md)   
 [dbo.sysdownloadlist &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysdownloadlist-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
