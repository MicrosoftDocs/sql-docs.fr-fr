---
title: Statistiques automatiques
description: Décrit la fonctionnalité de statistiques automatiques introduite dans Analytics Platform System AU7.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 4311195ce4b748ffeb73c4a001086b3f199b2c5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052780"
---
# <a name="configure-auto-statistics"></a>Configurer les statistiques automatiques

Découvrez comment configurer des Data Warehouse parallèles pour utiliser les statistiques automatiques pour la création et la mise à jour automatique des statistiques.  Utilisez cette fonctionnalité pour améliorer les plans de requête et, par conséquent, améliorer les performances des requêtes.

**S’applique à :** APS (à partir de 2016-AU7)

## <a name="what-are-statistics"></a>Que sont les statistiques ?
Les statistiques relatives à l’optimisation des requêtes sont des objets qui contiennent des informations statistiques sur la distribution des valeurs dans une ou plusieurs colonnes d’une table. L'optimiseur de requête utilise ces statistiques pour estimer le nombre de lignes, également appelé cardinalité, dans le résultat de la requête. Ces estimations de cardinalité permettent à l'optimiseur de requête de créer un plan de requête de haute qualité. Par exemple, dans APS, l’optimiseur de requête MPP utilise des estimations de cardinalité pour choisir de lire ou de répliquer la plus petite des deux tables utilisées dans une clause de jointure et d’améliorer les performances des requêtes.  Pour plus d’informations, consultez [Statistics](../relational-databases/statistics/statistics.md) and [DBCC SHOW_STATISTICS](../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)

## <a name="what-are-auto-statistics"></a>Que sont les statistiques automatiques ?
Les statistiques automatiques sont des statistiques créées par l’optimiseur de requête et mises à jour automatiquement pour améliorer le plan de requête. Les statistiques peuvent devenir obsolètes après les chargements, les insertions, les mises à jour et les suppressions. Sans les statistiques automatiques, vous devez effectuer votre propre analyse pour comprendre quelles colonnes nécessitent des statistiques et quand les statistiques doivent être mises à jour.

Les statistiques automatiques incluent les trois paramètres suivants : 

### <a name="auto_create_statistics"></a>AUTO_CREATE_STATISTICS
Quand l’option de création automatique de statistiques AUTO_CREATE_STATISTICS est activée, l’optimiseur de requête crée les statistiques nécessaires sur les colonnes individuelles du prédicat de requête pour améliorer les estimations de cardinalité pour le plan de requête. Ces statistiques de colonne unique sont créées sur les colonnes où ne figure pas déjà un histogramme au niveau d'un objet de statistiques existant.

### <a name="auto_update_statistics"></a>AUTO_UPDATE_STATISTICS 
Lorsque l'option de mise à jour automatique des statistiques AUTO_UPDATE_STATISTICS est activée, l'optimiseur de requête détermine si les statistiques sont obsolètes et les met éventuellement à jour lorsqu'elles sont utilisées par une requête. Les statistiques deviennent obsolètes si des opérations d’insertion, de mise à jour, de suppression ou de fusion changent la distribution des données dans la table ou la vue indexée. L'optimiseur de requête détermine si les statistiques sont obsolètes en comptant le nombre de modifications de données depuis la dernière mise à jour des statistiques et en comparant le nombre de modifications à un seuil. Ce seuil est basé sur le nombre de lignes contenues dans la table ou la vue indexée.

### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC
L'option de mise à jour asynchrone des statistiques AUTO_UPDATE_STATISTICS_ASYNC détermine si l'optimiseur de requête utilise des mises à jour de statistiques synchrones ou asynchrones. Pour APS, l’option de mise à jour asynchrone des statistiques est activée par défaut, et l’optimiseur de requête met à jour les statistiques de manière asynchrone. L’option AUTO_UPDATE_STATISTICS_ASYNC s’applique aux objets de statistiques créés pour les index, aux colonnes uniques contenues dans les prédicats de requête et aux statistiques créées à l’aide de l’instruction CREATE STATISTICS.

## <a name="configuration-settings-for-system-administrators"></a>Paramètres de configuration pour les administrateurs système
Après la mise à niveau vers APS AU7, les statistiques automatiques sont activées par défaut. L’administrateur système peut activer ou désactiver les statistiques automatiques avec l’option de [commutateur de fonctionnalité](appliance-feature-switch.md) dans le Configuration Manager de l’appliance.  Une fois activé, les utilisateurs peuvent modifier les paramètres de statistiques par base de données.
La modification d’une valeur de commutateur de fonctionnalité nécessite un redémarrage du service sur APS.

## <a name="change-auto-statistics-settings-on-a-database"></a>Modifier les paramètres de statistiques automatiques sur une base de données
Lorsque les statistiques automatiques sont activées par l’administrateur système, vous pouvez utiliser [ALTER DATABASE (Parallel Data Warehouse)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) pour modifier les paramètres de statistiques sur une base de données. Si le commutateur de fonctionnalité de statistiques automatiques est activé par l’administrateur système, les statistiques automatiques sont activées pour toutes les nouvelles bases de données créées après la mise à niveau vers AU7. Toutes les bases de données qui existaient avant la mise à niveau vers AU7 ont des statistiques automatiques désactivées. L’exemple suivant active les statistiques automatiques sur la base de données myPDW existante.

```sql
ALTER DATABASE myPDW SET AUTO_CREATE_STATISTICS ON
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS ON 
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS_ASYNC ON
```
 
AUTO_UPDATE option STATISTICS_ASYNC ne fonctionne que si la AUTO_UPDATE_STATISTICS est activée.  Par conséquent, les statistiques ne sont pas mises à jour lorsque AUTO_UPDATE_STATISTICS est désactivé et AUTO_UPDATE_STATISTICS_ASYNC est activé. 

### <a name="error-messages"></a>Messages d’erreur
Vous pouvez recevoir le message d’erreur « cette option n’est pas prise en charge dans PDW ».  Cette erreur se produit lorsque l’administrateur système n’a pas activé les statistiques automatiques et que vous essayez de définir l’une des options de statistiques automatiques dans ALTER DATABASE. 

### <a name="limitations-and-restrictions"></a>Limitations et restrictions
Les statistiques automatiques ne fonctionnent pas sur les tables externes. 

### <a name="check-the-current-values"></a>Vérifier les valeurs actuelles
La requête suivante renvoie les valeurs actuelles des paramètres de statistiques automatiques pour toutes les bases de données.

```sql
SELECT NAME
    , IS_AUTO_CREATE_STATS_ON 
    , IS_AUTO_UPDATE_STATS_ON
    , IS_AUTO_UPDATE_STATS_ASYNC_ON
FROM
    sys.databases;
```

Une valeur de retour de 1 signifie que le paramètre est activé, et 0 signifie que le paramètre est désactivé. 

## <a name="next-steps"></a>Étapes suivantes
Pour voir comment vos requêtes sont exécutées, consultez [surveillance des requêtes actives](monitoring-active-queries.md) .
