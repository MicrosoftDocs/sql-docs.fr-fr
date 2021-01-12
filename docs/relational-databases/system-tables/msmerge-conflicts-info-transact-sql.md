---
description: MSmerge_conflicts_info (Transact-SQL)
title: MSmerge_conflicts_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_conflicts_info_TSQL
- MSmerge_conflicts_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflicts_info system table
ms.assetid: 6b76ae96-737a-4000-a6b6-fcc8772c2af4
author: cawrites
ms.author: chadam
ms.openlocfilehash: da7de7b1c8c18a74fc5a255027507853d195813b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091470"
---
# <a name="msmerge_conflicts_info-transact-sql"></a>MSmerge_conflicts_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Le tableau **MSmerge_conflicts_info** effectue le suivi des conflits qui se produisent lors de la synchronisation d’un abonnement à une publication de fusion. La perte des données de ligne pour les conflits est stockée dans la table [MSmerge_conflict_publication_article](../../relational-databases/system-tables/msmerge-conflict-publication-article-transact-sql.md) pour l’article où le conflit s’est produit. Cette table est stockée dans la base de données de publication du serveur de publication et dans la base de données d'abonnement de l'Abonné.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Surnom de la table publiée.|  
|**rowguid**|**uniqueidentifier**|Identificateur de la ligne de conflit|  
|**origin_datasource**|**nvarchar(255)**|Nom de la base de données d'où provient la modification conflictuelle|  
|**conflict_type**|**int**|Type de conflit qui s'est produit, qui peut être l'un des suivants :<br /><br /> **1** = conflit de mise à jour : le conflit est détecté au niveau de la ligne.<br /><br /> **2** = conflit de mise à jour de colonne : conflit détecté au niveau de la colonne.<br /><br /> **3** = conflit de suppression de la mise à jour : la suppression gagne le conflit.<br /><br /> **4** = mise à jour du conflit de suppression WINS : le rowguid supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **5** = échec de l’insertion du téléchargement : l’insertion à partir de l’abonné n’a pas pu être appliquée au serveur de publication.<br /><br /> **6** = échec du téléchargement de l’insertion : l’insertion à partir du serveur de publication n’a pas pu être appliquée au niveau de l’abonné.<br /><br /> **7** = échec du chargement de la suppression : la suppression sur l’abonné n’a pas pu être téléchargée sur le serveur de publication.<br /><br /> **8** = échec de la suppression du téléchargement : la suppression sur le serveur de publication n’a pas pu être téléchargée sur l’abonné.<br /><br /> **9** = échec de la mise à jour du téléchargement : la mise à jour sur l’abonné n’a pas pu être appliquée au serveur de publication.<br /><br /> **10** = échec de la mise à jour du téléchargement : la mise à jour sur le serveur de publication n’a pas pu être appliquée à l’abonné.<br /><br /> **11** = résolution<br /><br /> **12** = suppression WINS des enregistrements logiques : l’enregistrement logique supprimé qui perd le conflit est enregistré dans cette table.<br /><br /> **13** = conflit d’enregistrements logiques insertion d’une mise à jour : l’insertion sur un enregistrement logique est en conflit avec une mise à jour.<br /><br /> **14** = conflit de suppression de la mise à jour WINS de l’enregistrement logique : l’enregistrement logique mis à jour qui perd le conflit est enregistré dans cette table.|  
|**reason_code**|**int**|Code d’erreur qui peut être sensible au contexte. Dans le cas de conflits update-Update et Update-Delete, la valeur utilisée pour cette colonne est la même que la **conflict_type**. Par contre, dans les conflits d'échec de modification, le code de la raison correspond à l'erreur qui a empêché l'Agent de fusion d'appliquer la modification. Par exemple, si le Agent de fusion ne peut pas appliquer une insertion sur l’abonné en raison d’une violation de clé primaire, il journalise un **conflict_type** de 6 (« échec du téléchargement de l’insertion ») et un **reason_code** de 2627, qui est le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] message d’erreur interne pour une violation de clé primaire : «violation de la contrainte% ls'%. * ls'. Impossible d’insérer une clé en double dans l’objet'% '. \* ls'.»|  
|**reason_text**|**nvarchar (720)**|Description de l’erreur qui peut être contextuelle.|  
|**pubid**|**uniqueidentifier**|Identificateur de la publication.|  
|**MSrepl_create_time**|**datetime**|Heure à laquelle le conflit s'est produit.|  
|**origin_datasource_id**|**uniqueidentifier**|Identificateur de la base de données d'où provient la modification à l'origine du conflit|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
