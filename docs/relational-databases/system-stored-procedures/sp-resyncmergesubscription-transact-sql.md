---
description: sp_resyncmergesubscription (Transact-SQL)
title: sp_resyncmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_resyncmergesubscription_TSQL
- sp_resyncmergesubscription
helpviewer_keywords:
- sp_resyncmergesubscription
ms.assetid: e04d464a-60ab-4b39-a710-c066025708e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 530c93cbb99db63c7ced6e454ea78b4f6a0f1ebf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183087"
---
# <a name="sp_resyncmergesubscription-transact-sql"></a>sp_resyncmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Resynchronise un abonnement de fusion avec un état de validation connu que vous spécifiez. Ceci permet de forcer la convergence ou de synchroniser la base de données d'abonnement par rapport à un point dans le temps, tel que la dernière validation qui a abouti, ou à une date spécifiée. L'instantané n'est pas réappliqué lors de la resynchronisation d'un abonnement à l'aide de cette méthode. Cette procédure stockée n'est pas utilisée pour les abonnements de réplication d'instantané ou de réplication transactionnelle. Cette procédure stockée est exécutée sur la base de données d’abonnement sur la base de données de publication, sur la base de données de publication ou sur l’abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_resyncmergesubscription [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
        , [ @publication = ] 'publication'   
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @resync_type = ] resync_type ]  
    [ , [ @resync_date_str = ] resync_date_string ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, avec NULL comme valeur par défaut. La valeur NULL est valide si la procédure stockée est exécutée sur le serveur de publication. Si la procédure stockée est exécutée sur l'Abonné, un serveur de publication doit être spécifié.  
  
`[ @publisher_db = ] 'publisher_db'` Nom de la base de données de publication. *publisher_db* est de **type sysname**, avec NULL comme valeur par défaut. La valeur NULL est valide si la procédure stockée est exécutée dans la base de données de publication du serveur de publication. Si la procédure stockée est exécutée sur l'Abonné, un serveur de publication doit être spécifié.  
  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @subscriber = ] 'subscriber'` Nom de l’abonné. *Subscriber* est de **type sysname**, avec NULL comme valeur par défaut. La valeur NULL est valide si la procédure stockée est exécutée sur l'Abonné. Si la procédure stockée est exécutée sur le serveur de publication, un Abonné doit être spécifié.  
  
`[ @subscriber_db = ] 'subscriber_db'` Nom de la base de données d’abonnement. *subscription_db* est de **type sysname**, avec NULL comme valeur par défaut. La valeur NULL est valide si la procédure stockée est exécutée dans la base de données d'abonnement de l'Abonné. Si la procédure stockée est exécutée sur le serveur de publication, un Abonné doit être spécifié.  
  
`[ @resync_type = ] resync_type` Définit à quel moment la resynchronisation doit commencer. *resync_type* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|La synchronisation démarre après l'instantané initial. Il s'agit de l'option qui utilise le plus de ressources, dans la mesure où toutes les modifications depuis l'instantané initial sont réappliquées à l'Abonné.|  
|**1**|La synchronisation démarre à partir de la dernière validation réussie. Toutes les générations nouvelles ou incomplètes effectuées depuis la dernière validation réussie sont réappliquées à l'Abonné.|  
|**2**|La synchronisation démarre à partir de la date indiquée dans *resync_date_str*. Toutes les générations nouvelles ou incomplètes effectuées depuis la date sont réappliquées à l'Abonné.|  
  
`[ @resync_date_str = ] resync_date_string` Définit la date de début de la resynchronisation. *resync_date_string* est de type **nvarchar (30)**, avec NULL comme valeur par défaut. Ce paramètre est utilisé lorsque le *resync_type* est une valeur de **2**. La date donnée est convertie en sa valeur **DateTime** équivalente.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_resyncmergesubscription** est utilisé dans la réplication de fusion.  
  
 La valeur **0** pour le paramètre *resync_type* , qui réapplique toutes les modifications depuis l’instantané initial, peut nécessiter de nombreuses ressources, mais peut-être beaucoup moins qu’une réinitialisation complète. Par exemple, si l'instantané initial a été fourni un mois auparavant, cette valeur provoque la réapplication des données du mois écoulé. Si l'instantané contient 1 gigaoctet (Go) de données et que le nombre de modifications depuis le mois dernier correspond à 2 mégaoctets (Mo) de données, il est plus efficace de réappliquer les données que de réappliquer l'ensemble de l'instantané de 1 Go.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_resyncmergesubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
