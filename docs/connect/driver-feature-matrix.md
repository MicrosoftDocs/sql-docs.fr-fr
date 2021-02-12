---
title: Matrice de prise en charge des fonctionnalités des pilotes
description: Découvrez les fonctionnalités courantes prises en charge dans les pilotes pour SQL Server et où trouver des informations à leur sujet.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-daenge
ms.openlocfilehash: 6e6abaf1e8538b32480c794ce26a98b4ec9220af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042433"
---
# <a name="driver-feature-support-matrix-for-microsoft-sql-server"></a>Matrice de prise en charge des fonctionnalités des pilotes pour Microsoft SQL Server

Si vous envisagez d’utiliser une fonctionnalité dans Microsoft SQL Server, sachez qu’elle ne sera pas nécessairement disponible dans tous les pilotes pour différentes raisons, notamment :

- La fonctionnalité ne s’applique pas à la technologie du pilote.
- La fonctionnalité est nouvelle et n’a pas encore été implémentée sur tous les pilotes.
- La fonctionnalité n’est pas demandée dans le pilote.
- D’autres fonctionnalités sont implémentées en premier.

Nous aimerions que tous les pilotes prennent en charge l’ensemble des fonctionnalités et nous efforçons de garantir la parité des fonctionnalités entre les pilotes. Toutefois, cela n’est pas toujours possible. Pour vous aider à choisir le pilote adapté à vos besoins, voici la liste des fonctionnalités courantes et des pilotes qui les implémentent.

