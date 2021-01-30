---
title: sp_replmonitorhelpmergesessiondetail (T-SQL)
description: Décrit la procédure stockée sp_replmonitorhelpmergesessiondetail qui retourne des informations détaillées au niveau de l’article sur une session de Agent de fusion de réplication spécifique.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpmergesessiondetail
- sp_replmonitorhelpmergesessiondetail_TSQL
helpviewer_keywords:
- sp_replmonitorhelpmergesessiondetail
ms.assetid: 805c92fc-3169-410c-984d-f37e063b791d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba050d49579e217fa5be5a2dedcd7aa21cf0c99d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204379"
---
# <a name="sp_replmonitorhelpmergesessiondetail-transact-sql"></a>sp_replmonitorhelpmergesessiondetail (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Retourne des informations détaillées de niveau article sur une session spécifique de l'Agent de fusion des réplications, qui permettent de surveiller la réplication des fusions. Le jeu de résultats comprend une ligne de détails pour chaque article synchronisé pendant la session. Il comprend également une ligne qui représente l'initialisation de la session et des lignes qui récapitulent les différentes phases de téléchargement (Upload et Download) de la session. Cette procédure stockée est exécutée sur la base de données de distribution du serveur de distribution ou sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorhelpmergesessiondetail [ @session_id = ] session_id  
```  
  
## <a name="arguments"></a>Arguments  
`[ @session_id = ] session_id` Spécifie une session d’agent. *session_id* est de **type int** sans valeur par défaut.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**PhaseID**|**int**|Phase de la session de synchronisation, qui peut prendre l'une des valeurs suivantes :<br /><br /> **0** = ligne d’initialisation ou de résumé<br /><br /> **1** = téléchargement<br /><br /> **2** = téléchargement|  
|**ArticleName**|**sysname**|Nom de l'article en cours de synchronisation. **ArticleName** contient également des informations de synthèse pour les lignes du jeu de résultats qui ne représentent pas les détails de l’article.|  
|**PercentComplete**|**decimal**|Indique le pourcentage de modifications appliqué dans une ligne de détails d'article donnée pour des sessions en cours d'exécution ou ayant échoué.|  
|**RelativeCost**|**decimal**|Indique le temps consacré à la synchronisation de l'article en pourcentage de la durée de synchronisation totale pour la session.|  
|**Durée**|**int**|Durée de la session de l'Agent.|  
|**Inserts**|**int**|Nombre d'insertions dans une session.|  
|**Mises à jour**|**int**|Nombre de mises à jour dans une session.|  
|**Suppressions**|**int**|Nombre de suppressions dans une session.|  
|**Conflits**|**int**|Nombre de conflits qui se sont produits dans une session.|  
|**ErrorID**|**int**|ID d'une erreur de session.|  
|**SeqNo**|**int**|Ordre des sessions dans le jeu de résultats.|  
|**RowType**|**int**|Indique le type d'informations que représente chaque ligne du jeu de résultats.<br /><br /> **0** = initialisation<br /><br /> **1** = Résumé du téléchargement<br /><br /> **2** = détail du chargement de l’article<br /><br /> **3** = Résumé du téléchargement<br /><br /> **4** = détail du téléchargement de l’article|  
|**SchemaChanges**|**int**|Nombre de modifications de schéma dans une session.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorhelpmergesessiondetail** est utilisé pour surveiller la réplication de fusion.  
  
 Lorsqu’elle est exécutée sur l’abonné, **sp_replmonitorhelpmergesessiondetail** renvoie uniquement des informations détaillées sur les 5 dernières sessions agent de fusion.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle de base de données fixe **db_owner** ou **replmonitor** sur la base de données de distribution sur le serveur de distribution ou sur la base de données d’abonnement sur l’abonné peuvent exécuter **sp_replmonitorhelpmergesessiondetail**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
