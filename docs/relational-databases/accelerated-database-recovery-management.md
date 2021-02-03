---
description: Gérer la récupération de base de données accélérée
title: Gérer la récupération de base de données accélérée | Microsoft Docs
ms.date: 08/12/2019
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
ms.openlocfilehash: cfd5a901f38dacf9e17baff4d65363796ab3cd73
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236110"
---
# <a name="manage-accelerated-database-recovery"></a>Gérer la récupération de base de données accélérée

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver2019.md)]

## <a name="enabling-and-controlling-adr"></a>Activation et contrôle de la récupération de base de données accélérée

La récupération de base de données accélérée est désactivée par défaut dans [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] et peut être contrôlée avec la syntaxe DDL :
```sql
ALTER DATABASE [DB] SET ACCELERATED_DATABASE_RECOVERY = {ON | OFF}
[(PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name }) ];

```

Utilisez cette syntaxe pour contrôler si la fonctionnalité est activée ou désactivée, et désignez un groupe de fichiers spécifique pour les données du *magasin de versions persistantes*. Si aucun groupe de fichiers n’est spécifié, le magasin de versions persistantes est stocké dans le groupe de fichiers PRIMARY.

## <a name="managing-the-persistent-version-store-filegroup"></a>Gestion du groupe de fichiers du magasin de versions persistantes
La fonctionnalité Récupération de base de données accélérée est basée sur les changements versionnés, les différentes versions d’un élément de données étant conservées dans le magasin de versions persistantes.
Vous devez prendre en compte certaines considérations quant à l’emplacement du magasin de versions persistantes et à la gestion de la taille des données qui y sont stockées.

### <a name="to-enable-adr-without-specifying-a-filegroup"></a>Pour activer la récupération de base de données accélérée sans spécifier de groupe de fichiers

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON;
GO
```

Dans ce cas, quand le groupe de fichiers du magasin de versions persistantes n’est pas spécifié, c’est le groupe de fichiers `PRIMARY` qui contient ses données.

### <a name="to-enable-adr-and-specify-that-the-pvs-should-be-stored-in-the-versionstorefg-filegroup"></a>Pour activer la récupération de base de données accélérée et spécifier que le magasin de versions persistantes doit être stocké dans le groupe de fichiers [VersionStoreFG]

Avant d’exécuter ce script, créez le groupe de fichiers.

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
(PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
```

### <a name="to-disable-the-adr-feature"></a>Pour désactiver la fonctionnalité Récupération de base de données accélérée

```sql
ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
GO
```

Même après la désactivation de la fonctionnalité Récupération de base de données accélérée, il restera des versions stockées dans le magasin de versions persistantes, qui sont toujours nécessaires au système pour la restauration logique.

### <a name="change-the-location-of-the-pvs-to-a-different-filegroup"></a>Changer l’emplacement du magasin de versions persistantes pour un autre groupe de fichiers

Il peut être nécessaire de déplacer l’emplacement du magasin de versions persistantes vers un autre groupe de fichiers pour différentes raisons. Par exemple, le magasin de versions persistantes peut nécessiter plus d’espace ou un stockage plus rapide.

Le changement d’emplacement du magasin de versions persistantes est un processus en trois étapes.

1. Désactivez la fonctionnalité Récupération de base de données accélérée.

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = OFF;
   GO
   ```

2. Attendez que toutes les versions stockées dans le magasin de versions persistantes puissent être libérées.

   Pour être en mesure d’activer la récupération de base de données accélérée avec un nouvel emplacement pour le magasin de versions persistantes, vous devez d’abord vérifier que toutes les informations des versions ont été éliminées de l’emplacement précédent du magasin de versions persistantes. Pour forcer le nettoyage, exécutez cette commande :

   ```sql
   EXEC sys.sp_persistent_version_cleanup [database name]
   ```

   La procédure stockée `sys.sp_persistent_version_cleanup` est synchrone, ce qui signifie qu’elle ne se termine pas tant que toutes les informations des versions ne sont pas nettoyées dans le magasin de versions persistantes actuel.  Une fois l’opération terminée, vous pouvez vérifier que les informations des versions sont effectivement supprimées en interrogeant la vue de gestion dynamique `sys.dm_persistent_version_store_stats` et en examinant la valeur de `persistent_version_store_size_kb`.

   ```sql
   SELECT DB_Name(database_id), persistent_version_store_size_kb 
   FROM sys.dm_tran_persistent_version_store_stats where database_id = [MyDatabaseID]
   ```

   Quand la valeur de persistent_version_store_size_kb est 0, vous pouvez réactiver la fonctionnalité Récupération de base de données accélérée, en configurant le magasin de versions persistantes pour qu’il se trouve dans le nouveau groupe de fichiers.

1. Activer la récupération de base de données accélérée en spécifiant le nouvel emplacement du magasin de versions persistantes

   ```sql
   ALTER DATABASE [MyDatabase] SET ACCELERATED_DATABASE_RECOVERY = ON
   (PERSISTENT_VERSION_STORE_FILEGROUP = [VersionStoreFG])
   ```

## <a name="troubleshooting"></a>Dépannage

> [!NOTE]
> Cette section s’applique également à Azure SQL Database.

Interrogez `sys.dm_tran_persistent_version_store_stats` pour vérifier les tailles du magasin de versions persistantes.

Vérifiez la taille de `% of DB`. Notez également la différence par rapport à la taille standard.

Un magasin de versions persistantes est considéré comme grand s’il est significativement plus grand que la base de référence ou s’il est proche de 50 % de la taille de la base de données. 

1. Récupérez `oldest_active_transaction_id` et vérifiez si cette transaction a été active pendant une longue période temps en interrogeant `sys.dm_tran_database_transactions` avec l’ID de transaction.

   Les transactions actives empêchent le nettoyage du magasin de versions persistantes.

1. Si la base de données fait partie d’un groupe de disponibilité, vérifiez la `secondary_low_water_mark`. C’est la même chose que la `low_water_mark_for_ghosts` indiquée par `sys.dm_hadr_database_replica_states`. Interrogez `sys.dm_hadr_database_replica_states` pour voir si un des réplicas contient cette valeur, car ceci empêchera également le nettoyage du magasin de versions persistantes.
1. Vérifiez `min_transaction_timestamp` (ou `online_index_min_transaction_timestamp` si le magasin de versions persistantes en ligne est en attente) et, sur cette base, vérifiez `sys.dm_tran_active_snapshot_database_transactions` pour la colonne `transaction_sequence_num` pour trouver la session qui a l’ancienne transaction d’instantané bloquant le nettoyage du magasin de versions persistantes.
1. Si aucun des éléments ci-dessus ne s’applique, cela signifie que le nettoyage est bloqué par des transactions abandonnées. Recherchez l’heure la plus récente de `aborted_version_cleaner_last_start_time` et de `aborted_version_cleaner_last_end_time` pour voir si le nettoyage des transactions abandonnées est terminé. `oldest_aborted_transaction_id` doit passer plus haut après la fin du nettoyage des transactions abandonnées.
1. Si la transaction abandonnée n’a pas été effectuée avec succès récemment, recherchez dans le journal des erreurs des messages indiquant des problèmes concernant `VersionCleaner`.
