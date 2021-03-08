---
title: Utilisation de l’authentification Azure Active Directory avec SqlClient
description: Décrit comment utiliser les modes d’authentification Azure Active Directory pris en charge pour se connecter à des sources de données Azure SQL avec SqlClient
ms.date: 11/20/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: karinazhou
ms.author: v-jizho2
ms.reviewer: v-daenge
ms.openlocfilehash: c57c2d10854ed902a6230eafc3a912cd0508c989
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835986"
---
# <a name="using-azure-active-directory-authentication-with-sqlclient"></a>Utilisation de l’authentification Azure Active Directory avec SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

Cet article explique comment se connecter à des sources de données Azure SQL à l’aide de l’authentification Azure Active Directory (Azure AD) à partir d’une application .NET avec SqlClient.

L’authentification Azure AD utilise des identités dans Azure AD pour accéder aux sources de données Azure SQL, telles qu’Azure SQL Database, Azure SQL Managed Instance et Azure Synapse Analytics. L’espace de noms **Microsoft.Data.SqlClient** permet aux applications clientes de spécifier des informations d’identification Azure AD dans différents modes d’authentification lorsqu’elles se connectent à Azure SQL Database.

Lorsque vous définissez la propriété de connexion `Authentication` dans la chaîne de connexion, le client peut choisir un mode d’authentification Azure AD favori en fonction de la valeur fournie :

- La version la plus ancienne **Microsoft.Data.SqlClient** prend en charge `Active Directory Password` pour .NET Framework, .NET Core et .NET Standard. Elle prend également en charge l’authentification `Active Directory Integrated` et l’authentification `Active Directory Interactive` pour .NET Framework. 

- À compter de **Microsoft.Data.SqlClient** 2.0.0, le support de l’authentification `Active Directory Integrated` et de l’authentification `Active Directory Interactive` a été étendu sur .NET Framework, .NET Core et .NET Standard. 

  Un nouveau mode d’authentification `Active Directory Service Principal` est également ajouté à SqlClient 2.0.0. Il utilise l’ID client et le secret d’une identité de principal du service pour effectuer l’authentification. 

- D’autres modes d’authentification sont ajoutés dans **Microsoft.Data.SqlClient** 2.1.0, y compris `Active Directory Device Code Flow` et `Active Directory Managed Identity` (également appelé `Active Directory MSI`). Ces nouveaux modes permettent à l’application d’acquérir un jeton d’accès pour se connecter au serveur. 

Pour plus d’informations sur l’authentification Azure AD au-delà de ce que décrivent les sections suivantes, consultez [Connexion à SQL Database à l’aide de l’authentification Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview).

## <a name="setting-azure-active-directory-authentication"></a>Définition de l’authentification Azure Active Directory

Lorsque l’application se connecte à des sources de données Azure SQL à l’aide de l’authentification Azure AD, elle doit fournir un mode d’authentification valide. La table suivante répertorie les modes d’authentification pris en charge. L’application spécifie un mode à l’aide de la propriété de connexion `Authentication` dans la chaîne de connexion.

| Value | Description  | Infrastructure    | Version Microsoft.Data.SqlClient |
|:--|:--|:--|:--:|
| Mot de passe Active Directory | S’authentifier avec une identité Azure AD à l’aide d’un nom d’utilisateur et d’un mot de passe | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+  | 1.0+|
| Intégration Active Directory |S’authentifier avec une identité Azure AD à l’aide de l’authentification intégrée. | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Azure Directory interactif | S’authentifier avec une identité Azure AD à l’aide de l’authentification interactive | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+<sup>1</sup> |
| Principal de service Azure Directory | S’authentifier avec une identité d’Azure AD à l’aide de l’ID client et du secret d’une identité de principal du service | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.0.0+ |
| Flux de code de l’appareil Azure Directory | S’authentifier avec une identité Azure AD à l’aide du mode de flux de code de l’appareil | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.1.0+ |
| Identité managée Active Directory, <br>MSI Active Directory | S’authentifier avec une identité Azure AD à l’aide d’une identité managée attribuée par le système ou par l’utilisateur | .NET Framework 4.6+, .NET Core 2.1+, .NET Standard 2.0+ | 2.1.0+ |

