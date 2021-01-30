---
description: sp_replcmds (Transact-SQL)
title: sp_replcmds (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replcmds_TSQL
- sp_replcmds
helpviewer_keywords:
- sp_replcmds
ms.assetid: 7e932f80-cc6e-4109-8db4-2b7c8828df73
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d8d61544e171e102c04f829ab7c302048c930d21
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193514"
---
# <a name="sp_replcmds-transact-sql"></a>sp_replcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Retourne les commandes pour les transactions signalées pour la réplication. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
> [!IMPORTANT]  
>  La procédure **sp_replcmds** doit être exécutée uniquement pour résoudre les problèmes de réplication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replcmds [ @maxtrans = ] maxtrans  
```  
  
## <a name="arguments"></a>Arguments  
`[ @maxtrans = ] maxtrans` Nombre de transactions à propos desquelles retourner des informations. *maxtrans* est de **type int**, avec **1** comme valeur par défaut, qui spécifie la prochaine transaction en attente de distribution.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**ID de l’article**|**int**|ID de l’article.|  
|**partial_command**|**bit**|Indique s'il s'agit d'une commande partielle|  
|**command**|**varbinary (1024)**|La valeur de commande.|  
|**xactid**|**binary(10)**|ID de la transaction.|  
|**xact_seqno**|**varbinary(16)**|Numéro de séquence de transaction.|  
|**publication_id**|**int**|ID de la publication.|  
|**command_id**|**int**|ID de la commande dans [MSrepl_commands](../../relational-databases/system-tables/msrepl-commands-transact-sql.md).|  
|**command_type**|**int**|Type de commande.|  
|**originator_srvname**|**sysname**|Serveur d'origine de la transaction.|  
|**originator_db**|**sysname**|Base de données d'origine de la transaction.|  
|**pkHash**|**int**|À usage interne uniquement|  
|**originator_publication_id**|**int**|ID de la publication d'origine de la transaction.|  
|**originator_db_version**|**int**|Version de la base de données d'origine de la transaction.|  
|**originator_lsn**|**varbinary(16)**|Identifie le numéro séquentiel dans le journal (LSN) de la commande dans la publication d'origine.|  
  
## <a name="remarks"></a>Notes  
 **sp_replcmds** est utilisé par le processus de lecture du journal dans la réplication transactionnelle.  
  
 La réplication traite le premier client qui exécute **sp_replcmds** dans une base de données donnée en tant que lecteur de journal.  
  
 Cette procédure peut générer des commandes pour des tables propriétaires qualifiées, ou ne pas qualifier le nom de la table (valeur par défaut). L'ajout de noms de table qualifiés autorise la réplication des données à partir de tables appartenant à un utilisateur spécifique dans une base de données, vers des tables appartenant à ce même utilisateur dans une autre base de données.  
  
> [!NOTE]  
>  Étant donné que le nom de table figurant dans la base de données source est qualifié par le nom du propriétaire, le propriétaire de la table dans la base de données cible doit porter le même nom de propriétaire.  
  
 Les clients qui tentent d’exécuter des **sp_replcmds** au sein de la même base de données reçoivent l’erreur 18752 jusqu’à ce que le premier client se déconnecte. Une fois que le premier client se déconnecte, un autre client peut exécuter **sp_replcmds** et devient le nouveau lecteur de journal.  
  
 Un message d’avertissement numéro 18759 est ajouté au [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Journal des erreurs et au [!INCLUDE[msCoName](../../includes/msconame-md.md)] Journal des applications Windows si **sp_replcmds** ne parvient pas à répliquer une commande de texte parce que le pointeur de texte n’a pas été récupéré dans la même transaction.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_replcmds**.  
  
## <a name="see-also"></a>Voir aussi  
 [Messages d’erreur](../../relational-databases/native-client-odbc-error-messages/error-messages.md)   
 [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
