---
title: Défaillances possibles pendant la mise en miroir d’une base de données | Microsoft Docs
description: En savoir plus sur les défaillances dans les sessions de mise en miroir de bases de données provoquées par des problèmes physiques, de système d’exploitation ou de SQL Server, ainsi que sur la façon de répondre aux erreurs.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- time-out period [SQL Server database mirroring]
- soft errors [SQL Server]
- database mirroring [SQL Server], troubleshooting
- timeout errors [SQL Server]
- troubleshooting [SQL Server], database mirroring
- hard errors
- failed database mirroring sessions [SQL Server]
ms.assetid: d7031f58-5f49-4e6d-9a62-9b420f2bb17e
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2cd8b70c401a0cfc3b12f76d289b395f3d9874ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343741"
---
# <a name="possible-failures-during-database-mirroring"></a>Défaillances possibles pendant la mise en miroir d’une base de données
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Des problèmes physiques, de système d'exploitation ou propres à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être responsables de l'échec d'une session de mise en miroir de bases de données. La mise en miroir de bases de données ne contrôle pas régulièrement les composants sur lesquels Sqlservr.exe s'appuie pour vérifier s'ils fonctionnent correctement ou s'ils ont échoué. Toutefois, pour certains types d'échecs, le composant affecté signale une erreur à Sqlservr.exe. Une erreur signalée par un autre composant est appelée *erreur matérielle*. La mise en miroir de bases de données implémente ses propres mécanismes de délai d'attente pour détecter les autres erreurs qui passeraient sinon inaperçues. En cas de délai d’attente de la mise en miroir, une *erreur logicielle* se produit quand la mise en miroir de bases de données détermine qu’une défaillance s’est produite. Toutefois, certaines erreurs qui se produisent au niveau de l'instance SQL Server n'entraîne pas de délai d'attente de la mise en miroir et peuvent ne pas être détectées.  
  
> [!IMPORTANT]  
>  Les défaillances dans les bases de données autres que les bases de données mises en miroir ne sont pas détectables dans une session de mise en miroir de bases de données. En outre, il est peu vraisemblable qu'une défaillance du disque de données soit détectée, sauf si la base de données est redémarrée en raison de la défaillance d'un disque de données.  
  
 Le temps de détection de l'erreur et, par extension, le temps de réaction de la session de mise en miroir face à l'erreur varient selon que l'erreur est matérielle ou logicielle. Certaines erreurs matérielles, telles que les pannes de réseau sont signalées immédiatement. Cependant, il arrive parfois que des délais d'attente spécifiques à un composant retardent la signalisation de certaines erreurs matérielles. Pour les erreurs logicielles, la durée du délai d'attente de la mise en miroir détermine le temps de la détection d'erreur. Par défaut, cette valeur est de 10 secondes. Il s'agit de la valeur minimale recommandée.  
  
## <a name="failures-due-to-hard-errors"></a>Défaillances dues à des erreurs matérielles  
 Parmi les causes possibles d'erreurs matérielles figurent notamment, sans s'y limiter, les situations suivantes :  
  
-   une connexion ou un câble coupé ;  
  
-   une carte réseau défaillante ;  
  
-   un changement de routeur ;  
  
-   des modifications au pare-feu ;  
  
-   une reconfiguration du point de terminaison ;  
  
-   la perte du lecteur où se trouve le journal de transactions ;  
  
-   une défaillance du système d'exploitation ou d'un processus.  
  
 Par exemple, lorsque le lecteur de journalisation de la base de données principale cesse de répondre et échoue, le système d'exploitation informe Sqlservr.exe qu'une erreur grave s'est produite.  
  
 Certains composants, tels que les composants réseau et certains sous-systèmes d'E/S, ont leurs propres délais d'attente pour détecter les défaillances. Ces délais sont indépendants de la mise en miroir de bases de données qui n'en a pas connaissance et n'est pas tenue au courant de leur comportement. Dans ces cas, la période de délai d'attente augmente la durée entre une défaillance et la réception de son erreur matérielle par la mise en miroir de la base de données.  
  
> [!NOTE]  
>  C'est uniquement dans le cas des erreurs logicielles que la détection des erreurs appliquée à la mise en miroir des bases de données est active. Pour plus d'informations, consultez « Défaillances dues à des erreurs logicielles » plus loin dans cette rubrique.  
  
 Pour vous aider à interpréter les conditions d'erreur qui se produisent sur le réseau, demandez à un ingénieur réseau quels messages d'erreur sont envoyés à un port lorsque les événements ci-dessous se produisent sur une connexion TCP :  
  
-   le DNS ne fonctionne pas ;  
  
-   les câbles sont débranchés ;  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows comprend un pare-feu qui bloque un port spécifique ;  
  
-   l'application de surveillance d'un port ne fonctionne pas ;  
  
-   un serveur Windows a été renommé ;  
  
