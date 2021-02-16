---
title: 'Accéder aux données externes : SQL Server - PolyBase'
description: Découvrez comment utiliser PolyBase sur une instance SQL Server pour interroger des données externes dans une autre instance SQL Server. Créez des tables externes pour référencer des données externes.
ms.date: 10/06/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: dbd62f695128965e1b60f9b027ddc06c003a8b71
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351776"
---
# <a name="configure-polybase-to-access-external-data-in-sql-server"></a>Configurer PolyBase pour accéder à des données externes dans SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cet article explique comment utiliser PolyBase sur une instance de SQL Server pour interroger des données externes dans une autre instance SQL Server.

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas installé PolyBase, consultez [Installation de PolyBase](polybase-installation.md). Cet article décrit les prérequis pour l’installation. Une fois l’installation terminée, veillez également à [activer Polybase](polybase-installation.md#enable).

La source de données externe SQL Server utilise l’authentification SQL.

La [clé principale](../../t-sql/statements/create-master-key-transact-sql.md) doit être créée avant les informations d’identification incluses dans l’étendue de la base de données. 

## <a name="configure-a-sql-server-external-data-source"></a>Configurer une source de données externes SQL Server

Pour interroger les données d’une source de données SQL Server, vous devez créer des tables externes pour référencer les données externes. Cette section fournit un exemple de code pour créer ces tables externes.
 
Pour des performances de requêtes optimales, créez des statistiques sur les colonnes de tables externes, en particulier celles utilisées pour les jointures, les filtres et les agrégats.

Les commandes Transact-SQL suivantes sont utilisées dans cette section :

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. Créez des informations d’identification incluses dans l’étendue de la base de données pour accéder à la source SQL Server. L’exemple suivant crée des informations d’identification pour la source de données externe avec `IDENTITY = 'username'` et `SECRET = 'password'`.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL SqlServerCredentials
    WITH IDENTITY = 'username', SECRET = 'password';
    ```
   >[!IMPORTANT]
   >Le connecteur ODBC SQL pour Polybase prend uniquement en charge l’authentification de base (l’authentification Kerberos n’est pas prise en charge).

1. Créez une source de données externe avec [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md). L’exemple suivant :

   - Crée une source de données externe pour nommée `SQLServerInstance`.
   - Identifie la source de données externe (`LOCATION = '<vendor>://<server>[:<port>]'`). Dans l’exemple, elle pointe vers une instance par défaut de SQL Server.
   - Indique si le calcul doit faire l’objet d’un push vers la source (`PUSHDOWN`). `PUSHDOWN` est `ON` par défaut.

   Pour finir, l’exemple utilise les informations d’identification créées précédemment.

    ```sql
    CREATE EXTERNAL DATA SOURCE SQLServerInstance
        WITH ( LOCATION = 'sqlserver://SqlServer',
        PUSHDOWN = ON,
        CREDENTIAL = SQLServerCredentials);
    ```

1. Éventuellement, créez des statistiques sur une table externe.

  Pour des performances de requêtes optimales, créez des statistiques sur les colonnes de tables externes, en particulier celles utilisées pour les jointures, les filtres et les agrégats.

  ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY)
    WITH FULLSCAN;
  ```

>[!IMPORTANT]
>Une fois que vous avez créé une source de données externes, vous pouvez utiliser la commande [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) afin de créer une table requêtable sur cette source.

## <a name="sql-server-connector-compatible-types"></a>Types compatibles avec le connecteur SQL Server

Vous pouvez établir une connexion avec d’autres sources de données qui reconnaissent les connexions SQL Server. Utilisez le connecteur PolyBase SQL Server pour créer une table externe Azure Synapse Analytics et Azure SQL Database. Pour accomplir cette tâche, suivez les étapes indiquées précédemment. Vérifiez que les informations d’identification incluses dans l’étendue de la base de données, l’adresse du serveur, le port et la chaîne d’emplacement sont bien ceux de la source de données compatible à laquelle vous souhaitez vous connecter.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur PolyBase, consultez [Vue d’ensemble de SQL Server PolyBase](polybase-guide.md).