<sup>1</sup> Avant **Microsoft.Data.SqlClient** 2.0.0, les modes d’authentification `Active Directory Integrated` et `Active Directory Interactive` ne sont pris en charge que sur .NET Framework 4.6+.

## <a name="using-active-directory-password-authentication"></a>Utilisation de l’authentification par mot de passe Azure Directory

Le mode d’authentification `Active Directory Password` prend en charge l’authentification auprès des sources de données Azure avec Azure AD pour les utilisateurs Azure AD natifs ou fédérés. Lorsque vous utilisez ce mode, les informations d’identification de l’utilisateur doivent être fournies dans la chaîne de connexion. L'exemple suivant indique comment utiliser l'authentification `Active Directory Password`.

```c#
// Use your own server, database, user ID, and password.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Password; Database=testdb; User Id=user@domain.com; Password=***";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-integrated-authentication"></a>Utilisation de l’authentification intégrée Azure Directory

Pour utiliser le mode d’authentification `Active Directory Integrated`, vous devez fédérer l’instance Active Directory locale avec Azure AD dans le cloud. Vous pouvez effectuer une fédération en utilisant des services de fédération Active Directory (AD FS), par exemple.

Lorsque vous êtes connecté à un ordinateur joint à un domaine, vous pouvez accéder aux sources de données Azure SQL sans être invité à entrer des informations d’identification avec ce mode. Vous ne pouvez pas spécifier le nom d’utilisateur ni le mot de passe dans la chaîne de connexion pour les applications .NET Framework. Le nom d'utilisateur est facultatif dans la chaîne de connexion pour les applications .NET Core et .NET Standard. Vous ne pouvez pas définir la propriété `Credential` de SqlConnection dans ce mode. 

L’extrait de code suivant est un exemple de l’utilisation en cours de l’authentification `Active Directory Integrated`.

```c#
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is optional for .NET Core and .NET Standard.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Integrated; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-interactive-authentication"></a>Utilisation de l’authentification interactive Azure Directory

L’authentification `Active Directory Interactive` prend en charge la technologie d’authentification multifacteur pour se connecter aux sources de données Azure SQL. Si vous fournissez ce mode d’authentification dans la chaîne de connexion, un écran d’authentification Azure s’affiche et demande à l’utilisateur d’entrer des informations d’identification valides. Vous pouvez indiquer le mot de passe dans la chaîne de connexion. 

Vous ne pouvez pas définir la propriété `Credential` de SqlConnection dans ce mode. Avec **Microsoft.Data.SqlClient** 2.0.0 et versions ultérieures, le nom d’utilisateur est autorisé dans la chaîne de connexion lorsque vous êtes en mode interactif. 

L'exemple suivant indique comment utiliser l'authentification `Active Directory Interactive`.

```c#
// Use your own server, database, and user ID.
// User ID is optional.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb; User Id=user@domain.com";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

// User ID is not provided.
string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory Interactive; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="using-active-directory-service-principal-authentication"></a>Utilisation de l’authentification par principal de service Azure Active Directory

En mode d’authentification `Active Directory Service Principal`, l’application cliente peut se connecter aux sources de données Azure SQL en fournissant l’ID client et le secret d’une identité de principal du service. L’authentification du principal de service implique :
1. Configuration d’une inscription d’application avec un secret.
1. Octroi d’autorisations à l’application dans l’instance Azure SQL Database.
1. Connexion avec les informations d’identification correctes. 

L'exemple suivant indique comment utiliser l'authentification `Active Directory Service Principal`.

```c#
// Use your own server, database, app ID, and secret.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Service Principal; Database=testdb; User Id=AppId; Password=secret";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-device-code-flow-authentication"></a>Utilisation de l’authentification du flux de code de l’appareil Active Directory

Avec la [bibliothèque d’authentification Microsoft](/azure/active-directory/develop/msal-overview) pour .NET (MSAL.NET), l’authentification `Active Directory Device Code Flow` permet à l’application cliente de se connecter aux sources de données Azure SQL à partir d’appareils et de systèmes d’exploitation qui ne disposent pas d’un navigateur Web interactif. L’authentification interactive sera effectuée sur un autre appareil. Pour plus d’informations sur l’authentification du flux de code de l’appareil, consultez [Flux de code de l’appareil OAuth 2.0](/azure/active-directory/develop/v2-oauth2-device-code). 

