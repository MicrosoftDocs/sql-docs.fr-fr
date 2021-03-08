---
title: Dépendances de fonctionnalité du pilote Microsoft JDBC
description: Découvrez les dépendances du pilote Microsoft JDBC pour SQL Server et comment les satisfaire.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 939a8773-2583-49a4-bf00-6b892fbe39dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 61542eafadc26a96c01204472c6fd78997787f15
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836956"
---
# <a name="feature-dependencies-of-the-microsoft-jdbc-driver-for-sql-server"></a>Dépendances de fonctionnalité de Microsoft JDBC Driver pour SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cet article répertorie les versions dont dépend le pilote JDBC Microsoft pour SQL Server. Le projet comporte les dépendances suivantes.

## <a name="compile-time"></a>Durée de compilation

 - `com.microsoft.azure:azure-keyvault` : fournisseur Azure Key Vault pour la fonctionnalité Azure Key Vault Always Encrypted. (facultatif)
 - `com.microsoft.azure:azure-identity` : bibliothèque Azure Active Directory pour Java pour les fonctionnalités d’authentification Azure Active Directory et Azure Key Vault. (facultatif)
 - `org.antlr:antlr4-runtime`: fonctionnalité ANTLR 4 Runtime pour useFmtOnly. (facultatif)
 - `org.osgi:org.osgi.core`: bibliothèque OSGi Core pour la prise en charge d’OSGi Framework.
 - `org.osgi:org.osgi.compendium`: bibliothèque OSGi Compendium pour la prise en charge d’OSGi Framework.
 - `com.google.code.gson`: analyseur JSON pour la fonctionnalité Always Encrypted avec enclaves sécurisées. (facultatif)
 - `org.bouncycastle.bcprov-jdk15on`: fournisseur Bouncy Castle pour la fonctionnalité Always Encrypted avec enclaves sécurisées (utilisation de Java 8 uniquement). (facultatif)

## <a name="test-time"></a>Durée du test

Les projets spécifiques qui nécessitent une de ces fonctionnalités doivent déclarer explicitement les dépendances respectifs dans leur fichier POM.

**Par exemple :** lorsque vous utilisez la fonctionnalité d’authentification Azure Active Directory, vous devez redéclarer la dépendance `adal4j` dans le fichier POM du projet. Consultez l’extrait de code suivant :

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
    <scope>compile</scope>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.6.5</version>
</dependency>

<dependency>
    <groupId>com.microsoft.rest</groupId>
    <artifactId>client-runtime</artifactId>
    <version>1.7.4</version>
</dependency>
```

**Par exemple :** lorsque vous utilisez la fonctionnalité Azure Key Vault, vous devez redéclarer la dépendance `azure-keyvault` et la dépendance `adal4j` dans le fichier POM du projet. Consultez l’extrait de code suivant :

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
    <scope>compile</scope>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.6.5</version>
</dependency>

<dependency>
    <groupId>com.microsoft.rest</groupId>
    <artifactId>client-runtime</artifactId>
    <version>1.7.4</version>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.2.4</version>
</dependency>
```

## <a name="dependency-requirements-for-the-jdbc-driver"></a>Exigences relatives à la dépendance pour le pilote JDBC

### <a name="working-with-the-azure-key-vault-provider"></a>Utilisation du fournisseur Azure Key Vault :

