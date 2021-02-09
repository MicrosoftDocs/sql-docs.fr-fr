---
title: Connexion avec l'authentification Azure Active Directory
description: Découvrez comment développer des applications Java qui utilisent la fonctionnalité d’authentification Azure Active Directory avec le pilote JDBC Microsoft pour SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9c9d97be-de1d-412f-901d-5d9860c3df8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cbcf01d51ef4abe344b66529d72476a7d1385bbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168729"
---
# <a name="connecting-using-azure-active-directory-authentication"></a>Connexion avec l'authentification Azure Active Directory

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Cet article donne des informations sur le développement d’applications Java pour utiliser la fonctionnalité d’authentification Azure Active Directory avec le pilote JDBC Microsoft pour SQL Server.

Vous pouvez utiliser l’authentification Azure Active Directory (Azure AD), qui est un mécanisme de connexion à Azure SQL Database v12 à l’aide d’identités dans Azure Active Directory. Utilisez l’authentification Azure Active Directory pour gérer de manière centralisée les identités des utilisateurs de base de données et comme alternative à l’authentification SQL Server. JDBC Driver vous permet de spécifier vos informations d’identification Azure Active Directory dans la chaîne de connexion JDBC pour vous connecter à Azure SQL Database. Pour savoir comment configurer l’authentification Azure Active Directory, consultez [Se connecter à une base de données SQL avec l’authentification Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview). 

Les propriétés de connexion pour la prise en charge de l’authentification Azure Active Directory dans le pilote JDBC Microsoft pour SQL Server sont les suivantes :
*   **authentication** :  utilisez cette propriété pour indiquer la méthode d’authentification SQL à utiliser pour la connexion. Les valeurs possibles sont les suivantes : 
    * **ActiveDirectoryMSI**
        * Prise en charge depuis la version **7.2** du pilote, l’authentification `authentication=ActiveDirectoryMSI` peut être utilisée pour se connecter à une instance Azure SQL Database/Synapse Analytics à partir d’une ressource Azure où la prise en charge de l’« identité » est activée. L’authentification **msiClientId** peut également être spécifiée dans les propriétés Connection/DataSource avec ce mode d’authentification, qui doit contenir l’ID client d’une identité managée à utiliser pour acquérir **accessToken** qui permettra d’établir la connexion.
    * **ActiveDirectoryIntegrated**
        * Prise en charge depuis la version **6.0** du pilote, l’authentification `authentication=ActiveDirectoryIntegrated` peut être utilisée pour se connecter à une instance Azure SQL Database/Synapse Analytics en utilisant l’authentification intégrée. Pour utiliser ce mode d’authentification, vous devez fédérer les services de fédération Active Directory (AD FS) locaux avec Azure Active Directory dans le cloud. Une fois la configuration terminée, vous pouvez vous connecter en ajoutant la bibliothèque Native « mssql-jdbc_auth-\<version>-\<arch>.dll » au chemin de la classe d’application sur le système d’exploitation Windows ou en configurant un ticket Kerberos pour le support de l’authentification multiplateforme. Vous pourrez accéder à Azure SQL Database/Azure Synapse Analytics sans être invité à entrer des informations d’identification lorsque vous vous connecterez à un ordinateur joint au domaine.

    * **ActiveDirectoryPassword**
        * Prise en charge depuis la version **6.0** du pilote, `authentication=ActiveDirectoryPassword` peut être utilisée pour se connecter à Azure SQL Database/Synapse Analytics en utilisant un nom de principal et un mot de passe Azure AD.
    * **ActiveDirectoryInteractive**
        * Prise en charge depuis la version **9.2** du pilote, `authentication=ActiveDirectoryInteractive` peut être utilisée pour se connecter à Azure SQL Database/Synapse Analytics en utilisant un flux d’authentification interactif (authentification multifacteur).
    * **ActiveDirectoryServicePrincipal**
        * Prise en charge depuis la version **9.2** du pilote, `authentication=ActiveDirectoryServicePrincipal` peut être utilisée pour se connecter à Azure SQL Database/Synapse Analytics en utilisant l’ID et le secret client d’une identité de principal de service.

    * **SqlPassword**
        * Utilisez `authentication=SqlPassword` pour vous connecter à un serveur SQL Server avec les propriétés userName/user et password.
    * **NotSpecified**
        * Utilisez l’authentification `authentication=NotSpecified` ou conservez-la comme authentification par défaut si aucune de ces méthodes d’authentification n’est nécessaire.

