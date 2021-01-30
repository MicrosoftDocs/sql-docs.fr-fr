---
description: sp_helpdistributiondb (Transact-SQL)
title: sp_helpdistributiondb (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistributiondb_TSQL
- sp_helpdistributiondb
helpviewer_keywords:
- sp_helpdistributiondb
ms.assetid: a2917020-26d1-4011-99f8-9212d120fd2d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e9740605d1f3eca456ba506a692b5fc827273e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179300"
---
# <a name="sp_helpdistributiondb-transact-sql"></a>sp_helpdistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Affiche les propriétés de la base de données de distribution spécifiée. Cette procédure stockée est exécutée au niveau du serveur de distribution sur la base de données de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpdistributiondb [ [ @database= ] 'database_name' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @database = ] 'database_name'` Nom de la base de données pour laquelle les propriétés sont retournées. *database_name* est de **type sysname**, avec la valeur par défaut **%** pour toutes les bases de données associées au serveur de distribution et sur lesquelles l’utilisateur dispose d’autorisations.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nom de la base de données de distribution.|  
|**min_distretention**|**int**|Durée minimale de rétention (exprimée en heures) avant la suppression des transactions|  
|**max_distretention**|**int**|Durée maximale de rétention (exprimée en heures) avant la suppression des transactions|  
|**history retention**|**int**|Nombre d'heures de rétention de l'historique|  
|**history_cleanup_agent**|**sysname**|Nom de l'Agent de nettoyage de l'historique|  
|**distribution_cleanup_agent**|**sysname**|Nom de l'Agent de nettoyage de distribution.|  
|**statut**|**int**|À usage interne uniquement|  
|**data_folder**|**nvarchar(255)**|Nom du répertoire utilisé pour stocker les fichiers de la base de données.|  
|**data_file**|**nvarchar(255)**|Nom du fichier de base de données.|  
|**data_file_size**|**int**|Taille initiale du fichier de données en mégaoctets.|  
|**log_folder**|**nvarchar(255)**|Nom du répertoire du fichier de base de données.|  
|**log_file**|**nvarchar(255)**|Nom du fichier journal.|  
|**log_file_size**|**int**|Taille initiale du fichier du journal en mégaoctets.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpdistributiondb** est utilisé dans tous les types de réplications.  
  
## <a name="permissions"></a>Autorisations  
 Les membres du rôle de base de données fixe **db_owner** ou du rôle **replmonitor** dans une base de données de distribution et les utilisateurs de la liste d’accès à la publication d’une publication utilisant la base de données de distribution peuvent exécuter **sp_helpdistributiondb** pour retourner les informations relatives aux fichiers. Les membres du rôle **public** peuvent exécuter **sp_helpdistributiondb** pour renvoyer des informations non liées aux fichiers pour les bases de données de distribution auxquelles ils ont accès.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’un serveur de distribution et d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)   
 [sp_changedistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_dropdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
