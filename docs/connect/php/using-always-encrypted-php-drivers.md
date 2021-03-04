---
title: Utilisation d’Always Encrypted avec Microsoft Drivers for PHP for SQL Server
description: Découvrez comment utiliser Always Encrypted avec les pilotes PHP pour SQL Server pour protéger des données sensibles dans votre application.
ms.date: 12/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: ''
ms.author: v-daenge
author: David-Engel
manager: v-mabarw
ms.openlocfilehash: 4c16930d75d191c83f3f6eead028bc9913556e3c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837136"
---
# <a name="using-always-encrypted-with-the-php-drivers-for-sql-server"></a>Utilisation d’Always Encrypted avec Microsoft Drivers for PHP for SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>Applicable à
 -   Microsoft Drivers 5.2 for PHP for SQL Server
 
## <a name="introduction"></a>Introduction

Cet article fournit des informations sur la façon de développer des applications PHP à l’aide d’[Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) et de [PHP Drivers for SQL Server](../../connect/php/Microsoft-php-driver-for-sql-server.md).

Always Encrypted permet aux applications clientes de chiffrer des données sensibles et de ne jamais révéler les données ou les clés de chiffrement à SQL Server ou Azure SQL Database. Un pilote compatible avec Always Encrypted, comme ODBC Driver for SQL Server, chiffre et déchiffre de manière transparente les données sensibles dans l’application cliente. Le pilote détermine automatiquement les paramètres de requêtes qui correspondent aux colonnes de base de données sensibles (protégées avec Always Encrypted) et chiffre les valeurs de ces paramètres avant de transmettre les données à SQL Server ou Azure SQL Database. De même, il déchiffre de manière transparente les données récupérées dans les colonnes de base de données chiffrées, qui figurent dans les résultats de la requête. Pour plus d’informations, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Les pilotes PHP pour SQL Server utilisent ODBC Driver pour SQL Server afin de chiffrer les données sensibles.

