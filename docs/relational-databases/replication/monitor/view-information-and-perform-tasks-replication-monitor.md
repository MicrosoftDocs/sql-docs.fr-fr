---
title: Afficher des informations et effectuer des tâches (Moniteur de réplication)
description: Découvrez comment afficher des informations et effectuer diverses tâches à l’aide du moniteur de réplication dans SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing publication information
- publications [SQL Server replication], viewing information
- publications [SQL Server replication], Replication Monitor tasks
ms.assetid: 92e28a07-d6a7-461b-a0b3-bd9bc6afcbe5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 24eb21c5907e343ec433316fbdceebfc3864cedc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340748"
---
# <a name="view-information-and-perform-tasks-using-replication-monitor"></a>Afficher des informations et effectuer des tâches à l’aide du moniteur de réplication
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
Le moniteur de réplication fournit un certain nombre d’onglets et d’options vous permettant d’afficher des informations et d’effectuer des tâches diverses. Cet article décrit les informations que vous pouvez afficher et les tâches que vous pouvez accomplir à l’aide du moniteur de réplication. 


## <a name="for-a-publication"></a>Pour une publication

### <a name="view-information"></a>Afficher des informations
Le moniteur de réplication comporte plusieurs onglets contenant des informations relatives à la publication sélectionnée :  
  
-   **Tous les abonnements** - Cet onglet affiche des informations sur tous les abonnements à la publication sélectionnée.  
  
-   **Agents** - Cet onglet affiche des informations sur tous les agents utilisés par une publication :   
    -   Agent d'instantané, utilisé par toutes les publications   
    -   Agent de lecture du journal, utilisé par toutes les publications transactionnelles   
    -   Agent de lecture de la file d'attente, utilisé par les publications transactionnelles qui possèdent des abonnements mis à jour en attente  
  
-   **Avertissements** - Cet onglet permet de spécifier des avertissements et des alertes pour les agents.  
  
-   **Jetons de suivi** (réplication transactionnelle uniquement) - Cet onglet permet de mesurer la latence, à savoir le temps écoulé entre la validation d’une transaction sur le serveur de publication et la validation de la transaction correspondante sur l’Abonné.  
  
 Pour plus d’informations sur les options de chaque onglet, sélectionnez l’onglet dans le volet de droite, puis **Aide** dans la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Effectuer des tâches
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis sélectionnez une publication.   
2.  Pour afficher et modifier les propriétés de la publication, cliquez avec le bouton droit sur celle-ci, puis sélectionnez **Propriétés**.    
3.  Pour afficher des informations sur les abonnements, sélectionnez l’onglet **Tous les abonnements**, cliquez avec le bouton droit sur l’abonnement, puis sélectionnez **Propriétés**. Vous pouvez également accéder à des informations plus détaillées et effectuer certaines tâches dans cet onglet. 
4.  Pour afficher des informations sur les agents, sélectionnez l’onglet **Agents**. Vous pouvez également accéder à des informations plus détaillées et effectuer certaines tâches dans cet onglet. 
5.  Pour afficher des informations sur les avertissements d’agent et les seuils, sélectionnez l’onglet **Avertissements**. Pour plus d’informations, voir [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
6.  Pour afficher des informations sur les jetons de suivi, sélectionnez l’onglet **Jetons de suivi**. Pour plus d'informations sur l'utilisation des jetons de suivi, consultez [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md).  
  
## <a name="for-a-publisher"></a>Pour un serveur de publication 

### <a name="view-information"></a>Afficher des informations
Le moniteur de réplication contient les onglets ci-après qui donnent des informations sur le serveur de publication sélectionné :   
-   **Publications** - Donne des informations sur toutes les publications au niveau du serveur de publication sélectionné.   
-   **Liste de suivi des abonnements** - Affiche des informations sur les abonnements de toutes les publications disponibles sur le serveur de publication sélectionné, qui ont des erreurs, des avertissements ou les performances les plus faibles. Cet onglet ne s’affiche pas pour les serveurs de distribution exécutant des versions antérieures à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].    
-   **Agents** - Cet onglet affiche des informations détaillées sur les agents et les travaux utilisés par tous les types de réplication. L'onglet permet également de démarrer et d'arrêter chaque agent ou travail. Pour plus d'informations sur les options proposées dans chaque onglet, cliquez sur l'onglet dans le volet droit, puis sur **Aide** dans la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Effectuer des tâches
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.   
2.  Pour afficher des informations sur toutes les publications, cliquez sur l'onglet **Publications** .    
3.  Pour consulter des informations sur les abonnements, cliquez sur l'onglet **Liste de suivi des abonnements** . Vous pouvez également accéder à des informations plus détaillées et effectuer des tâches à partir de cet onglet :   
    -   Pour afficher des informations détaillées sur l'agent associé à un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**.    
    -   Pour afficher les propriétés d'un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Propriétés**.    
    -   Pour synchroniser un abonnement par envoi de données (push), cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.   
    -   Pour réinitialiser un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Réinitialiser l'abonnement**.   
