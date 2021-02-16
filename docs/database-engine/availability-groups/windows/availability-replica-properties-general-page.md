---
title: Page Général (Propriétés du réplica de disponibilité)
description: Description des différentes propriétés disponibles dans la page « Général » de la page « Propriétés du réplica de disponibilité » dans SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: end-user-help
f1_keywords:
- sql13.swb.availabilityreplicaproperties.general.f1
ms.assetid: 8318fefb-e045-4fab-8507-e1951fc7cec6
author: cawrites
ms.author: chadam
ms.openlocfilehash: e81251f9d5cb1f87f3b823b9fc23bcb69c26e79e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343833"
---
# <a name="availability-replica-properties-general-page-for-always-on-availability-groups"></a>Propriétés du réplica de disponibilité (page Général) pour les groupes de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Utilisez cette boîte de dialogue pour afficher les propriétés d’un réplica de disponibilité.  
  
## <a name="task-list"></a>Liste des tâches  
 **Pour afficher les propriétés d'un réplica de disponibilité**  
  
-   [Afficher les propriétés d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-replica-properties-sql-server.md)  
  
-   [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur  
 **Nom du groupe de disponibilité**  
 Nom du groupe de disponibilité. Il s'agit d'un nom spécifié par l'utilisateur qui doit être unique dans le cluster de basculement Windows Server (WSFC).  
  
 **Instance de serveur**  
 Nom de serveur de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge ce réplica et, pour une instance non définie par défaut, son nom d'instance.  
  
 **Rôle**  
 **Primaire**  
 Actuellement le réplica principal.  
  
 **Secondaire**  
 Actuellement un réplica secondaire.  
  
 **Résolution**  
 Actuellement le rôle de réplica est en cours d'être résolu en rôle principal ou secondaire.  
  
 **Mode de disponibilité**  
 Mode de disponibilité du réplica, parmi :  
  
 **Validation asynchrone**  
 Le réplica principal peut valider des transactions sans attendre que le réplica secondaire écrive le journal sur le disque.  
  
 **Validation synchrone**  
 Le réplica principal attend pour valider une transaction donnée que le réplica secondaire ait écrit la transaction sur le disque.  
  
 Pour plus d’informations, consultez [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 **Failover mode**  
 Mode de basculement du réplica, parmi :  
  
 **Automatique**  
 Basculement automatique. Le réplica est une cible pour les basculements automatiques. Pris en charge uniquement si le mode de disponibilité est défini en mode de validation synchrone.  
  
 **Manuel**  
 Basculement manuel. Le réplica peut uniquement être basculé manuellement par l'administrateur de base de données.  
  
 **Connexions en rôle principal**  
 Type de connexions clientes pris en charge lorsque le réplica joue le rôle principal.  
  
 **Autoriser toutes les connexions**  
 Toutes les connexions aux bases de données sont autorisées dans le réplica principal. Il s'agit du paramètre par défaut.  
  
 **Autoriser les connexions en lecture/écriture**  
 Les connexions où la propriété de connexion d’intention de l’application a la valeur **ReadOnly** ne sont pas autorisées. Lorsque la propriété d’intention d’application a la valeur **ReadWrite** ou si la propriété de connexion d’intention d’application n’est pas définie, la connexion est autorisée.  
  
 **Lisible secondaire**  
 Si un réplica de disponibilité qui revêt le rôle secondaire (autrement dit, un réplica secondaire) peut accepter les connexions des clients, les options suivantes sont disponibles :  
  
 **Non**  
 Aucune connexion directe n'est autorisée aux bases de données secondaires de ce réplica. Elles ne sont pas disponibles pour l'accès en lecture. Il s'agit du paramètre par défaut.  
  
 **Tentative de lecture uniquement**  
 Seules les connexions en lecture seule directes sont autorisées aux bases de données secondaires de ce réplica. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
 **Oui**  
 Toutes les connexions sont autorisées aux bases de données secondaires de ce réplica, mais uniquement pour l'accès en lecture. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
 Pour plus d’informations, consultez [Secondaires actifs : Réplicas secondaires accessibles en lecture &#40;groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
 **Délai d'attente de session (secondes)**  
 Délai d'attente en secondes. Le délai d’expiration est le temps maximum pendant lequel le réplica attend de recevoir un message d’un autre réplica avant de considérer que la connexion entre les réplicas principal et secondaire a échoué. Le délai d'expiration de session détecte si les réplicas secondaries sont connectés au réplica primaire. En cas de détection d’un échec de connexion avec un réplica secondaire, le réplica principal considère que le réplica secondaire présente l’état NOT_SYNCHRONIZED. En cas de détection d'un échec de connexion avec le réplica principal, un réplica secondaire tente simplement de se reconnecter.  
  
> [!NOTE]  
>  Les délais d'expiration de session ne provoquent pas de basculements automatiques.  
  
 **URL de point de terminaison**  
 Représentation de chaîne du point de terminaison de mise en miroir de bases de données spécifié par l'utilisateur, qui est utilisé par les connexions entre les réplicas principal et secondaire pour la synchronisation des données. Pour plus d’informations sur la syntaxe des URL de point de terminaison, consultez [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