-   un serveur Windows a été redémarré ;  
  
> [!NOTE]  
>  La mise en miroir ne protège pas des problèmes spécifiques au client qui accède aux serveurs. Par exemple, considérez un cas où une carte réseau publique traite les connexions clientes vers l'instance de serveur principal, tandis qu'une carte d'interface réseau privée traite l'ensemble du trafic de mise en miroir entre les instances de serveur. Dans ce cas, la défaillance de la carte réseau publique empêche les clients d'accéder à la base de données, même si la base de données continue d'être en miroir.  
  
## <a name="failures-due-to-soft-errors"></a>Défaillances dues à des erreurs logicielles  
 Parmi les conditions susceptibles de provoquer des délais d'attente de mise en miroir figurent notamment, sans s'y limiter, les situations suivantes :  
  
-   des erreurs réseau, par exemple, un dépassement de délai de la liaison TCP, des paquets supprimés ou endommagés, des paquets dans le désordre ;  
  
-   un système d’exploitation, un serveur ou une base de données qui ne répond pas ;  
  
-   un serveur Windows en cours de dépassement d'un délai d'expiration ;  
  
-   des ressources informatiques insuffisantes, telles qu'une surcharge de l'unité centrale ou du disque, la taille limite atteinte par le journal de transactions ou l'absence de mémoire ou de threads disponibles. Dans de telles situations, vous devez augmenter le délai d'attente, réduire la charge de travail ou remplacer les matériels pour qu'ils supportent la charge de travail.  
  
### <a name="the-mirroring-time-out-mechanism"></a>Mécanisme du délai d'attente de la mise en miroir  
 Comme les erreurs logicielles ne sont pas directement détectables par une instance de serveur, une erreur logicielle peut entraîner le délai d'attente indéfini d'une instance de serveur. Pour empêcher cette situation, la mise en miroir de bases de données met en œuvre son propre mécanisme de délai d'attente basé sur l'envoi d'un ping par chaque instance de serveur d'une session de mise en miroir sur chaque connexion ouverte, à intervalles fixes.  
  
 Pour garder une connexion ouverte, une instance de serveur doit recevoir un ping sur cette connexion au cours du délai d'attente défini, auquel s'ajoute le temps nécessaire à l'envoi d'un ping supplémentaire. La réception d'un ping au cours du délai d'attente indique que la connexion est toujours ouverte et que les instances de serveur communiquent via celle-ci. À la réception du ping, une instance de serveur réinitialise son compteur de délai d'attente sur cette connexion.  
  
 Si aucun ping n'est reçu sur une connexion durant le délai d'attente, une instance de serveur considère que la connexion a expiré. L'instance de serveur ferme la connexion ayant expiré et traite l'événement d'expiration en fonction de l'état et du mode de fonctionnement de la session.  
  
 Même si l'autre serveur fonctionne en réalité correctement, un dépassement de délai est considéré comme une défaillance. Si la valeur du délai d'attente est trop courte pour la réactivité normale d'un partenaire, des fausses défaillances peuvent se produire. Une fausse défaillance se produit lorsqu'une instance de serveur réussit à contacter une autre instance de serveur dont le temps de réponse est tellement lent que ses pings ne sont pas reçus avant l'expiration du délai d'attente.  
  
 Dans des sessions en mode hautes performances, le délai d'attente est toujours de 10 secondes. Ce délai est suffisant le plus souvent pour éviter de fausses défaillances. Dans les sessions en mode haute sécurité, le délai d'attente par défaut est de 10 secondes, mais vous pouvez modifier la durée. Pour éviter des fausses défaillances, la durée recommandée du délai d'attente de la mise en miroir est 10 secondes ou plus.  
  
 **Pour modifier la valeur du délai d'attente (en mode haute sécurité uniquement).**  
  
-   Utilisez l’instruction [ALTER DATABASE \<database> SET PARTNER TIMEOUT \<integer>](../../t-sql/statements/alter-database-transact-sql.md).  
  
 **Pour afficher la valeur actuelle du délai d'attente**  
  
-   Exécutez une requête sur **mirroring_connection_timeout** dans [sys.database_mirroring](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
## <a name="responding-to-an-error"></a>Réponse à une erreur  
 Quel que soit le type d'erreur, une instance de serveur qui détecte une erreur réagit comme il le doit en fonction du rôle de l'instance, du mode d'opération de la session et de l'état des autres connexions de la session. Pour plus d’informations sur les conséquences de la perte d’un partenaire, consultez [Modes de fonctionnement de la mise en miroir de bases de données](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Estimer l’interruption de service au cours d’un basculement de rôle &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/estimate-the-interruption-of-service-during-role-switching-database-mirroring.md)   
 [Modes de fonctionnement de la mise en miroir de bases de données](../../database-engine/database-mirroring/database-mirroring-operating-modes.md)   
 [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