4.  Pour consulter les informations sur les agents, cliquez sur l'onglet **Agents** . Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :    
    -   Pour afficher des informations détaillées sur un agent (telles que des messages d'informations et des messages d'erreur), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Afficher les détails**.  
    -   Pour afficher des informations détaillées sur le travail qui exécute l'agent (par exemple, des détails sur la planification, les étapes de travail, etc.), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Propriétés**.    
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).    
    -   Pour démarrer un agent qui n'est pas en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Démarrer l'agent**.  
    -   Pour arrêter un agent qui est en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Arrêter l'agent**.  


## <a name="for-a-subscription"></a>Pour un abonnement 

### <a name="view-information"></a>Afficher des informations
  Le Moniteur de réplication propose les onglets suivants, qui comportent des informations sur les abonnements :    
-   **Tous les abonnements** - Affiche des informations sur tous les abonnements à la publication sélectionnée.   
-   **Liste de suivi des abonnements** - Affiche des informations sur les abonnements de toutes les publications disponibles sur le serveur de publication sélectionné, qui ont des erreurs, des avertissements ou les performances les plus faibles. Cet onglet ne s’affiche pas pour les serveurs de distribution exécutant des versions antérieures à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Pour plus d'informations sur les options de chaque onglet, cliquez sur l'onglet dans le volet de droite puis cliquez sur l'option **Aide** dans la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Effectuer des tâches
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.   
2.  Pour afficher des informations sur les abonnements, cliquez sur l'onglet **Tous les abonnements** . Pour n'afficher que les abonnements qui se trouvent dans un état donné, par exemple en cours de synchronisation, sélectionnez une option dans la liste déroulante **Afficher** .    
3.  Pour afficher et modifier les propriétés de l'abonnement, cliquez avec le bouton droit sur celui-ci puis cliquez sur **Propriétés**. Vous pouvez également accéder à des informations plus détaillées et effectuer certaines tâches dans cet onglet. 
  
### <a name="to-view-information-and-perform-tasks-for-subscriptions-in-the-subscription-watch-list-tab"></a>Pour afficher des informations et exécuter des tâches dans l'onglet Liste de suivi des abonnements  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.    
2.  Pour consulter des informations sur les abonnements, cliquez sur l'onglet **Liste de suivi des abonnements** .    
3.  Sélectionnez le type d’abonnement à afficher dans la liste déroulante **Afficher les abonnements\<SubscriptionType>** . Pour n'afficher que les abonnements qui se trouvent dans un état donné, par exemple en cours de synchronisation, sélectionnez une option dans la liste déroulante **Afficher** .    
4.  Pour afficher et modifier les propriétés de l'abonnement, cliquez avec le bouton droit sur celui-ci puis cliquez sur **Propriétés**. Vous pouvez également accéder à des informations plus détaillées et effectuer certaines tâches dans cet onglet. 
  
  
## <a name="for-publication-agents"></a>Pour les Agents de publication
Le moniteur de réplication fournit l'onglet **Agents** qui contient des informations sur les agents associés à la publication sélectionnée. L’Agent de distribution et l’Agent de fusion sont associés à des abonnements. 
  
### <a name="view-information"></a>Afficher des informations
 Cet onglet affiche des informations sur les agents suivants :  
  
-   Agent d'instantané, utilisé par toutes les publications  
-   Agent de lecture du journal, utilisé par toutes les publications transactionnelles  
-   Agent de lecture de la file d'attente, utilisé par les publications transactionnelles qui possèdent des abonnements mis à jour en attente 
  
 Pour afficher plus d'informations sur les options de cet onglet, sélectionnez **Aide** sur la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-publication"></a>Pour afficher des informations et effectuer des tâches pour les agents associés à une publication  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.    
2.  Pour consulter les informations sur les agents, cliquez sur l'onglet **Agents** . Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :    
    -   Pour afficher des informations détaillées sur un agent (telles que des messages d'informations et des messages d'erreur), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Afficher les détails**.  
    -   Pour afficher des informations détaillées sur le travail qui exécute l'agent (par exemple, des détails sur la planification, les étapes de travail, etc.), cliquez avec le bouton droit sur l'agent, puis cliquez sur **Propriétés**.    
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).   
    -   Pour démarrer un agent qui n'est pas en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Démarrer l'agent**.   
    -   Pour arrêter un agent qui est en cours d'exécution, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Arrêter l'agent**.  
  
