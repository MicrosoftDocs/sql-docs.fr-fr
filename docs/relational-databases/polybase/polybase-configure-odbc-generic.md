---
title: 'Accéder aux données externes : Types génériques ODBC - PolyBase'
description: PolyBase dans SQL Server vous permet de vous connecter à des sources de données compatibles par le biais du connecteur ODBC. Installez le pilote ODBC et créez des tables externes.
ms.date: 07/16/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: e9cdd3692c32b57917e6db0e358c22f9c0dcd15a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351843"
---
# <a name="configure-polybase-to-access-external-data-with-odbc-generic-types"></a>Configurer PolyBase pour accéder à des données externes avec des types génériques ODBC

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

PolyBase dans SQL Server 2019 vous permet de vous connecter à des sources de données compatibles avec ODBC à l’aide du connecteur ODBC.

Cet article explique comment créer la configuration de la connectivité à l’aide d’une source de données ODBC. L’aide fournie utilise un pilote ODBC spécifique comme exemple. Contactez votre fournisseur ODBC pour obtenir des exemples spécifiques. Pour déterminer les options de chaîne de connexion appropriées, référez-vous à la documentation du pilote ODBC relative à votre source de données. Les exemples présentés dans cet article peuvent ne s’appliquer à aucun pilote ODBC spécifique.

## <a name="prerequisites"></a>Prérequis

>[!NOTE]
>Cette fonctionnalité nécessite SQL Server sur Windows.

* PolyBase doit être installé et activé pour votre [installation de PolyBase](polybase-installation.md) dans l’instance SQL Server.

* La [clé principale](../../t-sql/statements/create-master-key-transact-sql.md) doit être créée avant les informations d’identification incluses dans l’étendue de la base de données.

## <a name="install-the-odbc-driver"></a>Installer le pilote ODBC

Téléchargez et installez le pilote ODBC de la source de données à laquelle vous souhaitez vous connecter sur chacun des nœuds PolyBase. Une fois que le pilote est correctement installé, vous pouvez voir et tester le pilote à partir de l’**Administrateur de sources de données ODBC**.

![Groupes de scale-out PolyBase](../../relational-databases/polybase/media/polybase-odbc-admin.png) 

Dans l’exemple ci-dessus, le nom du pilote est entouré en rouge. Utilisez ce nom quand vous créez la source de données externe.

> [!IMPORTANT]
> Afin d’améliorer les performances des requêtes, activez le regroupement de connexions. Cette opération peut être effectuée à partir de l’**Administrateur de sources de données ODBC**.

## <a name="create-dependent-objects-in-sql-server"></a>Créer des objets dépendants dans SQL Server

Pour utiliser la source de données ODBC, vous devez d’abord créer quelques objets pour finaliser la configuration.

Les commandes Transact-SQL suivantes sont utilisées dans cette section :

* [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
* [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 

1. Créez des informations d’identification incluses dans l’étendue de la base de données pour accéder à la source ODBC.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL <credential_name> WITH IDENTITY = '<username>', Secret = '<password>';
    ```

    Par exemple, l’exemple suivant crée des informations d’identification nommées `credential_name`, avec l’identité `username` et un mot de passe complexe.

    ```sql
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'BycA4ZjrE#*2W%!';
    ```

1. Créez une source de données externe avec [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

    ```sql
    CREATE EXTERNAL DATA SOURCE <external_data_source_name>
    WITH ( LOCATION = 'odbc://<ODBC server address>[:<port>]',
    CONNECTION_OPTIONS = 'Driver={<Name of Installed Driver>};
    ServerNode = <name of server  address>:<Port>',
    -- PUSHDOWN = [ON] | OFF,
    CREDENTIAL = <credential_name> );
    ```

    L’exemple suivant crée une source de données externe :
    * `external_data_source_name` nommée
    * Situé sur le serveur ODBC `SERVERNAME` et sur le port `4444`
    * Connexion avec `CData ODBC Driver For SAP 2015` : il s’agit du pilote créé dans [Installer le pilote ODBC](#install-the-odbc-driver)
    * Sur `ServerNode` `sap_server_node`, port `5555`
    * Configuré pour un traitement transmis au serveur (`PUSHDOWN = ON`)
    * Utilisation des informations d’identification `credential_name`

    ```sql
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH ( LOCATION = 'odbc://SERVERNAME:4444',
    CONNECTION_OPTIONS = 'Driver={CData ODBC Driver For SAP 2015};
    ServerNode = sap_server_node:5555',
    PUSHDOWN = ON,
    CREDENTIAL = credential_name );
    ```
    
## <a name="create-an-external-table"></a>Créer une table externe

Une fois que vous avez créé les objets dépendants, vous pouvez créer des tables externes à l’aide de T-SQL. 

Les commandes Transact-SQL suivantes sont utilisées dans cette section :
* [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md)
* [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. Créez une ou plusieurs tables externes.

   Créez une table externe. Vous devrez référencer la source de données externe créée ci-dessus à l’aide de l’argument `DATA_SOURCE` et spécifier la table source comme `LOCATION`. Vous n’avez pas besoin de référencer toutes les colonnes, mais vous devez vous assurer que les types sont correctement mappés.  

   ```sql
     CREATE EXTERNAL TABLE <your_table_name>
     (
     <col1_name>     DECIMAL(38) NOT NULL,
     <col2_name>     DECIMAL(38) NOT NULL,
     <col3_name>     CHAR COLLATE Latin1_General_BIN NOT NULL
     )
     WITH (
     LOCATION='<sap_table_name>',
     DATA_SOURCE= <external_data_source_name>
     )
     ;
   ```

   > [!NOTE]
   > Remarque : vous pouvez réutiliser les objets dépendants pour toutes les tables externes à l’aide de cette source de données externe.

1. **Facultatif :** Créez des statistiques sur une table externe.

    Pour des performances de requêtes optimales, nous vous recommandons de créer des statistiques sur les colonnes de table externe, en particulier celles utilisées pour les jointures, les filtres et les agrégats.

    ```sql
    CREATE STATISTICS statistics_name ON contact (FirstName) WITH FULLSCAN; 
    ```
    
## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur PolyBase, consultez [Vue d’ensemble de SQL Server PolyBase](polybase-guide.md).
