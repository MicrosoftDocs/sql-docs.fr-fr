---
title: Azure Active Directory
description: Découvrez comment utiliser l’authentification Azure Active Directory avec les pilotes Microsoft pour PHP pour SQL Server.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- azure active directory, authentication, access token
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae3e734dee5f8ac46de2646cca9c37a7ed7748df
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076957"
---
# <a name="connect-using-azure-active-directory-authentication"></a>Se connecter à l’aide de l’authentification Azure Active Directory
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Azure Active Directory](/azure/active-directory/active-directory-whatis) (Azure AD) est une technologie de gestion centralisée des identifiants utilisateur qui représente une solution de substitution à [l’authentification SQL Server](how-to-connect-using-sql-server-authentication.md). Elle rend possible la connexion à Microsoft Azure SQL Database et à Azure Synapse Analytics avec des identités fédérées dans Azure AD à l’aide d’un nom d’utilisateur et d’un mot de passe, de l’authentification intégrée Windows ou d’un jeton d’accès Azure AD. Les pilotes PHP pour SQL Server offrent une prise en charge partielle de ces fonctionnalités.

Pour utiliser Azure AD, employez le mot clé **Authentication** ou le mot clé **AccessToken** (mutuellement exclusifs), comme le montre le tableau suivant. Pour plus d’informations techniques, consultez [Utiliser Azure Active Directory avec le pilote ODBC](../odbc/using-azure-active-directory.md).

|Mot clé|Valeurs|Description|
|-|-|-|
|**AccessToken**|Non défini (par défaut)|Mode d’authentification déterminé par les autres mots clés. Pour plus d’informations, consultez [Connection Options](connection-options.md). |
||Chaîne d’octets|Jeton d’accès Azure AD extrait d’une réponse JSON OAuth. La chaîne de connexion ne doit pas contenir l’identifiant utilisateur, le mot de passe ou le mot clé Authentication (version 17 ou ultérieure du pilote ODBC requise sur Linux ou macOS). |
|**Authentification**|Non défini (par défaut)|Mode d’authentification déterminé par les autres mots clés. Pour plus d’informations, consultez [Connection Options](connection-options.md). |
||`SqlPassword`|Authentification directe auprès d’une instance SQL Server (qui peut être une instance Azure) à l’aide d’un nom d’utilisateur et d’un mot de passe, qui doivent être transmis dans chaîne de connexion avec les mots clés **UID** et **PWD**. |
||`ActiveDirectoryPassword`|Authentification avec une identité Azure Active Directory à l’aide d’un nom d’utilisateur et d’un mot de passe, qui doivent être transmis dans chaîne de connexion avec les mots clés **UID** et **PWD**. |
||`ActiveDirectoryMsi`|Authentification avec une identité managée affectée par le système ou par l’utilisateur (version 17.3.1.1 ou ultérieure du pilote ODBC requise). Pour obtenir une vue d’ensemble et des tutoriels, consultez [À quoi correspondent les identités managées pour les ressources Azure ?](/azure/active-directory/managed-identities-azure-resources/overview).|
||`ActiveDirectoryServicePrincipal`|Authentification en utilisant des objets de principal de service (nécessite le pilote ODBC version 17.7 ou ultérieure). Pour obtenir plus d’informations et des exemples, consultez [Objets application et de principal de service dans Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals).|

Le mot clé **Authentication** affecte les paramètres de sécurité de la connexion. S’il est défini dans la chaîne de connexion, le mot clé **Encrypt** a alors par défaut la valeur true, ce qui signifie que le client demandera un chiffrement. Par ailleurs, le certificat de serveur sera validé indépendamment du paramètre de chiffrement, sauf si **TrustServerCertificate** a la valeur true (**false** par défaut). Cette fonctionnalité est à distinguer de l’ancienne méthode de connexion, moins sécurisée, selon laquelle le certificat de serveur n’était validé que si le chiffrement était spécifiquement demandé dans la chaîne de connexion.

#### <a name="limitations"></a>Limites

