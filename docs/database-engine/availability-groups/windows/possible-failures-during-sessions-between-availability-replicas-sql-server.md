---
title: Causes possibles des problèmes de connectivité entre des réplicas de disponibilité
description: Cette rubrique décrit les différentes causes possibles d’un échec de connexion entre des réplicas membres d’un groupe de disponibilité Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- troubleshooting [SQL Server], HADR
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], troubleshooting
ms.assetid: cd613898-82d9-482f-a255-0230a6c7d6fe
author: cawrites
ms.author: chadam
ms.openlocfilehash: e96836a85fb75cc6436530356e8f38ea1a1e1882
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642539"
---
# <a name="determine-possible-reason-for-connectivity-failures-between-availability-replicas"></a>Déterminer la cause possible des problèmes de connectivité entre des réplicas de disponibilité
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Des problèmes physiques, de système d'exploitation ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] peuvent provoquer un échec dans une session entre deux réplicas de disponibilité. Un réplica de disponibilité ne contrôle pas régulièrement les composants sur lesquels Sqlservr.exe s'appuie pour vérifier s'ils fonctionnent correctement ou s'ils ont échoué. Toutefois, pour certains types d'échecs, le composant affecté signale une erreur à Sqlservr.exe. Une erreur signalée par un autre composant est appelée *erreur matérielle*. Pour détecter les autres erreurs qui passeraient sinon inaperçues, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] implémente son propre mécanisme de délai d'expiration de session. Spécifie le délai d'expiration de la session en secondes. Ce délai d'attente est la durée maximale pendant laquelle une instance de serveur attend de recevoir un message PING d'une autre instance avant de considérer que cette dernière est déconnectée. Quand un délai d’expiration de session se produit entre deux réplicas de disponibilité, les réplicas de disponibilité partent du principe qu’une erreur s’est produite et déclarent une *erreur logicielle*.  
  
> [!IMPORTANT]  
>  Les défaillances dans les bases de données autres que la base de données primaire ne sont pas détectables. En outre, il est peu vraisemblable qu'une défaillance du disque de données soit détectée, sauf si la base de données est redémarrée en raison de la défaillance d'un disque de données.  
  
 Le temps de détection de l'erreur et, par extension, le temps de réaction face à une erreur, varient selon que l'erreur est matérielle ou logicielle. Certaines erreurs matérielles, telles que les pannes de réseau sont signalées immédiatement. Cependant, il arrive parfois que des délais d'attente spécifiques à un composant retardent la signalisation de certaines erreurs matérielles. Pour les erreurs logicielles, la durée du délai d'expiration de session détermine la vitesse de détection de l'erreur. Par défaut, cette valeur est de 10 secondes. Il s'agit de la valeur minimale recommandée.  
  
## <a name="failures-due-to-hard-errors"></a>Défaillances dues à des erreurs matérielles  
 Parmi les causes possibles d'erreurs matérielles figurent notamment, sans s'y limiter, les situations suivantes :  
  
-   une connexion ou un câble coupé ;  
  
-   une carte réseau défaillante ;  
  
-   un changement de routeur ;  
  
-   des modifications au pare-feu ;  
  
-   une reconfiguration du point de terminaison ;  
  
-   la perte du lecteur où se trouve le journal de transactions ;  
  
-   une défaillance du système d'exploitation ou d'un processus.  
  
 Par exemple, lorsque le lecteur de journalisation de la base de données primaire cesse de répondre et échoue, le système d'exploitation informe Sqlservr.exe qu'une erreur grave s'est produite.  
  
 Certains composants, tels que les composants réseau et certains sous-systèmes d'E/S, ont leurs propres délais d'attente pour détecter les défaillances. Ces délais sont indépendants de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], qui n'en a pas connaissance et n'est pas tenu au courant de leur comportement. Dans ces cas, le délai d'attente augmente la durée entre une défaillance et la réception de son erreur matérielle par le réplica de disponibilité.  
  
> [!NOTE]  
>  C'est uniquement dans le cas des erreurs logicielles que la détection des erreurs appliquée aux réplicas de disponibilité est active. Pour plus d'informations, consultez « Défaillances dues à des erreurs logicielles » plus loin dans cette rubrique.  
  
 Pour vous aider à interpréter les conditions d'erreur qui se produisent sur le réseau, demandez à un ingénieur réseau quels messages d'erreur sont envoyés à un port lorsque les événements ci-dessous se produisent sur une connexion TCP :  
  
