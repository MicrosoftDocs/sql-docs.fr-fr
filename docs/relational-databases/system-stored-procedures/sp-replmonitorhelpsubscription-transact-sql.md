---
description: sp_replmonitorhelpsubscription (Transact-SQL)
title: sp_replmonitorhelpsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replmonitorhelpsubscription_TSQL
- sp_replmonitorhelpsubscription
helpviewer_keywords:
- sp_replmonitorhelpsubscription
ms.assetid: a681b2db-c82d-4624-a10c-396afb0ac42f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 898104516115207bd0cee45bf05f187922c13f6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204363"
---
# <a name="sp_replmonitorhelpsubscription-transact-sql"></a>sp_replmonitorhelpsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Renvoie des informations sur l'état actuel d'abonnements appartenant à une ou plusieurs publications du serveur de publication, et retourne une ligne pour chaque abonnement retourné. Cette procédure stockée, utilisée pour surveiller la réplication, est exécutée sur la base de données du serveur de distribution.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_replmonitorhelpsubscription [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
    [ , [ @mode = ] mode ]  
    [ , [ @topnum = ] topnum ]   
    [ , [ @exclude_anonymous = ] exclude_anonymous ]   
    [ , [ @refreshpolicy = ] refreshpolicy ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication dont l’État est en cours d’analyse. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut. Si la **valeur est null**, les informations sont retournées pour tous les serveurs de publication qui utilisent le serveur de distribution.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données publiée. *publisher_db* est de **type sysname**, avec NULL comme valeur par défaut. Si la valeur est NULL, les informations retournées concernent toutes les bases de données publiées situées sur le serveur de publication.  
  
`[ @publication = ] 'publication'` Nom de la publication en cours d’analyse. *publication* est de **type sysname**, avec NULL comme valeur par défaut.  
  
`[ @publication_type = ] publication_type` Si le type de publication. *publication_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Publication transactionnelle.|  
|**1**|Publication d'instantané.|  
|**2**|Publication de fusion.|  
|NULL (par défaut)|La réplication tente de déterminer le type de publication.|  
  
`[ @mode = ] mode` Mode de filtrage à utiliser pour retourner les informations de surveillance d’abonnement. le *mode* est **int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|Retourne tous les abonnements.|  
|**1**|Retourne uniquement les abonnements qui ont des erreurs.|  
|**2**|Retourne uniquement les abonnements qui ont généré des avertissements de mesure de seuil.|  
|**3**|Retourne uniquement les abonnements qui ont des erreurs ou ayant généré des avertissements de mesure de seuil.|  
|**4**|Retourne les 25 principaux abonnements les moins performants.|  
|**5**|Retourne les 50 abonnements les moins performants.|  
|**6**|Retourne uniquement les abonnements en cours de synchronisation.|  
|**7**|Retourne uniquement les abonnements qui ne sont pas en cours de synchronisation.|  
  
`[ @topnum = ] topnum` Limite le jeu de résultats au nombre d’abonnements spécifié au début des données retournées. *topnum* est de **type int**, sans valeur par défaut.  
  
`[ @exclude_anonymous = ] exclude_anonymous` Indique si les abonnements extraits anonymes sont exclus du jeu de résultats. *exclude_anonymous* est de **bit**, avec **0** comme valeur par défaut ; la valeur **1** signifie que les abonnements anonymes sont exclus et la valeur **0** indique qu’ils sont inclus.  
  
`[ @refreshpolicy = ] refreshpolicy` À usage interne uniquement.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**statut**|**int**|Examine l'état de tous les agents de réplication associés à la publication, puis retourne l'état le plus élevé dans l'ordre suivant :<br /><br /> **6** = échec<br /><br /> **5** = nouvelle tentative<br /><br /> **2** = arrêté<br /><br /> **4** = inactif<br /><br /> **3** = en cours<br /><br /> **1** = démarré|  
|**warning**|**int**|Avertissement de seuil maximal généré par un abonnement appartenant à la publication, qui peut être le résultat OR logique d'au moins l'une des valeurs suivantes.<br /><br /> **1** = expiration : un abonnement à une publication transactionnelle n’a pas été synchronisé dans le seuil de la période de rétention.<br /><br /> **2** = latence : le temps nécessaire à la réplication des données d’un serveur de publication transactionnel vers l’abonné dépasse le seuil, en secondes.<br /><br /> **4** = mergeexpiration-un abonnement à une publication de fusion n’a pas été synchronisé dans le seuil de la période de rétention.<br /><br /> **8** = mergefastrunduration-le temps nécessaire pour effectuer la synchronisation d’un abonnement de fusion dépasse le seuil, en secondes, sur une connexion réseau rapide.<br /><br /> **16** = mergeslowrunduration-le temps nécessaire pour effectuer la synchronisation d’un abonnement de fusion dépasse le seuil, en secondes, sur une connexion réseau lente ou d’accès à distance.<br /><br /> **32** = mergefastrunspeed-la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas réussi à maintenir le taux de seuil, en lignes par seconde, sur une connexion réseau rapide.<br /><br /> **64** = mergeslowrunspeed-la vitesse de transmission des lignes pendant la synchronisation d’un abonnement de fusion n’a pas réussi à maintenir le taux de seuil, en lignes par seconde, sur une connexion réseau lente ou d’accès à distance.|  
|**côté**|**sysname**|Nom de l'Abonné.|  
|**subscriber_db**|**sysname**|Nom de la base de données utilisée pour l'abonnement.|  
|**publisher_db**|**sysname**|Nom de la base de données de publication.|  
|**édition**|**sysname**|Nom d'une publication.|  
|**publication_type**|**int**|Type de publication, qui peut prendre l’une des valeurs suivantes :<br /><br /> **0** = publication transactionnelle<br /><br /> **1** = publication d’instantané<br /><br /> **2** = publication de fusion|  
|**sous-type**|**int**|Type d'abonnement, qui peut prendre l'une des valeurs suivantes :<br /><br /> **0** = Push<br /><br /> **1** = extraction<br /><br /> **2** = anonyme|  
|**latence**|**int**|Latence maximale, en secondes, des modifications de données propagées par l'Agent de lecture du journal ou l'Agent de distribution pour une publication transactionnelle.|  
|**latencythreshold**|**int**|Latence maximale de la publication transactionnelle au-delà de laquelle un avertissement est déclenché.|  
|**agentnotrunning**|**int**|Durée, en heures, pendant laquelle l'Agent n'a pas été exécuté.|  
|**agentnotrunningthreshold**|**int**|Durée, en heures, pendant laquelle l'Agent n'a pas été exécuté avant le déclenchement d'un avertissement.|  
|**timetoexpiration**|**int**|Durée, en heures, au terme de laquelle l'abonnement expire s'il n'est pas synchronisé.|  
|**expirationthreshold**|**int**|Durée, en heures, au terme de laquelle l'expiration de l'abonnement déclenche un avertissement.|  
|**last_distsync**|**datetime**|Date et heure de la dernière exécution de la Agent de distribution.|  
|**distribution_agentname**|**sysname**|Nom du travail d'Agent de distribution pour l'abonnement à une publication transactionnelle.|  
|**mergeagentname**|**sysname**|Nom du travail d'Agent de fusion pour l'abonnement à une publication de fusion.|  
|**mergesubscriptionfriendlyname**|**sysname**|Nom convivial donné à l'abonnement.|  
|**mergeagentlocation**|**sysname**|Nom du serveur sur lequel l'Agent de fusion est exécuté.|  
|**mergeconnectiontype**|**int**|Connexion utilisée lors de la synchronisation d'un abonnement à une publication de fusion ; ce paramètre peut prendre l'une des valeurs suivantes :<br /><br /> **1** = réseau local (LAN)<br /><br /> **2** = connexion d’accès réseau à distance<br /><br /> **3** = synchronisation Web.|  
|**mergePerformance**|**int**|Performances de la dernière synchronisation comparées à toutes les synchronisations de l'abonnement, calculées en divisant la vitesse de transmission de la dernière synchronisation par la moyenne de toutes les vitesses de transmission antérieures.|  
|**mergerunspeed**|**float**|Vitesse de transmission de la dernière synchronisation de l'abonnement.|  
|**mergerunduration**|**int**|Durée qui a été nécessaire à la dernière synchronisation de l'abonnement.|  
|**monitorranking**|**int**|Valeur de classement des abonnements dans l'ensemble de résultats ; ce paramètre peut prendre l'une des valeurs suivantes :<br /><br /> Dans le cas d'une publication transactionnelle :<br /><br /> **60** = erreur<br /><br /> **56** = AVERTISSEMENT : critique pour les performances<br /><br /> **52** = AVERTISSEMENT : expire bientôt ou expiré<br /><br /> **50** = AVERTISSEMENT : abonnement non initialisé<br /><br /> **40** = nouvelle tentative de la commande qui a échoué<br /><br /> **30** = non exécuté (succès)<br /><br /> **20** = en cours d’exécution (démarré, en cours d’exécution ou inactif)<br /><br /> Dans le cas d'une publication de fusion :<br /><br /> **60** = erreur<br /><br /> **56** = AVERTISSEMENT : critique pour les performances<br /><br /> **54** = AVERTISSEMENT : fusion longue<br /><br /> **52** = AVERTISSEMENT : expire bientôt<br /><br /> **50** = AVERTISSEMENT : abonnement non initialisé<br /><br /> **40** = nouvelle tentative de la commande qui a échoué<br /><br /> **30** = en cours d’exécution (démarré, en cours d’exécution ou inactif)<br /><br /> **20** = non exécuté (succès)|  
|**distributionagentjobid**|**Binary(16**|ID du travail d'Agent de distribution pour les abonnements à une publication transactionnelle.|  
|**mergeagentjobid**|**Binary(16**|ID du travail d'Agent de fusion pour les abonnements à une publication de fusion.|  
|**distributionagentid**|**int**|ID du travail d'Agent de distribution pour l'abonnement.|  
|**distributionagentprofileid**|**int**|ID du profil d'Agent utilisé par l'Agent de distribution.|  
|**mergeagentid**|**int**|ID du travail d'Agent de fusion pour l'abonnement.|  
|**mergeagentprofileid**|**int**|ID du profil d'Agent utilisé par l'Agent de fusion.|  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_replmonitorhelpsubscription** est utilisé avec tous les types de réplications.  
  
 **sp_replmonitorhelpsubscription** commande le jeu de résultats en fonction de la gravité de l’état de l’abonnement, qui est déterminée par la valeur de *monitorranking*. Par exemple, les lignes de tous les abonnements se trouvant dans un état d'erreur précèdent les lignes des abonnements se trouvant dans un état d'avertissement.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle de base de données fixe **db_owner** ou **replmonitor** sur la base de données de distribution peuvent exécuter **sp_replmonitorhelpsubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Surveiller la réplication par programmation](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