Lorsque ce mode est en cours d’utilisation, vous ne pouvez pas définir la propriété `Credential` de `SqlConnection`. En outre, le nom d’utilisateur et le mot de passe ne doivent pas être spécifiés dans la chaîne de connexion. 

L’extrait de code suivant est un exemple de l’utilisation de l’authentification `Active Directory Device Code Flow`.

```c#
// Use your own server and database.
string ConnectionString = @"Server=demo.database.windows.net; Authentication=Active Directory Device Code Flow; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString)) {
    conn.Open();
}
```


## <a name="using-active-directory-managed-identity-authentication"></a>Utilisation de l’authentification de l’identité managée Active Directory

*Identités managées* pour les ressources Azure est le nouveau nom du service anciennement nommé Managed Service Identity (MSI). Lorsqu’une application cliente utilise une ressource Azure pour accéder à un service Azure qui prend en charge l’authentification Azure AD, vous pouvez utiliser des identités managées pour l’authentification en fournissant une identité pour la ressource Azure dans Azure AD. Vous pouvez ensuite utiliser cette identité pour obtenir des jetons d’accès. Cette méthode d’authentification peut éliminer la nécessité de gérer les informations d’identification et les secrets.

Il existe deux types d’identités administrées :

- Une _identité managée attribué par le système_ est créée sur une instance de service dans Azure AD. Elle est liée au cycle de vie de cette instance de service. 
- Une _identité managée attribuée par l'utilisateur_ est créée en tant que ressource Azure autonome. Elle peut être attribuée à une ou plusieurs instances d’un service Azure. 

Pour plus d’informations sur les identités managées, consultez [À propos des identités managées pour les ressources Azure](/azure/active-directory/managed-identities-azure-resources/overview).

Depuis **Microsoft.Data.SqlClient** 2.1.0, le pilote prend en charge l’authentification pour Azure SQL Database, Azure Synapse Analytics et Azure SQL Managed Instance en acquérant des jetons d’accès par le biais d’une identité managée. Pour utiliser cette authentification, spécifiez `Active Directory Managed Identity` ou `Active Directory MSI` dans la chaîne de connexion et aucun mot de passe n’est requis. 

Vous ne pouvez pas définir la propriété `Credential` de `SqlConnection` dans ce mode. Pour une identité managée attribuée par l’utilisateur, le nom d'utilisateur doit être fourni. 

L’exemple suivant montre comment utiliser l’authentification `Active Directory Managed Identity` avec une identité gérée attribuée par le système.

```c#
// For system-assigned managed identity
// Use your own server and database.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```

L’exemple suivant décrit l’authentification `Active Directory Managed Identity` avec une identité managée attribuée par l’utilisateur.

```c#
// For user-assigned managed identity
// Use your own server, database, and user ID.
string ConnectionString1 = @"Server=demo.database.windows.net; Authentication=Active Directory Managed Identity; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString1)) {
    conn.Open();
}

string ConnectionString2 = @"Server=demo.database.windows.net; Authentication=Active Directory MSI; User Id=ObjectIdOfManagedIdentity; Database=testdb";

using (SqlConnection conn = new SqlConnection(ConnectionString2)) {
    conn.Open();
}
```


## <a name="customizing-active-directory-authentication"></a>Personnalisation de l’authentification Azure Directory

Outre l’utilisation de l’authentification Active Directory intégrée au pilote, **Microsoft.Data.SqlClient** 2.1.0 et versions ultérieures fournissent aux applications la possibilité de personnaliser l’authentification Active Directory. La personnalisation est basée sur la classe `ActiveDirectoryAuthenticationProvider`, qui est dérivée de la classe abstraite [`SqlAuthenticationProvider`](/dotnet/api/system.data.sqlclient.sqlauthenticationprovider). 

Pendant l’authentification Active Directory, l’application cliente peut définir sa propre classe `ActiveDirectoryAuthencationProvider` par l’une des deux opérations suivantes :