-   le DNS ne fonctionne pas ;  
  
-   les câbles sont débranchés ;  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows comprend un pare-feu qui bloque un port spécifique ;  
  
-   l'application de surveillance d'un port ne fonctionne pas ;  
  
-   un serveur Windows a été renommé ;  
  
-   un serveur Windows a été redémarré ;  
  
> [!NOTE]  
>  [!INCLUDE[ssHADRc](../../../includes/sshadrc-md.md)] ne protège pas des problèmes spécifiques au client qui accède aux serveurs. Prenons l'exemple d'un cas dans lequel une carte réseau publique gère des connexions clientes au réplica principal, tandis qu'une carte d'interface réseau privée gère le trafic entre les instances de serveur qui hébergent les réplicas d'un groupe de disponibilité. Dans ce cas, l'échec de la carte réseau publique empêcherait les clients d'accéder aux bases de données.  
  
## <a name="failures-due-to-soft-errors"></a>Défaillances dues à des erreurs logicielles  
 Parmi les conditions susceptibles de provoquer des délais d'expiration de session figurent notamment, sans s'y limiter, les situations suivantes :  
  
-   des erreurs réseau, par exemple, un dépassement de délai de la liaison TCP, des paquets supprimés ou endommagés, des paquets dans le désordre ;  
  
-   un système d’exploitation, un serveur ou une base de données qui ne répond pas ;  
  
-   un serveur Windows en cours de dépassement d'un délai d'expiration ;  
  
-   des ressources informatiques insuffisantes, telles qu'une surcharge de l'unité centrale ou du disque, la taille limite atteinte par le journal de transactions ou l'absence de mémoire ou de threads disponibles. Dans de telles situations, vous devez augmenter le délai d'attente, réduire la charge de travail ou remplacer les matériels pour qu'ils supportent la charge de travail.  
  
### <a name="the-session-timeout-mechanism"></a>Mécanisme du délai d'expiration de session  
 Étant donné que les erreurs logicielles ne sont pas détectables directement par une instance de serveur, lorsqu'elles surviennent, un réplica de disponibilité peut attendre indéfiniment une réponse de l'autre réplica de disponibilité dans une session. Pour éviter cela, [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] implémente son propre mécanisme de délai d'expiration de session, selon lequel les réplicas de disponibilités connectés envoient une commande ping sur chaque connexion ouverte à un intervalle fixe. La réception d'un ping au cours du délai d'attente indique que la connexion est toujours ouverte et que les instances de serveur communiquent via celle-ci. À la réception du ping, un réplica réinitialise son compteur de délai d'expiration sur cette connexion. Pour plus d’informations sur la relation entre le mode de disponibilité et les délais d’expiration de session, consultez [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 Les réplicas primaire et secondaire s'envoient réciproquement une commande ping pour signaler qu'ils sont toujours actifs, et la limite définie par le délai d'expiration de session empêche que l'un ou l'autre réplica attende indéfiniment de recevoir une commande ping de l'autre réplica. La limite définie par le délai d'expiration de session est une propriété de réplica configurable par l'utilisateur dont la valeur par défaut est de 10 secondes. La réception d'un ping au cours du délai d'attente indique que la connexion est toujours ouverte et que les instances de serveur communiquent via celle-ci. À la réception d'un ping, un réplica de disponibilité réinitialise son compteur de délai d'expiration de session sur cette connexion.  
  
 Si aucun ping n'est reçu de l'autre réplica au cours de la période d'expiration de session, la connexion expire. La connexion est fermée, et le réplica expiré passe à l'état DISCONNECTED. Même si un réplica déconnecté est configuré pour le mode de validation synchrone, les transactions n'attendront pas que ce réplica se reconnecte et se resynchronise.  
  
## <a name="responding-to-an-error"></a>Réponse à une erreur  
 Quel que soit le type d'erreur, une instance de serveur qui détecte une erreur réagit comme il se doit en fonction du rôle de l'instance, du mode de disponibilité de la session et de l'état des autres connexions de la session. Pour plus d’informations sur les conséquences de la perte d’un partenaire, consultez [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
## <a name="related-tasks"></a>Tâches associées  
 **Pour modifier la valeur du délai d'expiration (mode de disponibilité avec validation synchrone uniquement)**  
  
-   [Modifier le délai d’expiration de session pour un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Pour afficher la valeur actuelle du délai d'attente**  
  
-   Interroger **session_timeout** dans [sys.availability_replicas &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