## <a name="prerequisites"></a>Prérequis

 -   Configurez Always Encrypted dans votre base de données. Pour cela, vous devez mettre en service des clés Always Encrypted et configurer le chiffrement pour les colonnes de base de données sélectionnées. Si vous n’avez pas déjà une base de données dans laquelle est configuré Always Encrypted, suivez les instructions de [Prise en main d’Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). En particulier, votre base de données doit contenir les définitions de métadonnées pour une clé principale de colonne (CMK), une clé de chiffrement de colonne (CEK) et une table contenant une ou plusieurs colonnes chiffrées à l’aide de cette clé CEK.
 -   Vérifiez qu’ODBC Driver pour SQL Server version 17 ou ultérieure est installé sur votre ordinateur de développement. Pour plus de détails, consultez [ODBC Driver pour SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="enabling-always-encrypted-in-a-php-application"></a>Activation d’Always Encrypted dans une application PHP

Le moyen le plus simple d’activer le chiffrement des paramètres ciblant des colonnes chiffrées, et le déchiffrement des résultats de requête, est de définir la valeur du mot clé de chaîne de connexion `ColumnEncryption` sur `Enabled`. Voici des exemples d’activation d’Always Encrypted dans les pilotes SQLSRV et PDO_SQLSRV :

SQLSRV :
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled");
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV :
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

L’activation d’Always Encrypted ne suffit pas à la réussite du chiffrement ou du déchiffrement ; vous devez également garantir ce qui suit :
 -   L’application dispose des autorisations de base de données VIEW ANY COLUMN MASTER KEY DEFINITION et VIEW ANY COLUMN ENCRYPTION KEY DEFINITION qui sont nécessaires pour accéder aux métadonnées des clés Always Encrypted dans la base de données. Pour plus d’informations, consultez [Autorisation de base de données](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
 -   L’application peut accéder à la clé CMK qui protège les clés CEK pour les colonnes chiffrées interrogées. Cette condition dépend du fournisseur de magasin de clés qui stocke la clé CMK. Pour plus d’informations, consultez [Utilisation de magasins de clés principales de colonne](#working-with-column-master-key-stores).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Récupération et modification des données dans des colonnes chiffrées

Une fois que vous avez activé Always Encrypted sur une connexion, vous pouvez utiliser les API SQLSRV standard (voir [Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)) ou les API PDO_SQLSRV (voir [Informations de référence sur l’API du pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)) pour récupérer ou modifier des données dans des colonnes de base de données chiffrées. En supposant que votre application dispose des autorisations de base de données requises et puisse accéder à la clé principale de colonne, le pilote chiffrera tous les paramètres de requête qui ciblent des colonnes chiffrées et déchiffrera les données récupérées des colonnes chiffrées, se comportant de manière transparente pour l’application comme si les colonnes n’étaient pas chiffrées.

Si Always Encrypted n’est pas activé, les requêtes ayant des paramètres qui ciblent des colonnes chiffrées échouent. Les données peuvent toujours être récupérées à partir des colonnes chiffrées, tant que la requête n’a aucun paramètre qui cible des colonnes chiffrées. Toutefois, le pilote ne tente aucun déchiffrement et l’application reçoit les données chiffrées binaires (sous la forme de tableaux d’octets).

Le tableau ci-dessous récapitule le comportement des requêtes, selon qu’Always Encrypted est activé ou non :

|Caractéristique de la requête|Always Encrypted est activé et l’application peut accéder aux clés et à leurs métadonnées|Always Encrypted est activé et l’application ne peut pas accéder aux clés et à leurs métadonnées|Always Encrypted est désactivé|
|---|---|---|---|
|Paramètres ciblant des colonnes chiffrées.|Des valeurs de paramètres sont chiffrées en toute transparence.|Error|Error|
|Récupération des données à partir de colonnes chiffrées, sans paramètres ciblant des colonnes chiffrées.|Les résultats de colonnes chiffrées sont déchiffrés de manière transparente. L’application reçoit des valeurs de colonne en texte clair. |Error|Les résultats des colonnes chiffrées ne sont pas déchiffrés. L’application reçoit des valeurs chiffrées sous la forme de tableaux d’octets.|
 
Les exemples suivants illustrent la récupération et la modification de données dans des colonnes chiffrées. Les exemples partent du principe que la table a le schéma suivant. Les colonnes SSN et BirthDate sont chiffrées.
```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="data-insertion-example"></a>Exemple d’insertion de données

Les exemples suivants montrent comment utiliser les pilotes SQLSRV et PDO_SQLSRV pour insérer une ligne dans la table Patients. Notez les points suivants :
 -   L’exemple de code ne contient aucun élément spécifique au chiffrement. Le pilote détecte et chiffre automatiquement les valeurs des paramètres de BirthDate et SSN, qui ciblent des colonnes chiffrées. Ce mécanisme rend le chiffrement transparent pour l’application.
 -   Les valeurs insérées dans les colonnes de base de données, y compris les colonnes chiffrées, sont passées en tant qu’objets liés. L’utilisation de paramètres est facultative lors de l’envoi de valeurs aux colonnes non chiffrées (même si elle est vivement recommandée, car elle contribue à empêcher l’injection SQL), mais elle est nécessaire pour les valeurs qui ciblent des colonnes chiffrées. Si les valeurs insérées dans les colonnes SSN ou BirthDate ont été passées en tant que littéraux incorporés dans l’instruction de requête, la requête échoue car le pilote ne tente pas de chiffrer ou de traiter les littéraux dans les requêtes. Par conséquent, le serveur les rejettera en les considérant comme incompatibles avec les colonnes chiffrées.
 -   Lors de l’insertion de valeurs à l’aide de paramètres de liaison, un type SQL identique au type de données de la colonne cible ou dont la conversion vers le type de données de la colonne cible est prise en charge doit être passé à la base de données. Cette exigence est due au fait qu’Always Encrypted prend en charge peu de conversions de types (pour plus d’informations, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)). Les deux pilotes PHP, SQLSRV et PDO_SQLSRV, disposent chacun d’un mécanisme qui permet à l’utilisateur de déterminer le type SQL de la valeur. Par conséquent, l’utilisateur n’a pas besoin de fournir explicitement le type SQL.
  -   Pour le pilote SQLSRV, l’utilisateur dispose de deux options :
   -   Utiliser le pilote PHP pour déterminer et définir le type SQL approprié. Dans ce cas, l’utilisateur doit utiliser `sqlsrv_prepare` et `sqlsrv_execute` pour exécuter une requête paramétrable.
   -   Définir explicitement le type SQL.
  -   Pour le pilote PDO_SQLSRV, l’utilisateur n’a pas la possibilité de définir explicitement le type SQL d’un paramètre. Le pilote PDO_SQLSRV aide automatiquement l’utilisateur à déterminer le type SQL lors de la liaison d’un paramètre.
 -   Pour permettre aux pilotes de déterminer le type SQL, certaines limitations s’appliquent :
  -   Pilote SQLSRV :
   -   Si l’utilisateur souhaite que le pilote détermine les types SQL pour les colonnes chiffrées, l’utilisateur doit utiliser `sqlsrv_prepare` et `sqlsrv_execute`.
   -   Si `sqlsrv_query` est préféré, l’utilisateur doit spécifier les types SQL pour tous les paramètres. Le type SQL spécifié doit inclure la longueur de chaîne pour les types de chaînes, et l’échelle et la précision des types décimaux.
  -   Pilote PDO_SQLSRV :
   -   L’attribut d’instruction `PDO::SQLSRV_ATTR_DIRECT_QUERY` n’est pas pris en charge dans une requête paramétrable.
   -   L’attribut d’instruction `PDO::ATTR_EMULATE_PREPARES` n’est pas pris en charge dans une requête paramétrable.
   
Pilote SQLSRV et [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) :
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel;
$birthDate = "1996-10-19";
$params = array($ssn, $firstName, $lastName, $birthDate);
// during sqlsrv_prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = sqlsrv_prepare($conn, $query, $params);
sqlsrv_execute($stmt);
```

Pilote SQLSRV et [sqlsrv_query](../../connect/php/sqlsrv-query.md) :
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel";
$birthDate = "1996-10-19";
// need to provide the SQL types for ALL parameters  
// note the SQL types (including the string length) have to be the same at the column definition
$params = array(array(&$ssn, null, null, SQLSRV_SQLTYPE_CHAR(11)),
                array(&$firstName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$lastName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$birthDate, null, null, SQLSRV_SQLTYPE_DATE));
sqlsrv_query($conn, $query, $params);
```

Pilote PDO_SQLSRV et [PDO::prepare](../../connect/php/pdo-prepare.md) :
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Able";
$birthDate = "1996-10-19";
// during PDO::prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
$stmt->bindParam(2, $firstName);
$stmt->bindParam(3, $lastName);
$stmt->bindParam(4, $birthDate);
$stmt->execute();
```

### <a name="plaintext-data-retrieval-example"></a>Exemple d’extraction de données en texte clair

L’exemple suivant montre le filtrage de données basé sur des valeurs chiffrées, ainsi que la récupération de données en texte clair à partir de colonnes chiffrées à l’aide des pilotes SQLSRV et PDO_SQLSRV. Notez les points suivants :
 -   La valeur utilisée dans la clause WHERE pour filtrer la colonne SSN doit être transmise avec un paramètre bind, afin que le pilote puisse la chiffrer de manière transparente avant de l’envoyer au serveur.
 -   Lors de l’exécution d’une requête avec des paramètres liés, les pilotes PHP déterminent automatiquement le type SQL de l’utilisateur, sauf si ce dernier spécifie explicitement le type SQL lors de l’utilisation du pilote SQLSRV.
 -   Toutes les valeurs imprimées par le programme sont en texte clair, car le pilote déchiffre de manière transparente les données récupérées à partir des colonnes SSN et BirthDate.
 
Remarque : Les requêtes peuvent effectuer des comparaisons d’égalité sur des colonnes chiffrées uniquement si le chiffrement est déterministe. Pour plus d’informations, consultez [Sélection d’un chiffrement déterministe ou aléatoire](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

SQLSRV :
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = sqlsrv_prepare($conn, $query, array(&$ssn));
// during sqlsrv_execute, the driver encrypts the ssn value and passes it to the database
sqlsrv_execute($stmt);
// fetch like usual
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV :
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
// during PDOStatement::execute, the driver encrypts the ssn value and passes it to the database
$stmt->execute();
// fetch like usual
$row = $stmt->fetch();
```

### <a name="ciphertext-data-retrieval-example"></a>Exemple d’extraction de données chiffrées

Si Always Encrypted n’est pas activé, une requête peut toujours récupérer des données à partir de colonnes chiffrées, tant qu’aucun de ses paramètres ne ciblent des colonnes chiffrées.

Les exemples suivants illustrent la récupération de données chiffrées binaires à partir de colonnes chiffrées à l’aide des pilotes SQLSRV et PDO_SQLSRV. Notez les points suivants :
 -   Étant donné qu’Always Encrypted n’est pas activé dans la chaîne de connexion, la requête retourne des valeurs SSN et BirthDate chiffrées sous la forme de tableaux d’octets (le programme convertit les valeurs en chaînes).
 -   Une requête qui récupère des données à partir de colonnes chiffrées lorsqu’Always Encrypted est désactivé peut avoir des paramètres, tant qu’aucun d’eux ne cible une colonne chiffrée. La requête suivante filtre en fonction des noms (LastName), qui ne sont pas chiffrés dans la base de données. Si la requête filtre par SSN ou BirthDate, la requête échoue.
 
SQLSRV :
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = sqlsrv_prepare($conn, $query, array(&$lastName));
sqlsrv_execute($stmt);
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV :
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $lastName);
$stmt->execute();
$row = $stmt->fetch();
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Éviter les problèmes courants lors de l’interrogation de colonnes chiffrées

Cette section décrit les catégories d’erreurs courantes liées à l’interrogation des colonnes chiffrées à partir d’applications PHP, et explique comment éviter ces erreurs.

#### <a name="unsupported-data-type-conversion-errors"></a>Erreurs liées à la conversion de types de données non pris en charge

Always Encrypted ne prend en charge que peu de conversions de types de données chiffrées. Pour obtenir la liste détaillée des conversions de types prises en charge, consultez [Always Encrypted (moteur de base de données)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Procédez comme suit pour éviter les erreurs de conversion de type de données :
 -   Lorsque vous utilisez le pilote SQLSRV avec `sqlsrv_prepare` et `sqlsrv_execute`, le type SQL ainsi que la taille de colonne et le nombre de chiffres décimaux du paramètre sont déterminés automatiquement.
 -   Lorsque vous utilisez le pilote PDO_SQLSRV pour exécuter une requête, le type SQL ainsi que la taille de colonne et le nombre de chiffres décimaux du paramètre sont également déterminés automatiquement.
 -   Lorsque vous utilisez le pilote SQLSRV avec `sqlsrv_query` pour exécuter une requête :
  -   Le type SQL du paramètre est exactement le même que le type de la colonne cible, ou la conversion du type SQL vers le type de la colonne est prise en charge.
  -   La précision et l’échelle des paramètres ciblant les colonnes des types de données SQL Server `decimal` et `numeric` sont les mêmes que celles configurées pour la colonne cible.
  -   La précision des paramètres ciblant les colonnes des types de données SQL Server `datetime2`, `datetimeoffset` ou `time` n’est pas supérieure à celle de la colonne cible dans les requêtes qui modifient les valeurs de la colonne cible.
 -   N’utilisez pas d’attributs d’instruction PDO_SQLSRV `PDO::SQLSRV_ATTR_DIRECT_QUERY` ou `PDO::ATTR_EMULATE_PREPARES` dans une requête paramétrable
 
#### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Erreurs dues au passage de texte en clair au lieu de valeurs chiffrées

Toute valeur qui cible une colonne chiffrée doit être chiffrée avant d’être envoyée au serveur. Toute tentative d’insertion, de modification ou de filtrage par une valeur en texte clair dans une colonne chiffrée entraîne une erreur. Pour éviter ces erreurs, effectuez les vérifications suivantes :
 -   Always Encrypted est activé (dans la chaîne de connexion, définissez le mot clé `ColumnEncryption` sur `Enabled`).
 -   Utilisez un paramètre de liaison pour envoyer des données ciblant des colonnes chiffrées. L’exemple suivant illustre une requête qui filtre incorrectement une colonne chiffrée (SSN) à l’aide d’un littéral ou d’une constante :
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

## <a name="controlling-performance-impact-of-always-encrypted"></a>Contrôle de l’impact d’Always Encrypted sur les performances

Always Encrypted étant une technologie de chiffrement côté client, la dégradation des performances s’observe côté client, et non dans la base de données. Outre le coût des opérations de chiffrement et de déchiffrement, les autres sources de dégradation des performances côté client sont les suivantes :
 -   Allers-retours supplémentaires vers la base de données afin de récupérer des métadonnées pour les paramètres de requête.
 -   Appels au magasin de clés principales de colonne pour accéder à une clé principale de colonne.
 
### <a name="round-trips-to-retrieve-metadata-for-query-parameters"></a>Allers-retours vers la base de données en vue de la récupération des métadonnées pour les paramètres de requête.

Si Always Encrypted est activé pour une connexion, le pilote ODBC appelle par défaut [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) pour chaque requête paramétrable, en passant l’instruction de requête (sans valeurs de paramètre) à SQL Server. Cette procédure stockée analyse l’instruction de requête afin de savoir si des paramètres doivent être chiffrés. Si c’est le cas, elle retourne pour chaque paramètre des informations relatives au chiffrement qui permettent au pilote de les chiffrer.

Étant donné que les pilotes PHP permettent à l’utilisateur de lier un paramètre dans une instruction préparée sans fournir le type SQL, lors de la liaison d’un paramètre dans une connexion Always Encrypted activée, les pilotes PHP appellent [SQLDescribeParam](../../odbc/reference/syntax/sqldescribeparam-function.md) sur le paramètre pour obtenir le type SQL, la taille de la colonne et les chiffres décimaux. Les métadonnées sont ensuite utilisées pour appeler [SQLBindParameter]( ../../odbc/reference/syntax/sqlbindparameter-function.md). Ces appels `SQLDescribeParam` supplémentaires ne nécessitent pas d’autres allers-retours à la base de données, car le pilote ODBC a déjà stocké les informations côté client lors de l’appel `sys.sp_describe_parameter_encryption`.

Les comportements précédents garantissent un haut niveau de transparence à l’application cliente (et au développeur d’applications), qui n’a pas besoin de connaître les requêtes qui accèdent à des colonnes chiffrées, tant que les valeurs ciblant des colonnes chiffrées sont passées au pilote dans les paramètres.

Contrairement à ODBC Driver pour SQL Server, l’activation d’Always Encrypted au niveau de l’instruction/requête n’est pas encore prise en charge dans les pilotes PHP. 

### <a name="column-encryption-key-caching"></a>Mise en cache des clés de chiffrement de colonne

Afin de réduire le nombre d’appels à un magasin de clés principales de colonne pour déchiffrer les clés de chiffrement de colonne (CEK), le pilote met en cache les clés CEK en clair dans la mémoire. Après avoir reçu la clé CEK (ECEK) chiffrée à partir des métadonnées de la base de données, le pilote ODBC tente d’abord de trouver la clé CEK en clair correspondant à la valeur de clé chiffrée qui se trouve dans le cache. Le pilote appelle le magasin de clés contenant la clé CMK uniquement s’il ne trouve pas la clé CEK en clair correspondante dans le cache.

Remarque : Dans ODBC Driver for SQL Server, les entrées dans le cache sont supprimées après un délai d’attente de deux heures. Ce comportement signifie que, pour une clé CEK chiffrée, le pilote ne contacte le magasin de clés qu’une seule fois au cours de la durée de vie de l’application, ou toutes les deux heures, la plus petite des deux valeurs étant retenue.

## <a name="working-with-column-master-key-stores"></a>Utilisation de magasins de clés principales de colonne

Pour chiffrer ou déchiffrer des données, le pilote doit obtenir une clé CEK configurée pour la colonne cible. Les clés CEK sont stockées sous forme chiffrée (on parle alors de clés ECEK) dans les métadonnées de la base de données. Chaque clé CEK a une clé CMK correspondante qui a été utilisée pour la chiffrer. Les [métadonnées de base de données](../../t-sql/statements/create-column-master-key-transact-sql.md) ne stockent pas la clé CMK proprement dite ; elles contiennent uniquement le nom du magasin de clés et des informations que ce dernier peut utiliser pour localiser la clé CMK.

Pour obtenir la valeur de texte en clair d’une clé ECEK, le pilote obtient d’abord les métadonnées relatives à la clé CEK et sa clé CMK correspondante, il utilise ces informations pour contacter le magasin de clés contenant la clé CMK, puis il lui demande de déchiffrer la clé ECEK. Le pilote communique avec un magasin de clés à l’aide d’un fournisseur de magasin de clés.

Pour Microsoft Driver 5.3.0 PHP pour SQL Server, seuls le fournisseur du magasin de certificats Windows et Azure Key Vault sont pris en charge. L’autre fournisseur de magasin de clés pris en charge par le pilote ODBC (fournisseur de magasin de clés personnalisé) n’est pas encore pris en charge.

### <a name="using-the-windows-certificate-store-provider"></a>Avec le fournisseur du magasin de certificats Windows

ODBC Driver for SQL Server sur Windows inclut un fournisseur de magasin de clés CMK intégré pour le Magasin de certificats Windows, nommé `MSSQL_CERTIFICATE_STORE`. (Ce fournisseur n’est pas disponible sur Mac OS ou Linux.) Avec ce fournisseur, la clé CMK est stockée localement sur l’ordinateur client, et aucune configuration supplémentaire par l’application n’est nécessaire pour l’utiliser avec le pilote. Toutefois, l’application doit avoir accès au certificat et à sa clé privée dans le magasin. Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

### <a name="using-azure-key-vault"></a>Utilisation d’Azure Key Vault

Azure Key Vault permet de stocker les clés de chiffrement, mots de passe et autres secrets à l’aide d’Azure, et peut être utilisé afin de stocker les clés pour Always Encrypted. ODBC Driver pour SQL Server (version 17 et ultérieures) inclut un fournisseur de magasin de clés principales intégré pour Azure Key Vault. Les options de connexion suivantes gèrent la configuration Azure Key Vault : `KeyStoreAuthentication`, `KeyStorePrincipalId`et `KeyStoreSecret`. 
 -   `KeyStoreAuthentication` peut prendre l’une des deux valeurs de chaîne possibles : `KeyVaultPassword` et `KeyVaultClientSecret`. Ces valeurs contrôlent le type d’informations d’identification d’authentification utilisées avec les deux autres mots clés.
 -   `KeyStorePrincipalId` prend une chaîne représentant un identificateur pour le compte qui cherche à accéder à Azure Key Vault. 
     -   Si `KeyStoreAuthentication` est définie sur `KeyVaultPassword`, `KeyStorePrincipalId` doit être le nom d’un utilisateur Azure Active Directory.
     -   Si `KeyStoreAuthentication` est définie sur `KeyVaultClientSecret`, `KeyStorePrincipalId` doit être un ID client d’application.
 -   `KeyStoreSecret` prend une chaîne représentant un secret d’information d’identification. 
     -   Si `KeyStoreAuthentication` est définie sur `KeyVaultPassword`, `KeyStoreSecret` doit être le mot de passe de l’utilisateur. 
     -   Si `KeyStoreAuthentication` est définie sur `KeyVaultClientSecret`, `KeyStoreSecret` doit être le secret d’application associé à l’ID client de l’application.

Les trois options doivent être présentes dans la chaîne de connexion pour utiliser Azure Key Vault. En outre, `ColumnEncryption` doit être définie sur `Enabled`. Si `ColumnEncryption` est définie sur `Disabled` mais que les options Azure Key Vault sont présentes, le script se poursuivra sans erreur, mais aucun chiffrement ne sera effectué.

Les exemples suivants montrent comment se connecter à SQL Server à l’aide d’Azure Key Vault.

SQLSRV :

En utilisant un compte Active Directory Azure :
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultPassword", "KeyStorePrincipalId"=>$AADUsername, "KeyStoreSecret"=>$AADPassword);
$conn = sqlsrv_connect($server, $connectionInfo);
```
En utilisant un ID client et un secret d’application Azure :
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultClientSecret", "KeyStorePrincipalId"=>$applicationClientID, "KeyStoreSecret"=>$applicationClientSecret);
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV : En utilisant un compte Active Directory Azure :
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultPassword; KeyStorePrincipalId = $AADUsername; KeyStoreSecret = $AADPassword;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```
En utilisant un ID client et un secret d’application Azure :
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultClientSecret; KeyStorePrincipalId = $applicationClientID; KeyStoreSecret = $applicationClientSecret;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

## <a name="limitations-of-the-php-drivers-when-using-always-encrypted"></a>Limitations des pilotes PHP lors de l’utilisation d’Always Encrypted

SQLSRV et PDO_SQLSRV :
 -   Linux et macOS ne prennent pas en charge le fournisseur de magasin de certificats Windows
 -   Forcer le chiffrement de paramètre
 -   Activation d’Always Encrypted au niveau de l’instruction 
 -   Lors de l’utilisation de la fonctionnalité Always Encrypted et des paramètres régionaux non-UTF8 sur Linux et macOS (notamment « en_US. ISO-8859-1 »), l’insertion de données Null ou d’une chaîne vide dans une colonne de type char(n) chiffrée risque de ne pas fonctionner si la page de codes 1252 n’a pas été installée sur votre système
 
SQLSRV uniquement :
 -   En utilisant `sqlsrv_query` pour le paramètre de liaison sans spécification du type SQL
 -   En utilisant `sqlsrv_prepare` pour les paramètres de liaison dans un lot d’instructions SQL  
 
PDO_SQLSRV uniquement :
 -   attribut d’instruction `PDO::SQLSRV_ATTR_DIRECT_QUERY` spécifié dans une requête paramétrable
 -   attribut d’instruction `PDO::ATTR_EMULATE_PREPARE` spécifié dans une requête paramétrable
 -   paramètres de liaison dans un lot d’instructions SQL
 
Les pilotes PHP héritent également des limitations imposées par ODBC Driver pour SQL Server et la base de données. Voir [Limitations du pilote ODBC lors de l’utilisation d’Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) et [Détails de la fonctionnalité Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md#feature-details).  
  
## <a name="see-also"></a>Voir aussi  
[Guide de programmation pour le pilote SQL PHP](../../connect/php/programming-guide-for-php-sql-driver.md)

[Informations de référence sur l’API du pilote SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Référence API du pilote PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  