- Utilisation d’une méthode de rappel personnalisée.
- Transmission d’un ID client d’application vers la bibliothèque MSAL via le pilote SqlClient pour l’extraction des jetons d’accès.

L’exemple suivant montre comment utiliser un rappel personnalisé lorsque l’authentification `Active Directory Device Code Flow` est en cours d’utilisation. 

[!code-csharp [AADAuthenticationCustomDeviceFlowCallback#1](~/../sqlclient/doc/samples/AADAuthenticationCustomDeviceFlowCallback.cs#1)]

Avec une classe de `ActiveDirectoryAuthenticationProvider` personnalisée, un ID client d’application définie par l’utilisateur peut être transmis à SqlClient lorsqu’un mode d’authentification Active Directory pris en charge est en cours d’utilisation. Les modes d’authentification Active Directory pris en charge comprennent `Active Directory Password`, `Active Directory Integrated`, `Active Directory Interactive`, `Active Directory Service Principal` et `Active Directory Device Code Flow`.

L’ID client de l’application est également configurable via `SqlAuthenticationProviderConfigurationSection` ou `SqlClientAuthenticationProviderConfigurationSection`. La propriété de configuration `applicationClientId` s’applique à .NET Framework 4.6+ et .NET Core 2.1+.

L’extrait de code suivant est un exemple d’utilisation d’une classe de `ActiveDirectoryAuthenticationProvider` personnalisée avec un ID client d’application définie par l’utilisateur lorsque l’authentification `Active Directory Interactive` est en cours d’utilisation.

[!code-csharp [ApplicationClientIdAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/ApplicationClientIdAzureAuthenticationProvider.cs#1)]

L’exemple suivant montre comment configurer un ID client d’application à l’aide d’une section de configuration.

```xml
<configuration>
  <configSections>
    <section name="SqlClientAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>

<!--or-->

<configuration>
  <configSections>
    <section name="SqlAuthenticationProviders"
             type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
  </configSections>
  <SqlAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>
```

## <a name="support-for-a-custom-sql-authentication-provider"></a>Support d’un fournisseur d’authentification SQL personnalisé

En raison d’une plus grande flexibilité, l’application cliente peut également utiliser son propre fournisseur pour l’authentification Active Directory au lieu d’utiliser la classe `ActiveDirectoryAuthenticationProvider`. Le fournisseur d’authentification personnalisé doit être une sous-classe de `SqlAuthenticationProvider` avec des méthodes substituées. 

L’exemple suivant montre comment utiliser un nouveau fournisseur d’authentification pour l’authentification `Active Directory Device Code Flow`.

[!code-csharp [CustomDeviceCodeFlowAzureAuthenticationProvider#1](~/../sqlclient/doc/samples/CustomDeviceCodeFlowAzureAuthenticationProvider.cs#1)]

Outre l’amélioration de l’expérience d’authentification `Active Directory Interactive`, **Microsoft.Data.SqlClient** 2.1.0 et versions ultérieures fournissent les API suivantes pour les applications clientes afin de personnaliser l’authentification interactive et l’authentification du flux de code de l’appareil.

```c#
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    // Sets a reference to the current System.Windows.Forms.IWin32Window that triggers the browser to be shown. 
    // Used to center the browser pop-up onto this window.
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    // Sets a reference to the ViewController (if using Xamarin.iOS), Activity (if using Xamarin.Android) IWin32Window, or IntPtr (if using .NET Framework). 
    // Used for invoking the browser for Active Directory Interactive authentication.
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Sets a callback method that's invoked with a custom web UI instance that will let the user sign in with Azure AD, present consent if needed, and get back the authorization code. 
    // Applicable when working with Active Directory Interactive authentication.
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core, and .NET Standard targeted applications
    // Clears cached user tokens from the token provider.
    public static void ClearUserTokenCache();
}
```


## <a name="see-also"></a>Voir aussi
- [Objets application et principal du service dans Azure Active Directory](/azure/active-directory/develop/app-objects-and-service-principals)
- [Flux d’authentification](/azure/active-directory/develop/msal-authentication-flows)
