---
title: Commutateur de fonctionnalité
description: Affiche des informations sur les deux commutateurs de fonctionnalités qui sont introduits dans Analytics Platform System AU7.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 656b1592dd50567f59744e39c24be7d93b5b220f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052830"
---
# <a name="appliance-feature-switches"></a>Commutateurs de fonctionnalités d’appliance

La page **commutateur de fonctionnalité** affiche des informations sur les commutateurs de fonctionnalités qui sont introduits dans Analytics Platform System AU7 et versions ultérieures. Utilisez cette page de configuration pour mettre à jour ou activer/désactiver des fonctionnalités et des paramètres dans Analytics Platform System.

> [!NOTE]
> Les modifications apportées aux valeurs de commutateur de fonctionnalité requièrent un redémarrage du service.

![Commutateur de fonctionnalité de l’appliance DWConfig](media/feature-switch/SQL_Server_PDW_DWConfig_feature_switch.png "Commutateur de fonctionnalité de l’appliance DWConfig")

## <a name="autostatsenabled"></a>AutoStatsEnabled

Contrôle la fonctionnalité de statistiques automatiques. Ce commutateur de fonctionnalité est défini sur true par défaut après la mise à niveau vers AU7. Toute base de données créée après la mise à niveau hérite de la création automatique et de la mise à jour asynchrone des statistiques. Pour les bases de données existantes, les administrateurs de base de données peuvent activer les statistiques automatiques avec [ALTER DATABASE (Parallel Data Warehouse)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw). Pour plus d’informations sur les statistiques, consultez [statistiques](../relational-databases/statistics/statistics.md).

## <a name="maxdopforinsertqueries"></a>MaxDOPForInsertQueries

Vous permet de choisir des paramètres MAXDOP supérieurs à 1 pour les opérations d’insertion/sélection. Les options pour ce paramètre sont 0, 1, 2 et 4, avec 1 comme valeur par défaut.

## <a name="optimizecommonsubexpressions"></a>OptimizeCommonSubExpressions

Améliore les performances des requêtes en éliminant le déplacement des données pour la sous-expression commune dans l’optimiseur de requête SQL. Vous trouverez une explication détaillée de cette fonctionnalité [ici](common-sub-expression-elimination.md).

## <a name="usecatalogqueries"></a>UseCatalogQueries

L’utilisation d’objets de catalogue pour certains appels de métadonnées au lieu d’utiliser SMO a montré une amélioration des performances. Définie sur true par défaut dans CU 7.1, ce commutateur contrôle ce comportement.

## <a name="dmsprocessstopmessagetimeoutinseconds"></a>DmsProcessStopMessageTimeoutInSeconds

Contrôle le temps pendant lequel le service de déplacement des données (DMS) attend la synchronisation sur un système occupé lorsqu’une requête impliquant le déplacement des données est annulée. La mise à jour vers AU7 définit cette valeur sur 900 secondes (15 minutes) par défaut. La plage valide est de 0-3600 secondes.
