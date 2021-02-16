---
description: Récupération de base de données accélérée
title: Récupération de base de données accélérée | Microsoft Docs
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- accelerated database recovery [SQL Server], recovery-only
- database recovery [SQL Server]
author: mashamsft
ms.author: mathoma
ms.reviewer: kfarlee
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 70b68fa62c523a4c65bb4e70538e1882bc67290d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350069"
---
# <a name="accelerated-database-recovery"></a>Récupération de base de données accélérée

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

La récupération de base de données accélérée améliore la disponibilité des bases de données, notamment en présence de transactions longues, grâce à une reconception du processus de récupération du moteur de base de données SQL. La récupération de base de données accélérée est désormais disponible dans SQL Server 2019. 

La récupération de base de données accélérée est également disponible pour les bases de données dans Azure SQL Database, Azure SQL Managed Instance et Azure Synapse SQL. La récupération de base de données accélérée est activée par défaut dans SQL Database et SQL Managed Instance et ne peut pas être désactivée. 

## <a name="overview"></a>Vue d’ensemble

Les principaux avantages de la récupération de base de données accélérée sont les suivants :

- **Récupération de base de données rapide et cohérente**

  Avec la récupération de base de données accélérée, les transactions longues n’ont pas d’impact sur le temps de récupération global, ce qui permet une récupération rapide et cohérente des bases de données, quel que soit le nombre de transactions actives dans le système ou la taille de ces transactions.

- **Annulation instantanée des transactions**

  Avec la récupération de base de données accélérée, l’annulation des transactions est instantanée, quelle que soit la durée d’activité de la transaction ou le nombre de mises à jour effectuées.

- **Troncation agressive du journal**

  Avec la récupération de base de données accélérée, le journal des transactions est tronqué de façon agressive, même en présence de transactions longues, ce qui l’empêche de croître hors de contrôle.

## <a name="the-current-database-recovery-process"></a>Le processus de récupération de base de données actuel

Sans la récupération de base de données accélérée, la récupération de base de données dans SQL Server suit le modèle de récupération [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) et se compose de trois phases, qui sont illustrées dans le diagramme suivant et expliquées plus en détail après celui-ci.

![processus de récupération actuel](./media/accelerated-database-recovery-concepts/current-recovery-process.png)

- **Phase d’analyse**

  SQL Server effectue une analyse vers l’avant du journal des transactions à partir du début du dernier point de contrôle réussi (ou du LSN de la page de modifications la plus ancienne) jusqu’à la fin, pour déterminer l’état de chaque transaction au moment de l’arrêt de SQL Server.

- **Phase de restauration par progression**

  SQL Server effectue une analyse vers l’avant du journal des transactions, de la transaction non validée la plus ancienne jusqu’à la fin, pour ramener la base de données à l’état où elle se trouvait au moment du plantage en réeffectuant toutes les opérations validées.

- **Phase d’annulation**

  Pour chaque transaction qui était active au moment du plantage, SQL Server parcourt le journal vers l’arrière, annulant les opérations effectuées par cette transaction.

En vertu de cette conception, le temps nécessaire au moteur de base de données pour effectuer une récupération à partir d’un redémarrage inattendu est (à peu près) proportionnel à la taille de la transaction active la plus longue dans le système au moment du plantage. La récupération nécessite l’annulation de toutes les transactions incomplètes. Le temps nécessaire est proportionnel au travail effectué par la transaction et à la durée pendant laquelle elle a été active. Ainsi, le processus de récupération SQL Server peut prendre beaucoup de temps en présence de transactions longues (comme des grosses opérations d’insertion en bloc ou des opérations de création d’index sur une grande table).

En outre, l’annulation d’une grosse transaction selon cette conception peut prendre beaucoup de temps, car elle utilise la même phase de récupération d’annulation que celle décrite plus haut.

De plus, le moteur de base de données ne peut pas tronquer le journal des transactions quand il existe des transactions longues, car leurs enregistrements correspondants dans le journal sont nécessaires pour les processus de récupération et d’annulation. Par conséquent, certains journaux de transactions deviennent très grands et consomment de grandes quantités d’espace disque.

## <a name="the-accelerated-database-recovery-process"></a>Le processus de récupération de base de données accélérée

La récupération de base de données accélérée résout les problèmes ci-dessus via une reconception complète du processus de récupération du moteur de base de données pour :

- Rendre la durée de la récupération constante/instantanée en évitant d’avoir à parcourir le journal à partir de/vers le début de la transaction active la plus ancienne. Avec la récupération de base de données accélérée, le journal des transactions est traité seulement à partir du dernier point de contrôle réussi (ou du LSN de la page de modifications la plus ancienne). Ainsi, le temps de récupération n’est pas affecté par les transactions longues.
- Minimiser l’espace nécessaire au journal des transactions, car il n’est plus nécessaire de traiter le journal pour l’ensemble de la transaction. Ainsi, le journal des transactions peut être tronqué de façon agressive à mesure que des points de contrôle et des sauvegardes se produisent.