## <a name="for-subscription-agents"></a>Pour les Agents d’abonnement

### <a name="view-information"></a>Afficher des informations
-   **Tous les abonnements** - Affiche des informations sur tous les abonnements à la publication sélectionnée.  
  
-   **Liste de suivi des abonnements** - Affiche des informations sur les abonnements de toutes les publications disponibles sur le serveur de publication sélectionné, qui ont des erreurs, des avertissements ou les performances les plus faibles. Cet onglet ne s’affiche pas pour les serveurs de distribution exécutant des versions antérieures à [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Pour plus d'informations sur les options de chaque onglet, cliquez sur l'onglet dans le volet de droite puis cliquez sur l'option **Aide** dans la barre de menus. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Effectuer des tâches
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.    
2.  Cliquez sur l'onglet **Tous les abonnements** pour afficher des informations sur les abonnements. Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :   
    -   Pour afficher des informations détaillées sur l'agent associé à un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**. Ces informations détaillées sont les suivantes : messages d'historique et d'erreur de l'agent, statistiques de performances pour la réplication transactionnelle et statistiques de synchronisation au niveau des articles pour la réplication de fusion.  
  
         Les onglets de la fenêtre des détails qui est ouverte dépendent du type d'abonnement : pour les abonnements d'instantané, l'onglet est **Historique du serveur de distribution vers l'Abonné**; pour les abonnements transactionnels, les onglets sont **Historique du serveur de publication vers le serveur de distribution**, **Historique du serveur de distribution vers l'Abonné** et **Commandes non distribuées**; pour les abonnements de fusion, l'onglet est **Historique de synchronisation**.  
  
    -   Pour synchroniser un abonnement par envoi de données (push), cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.    
    -   Pour réinitialiser un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Réinitialiser l'abonnement**.    
    -   Pour valider un seul abonnement de fusion, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Valider l'abonnement**. Pour valider tous les abonnements à une publication de fusion, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valide tous les abonnements**; pour valider tous les abonnements pour une publication transactionnelle, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valider les abonnements**.    
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
### <a name="subscription-watch-list-tab"></a>Onglet Liste de suivi des abonnements 
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, puis développez un serveur de publication.    
2.  Cliquez sur l'onglet **Liste de suivi des abonnements** pour afficher des informations sur les abonnements. Vous pouvez aussi accéder à des informations plus détaillées et effectuer des tâches sur cet onglet :   
    -   Pour afficher des informations détaillées sur l'agent associé à un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Afficher les détails**. Ces informations détaillées sont les suivantes : messages d'historique et d'erreur de l'agent, statistiques de performances pour la réplication transactionnelle et statistiques de synchronisation au niveau des articles pour la réplication de fusion.    
         Les onglets de la fenêtre des détails qui est ouverte dépendent du type d'abonnement : pour les abonnements d'instantané, l'onglet est **Historique du serveur de distribution vers l'Abonné**; pour les abonnements transactionnels, les onglets sont **Historique du serveur de publication vers le serveur de distribution**, **Historique du serveur de distribution vers l'Abonné** et **Performances**; pour les abonnements de fusion, l'onglet est **Historique de synchronisation**.  
  
    -   Pour synchroniser un abonnement par envoi de données (push), cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Démarrer la synchronisation**.    
    -   Pour réinitialiser un abonnement, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Réinitialiser l'abonnement**.    
    -   Pour valider un seul abonnement de fusion, cliquez avec le bouton droit sur l'abonnement, puis cliquez sur **Valider l'abonnement**. Pour valider tous les abonnements à une publication de fusion, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valide tous les abonnements**; pour valider tous les abonnements pour une publication transactionnelle, cliquez avec le bouton droit sur la publication, puis cliquez sur **Valider les abonnements**.    
    -   Pour gérer des profils pour l'agent, cliquez avec le bouton droit sur l'agent, puis cliquez sur **Profils d'agent**. Pour plus d’informations, voir [Utiliser des profils d’Agent de réplication](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  

    


## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par émission de données](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Afficher et modifier les propriétés d’un abonnement par extraction (pull)](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  
 [Définir des seuils et des avertissements dans le moniteur de réplication](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)   
 [Administration de l’Agent de réplication](../../../relational-databases/replication/agents/replication-agent-administration.md)    
  
  
