---
title: Abonnés de réplication et groupes de disponibilité (SQL Server)
description: Découvrez les conséquences de l’échec d’un groupe de disponibilité Always On contenant une base de données correspondant à un abonné de réplication dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/08/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- failover subscribers with AlwaysOn
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 0995f269-0580-43ed-b8bf-02b9ad2d7ee6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 310438f8a5f8bec6fa0b2b3ef5b17eb1b097f423
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344596"
---
# <a name="replication-subscribers-and-always-on-availability-groups-sql-server"></a>Abonnés de réplication et groupes de disponibilité Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Quand un groupe de disponibilité Always On contenant une base de données est un abonné de réplication et bascule, l’abonnement de réplication peut échouer. Pour les abonnés de réplication par émission transactionnelle, l’agent de distribution continue la réplication automatiquement après un basculement si l’abonnement a été créé avec le nom de l’écouteur de groupe de disponibilité. Pour les abonnés de réplication par réception transactionnelle, l’agent de distribution continue la réplication automatiquement après un basculement si l’abonnement a été créé avec le nom de l’écouteur de groupe de disponibilité et que le serveur de l’Abonné d’origine fonctionne. C’est parce que les travaux de l’agent de distribution sont uniquement créés sur l’Abonné d’origine (réplica principal du groupe de disponibilité). Pour les abonnés de fusion, un administrateur de réplication doit reconfigurer l'abonné manuellement, en recréant l'abonnement.  
  
## <a name="what-is-supported"></a>Ce qui est pris en charge  
 La réplication de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prend en charge le basculement automatique du serveur de publication et le basculement automatique des abonnés. Les abonnés de fusion peuvent faire partie d’un groupe de disponibilité ; toutefois, des actions manuelles sont requises pour configurer le nouvel abonné après un basculement. Les groupes de disponibilité ne peuvent pas être associés à des scénarios WebSync et SQL Server Compact.  
  
## <a name="how-to-create-transactional-subscription-in-an-always-on-environment"></a>Procédure de création d’un abonnement transactionnel dans un environnement Always On  
 Pour la réplication transactionnelle, utilisez la procédure suivante pour configurer et basculer un groupe de disponibilité d'abonné :  
  
1.  Avant de créer l’abonnement, ajoutez la base de données de l’abonné au groupe de disponibilité Always On approprié.  
  
2.  Ajoutez l'écouteur de groupe de disponibilité de l'abonné en tant que serveur lié à tous les nœuds du groupe de disponibilité. Cette étape garantit que tous les partenaires de basculement potentiels ont connaissance de l'écouteur et peuvent s'y connecter.  
  
3.  À l'aide du script de la section **Création d'un abonnement de réplication transactionnelle par émission de données** ci-dessous, créez l'abonnement avec le nom de l'écouteur du groupe de disponibilité de l'abonné. Après un basculement, le nom de l'écouteur demeure valide, alors que le nom du serveur réel de l'abonné dépend du nœud réel qui est devenu le nouveau nœud principal.  
  
    > [!NOTE]  
    >  L'abonnement doit être créé à l'aide d'un script [!INCLUDE[tsql](../../../includes/tsql-md.md)] ; il ne peut pas être créé à l'aide de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  Pour créer un abonnement par extraction  
  
    1.  À l’aide de l’exemple de script de la section **Création d’un abonnement de réplication transactionnelle par extraction** ci-dessous, créez l’abonnement avec le nom de l’écouteur du groupe de disponibilité de l’abonné. 
   
    2.  Après un basculement, créez le travail de l’agent de distribution sur le nouveau réplica principal à l’aide de la procédure stockée **sp_addpullsubscription_agent**. 
  
 Quand vous créez un abonnement par extraction, avec la base de données d’abonnement dans un groupe de disponibilité, après chaque basculement il est recommandé de désactiver le travail de l’agent de distribution sur l’ancien réplica principal et d’activer le travail sur le nouveau réplica principal.  
  
## <a name="creating-a-transactional-replication-push-subscription"></a>Création d'un abonnement de réplication transactionnelle par émission de données  
  
```  
-- commands to execute at the publisher, in the publisher database:  
use [<publisher database name>]  
EXEC sp_addsubscription @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @destination_db = N'<subscriber database name>',   
       @subscription_type = N'Push',   
       @sync_type = N'automatic', @article = N'all', @update_mode = N'read only', @subscriber_type = 0;  
GO  
  
EXEC sp_addpushsubscription_agent @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @subscriber_db = N'<subscriber database name>',   
       @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO  
```  

## <a name="creating-a-transactional-replication-pull-subscription"></a>Création d’un abonnement de réplication transactionnelle par extraction  
  
```  
-- commands to execute at the subscriber, in the subscriber database:  
use [<subscriber database name>]  
EXEC sp_addpullsubscription @publisher= N'<publisher name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>',
        @subscription_type = N'pull';
Go

EXEC sp_addpullsubscription_agent 
        @publisher =  N'<publisher name>', 
        @subscriber = N'<availability group listener name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>' ;
        @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO
```  
  
## <a name="to-resume-the-merge-agents-after-the-availability-group-of-the-subscriber-fails-over"></a>Pour récupérer les agents de fusion après le basculement du groupe de disponibilité de l'abonné  
 Pour une réplication de fusion, un administrateur de réplication doit reconfigurer l'abonné manuellement en procédant comme suit :  
  
1.  Exécutez **sp_subscription_cleanup** pour supprimer l’ancien abonnement pour l’abonné. Effectuez cette action sur le nouveau réplica principal (qui était auparavant le réplica secondaire).  
  
2.  Recréez l'abonnement en en créant un nouveau, en commençant par un nouvel instantané.  
  
> [!NOTE]  
>  Le processus actuel n’est pas pratique pour les abonnés de réplication de fusion ; toutefois, le scénario principal de la réplication de fusion implique des utilisateurs déconnectés (bureaux, ordinateurs portables, appareils combinés téléphoniques) qui n’utilisent pas les groupes de disponibilité Always On sur l’abonné.  
  
  