À un niveau plus général, la récupération de base de données accélérée permet de récupérer rapidement la base de données en gérant des versions pour toutes les modifications physiques de la base de données et en annulant seulement les opérations logiques, qui sont limitées et peuvent être annulées quasi instantanément. Les transactions qui étaient actives au moment d’un plantage sont marquées comme abandonnées, et les versions générées par ces transactions peuvent donc être ignorées par les requêtes utilisateur simultanées.

Le processus de récupération de base de données accélérée a les trois mêmes phases que le processus de récupération actuel. Le fonctionnement de ces phases avec la récupération de base de données accélérée est illustré dans le diagramme suivant.

![Processus de récupération de la récupération de base de données accélérée](./media/accelerated-database-recovery-concepts/adr-recovery-process.png)

- **Phase d’analyse**

  Le processus reste le même qu’aujourd’hui, avec l’ajout de la reconstruction du sLog et la copie des enregistrements de journal pour les opérations non versionnées.
  
- **Phase de restauration par progression**

  Divisée en deux sous-phases
  - Sous-phase 1

      Restauration par progression à partir du sLog (de la transaction non validée la plus ancienne jusqu’au dernier point de contrôle). La restauration par progression est une opération rapide, car elle ne doit traiter que quelques enregistrements du sLog.

  - Sous-phase 2

     La restauration par progression à partir du journal des transactions commence au dernier point de contrôle (au lieu de la transaction non validée la plus ancienne)
     
- **Phase d’annulation**

   Avec la récupération de base de données accélérée, la phase d’annulation se termine quasi instantanément via l’utilisation du sLog pour annuler les opérations non versionnées, et le magasin de versions persistantes avec la restauration logique pour effectuer les annulations basées sur la version au niveau des lignes.

Vous pouvez également regarder cette vidéo de 8 minutes qui explique la récupération de base de données accélérée

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Advanced-Database-Recovery--Data-Exposed/player?WT.mc_id=dataexposed-c9-niner]

## <a name="adr-recovery-components"></a>Composants de la récupération de base de données accélérée

Les quatre composants principaux de la récupération de base de données accélérée sont les suivants :

- **Magasin de versions persistantes**

  Le magasin de versions persistantes est un mécanisme du moteur de base de données permettant de conserver les versions des lignes générées dans la base de données elle-même, et non pas dans le magasin de versions `tempdb` traditionnelle. Le magasin de versions persistantes permet l’isolation des ressources et améliore la disponibilité des bases de données secondaires accessibles en lecture.

- **Rétablissement logique**

  Le rétablissement logique est le processus asynchrone responsable de l’exécution de l’annulation basée sur la version au niveau des lignes, qui fournit l’annulation instantanée des transactions et l’annulation pour toutes les opérations versionnées.

  - Effectue le suivi de toutes les transactions abandonnées
  - Effectue une annulation en utilisant le magasin de versions persistantes pour toutes les transactions utilisateur
  - Libère tous les verrous immédiatement après l’abandon de la transaction

- **sLog**

  sLog est un flux de journal en mémoire secondaire qui stocke les enregistrements de journal pour les opérations non versionnées (comme l’invalidation du cache de métadonnées, les acquisitions de verrou, etc.). Le sLog a les caractéristiques suivantes :

  - Il est de faible volume et en mémoire.
  - Il est conservé sur disque en étant sérialisé pendant le processus de point de contrôle.
  - Il est tronqué régulièrement à mesure que les transactions sont validées.
  - Il accélère les opérations de restauration par progression et d’annulation en traitant seulement les opérations non versionnées.  
  - Il permet la troncation agressive du journal des transactions en conservant seulement les enregistrements nécessaires du journal.

- **Nettoyeur**

  Le nettoyeur est le processus asynchrone qui sort de veille régulièrement et nettoie les versions des pages qui ne sont pas nécessaires.

## <a name="who-should-consider-accelerated-database-recovery"></a>Qui doit envisager d’utiliser la récupération de base de données accélérée ?

Les types de clients suivants doivent envisager d’activer la récupération de base de données accélérée :

- Les clients qui ont des charges de travail avec des transactions longues.
- Les clients qui ont rencontré des cas où des transactions actives provoquent une augmentation significative du journal des transactions.  
- Les clients qui ont connu de longues périodes d’indisponibilité d’une base de données en raison de la longue durée de la récupération de SQL Server (par exemple un redémarrage inattendu de SQL Server ou l’annulation manuelle de transactions).

>[!IMPORTANT]
>ADR n'est pas prise en charge pour les bases de données inscrites pour la mise en miroir de bases de données.

## <a name="see-also"></a>Voir aussi  

[Gérer la récupération de base de données accélérée](accelerated-database-recovery-management.md)