- [.NET](#table1)
- [ODBC](#table2)
- [OLE DB](#table2)
- [JDBC](#table2)
- [PHP](#table3)
- [Node.js/JavaScript](#table3)
- [Python](#table3)

| <a id="table1"></a>Fonctionnalité | [Microsoft.<wbr>Data.<wbr>SqlClient (.NET Core)](ado-net/microsoft-ado-net-sql-server.md) | [Microsoft.<wbr>Data.<wbr>SqlClient (.NET Framework)](ado-net/microsoft-ado-net-sql-server.md) | System.<wbr>Data.<wbr>SqlClient (.NET Core) | [System.<wbr>Data.<wbr>SqlClient (.NET Framework)](/dotnet/framework/data/adonet/sql/) |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [Oui](ado-net/sql/sqlclient-support-always-encrypted.md) | [Oui](ado-net/sql/sqlclient-support-always-encrypted.md) | | [Oui](ado-net/sql/sqlclient-support-always-encrypted.md) |
| [Always Encrypted avec enclaves sécurisées](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [Oui](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) | [Oui](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) | | [Oui](ado-net/sql/sqlclient-support-always-encrypted.md#enabling-always-encrypted-with-secure-enclaves) |
| [Authentification par jeton d’accès Azure Active Directory](/azure/active-directory/develop/access-tokens) | [Oui](/dotnet/api/system.data.sqlclient.sqlconnection.accesstoken) | [Oui](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) | [Oui](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) | [Oui](/dotnet/api/microsoft.data.sqlclient.sqlconnection.accesstoken) |
| [Authentification par mot de passe Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | | Oui |
| [Authentification intégrée Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | | Oui |
| [Authentification interactive (MFA) Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | | Oui |
| [Authentification Managed Identity Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | | |
| [Authentification par principal de service Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | [Oui](ado-net/sql/azure-active-directory-authentication.md) | | |
| [Authentification Windows intégrée](/windows-server/security/windows-authentication/windows-authentication-overview) | [Oui](ado-net/sql/authentication-sql-server.md) | [Oui](ado-net/sql/authentication-sql-server.md) | [Oui](/dotnet/framework/data/adonet/sql/authentication-in-sql-server) | [Oui](/dotnet/framework/data/adonet/sql/authentication-in-sql-server) |
| [Copie en bloc](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | [Oui](ado-net/sql/bulk-copy-operations-sql-server.md) | [Oui](ado-net/sql/bulk-copy-operations-sql-server.md) | [Oui](/dotnet/framework/data/adonet/sql/bulk-copy-operations-in-sql-server) | [Oui](/dotnet/framework/data/adonet/sql/bulk-copy-operations-in-sql-server) |
| [Métadonnées de niveau de confidentialité et de classification des données](../relational-databases/security/sql-data-discovery-and-classification.md) | [Oui](ado-net/sql/data-classification.md) | [Oui](ado-net/sql/data-classification.md) | Oui | Oui |
| [MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Oui](ado-net/sql/multiple-active-result-sets-mars.md) | [Oui](ado-net/sql/multiple-active-result-sets-mars.md) | [Oui](/dotnet/framework/data/adonet/sql/multiple-active-result-sets-mars) | [Oui](/dotnet/framework/data/adonet/sql/multiple-active-result-sets-mars) |
| [Types de données spatiales](../relational-databases/spatial/spatial-data-sql-server.md) | | Oui | | Oui |
| [Paramètres table](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | [Oui](ado-net/sql/table-valued-parameters.md) | [Oui](ado-net/sql/table-valued-parameters.md) | [Oui](/dotnet/framework/data/adonet/sql/table-valued-parameters) | [Oui](/dotnet/framework/data/adonet/sql/table-valued-parameters) |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Oui](ado-net/sql/sqlclient-support-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Oui](ado-net/sql/sqlclient-support-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Oui](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.multisubnetfailover?view=netcore-1.0&preserve-view=true) | [Oui](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.multisubnetfailover?view=netframework-4.8&preserve-view=true) |
| [Résolution transparente d’adresses IP réseau](odbc/using-transparent-network-ip-resolution.md) | | [Oui](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring?view=sqlclient-dotnet-1.1&preserve-view=true) | | [Oui](/dotnet/api/system.data.sqlclient.sqlconnection.connectionstring?view=netframework-4.8&preserve-view=true) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

| <a id="table2"></a>Fonctionnalité | [Microsoft ODBC Driver for SQL Server sur Windows](odbc/microsoft-odbc-driver-for-sql-server.md) | [Microsoft ODBC Driver for SQL Server sur Linux et macOS](odbc/microsoft-odbc-driver-for-sql-server.md) | [JDBC Driver pour SQL Server](jdbc/microsoft-jdbc-driver-for-sql-server.md) | [OLE DB Driver pour SQL Server](oledb/oledb-driver-for-sql-server.md) |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [Oui](odbc/using-always-encrypted-with-the-odbc-driver.md) | [Oui](odbc/using-always-encrypted-with-the-odbc-driver.md) | [Oui](jdbc/using-always-encrypted-with-the-jdbc-driver.md) |
| [Always Encrypted avec enclaves sécurisées](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [Oui](odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves) | [Oui](odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves) | [Oui](jdbc/using-always-encrypted-with-the-jdbc-driver.md) | |
| [Authentification par jeton d’accès Azure Active Directory](/azure/active-directory/develop/access-tokens) | [Oui](odbc/using-azure-active-directory.md#authenticating-with-an-access-token) | [Oui](odbc/using-azure-active-directory.md#authenticating-with-an-access-token) | [Oui](jdbc/connecting-using-azure-active-directory-authentication.md#connecting-using-access-token) | [Oui](oledb/features/using-azure-active-directory.md) |
| [Authentification par mot de passe Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) |  [Oui](odbc/using-azure-active-directory.md) | [Oui](odbc/using-azure-active-directory.md) | [Oui](jdbc/connecting-using-azure-active-directory-authentication.md) | [Oui](oledb/features/using-azure-active-directory.md) |
| [Authentification intégrée Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Oui](odbc/using-azure-active-directory.md) | [Oui](odbc/using-azure-active-directory.md) | [Oui](jdbc/connecting-using-azure-active-directory-authentication.md) | [Oui](oledb/features/using-azure-active-directory.md) |
| [Authentification interactive (MFA) Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Oui](odbc/using-azure-active-directory.md) | | [Oui](jdbc/connecting-using-azure-active-directory-authentication.md) | [Oui](oledb/features/using-azure-active-directory.md) |
| [Authentification Managed Identity Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) | [Oui](odbc/using-azure-active-directory.md) | [Oui](odbc/using-azure-active-directory.md) | [Oui](jdbc/connecting-using-azure-active-directory-authentication.md) | [Oui](oledb/features/using-azure-active-directory.md) |
| [Authentification par principal de service Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals) | [Oui](odbc/using-azure-active-directory.md) | [Oui](odbc/using-azure-active-directory.md) | [Oui](jdbc/connecting-using-azure-active-directory-authentication.md) | [Oui](oledb/features/using-azure-active-directory.md) |
| [Authentification Windows intégrée](/windows-server/security/windows-authentication/windows-authentication-overview) | Oui | [Oui](odbc/linux-mac/using-integrated-authentication.md) | [Oui](jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md) | Oui |
| [Copie en bloc](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | [Oui](../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md) | [Oui](../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md) | [Oui](jdbc/using-bulk-copy-with-the-jdbc-driver.md) | [Oui](oledb/features/performing-bulk-copy-operations.md) |
| [Métadonnées de recherche et de classification des données](../relational-databases/security/sql-data-discovery-and-classification.md) | [Oui](odbc/data-classification.md) | [Oui](odbc/data-classification.md) | [Oui](jdbc/data-discovery-classification-sample.md) | |
| [MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Oui](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Oui](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | | [Oui](oledb/features/using-multiple-active-result-sets-mars.md) |
| [Types de données spatiales](../relational-databases/spatial/spatial-data-sql-server.md) | | | [Oui](jdbc/use-spatial-datatypes.md) | |
| [Paramètres table](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | [Oui](../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md) | [Oui](../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md) | [Oui](jdbc/using-table-valued-parameters.md) | [Oui](oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md) |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Oui](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Oui](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Oui](jdbc/jdbc-driver-support-for-high-availability-disaster-recovery.md) | [Oui](oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) |
| [Résolution transparente d’adresses IP réseau](odbc/using-transparent-network-ip-resolution.md) | [Oui](odbc/using-transparent-network-ip-resolution.md) | [Oui](odbc/using-transparent-network-ip-resolution.md) | [Oui](jdbc/setting-the-connection-properties.md) | [Oui](oledb/features/using-transparent-network-ip-resolution.md) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

| <a id="table3"></a>Fonctionnalité | [Pilotes pour PHP pour SQL Server sur Windows](php/microsoft-php-driver-for-sql-server.md)<sup>[1](#note1)</sup> | [Pilotes pour PHP pour SQL Server sur Linux et macOS](php/microsoft-php-driver-for-sql-server.md)<sup>[1](#note1)</sup> | [Tedious (Node.js)](node-js/node-js-driver-for-sql-server.md) | [pyODBC (Python)](python/pyodbc/python-sql-driver-pyodbc.md)<sup>[1](#note1)</sup> |
| :-- | :-- | :-- | :-- | :-- |
| [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) | [Oui](php/using-always-encrypted-php-drivers.md) | [Oui](php/using-always-encrypted-php-drivers.md) | | Oui |
| [Always Encrypted avec enclaves sécurisées](../relational-databases/security/encryption/always-encrypted-enclaves.md) | [Oui](php/always-encrypted-secure-enclaves.md) | [Oui](php/always-encrypted-secure-enclaves.md) | | Oui |
| [Authentification par jeton d’accès Azure Active Directory](/azure/active-directory/develop/access-tokens) | [Oui](php/azure-active-directory.md) | [Oui](php/azure-active-directory.md) | [Oui](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | Oui |
| [Authentification par mot de passe Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Oui](php/azure-active-directory.md) | [Oui](php/azure-active-directory.md) | [Oui](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | Oui |
| [Authentification intégrée Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | [Oui](php/azure-active-directory.md) | [Oui](php/azure-active-directory.md) | | Oui |
| [Authentification interactive (MFA) Azure Active Directory](/azure/sql-database/sql-database-aad-authentication) | | | | Oui<sup>[2](#note2)</sup> |
| [Authentification Managed Identity Azure Active Directory](/azure/active-directory/managed-identities-azure-resources/overview) | [Oui](php/azure-active-directory.md) | [Oui](php/azure-active-directory.md) | [Oui](https://tediousjs.github.io/tedious/api-connection.html#function_newConnection) | Oui |
| [Authentification par principal de service Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals) | | | | |
| [Authentification Windows intégrée](/windows-server/security/windows-authentication/windows-authentication-overview) | [Oui](php/how-to-connect-using-windows-authentication.md) | [Oui](odbc/linux-mac/using-integrated-authentication.md) | | Oui |
| [Copie en bloc](../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md) | | | [Oui](https://tediousjs.github.io/tedious/bulk-load.html) | |
| [Métadonnées de recherche et de classification des données](../relational-databases/security/sql-data-discovery-and-classification.md) | Oui | Oui | | |
| [MARS (Multiple Active Result Sets)](../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md) | [Oui](php/how-to-disable-multiple-active-resultsets-mars.md) | [Oui](php/how-to-disable-multiple-active-resultsets-mars.md) | | Oui |
| [Types de données spatiales](../relational-databases/spatial/spatial-data-sql-server.md) | | | | |
| [Paramètres table](../relational-databases/tables/use-table-valued-parameters-database-engine.md) | | | [Oui](https://tediousjs.github.io/tedious/parameters.html) | Oui |
| [MultiSubnetFailover](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) | [Oui](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | [Oui](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | | [Oui](../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md#connecting-with-multisubnetfailover) |
| [Résolution transparente d’adresses IP réseau](odbc/using-transparent-network-ip-resolution.md) | [Oui](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | [Oui](php/php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md) | | [Oui](odbc/using-transparent-network-ip-resolution.md) |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<a id="note1"></a><sup>1</sup> Étant donné que ces pilotes s’appuient sur Microsoft ODBC Driver for SQL Server, il est nécessaire d’utiliser également une version de ce pilote qui prend en charge la fonctionnalité.

<a id="note2"></a><sup>2</sup> Uniquement sur Windows.

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
