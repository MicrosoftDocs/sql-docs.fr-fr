---
description: sp_helpmergeconflictrows (Transact-SQL)
title: sp_helpmergeconflictrows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergeconflictrows_TSQL
- sp_helpmergeconflictrows
helpviewer_keywords:
- sp_helpmergeconflictrows
ms.assetid: 131395a5-cb18-4795-a7ae-fa09d8ff347f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4aeb1205f3d929eed7b2717ff388d8e03891d9e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179259"
---
# <a name="sp_helpmergeconflictrows-transact-sql"></a>sp_helpmergeconflictrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Renvoie les lignes de la table de conflits spécifiée. Cette procédure stockée est exécutée sur l'ordinateur qui héberge la table de conflits.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_helpmergeconflictrows [ [ @publication = ] 'publication' ]  
        , [ @conflict_table = ] 'conflict_table'  
    [ , [ @publisher = ] 'publisher' ]   
    [ , [ @publisher_db = ] 'publsher_db' ]   
    [ , [ @logical_record_conflicts = ] logical_record_conflicts ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, avec la valeur par défaut **%** . Si la publication est spécifiée, tous les conflits qualifiés par la publication sont renvoyés. Par exemple, si la table **MSmerge_conflict_Customers** contient des lignes conflictuelles pour les publications **wa** et **ca** , le passage d’un nom de publication **ca** récupère les conflits qui se rapportent à la publication de l' **autorité de certification** .  
  
`[ @conflict_table = ] 'conflict_table'` Nom de la table de conflits. *conflict_table* est de **type sysname**, sans valeur par défaut. Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures, les tables de conflits sont nommées en utilisant les noms de format avec **MSmerge_conflict \_ _\_ article de publication_**, avec une table pour chaque article publié.  
  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données du serveur de publication. *publisher_db* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @logical_record_conflicts = ] logical_record_conflicts` Indique si le jeu de résultats contient des informations sur les conflits d’enregistrements logiques. *logical_record_conflicts* est de **type int**, avec 0 comme valeur par défaut. **1** signifie que des informations sur les conflits d’enregistrements logiques sont retournées.  
  
## <a name="result-sets"></a>Jeux de résultats  
 **sp_helpmergeconflictrows** retourne un jeu de résultats composé de la structure de la table de base et de ces colonnes supplémentaires.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**origin_datasource**|**varchar(255)**|Origine du conflit.|  
|**conflict_type**|**int**|Code indiquant le type de conflit :<br /><br /> **1** = conflit de mise à jour : le conflit est détecté au niveau de la ligne.<br /><br /> **2** = conflit de mise à jour de colonne : conflit détecté au niveau de la colonne.<br /><br /> **3** = conflit de suppression de la mise à jour : la suppression gagne le conflit.<br /><br /> **4** = mise à jour du conflit de suppression WINS : le rowguid supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **5** = échec de l’insertion du téléchargement : l’insertion à partir de l’abonné n’a pas pu être appliquée au serveur de publication.<br /><br /> **6** = échec du téléchargement de l’insertion : l’insertion à partir du serveur de publication n’a pas pu être appliquée au niveau de l’abonné.<br /><br /> **7** = échec du chargement de la suppression : la suppression sur l’abonné n’a pas pu être téléchargée sur le serveur de publication.<br /><br /> **8** = échec de la suppression du téléchargement : la suppression sur le serveur de publication n’a pas pu être téléchargée sur l’abonné.<br /><br /> **9** = échec de la mise à jour du téléchargement : la mise à jour sur l’abonné n’a pas pu être appliquée au serveur de publication.<br /><br /> **10** = échec de la mise à jour du téléchargement : la mise à jour sur le serveur de publication n’a pas pu être appliquée à l’abonné.<br /><br /> **12** = suppression WINS des enregistrements logiques : l’enregistrement logique supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **13** = conflit d’enregistrements logiques insertion d’une mise à jour : l’insertion sur un enregistrement logique est en conflit avec une mise à jour.<br /><br /> **14** = conflit de suppression de la mise à jour WINS de l’enregistrement logique : l’enregistrement logique mis à jour qui perd le conflit est enregistré dans cette table.|  
|**reason_code**|**int**|Code d'erreur pouvant dépendre du contexte.|  
|**reason_text**|**varchar (720)**|Description de l'erreur qui peut dépendre du contexte.|  
|**pubid**|**uniqueidentifier**|Identificateur de publication.|  
|**MSrepl_create_time**|**datetime**|Moment où l'information sur les conflits a été ajoutée.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_helpmergeconflictrows** est utilisé dans la réplication de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** , du rôle de base de données fixe **db_owner** et du rôle **replmonitor** dans la base de données de distribution peuvent exécuter **sp_helpmergeconflictrows**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher les informations de conflit pour les publications de fusion &#40;la programmation Transact-SQL de la réplication&#41;](../replication/view-and-resolve-data-conflicts-for-merge-publications.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