*   **AccessToken** : Utilisez cette propriété de connexion pour vous connecter à une base de données SQL avec un jeton d’accès. accessToken peut uniquement être défini à l’aide du paramètre Propriétés de la méthode getConnection() dans la classe DriverManager. Il ne peut pas être utilisé dans l’URL de connexion.  

Pour plus d’informations, consultez la propriété d’authentification à la page [Définition des propriétés de connexion](setting-the-connection-properties.md).  


## <a name="client-setup-requirements"></a>Configuration d'installation client requise
Pour l’authentification **ActiveDirectoryMSI**, les composants ci-dessous doivent être installés sur l’ordinateur client :
* Java 8 ou version ultérieure
* Pilote Microsoft JDBC 7.2 (ou version ultérieure) pour SQL Server
* L’environnement client doit être une ressource Azure et la prise en charge de la fonctionnalité « Identité » doit être activée.
* Un utilisateur de base de données autonome représentant l’identité managée affectée par le système ou l’identité managée affectée par l’utilisateur de votre ressource Azure ou l’un des groupes auquel appartient votre identité managée, doit exister dans la base de données cible et disposer de l’autorisation CONNECT.

Pour d’autres modes d’authentification, les composants ci-dessous doivent être installés sur l’ordinateur client :
* Java 7 ou version ultérieure
* Pilote Microsoft JDBC 6.0 (ou version ultérieure) pour SQL Server
* Si vous utilisez le mode d’authentification par jeton d’accès, vous avez besoin de [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) et de ses dépendances pour exécuter les exemples de cet article. Pour plus d’informations, consultez [Connexion avec un jeton d’accès](#connecting-using-access-token).
* Si vous utilisez le mode d’authentification **ActiveDirectoryPassword**, vous avez besoin de [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) et de ses dépendances. Pour plus d’informations, consultez la section [Connexion à l’aide du mode d’authentification ActiveDirectoryPassword](#connecting-using-activedirectorypassword-authentication-mode).
* Si vous utilisez le mode **ActiveDirectoryIntegrated**, vous avez besoin de [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) et de ses dépendances. Pour plus d’informations, consultez [Connexion à l’aide du mode d’authentification ActiveDirectoryIntegrated](#connecting-using-activedirectoryintegrated-authentication-mode).
* Si vous utilisez le mode **ActiveDirectoryInteractive**, vous avez besoin de [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) et de ses dépendances. Pour plus d’informations, consultez la section [Connexion à l’aide du mode d’authentification ActiveDirectoryInteractive](#connecting-using-activedirectoryinteractive-authentication-mode).
* Si vous utilisez le mode **ActiveDirectoryServicePrincipal**, vous avez besoin de [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) et de ses dépendances. Pour plus d’informations, consultez la section [Connexion à l’aide du mode d’authentification ActiveDirectoryServicePrincipal]()#connecting-using-activedirectoryserviceprincipal-authentication-mode section.


## <a name="connecting-using-activedirectorymsi-authentication-mode"></a>Connexion à l’aide du mode d’authentification ActiveDirectoryMSI
L’exemple suivant montre comment utiliser le mode `authentication=ActiveDirectoryMSI`. Exécutez cet exemple à partir d’une ressource Azure, par ex. une machine virtuelle Azure, App Service ou une application de fonction fédérée avec Azure Active Directory.

Avant d’exécuter l’exemple, remplacez le nom du serveur/de la base de données par le nom de votre serveur/base de données dans les lignes suivantes :

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
//Optional
ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used
```

Exemple d’utilisation du mode d’authentification ActiveDirectoryMSI :

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AAD_MSI {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryMSI");
        // Optional
        ds.setMSIClientId("94de34e9-8e8c-470a-96df-08110924b814"); // Replace with Client ID of User-Assigned Managed Identity to be used

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

L’exécution de cet exemple sur une machine virtuelle Azure récupère un jeton d’accès à partir de l’_identité managée affectée par le système_ ou de l’_identité managée affectée par l'utilisateur_ (si **msiClientId** est spécifié) et établit une connexion à l’aide du jeton d’accès extrait. Si une connexion est établie, le message suivant doit s’afficher :

```bash
You have successfully logged on as: <your Managed Identity username>
```

## <a name="connecting-using-activedirectoryintegrated-authentication-mode"></a>Connexion à l’aide du mode d’authentification ActiveDirectoryIntegrated
Avec la version 6,4, le pilote JDBC Microsoft ajoute la prise en charge de l’authentification ActiveDirectoryIntegrated à l’aide d’un ticket Kerberos sur plusieurs plateformes (Windows, Linux et macOS).
Pour plus d’informations, consultez [Définir le ticket Kerberos sur Windows, Linux et macOS](#set-kerberos-ticket-on-windows-linux-and-macos). Sinon, sur Windows, mssql-jdbc_auth-\<version>-\<arch>.dll peut également être utilisé pour l’authentification ActiveDirectoryIntegrated avec JDBC Driver.

> [!NOTE]
>  Si vous utilisez une version antérieure du pilote, activez ce [lien](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md) pour les dépendances respectives requises pour utiliser ce mode d’authentification. 

L’exemple suivant montre comment utiliser le mode `authentication=ActiveDirectoryIntegrated`. Exécutez cet exemple sur un ordinateur joint au domaine qui est fédéré avec Azure Active Directory. Un utilisateur de base de données autonome représentant votre utilisateur Azure AD, ou l’un des groupes dont vous faites partie, doit exister dans la base de données et doit disposer de l’autorisation CONNECT. 

Avant de générer et d’exécuter l’exemple, sur l’ordinateur client (sur lequel vous souhaitez exécuter l’exemple), téléchargez la bibliothèque [Azure-ActiveDirectory-Library-for-Java](https://github.com/AzureAD/azure-activedirectory-library-for-java) et ses dépendances, et incluez-les dans le chemin de la build Java

Avant d’exécuter l’exemple, remplacez le nom du serveur/de la base de données par le nom de votre serveur/base de données dans les lignes suivantes :

```java
ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
ds.setDatabaseName("demo"); // replace with your database name
```

Exemple d’utilisation du mode d’authentification ActiveDirectoryIntegrated :
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADIntegrated {
    public static void main(String[] args) throws Exception {

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database name
        ds.setAuthentication("ActiveDirectoryIntegrated");

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```

L’exécution de cet exemple sur un ordinateur client utilise automatiquement votre ticket Kerberos et aucun mot de passe n’est requis. Si une connexion est établie, le message suivant doit s’afficher :

```
You have successfully logged on as: <your domain user name>
```

### <a name="set-kerberos-ticket-on-windows-linux-and-macos"></a>Définir le ticket Kerberos sur Windows, Linux et macOS

Vous devez configurer un ticket Kerberos qui lie votre utilisateur actuel à un compte de domaine Windows. Vous trouverez ci-dessous un résumé des étapes clés.

#### <a name="windows"></a>Windows
JDK est fourni avec `kinit`, que vous pouvez utiliser pour obtenir un TGT à partir d’un centre de distribution de clés sur un ordinateur joint à un domaine fédéré avec Azure Active Directory.

##### <a name="step-1-ticket-granting-ticket-retrieval"></a>Étape 1 : Tester la récupération d’accords de ticket
- **Exécuter sur** :  Windows
- **Action** :
  - Utilisez la commande `kinit username@DOMAIN.COMPANY.COM` pour obtenir un TGT du KDC. Vous serez alors invité à entrer votre mot de passe de domaine.
  - Utilisez `klist` pour afficher les tickets disponibles. Si l’exécution de kinit a réussi, vous devez voir un ticket de krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.

> [!NOTE]
>  Vous devrez peut-être spécifier un fichier `.ini` avec `-Djava.security.krb5.conf` pour que votre application localise KDC.

#### <a name="linux-and-macos"></a>Linux et macOS

##### <a name="requirements"></a>Spécifications
Accédez à un ordinateur Windows joint à un domaine pour interroger votre contrôleur de domaine Kerberos.

##### <a name="step-1-find-kerberos-kdc"></a>Étape 1 : Rechercher Kerberos KDC
- **Exécuter sur** : Ligne de commande Windows
- **Action** : `nltest /dsgetdc:DOMAIN.COMPANY.COM` (où « DOMAIN.COMPANY.COM » correspond au nom de votre domaine)
- **Exemple de sortie**
  ```
  DC: \\co1-red-dc-33.domain.company.com
  Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
  ...
  The command completed successfully
  ```
- **Informations à extraire** Nom du contrôleur de domaine, dans ce cas `co1-red-dc-33.domain.company.com`

##### <a name="step-2-configuring-kdc-in-krb5conf"></a>Étape 2 : configurer KDC dans krb5.conf
- **Exécuter sur** : Linux/macOS
- **Action** : Modifiez /etc/krb5.conf dans l’éditeur de votre choix. Configurez les clés suivantes
  ```
  [libdefaults]
    default_realm = DOMAIN.COMPANY.COM
   
  [realms]
  DOMAIN.COMPANY.COM = {
     kdc = co1-red-dc-28.domain.company.com
  }
  ```
  Enregistrez ensuite le fichier krb5.conf et quittez

> [!NOTE]
>  Le domaine doit être en MAJUSCULES.

##### <a name="step-3-testing-the-ticket-granting-ticket-retrieval"></a>Étape 3 : tester la récupération d’accords de ticket
- **Exécuter sur** : Linux/macOS
- **Action** :
  - Utilisez la commande `kinit username@DOMAIN.COMPANY.COM` pour obtenir un TGT du KDC. Vous serez alors invité à entrer votre mot de passe de domaine.
  - Utilisez `klist` pour afficher les tickets disponibles. Si l’exécution de kinit a réussi, vous devez voir un ticket de krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.

## <a name="connecting-using-activedirectorypassword-authentication-mode"></a>Connexion à l’aide du mode d’authentification ActiveDirectoryPassword
L’exemple suivant montre comment utiliser le mode `authentication=ActiveDirectoryPassword`.

Avant de créer et d’exécuter l’exemple :
1.  Sur l’ordinateur client (sur lequel vous souhaitez exécuter l’exemple), téléchargez la [bibliothèque azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) et ses dépendances et incluez-les dans le chemin du build Java
2.  Localisez les lignes de code suivantes et remplacez le nom du serveur/de la base de données par le nom de votre serveur/base de données.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Localisez les lignes de code suivantes et remplacez le nom d’utilisateur par le nom de l’utilisateur Azure AD auquel vous souhaitez vous connecter.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ds.setPassword("password");     // replace with your password
    ```

Exemple d’utilisation du mode d’authentification ActiveDirectoryPassword :
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADUserPassword {
    
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        ds.setPassword("password"); // Replace with your password
        ds.setAuthentication("ActiveDirectoryPassword");
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Si une connexion est établie, le message de sortie suivant doit s’afficher :
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Une base de données utilisateur autonome doit exister, et un utilisateur de base de données autonome représentant l’utilisateur Azure AD spécifié ou l’un des groupes spécifiés dont l’utilisateur Azure AD fait partie doit exister dans la base de données et doit disposer de l’autorisation CONNECT (excepté pour l’administrateur de serveur ou de groupe Azure Active Directory).

## <a name="connecting-using-activedirectoryinteractive-authentication-mode"></a>Connexion à l’aide du mode d’authentification ActiveDirectoryInteractive 
L’exemple suivant montre comment utiliser le mode `authentication=ActiveDirectoryInteractive`.

Avant de créer et d’exécuter l’exemple :
1.  Sur l’ordinateur client (sur lequel vous souhaitez exécuter l’exemple), téléchargez la [bibliothèque azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) et ses dépendances et incluez-les dans le chemin du build Java
2.  Localisez les lignes de code suivantes et remplacez le nom du serveur/de la base de données par le nom de votre serveur/base de données.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Localisez les lignes de code suivantes et remplacez le nom d’utilisateur par le nom de l’utilisateur Azure AD auquel vous souhaitez vous connecter.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

Exemple d’utilisation du mode d’authentification ActiveDirectoryInteractive :
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADInteractive {
    public static void main(String[] args) throws Exception{
        
        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryInteractive");
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Lors de l’exécution du programme, un navigateur s’affiche pour authentifier l’utilisateur. Ce que voit exactement l’utilisateur dépend de la configuration d’Azure AD. Cela peut ou non inclure des invites d’authentification multifacteur demandant un nom d’utilisateur, un mot de passe, un code PIN ou une deuxième authentification d’appareil, par exemple via un téléphone. Si plusieurs demandes d’authentification interactives sont effectuées dans le même programme, les demandes suivantes peuvent même ne rien demander à l’utilisateur si la bibliothèque d’authentification peut réutiliser un jeton d’authentification précédemment mis en cache.

Pour plus d'informations sur la configuration d'Azure AD afin d'exiger une authentification multifacteur, consultez [Bien démarrer avec Azure AD Multi-Factor Authentication dans le cloud](/azure/active-directory/authentication/howto-mfa-getstarted).

Pour consulter les captures d’écran de ces boîtes de dialogue, consultez [Configure multi-factor authentication for SQL Server Management Studio and Azure AD](/azure/azure-sql/database/authentication-mfa-ssms-configure) (Configurer l’authentification multifacteur pour SQL Server Management Studio et Azure AD).

Si l’authentification de l’utilisateur réussit, vous devez voir le message suivant dans le navigateur :
```
Authentication complete. You can close the browser and return to the application.
```
Notez que cela indique seulement la réussite de l’authentification de l’utilisateur, mais pas nécessairement la réussite de la connexion au serveur. Lors du retour à l’application, si la connexion au serveur est établie, vous devez voir le message suivant :
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Une base de données utilisateur autonome doit exister, et un utilisateur de base de données autonome représentant l’utilisateur Azure AD spécifié ou l’un des groupes spécifiés dont l’utilisateur Azure AD fait partie doit exister dans la base de données, et doit disposer de l’autorisation CONNECT (excepté pour l’administrateur du serveur ou le groupe Azure Active Directory).

## <a name="connecting-using-activedirectoryserviceprincipal-authentication-mode"></a>Connexion en utilisant le mode d’authentification ActiveDirectoryServicePrincipal
L’exemple suivant montre comment utiliser le mode `authentication=ActiveDirectoryServicePrincipal`.

Avant de créer et d’exécuter l’exemple :
1.  Sur l’ordinateur client (sur lequel vous souhaitez exécuter l’exemple), téléchargez la [bibliothèque azure-activedirectory-library-for-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) et ses dépendances et incluez-les dans le chemin du build Java
2.  Localisez les lignes de code suivantes et remplacez le nom du serveur/de la base de données par le nom de votre serveur/base de données.
    ```java
    ds.setServerName("aad-managed-demo.database.windows.net"); // replace 'aad-managed-demo' with your server name
    ds.setDatabaseName("demo"); // replace with your database name
    ```
3.  Localisez les lignes de code suivantes et remplacez le nom d’utilisateur par le nom de l’utilisateur Azure AD auquel vous souhaitez vous connecter.
    ```java
    ds.setUser("bob@cqclinic.onmicrosoft.com"); // replace with your user name
    ```

Exemple d’utilisation du mode d’authentification ActiveDirectoryInteractive :
```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

public class AADServicePrincipal {
    public static void main(String[] args) throws Exception{
        String principalId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your AAD secure principal ID.
        String principalSecret = "..."; // Replace with your AAD principal secret.

        SQLServerDataSource ds = new SQLServerDataSource();
        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name
        ds.setDatabaseName("demo"); // Replace with your database
    ds.setAuthentication("ActiveDirectoryServicePrincipal");
    ds.setAADSecurePrincipalId(principalId);
    ds.setAADSecurePrincipalSecret(principalSecret);
      
    // Optional
        ds.setUser("bob@cqclinic.onmicrosoft.com"); // Replace with your user name
        
        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
```
Si une connexion est établie, le message de sortie suivant doit s’afficher :
```
You have successfully logged on as: <your user name>
```

> [!NOTE]  
> Une base de données utilisateur autonome doit exister, et un utilisateur de base de données autonome représentant l’utilisateur Azure AD spécifié ou l’un des groupes spécifiés dont l’utilisateur Azure AD fait partie doit exister dans la base de données, et doit disposer de l’autorisation CONNECT (excepté pour l’administrateur du serveur ou le groupe Azure Active Directory).

## <a name="connecting-using-access-token"></a>Connexion à l’aide du jeton d’accès
Les applications/services peuvent récupérer un jeton d’accès auprès d’Azure Active Directory et l’utiliser pour se connecter à Azure SQL Database/Synapse Analytics.

> [!NOTE] 
> **accessToken** peut uniquement être défini à l’aide du paramètre Propriétés de la méthode getConnection() dans la classe DriverManager. Il ne peut pas être utilisé dans la chaîne de connexion.

L’exemple ci-dessous contient une application Java simple qui se connecte à Azure SQL Database/Synapse Analytics en utilisant l’authentification basée sur les jetons d’accès. Avant de créer et d’exécuter l’exemple, procédez comme suit :
1.  Créez un compte d’application dans Azure Active Directory pour votre service.
    1. Connectez-vous au portail Azure.
    2. Cliquez sur Azure Active Directory dans le volet de navigation gauche.
    3. Cliquez sur l’onglet « Inscriptions d’applications ».
    4. Dans le tiroir, cliquez sur « Nouvelle inscription d’application ».
    5. Entrez mytokentest comme nom convivial pour l’application, sélectionnez « Application web/API ».
    6. Nous n’avons pas besoin de SIGN ON URL. Il vous suffit de fournir tout ce qui suit : « https://mytokentest  ».
    7. Cliquez sur « Créer » en bas du volet.
    9. Toujours dans le portail Azure, cliquez sur l’onglet « Paramètres » de votre application, puis ouvrez l’onglet « Propriétés ».
    10. Recherchez la valeur « ID de l’application » (ou ID client) et copiez-la, car vous en aurez besoin plus tard pour configurer votre application (par exemple, 1846943b-ad04-4808-aa13-4702d908b5c1). Voir la capture instantanée suivante.
    11. Sous la section « Clés », créez une clé en renseignant le champ du nom, en sélectionnant la durée de la clé et en enregistrant la configuration (laissez le champ de la valeur vide). Après l’enregistrement, le champ de la valeur doit être renseigné automatiquement, copiez la valeur générée. Il s’agit du secret du client.
    12. Cliquez sur Azure Active Directory dans le volet de gauche. Sous « Inscriptions des applications », recherchez l’onglet « Points de terminaison ». Copiez l’URL sous « POINT DE TERMINAISON DE JETON OATH 2.0 ». Il s’agit de votre URL STS.
    
    ![Point de terminaison d’inscription de l’application Portail Azure - URL STS](media/jdbc_aad_token.png)  
2. Connectez-vous à votre base de données utilisateur SQL Server Azure en tant qu’administrateur Azure Active Directory et, à l’aide d’un approvisionnement de commande T-SQL, approvisionnez un utilisateur de base de données autonome pour votre principal d’application. Pour plus d’informations sur la création d’un administrateur Azure Active Directory et d’un utilisateur de base de données autonome, consultez [Connexion à SQL Database ou à Azure Synapse Analytics à l’aide de l’authentification Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).

    ```
    CREATE USER [mytokentest] FROM EXTERNAL PROVIDER
    ```

3.  Sur la machine cliente (sur laquelle vous voulez exécuter l’exemple), téléchargez la bibliothèque [microsoft-authentication-library-for-java](https://github.com/AzureAD/microsoft-authentication-library-for-java) et ses dépendances, et incluez-les dans le chemin de build Java. Notez que la bibliothèque microsoft-authentication-library-for-java est nécessaire seulement pour exécuter cet exemple spécifique. L’exemple utilise les API de cette bibliothèque pour récupérer le jeton d’accès à partir d’Azure AD. Si vous disposez déjà d’un jeton d’accès, vous pouvez ignorer cette étape. Notez que vous devez également supprimer la section contenant l’exemple de récupération du jeton d’accès.

Dans l’exemple suivant, remplacez l’URL STS, l’ID client, la clé secrète client, le nom du serveur et de la base de données par vos propres valeurs.

```java
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import com.microsoft.sqlserver.jdbc.SQLServerDataSource;

// The azure-activedirectory-library-for-java is needed to retrieve the access token from the AD.
import com.microsoft.aad.msal4j.ClientCredentialFactory;
import com.microsoft.aad.msal4j.ClientCredentialParameters;
import com.microsoft.aad.msal4j.ConfidentialClientApplication;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.IClientCredential;

public class AADTokenBased {

    public static void main(String[] args) throws Exception {

        // Retrieve the access token from the AD.
        String spn = "https://database.windows.net/";
        String stsurl = "https://login.microsoftonline.com/..."; // Replace with your STS URL.
        String clientId = "1846943b-ad04-4808-aa13-4702d908b5c1"; // Replace with your client ID.
        String clientSecret = "..."; // Replace with your client secret.

    String scope = spn +  "/.default";
    Set<String> scopes = new HashSet<>();
        scopes.add(scope);
    
    ExecutorService executorService = Executors.newSingleThreadExecutor();
    IClientCredential credential = ClientCredentialFactory.createFromSecret(clientSecret);
    ConfidentialClientApplication clientApplication = ConfidentialClientApplication
            .builder(clientId, credential).executorService(executorService).authority(stsurl).build();
    CompletableFuture<IAuthenticationResult> future = clientApplication
            .acquireToken(ClientCredentialParameters.builder(scopes).build());
            
    IAuthenticationResult authenticationResult = future.get();
    String accessToken = authenticationResult.accessToken();

        System.out.println("Access Token: " + accessToken);

        // Connect with the access token.
        SQLServerDataSource ds = new SQLServerDataSource();

        ds.setServerName("aad-managed-demo.database.windows.net"); // Replace with your server name.
        ds.setDatabaseName("demo"); // Replace with your database name.
        ds.setAccessToken(accessToken);

        try (Connection connection = ds.getConnection(); 
                Statement stmt = connection.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT SUSER_SNAME()")) {
            if (rs.next()) {
                System.out.println("You have successfully logged on as: " + rs.getString(1));
            }
        }
    }
}
``` 

Si la connexion peut être établie, le message de sortie suivant doit s’afficher :

```bash
Access Token: <your access token>
You have successfully logged on as: <your client ID>    
```