Sur Windows, le pilote ODBC sous-jacent prend en charge une valeur supplémentaire pour le mot clé **Authentication**, **ActiveDirectoryIntegrated**, mais les pilotes PHP ne la gèrent sur aucune plateforme.

## <a name="example---connect-using-sqlpassword-and-activedirectorypassword"></a>Exemple : Connexion avec SqlPassword et ActiveDirectoryPassword

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = array("UID"=>$myusername, "PWD"=>$mypassword, "Authentication"=>'SqlPassword');

$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=SqlPassword.\n";
    sqlsrv_close($conn);
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = array("Database"=>$azureDatabase,
                        "UID"=>$azureUsername,
                        "PWD"=>$azurePassword,
                        "Authentication"=>'ActiveDirectoryPassword');

$conn = sqlsrv_connect($azureServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    sqlsrv_close($conn);
}

?>
```

## <a name="example---connect-using-the-pdo_sqlsrv-driver"></a>Exemple : Connexion avec le pilote PDO_SQLSRV

```php
<?php
// First connect to a local SQL Server instance by setting Authentication to SqlPassword
$serverName = "myserver.mydomain";

$connectionInfo = "Database = $databaseName; Authentication = SqlPassword;";

try {
    $conn = new PDO("sqlsrv:server = $serverName ; $connectionInfo", $myusername, $mypassword);
    echo "Connected successfully with Authentication=SqlPassword.\n";
    $conn = null;
} catch (PDOException $e) {
    echo "Could not connect with Authentication=SqlPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}

// Now connect to an Azure SQL database by setting Authentication to ActiveDirectoryPassword
$azureServer = "myazureserver.database.windows.net";
$azureDatabase = "myazuredatabase";
$azureUsername = "myuid";
$azurePassword = "mypassword";
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryPassword;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer ; $connectionInfo", $azureUsername, $azurePassword);
    echo "Connected successfully with Authentication=ActiveDirectoryPassword.\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryPassword.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-azure-ad-access-token"></a>Exemple : Connexion avec un jeton d’accès Azure AD

### <a name="sqlsrv-driver"></a>Pilote SQLSRV

```php
<?php
// Using an access token to connect: do not use UID or PWD connection options
// Assume $accToken is the valid byte string extracted from an OAuth JSON response
$connectionInfo = array("Database"=>$azureAdDatabase, "AccessToken"=>$accToken);
$conn = sqlsrv_connect($azureAdServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Azure AD Access Token.\n";
    sqlsrv_close($conn);
}
?>
```

### <a name="pdo_sqlsrv-driver"></a>Pilote PDO_SQLSRV

```php
<?php
try {
    // Using an access token to connect: do not pass in $uid or $pwd
    // Assume $accToken is the valid byte string extracted from an OAuth JSON response
    $connectionInfo = "Database = $azureAdDatabase; AccessToken = $accToken;";
    $conn = new PDO("sqlsrv:server = $azureAdServer; $connectionInfo");
    echo "Connected successfully with Azure AD Access Token\n";
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Azure AD Access Token.\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-managed-identities-for-azure-resources"></a>Exemple : Connexion avec les identités managées pour les ressources Azure

### <a name="using-the-system-assigned-managed-identity-with-sqlsrv-driver"></a>Utiliser l’identité managée affectée par le système avec le pilote SQLSRV

Si vous vous connectez avec l’identité managée affectée par le système, n’utilisez pas les options UID et PWD.

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$connectionInfo = array('Database'=>$azureDatabase,
                        'Authentication'=>'ActiveDirectoryMsi');
$conn = sqlsrv_connect($azureServer, $connectionInfo);

if ($conn === false) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    print_r(sqlsrv_errors());
} else {
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (system-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    $stmt = sqlsrv_query($conn, $tsql);
    if ($stmt === false) {
        echo "Failed to run the simple query (system-assigned).\n";
        print_r(sqlsrv_errors());
    } else {
        while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
            echo $row['SQL_VERSION'] . PHP_EOL;
        }

        sqlsrv_free_stmt($stmt);
    }
    
    sqlsrv_close($conn);
}
?>
```

### <a name="using-the-user-assigned-managed-identity-with-pdo_sqlsrv-driver"></a>Utiliser l’identité managée affectée par l’utilisateur avec le pilote PDO_SQLSRV

Une identité managée affectée par l’utilisateur est créée en tant que ressource Azure autonome. Azure crée une identité dans le tenant Azure AD approuvé par l’abonnement utilisé. Une fois l’identité créée, elle peut être affectée à une ou plusieurs instances de service Azure. Copiez la valeur `Object ID` de cette identité et définissez-la comme nom d’utilisateur dans la chaîne de connexion. 

Ainsi, lorsque vous vous connecterez avec l’identité managée affectée par l’utilisateur, vous fournirez l’ID objet comme nom d’utilisateur, mais non le mot de passe.

```php
<?php

$azureServer = 'myazureserver.database.windows.net';
$azureDatabase = 'myazuredatabase';
$azureUser = '2d68f56e-9547-4dae-aee8-f3g28ab9674x';    // Object ID of the identity
$connectionInfo = "Database = $azureDatabase; Authentication = ActiveDirectoryMsi;";

try {
    $conn = new PDO("sqlsrv:server = $azureServer; $connectionInfo", $azureUser);
    echo "Connected successfully with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    
    $tsql = "SELECT @@Version AS SQL_VERSION";
    
    try {
        $stmt = $conn->query($tsql);
        $result = $stmt->fetchall(PDO::FETCH_ASSOC);
        print_r($result);

        unset($stmt);
    } catch (PDOException $e) {
        echo "Failed to run the simple query (user-assigned).\n";
    }
    unset($conn);
} catch (PDOException $e) {
    echo "Could not connect with Authentication=ActiveDirectoryMsi (user-assigned).\n";
    print_r($e->getMessage());
    echo "\n";
}
?>
```

## <a name="example---connect-using-service-principal-objects-in-azure-active-directory"></a>Exemple - Se connecter en utilisant des objets de principal de service dans Azure Active Directory

Pour s’authentifier en utilisant un objet de principal de service, vous avez besoin de l’[ID client de l’application](/azure/active-directory/develop/howto-create-service-principal-portal#get-tenant-and-app-id-values-for-signing-in) correspondante et du [secret client](/azure/active-directory/develop/howto-create-service-principal-portal#option-2-create-a-new-application-secret).


### <a name="sqlsrv-driver"></a>Pilote SQLSRV

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
$connectionInfo = array("Database"=>$adDatabase, 
                        "Authentication"=>"ActiveDirectoryServicePrincipal",
                        "UID"=>$adSPClientId,
                        "PWD"=>$adSPClientSecret);

$conn = sqlsrv_connect($adServer, $connectionInfo);
if ($conn === false) {
    echo "Could not connect using Azure AD Service Principal." . PHP_EOL;
    print_r(sqlsrv_errors());
}

sqlsrv_close($conn);

?>
```

### <a name="pdo_sqlsrv-driver"></a>Pilote PDO_SQLSRV

```php
<?php

$adServer = 'myazureserver.database.windows.net';
$adDatabase = 'myazuredatabase';
$adSPClientId = 'myAppClientId';
$adSPClientSecret = 'myClientSecret';

$conn = false;
try {
    $connectionInfo = "Database = $adDatabase; Authentication = ActiveDirectoryServicePrincipal;";
    $conn = new PDO("sqlsrv:server = $adServer; $connectionInfo", $adSPClientId, $adSPClientSecret);
} catch (PDOException $e) {
    echo "Could not connect using Azure AD Service Principal.\n";
    print_r($e->getMessage());
    echo PHP_EOL;
}

unset($conn);
?>
```


## <a name="see-also"></a>Voir aussi
[Utilisation d’Azure Active Directory avec ODBC Driver](../odbc/using-azure-active-directory.md)

[Que sont les identités managées pour les ressources Azure ?](/azure/active-directory/managed-identities-azure-resources/overview)