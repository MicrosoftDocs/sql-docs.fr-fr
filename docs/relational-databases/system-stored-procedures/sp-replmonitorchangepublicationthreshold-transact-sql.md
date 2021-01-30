---
title: sp_replmonitorchangepublicationthreshold (T-SQL)
description: Décrit la procédure stockée sp_replmonitorchangepublicationthreshold qui modifie la mesure du seuil d’analyse pour une publication.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorchangepublicationthreshold_TSQL
- sp_replmonitorchangepublicationthreshold
helpviewer_keywords:
- sp_replmonitorchangepublicationthreshold
ms.assetid: 2c3615d8-4a1a-4162-b096-97aefe6ddc16
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 62f360416f46eee0bbe685f6ebd1af7520526ee6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204385"
---
# <a name="sp_replmonitorchangepublicationthreshold-transact-sql"></a>sp_replmonitorchangepublicationthreshold (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifie la mesure du seuil de supervision pour une publication. Cette procédure stockée, utilisée pour surveiller la réplication, est exécutée sur la base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorchangepublicationthreshold [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @metric_id = ] metric_id ]   
    [ , [ @thresholdmetricname = ] 'thresholdmetricname'   
    [ , [ @value = ] value ]   
    [ , [ @shouldalert = ] shouldalert ]   
    [ , [ @mode = ] mode ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données publiée. *publisher_db* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication = ] 'publication'` Nom de la publication pour laquelle les attributs de seuil d’analyse sont modifiés. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @publication_type = ] publication_type` Si le type de publication. *publication_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Publication transactionnelle.|  
|**1**|Publication d'instantané.|  
|**2**|Publication de fusion.|  
|NULL (par défaut)|La réplication essaie de déterminer le type de publication.|  
  
`[ @metric_id = ] metric_id` ID de la métrique de seuil de publication en cours de modification. *metric_id* est de **type int**, avec NULL comme valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Nom de métrique|  
|-----------|-----------------|  
|**1**|**expiration** : contrôle l'expiration imminente des abonnements aux publications transactionnelles.|  
|**2**|**latency** : contrôle les performances des abonnements aux publications transactionnelles.|  
|**4**|**mergeexpiration** : contrôle l'expiration imminente des abonnements aux publications de fusion.|  
|**5**|**mergeslowrunduration** : contrôle la durée des synchronisations de fusion sur les connexions à faible bande passante (accès à distance).|  
|**6**|**mergefastrunduration** : contrôle la durée des synchronisations de fusion sur les connexions de réseau local (LAN) à bande passante élevée.|  
|**7**|**mergefastrunspeed** - supervise le taux de synchronisation des synchronisations de fusion sur des connexions à bande passante élevée (LAN).|  
|**8**|**mergeslowrunspeed** : contrôle la vitesse de synchronisation des synchronisations de fusion sur les connexions à faible bande passante (accès à distance).|  
  
 Vous devez spécifier *metric_id* ou *thresholdmetricname*. Si *thresholdmetricname* est spécifié, *metric_id* doit avoir la valeur null.  
  
`[ @thresholdmetricname = ] 'thresholdmetricname'` Nom de la métrique de seuil de publication en cours de modification. *thresholdmetricname* est de **type sysname**, avec NULL comme valeur par défaut. Vous devez spécifier *thresholdmetricname* ou *metric_id*. Si *metric_id* est spécifié, *thresholdmetricname* doit avoir la valeur null.  
  
`[ @value = ] value` Nouvelle valeur de la mesure du seuil de publication. la *valeur* est de **type int**, avec NULL comme valeur par défaut. Si la valeur est **null**, la valeur de la métrique n’est pas mise à jour.  
  
`[ @shouldalert = ] shouldalert` Indique si une alerte est générée lorsqu’une métrique de seuil de publication est atteinte. *ShouldAlert* est de type **bit**, avec NULL comme valeur par défaut. La valeur **1** signifie qu’une alerte est générée, tandis que la valeur **0** indique qu’une alerte n’est pas générée.  
  
`[ @mode = ] mode` Indique si la métrique du seuil de publication est activée. *mode* est de **type tinyint**, avec **1** comme valeur par défaut. La valeur **1** signifie que la surveillance de cette métrique est activée, et la valeur **2** signifie que la surveillance de cette métrique est désactivée.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorchangepublicationthreshold** est utilisé avec tous les types de réplications.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle de base de données fixe **db_owner** ou **replmonitor** dans la base de données de distribution peuvent exécuter **sp_replmonitorchangepublicationthreshold**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