- Pilote JDBC version 9.2.1 - Versions des dépendances : Azure-keyvault(version 4.2.1) et Azure-identity(version 1.1.3) et leurs dépendances ([exemple d’application](azure-key-vault-sample-version-9.2.md))
- Version 8.4.1 d JDBC Driver - Versions des dépendances : Azure-Keyvault (version 1.2.4), Adal4j (version 1.6.5), Client-Runtime-for-AutoRest (1.7.4) et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-7.0.md))
- Version 8.2.2 du pilote JDBC - Versions des dépendances : Azure-Keyvault (version 1.2.2), Adal4j (version 1.6.4), Client-Runtime-for-AutoRest (1.7.0) et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-7.0.md))
- Version 7.4.1 du pilote JDBC - versions des dépendances : Azure-Keyvault (version 1.2.1), Adal4j (version 1.6.4), Client-Runtime-for-AutoRest (1.6.10) et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-7.0.md))
- Version 7.2.2 du pilote JDBC - versions des dépendances : Azure-Keyvault (version 1.2.0), Azure-Keyvault-Webkey (version 1.2.0), Adal4j (version 1.6.3), Client-Runtime-for-AutoRest (1.6.5). et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-7.0.md))
- Version 7.0.0 du pilote JDBC - versions des dépendances : Azure-Keyvault (version 1.0.0), Adal4j (version 1.6.0) et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-7.0.md))
- Version 6.4.0 du pilote JDBC - versions des dépendances : Azure-Keyvault (version 1.0.0), Adal4j (version 1.4.0) et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-6.2.2.md))
- Version 6.2.2 du pilote JDBC - versions des dépendances : Azure-Keyvault (version 1.0.0), Adal4j (version 1.4.0) et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-6.2.2.md))
- Version 6.0.0 du pilote JDBC - versions des dépendances : Azure-Keyvault (version 0.9.7), Adal4j (version 1.3.0) et leurs dépendances ([exemple d'application](azure-key-vault-sample-version-6.0.0.md))

> [!NOTE]
> Avec les versions de pilote 6.2.2 et 6.4.0, la dépendance azure-keyvault-java avait été mise à jour vers la version 1.0.0. Toutefois, la nouvelle version n’était pas compatible avec la version précédente (0.9.7) et arrête l’implémentation existante dans le pilote. La nouvelle implémentation dans le pilote a requis des modifications de l’API qui, à son tour, arrête les programmes clients qui utilisent le fournisseur Azure Key Vault.
>
> Ce problème est résolu avec les dernières versions du pilote (7.0.0 et versions ultérieures). Le constructeur supprimé qui a utilisé le mécanisme de rappel d’authentification est à nouveau ajouté au fournisseur Azure Key Vault Provider pour garantir la compatibilité descendante.

### <a name="working-with-azure-active-directory-authentication"></a>Utilisation de l'authentification Azure Active Directory :

- Pilote JDBC version 9.2.1 - Versions des dépendances : Azure-identity(version 1.1.3) et ses dépendances.
- Version 8.4.1 JDBC Driver - Versions des dépendances : Adal4j (version 1.6.5), Client-Runtime-for-AutoRest (1.7.4) et leurs dépendances.
- Version 8.2.2 du pilote JDBC - Versions des dépendances : Adal4j (version 1.6.4), Client-Runtime-for-AutoRest (1.7.0) et leurs dépendances. Dans cette version du pilote, « sqljdbc_auth.dll » a été renommé par « mssql-jdbc_auth-\<version>-\<arch>.dll ».
- Version 7.4.1 du pilote JDBC - versions des dépendances : Adal4j (version 1.6.4), Client-Runtime-pour-AutoRest (1.6.10) et leurs dépendances
- Version 7.2.2 du pilote JDBC - versions des dépendances : Adal4j (version 1.6.3), Client-Runtime-for-AutoRest (1.6.5) et leurs dépendances
- Version 7.0.0 du pilote JDBC - versions des dépendances : Adal4j (version 1.6.0) et ses dépendances
- Version 6.4.0 du pilote JDBC - versions des dépendances : Adal4j (version 1.4.0) et ses dépendances
- Version 6.2.2 du pilote JDBC - versions des dépendances : Adal4j (version 1.4.0) et ses dépendances
- Version 6.0.0 du pilote JDBC - versions des dépendances : Adal4j (version 1.3.0) et ses dépendances. Dans cette version du pilote, vous ne pouvez vous connecter à l’aide du mode d’authentification _ActiveDirectoryIntegrated_ que sur un système d’exploitation Windows et à l’aide de sqljdbc_auth.dll et d’Active Directory Authentication Library pour SQL Server (ADALSQL.DLL).

À partir de la version 6.4.0 du pilote, les applications ne requièrent pas nécessairement l’utilisation d’ADALSQL.DLL sur les systèmes d’exploitation Windows. Pour les *systèmes d’exploitation autres que Windows*, le pilote a besoin d’un ticket Kerberos pour utiliser l’authentification ActiveDirectoryIntegrated. Pour plus d’informations sur la façon de se connecter à Active Directory à l’aide de Kerberos, consultez [Définir un ticket Kerberos sur Windows, Linux et macOS](connecting-using-azure-active-directory-authentication.md#set-kerberos-ticket-on-windows-linux-and-macos).

Pour les *systèmes d’exploitation Windows*, le pilote recherche le fichier sqljdbc_auth.dll par défaut et ne requiert pas l’installation d’un ticket Kerberos ou de dépendances de la bibliothèque Azure. Si sqljdbc_auth.dll n’est pas disponible, le pilote recherche le ticket Kerberos pour l’authentification auprès d’Active Directory comme sur d’autres systèmes d’exploitation.

À partir de la version 8.2.2 du pilote, « sqljdbc_auth.dll » se nomme « mssql-jdbc_auth-\<version>-\<arch>.dll ». Par exemple, « mssql-jdbc_auth-8.2.2.x64.dll ».

En plus du **mssql-jdbc_auth-\<version>-\<arch>.dll** (disponible dans le package de pilotes JDBC), la bibliothèque d’authentification Azure Active Directory (**ADAL.DLL**) doit également être installée pour l’authentification intégrée Active Directory. La bibliothèque ADAL peut être installée à partir de [Microsoft ODBC Driver for SQL Server](../odbc/download-odbc-driver-for-sql-server.md) ou de [Microsoft OLE DB Driver pour SQL Server](../oledb/download-oledb-driver-for-sql-server.md). Le pilote JDBC ne prend en charge que les versions **1.0.2028.318 et ultérieures** d’ADAL.DLL.


Vous pouvez obtenir un [exemple d’application](connecting-using-azure-active-directory-authentication.md) qui utilise cette fonctionnalité.

## <a name="see-also"></a>Voir aussi

[Référentiel GitHub du pilote JDBC](https://github.com/microsoft/mssql-jdbc)  
[Informations de référence sur l’API du pilote JDBC](reference/jdbc-driver-api-reference.md)
