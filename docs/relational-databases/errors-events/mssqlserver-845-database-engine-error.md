---
description: MSSQLSERVER_845
title: MSSQLSERVER_845 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 845 (Database Engine error)
ms.assetid: 8fff6ad4-234c-44be-b123-e25d5e1cd63e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: af7bef2a8888428e8d0b818278d712e73c3df513
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186765"
---
# <a name="mssqlserver_845"></a>MSSQLSERVER_845
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|845|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|BUFLATCH_TIMEOUT|  
|Texte du message|Dépassement du délai lors de l'attente du type de verrou de mémoire tampon %d, page %S_PGID, ID de base de données %d.|  
  
## <a name="explanation"></a>Explication  
Un processus était en attente d’obtention d’un verrou, mais n’a pas pu se le procurer suite à l’expiration du délai. Cette situation peut se produire si une opération d'E/S prend trop de temps à s'accomplir, souvent à cause d'autres tâches qui bloquent les processus système. Dans d'autres cas, elle est due à une défaillance matérielle.  
  
## <a name="cause"></a>Cause
Ce message d’erreur dépend de l’environnement global de votre système. Les circonstances suivantes peuvent conduire à la surcharge d’un système :

- Matériel qui ne répond pas à vos besoins en entrées/sorties (E/S) et mémoire
- Paramètres configurés et testés de manière incorrecte
- Conception inefficace

 L’erreur 845 peut se produire si votre système subit une charge importante et ne peut pas répondre aux demandes de la charge de travail. Voici quelques-unes des causes les plus courantes d’un environnement surchargé :

- Problèmes matériels
- Volumes compressés
- Paramètres de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] différents de ceux par défaut
- Requêtes ou conception d’index inefficaces
- Opérations AutoGrow ou AutoShrink fréquentes sur des bases de données

## <a name="user-action"></a>Action de l'utilisateur  
Pour prévenir cette erreur, procédez comme suit :  
  
- Déterminez si vous êtes en proie à des goulots d’étranglement matériels. L’article [Identifier les goulot d’étranglement](../performance/identify-bottlenecks.md) constitue un bon point de départ. Si nécessaire, mettez à niveau votre matériel pour qu’il puisse répondre aux besoins de votre environnement en matière de configuration, de requêtes et de charge.

- Vérifiez que l’ensemble de votre matériel fonctionne correctement. Vérifiez si vous avez des erreurs dans le journal et exécutez tous les diagnostics que votre fournisseur de matériel vous a procurés. Recherchez les erreurs d'E/S assimilées dans le journal des erreurs ou le journal des événements. Les erreurs d'E/S révèlent généralement un disque défectueux.  
- Vérifiez que vos volumes de disque ne sont pas compressés. Le stockage de fichiers de données et de fichiers journaux sur des lecteurs compressés n’est pas pris en charge. Consultez [Groupes de fichiers et fichiers de base de données](../databases/database-files-and-filegroups.md). Pour plus d’informations sur la prise en charge des lecteurs compressés, consultez l’article suivant : [Bases de données SQL Server non prises en charge sur les volumes compressés](https://support.microsoft.com/EN-US/help/231347)

- Vérifiez si les messages d’erreur disparaissent quand vous désactivez toutes les options de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suivantes :
   - L’[option priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)
   - L’[option lightweight pooling (mode fibre)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md)
   - L’[option set working set size](../../database-engine/configure-windows/set-working-set-size-server-configuration-option.md)

    Pour plus d’informations, consultez [Guide pratique pour déterminer les paramètres de configuration SQL Server appropriés](https://support.microsoft.com/EN-US/help/319942)

- Ajustez les requêtes pour réduire les ressources utilisées sur le système. L’ajustement des performances permettra de réduire la charge d’un système et d’améliorer le temps de réponse de chaque requête.
- Définissez l’option AutoShrink sur OFF pour réduire la surcharge qu’entraînent les modifications apportées à la taille de vos bases de données.
- Veillez à définir des incréments suffisamment espacés pour la propriété AutoGrow. Planifiez une tâche pour vérifier l’espace disponible dans vos bases de données, puis augmentez la taille des bases de données pendant les heures creuses.
- Recherchez dans le journal des erreurs les tâches improductives et d’autres erreurs critiques. Résolvez ces erreurs en premier, car elles peuvent révéler la cause racine du problème sous-jacent.
- Si des erreurs critiques telles que des assertions surviennent fréquemment, corrigez ces problèmes.
- Si les messages d’erreur 845 ne sont pas fréquents, vous pouvez ignorer ces erreurs.