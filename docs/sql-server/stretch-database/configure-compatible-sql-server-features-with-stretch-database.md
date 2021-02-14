---
description: Configurer les fonctionnalités de SQL Server compatibles avec Stretch Database
title: Configurer les fonctionnalités SQL Server compatibles
ms.date: 03/14/2017
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: c8121ede-1aec-459b-b7b0-1408bb3e62fb
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: c1f2389ea412b1cdc542f87ee25f3a95c34fa256
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080210"
---
# <a name="configure-compatible-sql-server-features-with-stretch-database"></a>Configurer les fonctionnalités de SQL Server compatibles avec Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Suivez des étapes simples pour configurer les fonctionnalités SQL Server suivantes afin de les rendre compatibles avec Stretch Database.
-   Always On
-   Always Encrypted
-   Transparent Data Encryption (TDE)
-   Tables temporelles

## <a name="configure-always-on-with-stretch-database"></a>Configurer Always On avec Stretch Database
Si vous utilisez Always On avec Stretch Database, vous devez vous assurer que la clé principale de base de données est disponible sur les réplicas secondaires. Stretch Database utilise la clé principale de base de données pour sécuriser les informations d’identification utilisées pour la connexion à la base de données Azure distante.

Après avoir configuré le groupe de disponibilité Always On, exécutez la procédure stockée **sp_control_dbmasterkey_password** sur chaque réplica secondaire et fournissez le mot de passe pour la base de données Stretch. Pour plus d’informations et des exemples, consultez [sp_control_dbmasterkey_password](../../relational-databases/system-stored-procedures/sp-control-dbmasterkey-password-transact-sql.md). 

## <a name="configure-always-encrypted-with-stretch-database"></a>Configurer Always Encrypted avec Stretch Database
Si vous souhaitez utiliser conjointement Always Encrypted et Stretch Database, vous devez configurer le chiffrement sur les colonnes sélectionnées avant d’activer la base de données Stretch sur la table.

Si vous avez déjà activé la base de données Stretch sur la table et que vous souhaitez utiliser les colonnes Always Encrypted, vous devez effectuer les opérations suivantes.
1.   Désactiver Stretch Database sur la table et afficher à nouveau les données distantes à partir d’Azure. Pour plus d’informations, consultez [Désactiver Stretch Database et récupérer les données distantes](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).
2.   Configurez Always Encrypted sur les colonnes sélectionnées.
3. Réactivez Stretch Database sur la table. Pour plus d'informations, consultez [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).

## <a name="configure-transparent-data-encryption-tde-with-stretch-database"></a>Configurer le chiffrement transparent des données (TDE) avec Stretch Database

Si le chiffrement transparent des données est activé sur votre base de données locale, il n’est pas automatiquement activé sur le point de terminaison distant de la base de données Stretch. Vous devez penser à activer le chiffrement transparent des données sur le point de terminaison distant après l’activation de Stretch sur votre base de données.

## <a name="configure-temporal-tables-with-stretch-database"></a>Configurer les tables temporelles avec Stretch Database
Si vous utilisez des tables temporelles, vous pouvez activer Stretch Database sur la table d’historique, mais pas sur la table actuelle.
-   Pour obtenir des conseils sur l’utilisation de tables temporelles avec Stretch Database, consultez [Gérer la rétention de données d’historique dans les tables temporelles avec version gérée par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md).
-   Pour filtrer les lignes pour migrer à partir de la table d’historique à l’aide d’une fenêtre glissante, consultez [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).
-   Vous ne pouvez pas activer Stretch Database sur la table d’historique temporelle si la table est optimisée en mémoire. Les tables optimisées en mémoire ne sont pas prises en charge.
