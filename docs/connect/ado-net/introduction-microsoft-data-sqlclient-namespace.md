---
title: Présentation de l’espace de noms Microsoft.Data.SqlClient
description: Découvrez l’espace de noms Microsoft.Data.SqlClient et la façon dont il est préférable de se connecter à SQL pour les applications .NET.
ms.date: 11/19/2020
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: e966e4f2f43ebe546d6baa0b757f682f3eca205b
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596361"
---
# <a name="introduction-to-microsoftdatasqlclient-namespace"></a>Présentation de l’espace de noms Microsoft.Data.SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="release-notes-for-microsoftdatasqlclient-21"></a>Notes de publication de Microsoft.Data.SqlClient 2.1

Les notes de publication sont également disponibles dans le référentiel GitHub : [Notes de publication 2.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.1).

### <a name="new-features"></a>Nouvelles fonctionnalités

### <a name="cross-platform-support-for-always-encrypted"></a>Support multiplateforme pour Always Encrypted
Microsoft.Data.SqlClient v2.1 étend le support d’Always Encrypted sur les plateformes suivantes :

| Support Always Encrypted | Support Always Encrypted avec enclave sécurisée  | Framework cible | Version Microsoft.Data.SqlClient | Système d’exploitation |
|:--|:--|:--|:--:|:--:|
| Oui | Oui | .NET Framework 4.6+ | 1.1.0+ | Windows |
| Oui | Oui | .NET Core 2.1+ | 2.1.0+<sup>1</sup> | Windows, Linux, macOS |
| Oui | Non <sup>2</sup> | .NET Standard 2.0 | 2.1.0+ | Windows, Linux, macOS |
| Oui | Oui | .NET Standard 2.1+ | 2.1.0+ | Windows, Linux, macOS |

> [!NOTE]
> <sup>1</sup> Avant Microsoft.Data.SqlClient version v2.1, Always Encrypted est pris en charge uniquement sur Windows.
> <sup>2</sup> Always Encrypted avec enclaves sécurisés n’est pas pris en charge sur .NET Standard 2.0.

### <a name="azure-active-directory-device-code-flow-authentication"></a>Authentification du flux de code de l’appareil Azure Active Directory
Microsoft.Data.SqlClient v2.1 fournit le support de l’authentification « Flux de code d’appareil » avec MSAL.NET.
Documentation de référence : [Flux de l’octroi de l’autorisation d’appareil OAuth2.0](/azure/active-directory/develop/v2-oauth2-device-code)

Exemple de chaîne de connexion :

`Server=<server>.database.windows.net; Authentication=Active Directory Device Code Flow; Database=Northwind;`

L’API suivante permet la personnalisation du mécanisme de rappel du flux de code de l’appareil :

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetDeviceCodeFlowCallback(Func<DeviceCodeResult, Task> deviceCodeFlowCallbackMethod)
}
```

### <a name="azure-active-directory-managed-identity-authentication"></a>Authentification Managed Identity Azure Active Directory
Microsoft.Data.SqlClient v2.1 introduit le support de l’authentification Azure Active Directory à l’aide des [identités managées](/azure/active-directory/managed-identities-azure-resources/overview).

Les mots clé d’authentification suivants sont pris en charge :
- Identité managée Active Directory
- Active Directory MSI (pour la compatibilité des pilotes inter MS SQL)

Exemples de chaînes de connexion :

```cs
// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; Initial Catalog={db};"

// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"
```

### <a name="azure-active-directory-interactive-authentication-enhancements"></a>Améliorations de l’authentification interactive Azure Active Directory
Microsoft.Data.SqlClient v2.1 ajoute les API suivantes pour personnaliser l’expérience d’authentification « Active Directory interactive » :

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void ClearUserTokenCache();
}
```

### <a name="sqlclientauthenticationproviders-configuration-section"></a>section de configuration `SqlClientAuthenticationProviders`
Microsoft.Data.SqlClient v2.1 introduit une nouvelle section de configuration, `SqlClientAuthenticationProviders` (un clone du `SqlAuthenticationProviders` existant). La section de configuration existante, `SqlAuthenticationProviders`, est toujours prise en charge à des fins de compatibilité descendante lorsque le type approprié est défini.

La nouvelle section permet aux fichiers config d’application de contenir à la fois une section SqlAuthenticationProviders pour System.Data.SqlClient et une section SqlClientAuthenticationProviders pour Microsoft.Data.SqlClient.


### <a name="azure-active-directory-authentication-using-an-application-client-id"></a>Authentification Azure Active Directory à l’aide d’un ID client d’application
Microsoft.Data.SqlClient v2.1 introduit le support de la transmission d’un ID client d’application définie par l’utilisateur à la bibliothèque d’authentification Microsoft. L’ID client de l’application est utilisé lors de l’authentification avec Azure Active Directory.

Les nouvelles API suivantes sont introduites :

1. Un nouveau constructeur a été introduit dans ActiveDirectoryAuthenticationProvider:\
_[S’applique à toutes les plateformes .NET (.NET Framework, .NET Core et .NET Standard)]_

```csharp
public ActiveDirectoryAuthenticationProvider(string applicationClientId)
```

Usage :
```csharp
string APP_CLIENT_ID = "<GUID>";
SqlAuthenticationProvider customAuthProvider = new ActiveDirectoryAuthenticationProvider(APP_CLIENT_ID);
SqlAuthenticationProvider.SetProvider(SqlAuthenticationMethod.ActiveDirectoryInteractive, customAuthProvider);

using (SqlConnection sqlConnection = new SqlConnection("<connection_string>")
{
    sqlConnection.Open();
}
```

2. Une nouvelle propriété de configuration a été introduite sous `SqlAuthenticationProviderConfigurationSection` et `SqlClientAuthenticationProviderConfigurationSection`:\
_[S’applique à .NET Framework et .NET Core]_

```csharp
internal class SqlAuthenticationProviderConfigurationSection : ConfigurationSection
{
    ...
    [ConfigurationProperty("applicationClientId", IsRequired = false)]
    public string ApplicationClientId => this["applicationClientId"] as string;
}

// Inheritance
internal class SqlClientAuthenticationProviderConfigurationSection : SqlAuthenticationProviderConfigurationSection
{ ... }
```

Usage :
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

### <a name="data-classification-v2-support"></a>Support de la classification des données v2
Microsoft.Data.SqlClient v2.1 introduit le support des informations de « classement de la sensibilité » de la classification des données. Les nouvelles API suivantes sont maintenant disponibles :

```csharp
public class SensitivityClassification
{
    public SensitivityRank SensitivityRank;
}

public class SensitivityProperty
{
    public SensitivityRank SensitivityRank;
}

public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}
```

### <a name="server-process-id-for-an-active-sqlconnection"></a>ID de processus serveur pour un actif `SqlConnection`
MicrosoftData. SqlClient v2.1 introduit une nouvelle propriété `SqlConnection`, `ServerProcessId`, sur une connexion active.

```csharp
public class SqlConnection
{
    // Returns the server process Id (SPID) of the active connection.
    public int ServerProcessId;
}
```

### <a name="trace-logging-support-in-native-sni"></a>Support du suivi des traces dans Native SNI
Microsoft.Data.SqlClient v2.1 étend l’implémentation `SqlClientEventSource` existante pour activer le suivi d’événements dans SNI.dll. Les événements doivent être capturés à l’aide d’un outil tel que Xperf.

Le suivi peut être activé en envoyant une commande à `SqlClientEventSource` comme illustré ci-dessous :

```csharp
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```


### <a name="command-timeout-connection-string-property"></a>Propriété de chaîne de connexion « Délai d’attente de la commande »
Microsoft.Data.SqlClient v2.1 introduit la propriété de chaîne de connexion « Délai d’attente de la commande » pour remplacer la valeur par défaut de 30 secondes. Le délai d’attente pour les commandes individuelles peut être substitué à l’aide de la propriété `CommandTimeout` sur SqlCommand.

Exemples de chaînes de connexion :

`"Server={serverURL}; Initial Catalog={db}; Integrated Security=true; Command Timeout=60"`

### <a name="removal-of-symbols-from-native-sni"></a>Suppression de symboles de Native SNI
Avec Microsoft.Data.SqlClient v2.1, nous avons supprimé les symboles introduits dans [v2.0.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI/2.0.0) de [Microsoft.Data.SqlClient.SNI. Runtime](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime) NuGet à partir de [v2.1.1](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime/2.1.1). Les symboles publics sont désormais publiés sur le serveur de symboles Microsoft pour les outils tels que BinSkim qui requièrent l’accès aux symboles publics.

### <a name="source-linking-of-microsoftdatasqlclient-symbols"></a>Liaison à la source des symboles Microsoft.Data.SqlClient
À compter de Microsoft.Data.SqlClient v2.1, les symboles Microsoft.Data.SqlClient sont liés à la source et publiés sur le serveur de symboles Microsoft pour une expérience de débogage améliorée sans avoir besoin de télécharger le code source.


## <a name="release-notes-for-microsoftdatasqlclient-20"></a>Notes de publication de Microsoft.Data.SqlClient 2.0

Les notes de publication sont également disponibles dans le référentiel GitHub : [Notes de publication 2.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.0).

### <a name="breaking-changes"></a>Changements cassants

- Le modificateur d’accès de l’interface du fournisseur d’enclave `SqlColumnEncryptionEnclaveProvider` (`public`) a été remplacé par `internal`.

- Les constantes de la classe `SqlClientMetaDataCollectionNames` ont été mises à jour pour refléter les modifications apportées à SQL Server.

- Le pilote effectue maintenant une validation du certificat de serveur lorsque le serveur SQL Server cible applique le chiffrement TLS, soit le comportement par défaut pour les connexions Azure.

- `SqlDataReader.GetSchemaTable()` retourne maintenant une `DataTable` vide à la place de `null`.

- Le pilote effectue maintenant un arrondi d’échelle décimale, calqué sur le comportement de SQL Server. À des fins de compatibilité descendante, le comportement de troncation précédent peut être activé à l’aide d’un commutateur AppContext.

- Pour les applications .NET Framework qui consomment **Microsoft.Data.SqlClient**, les fichiers SNI.dll, auparavant téléchargés dans les dossiers `bin\x64` et `bin\x86`, sont maintenant nommés `Microsoft.Data.SqlClient.SNI.x64.dll` et ` Microsoft.Data.SqlClient.SNI.x86.dll`, et téléchargés dans le répertoire `bin`.

- De nouveaux synonymes des propriétés de chaîne de connexion remplacent les anciennes propriétés lors de l’extraction de la chaîne de connexion auprès de `SqlConnectionStringBuilder` à des fins de cohérence. [En savoir plus](#new-connection-string-property-synonyms)

### <a name="new-features"></a>Nouvelles fonctionnalités

#### <a name="dns-failure-resiliency"></a>Résilience des échecs DNS

Le pilote met maintenant en cache les adresses IP issues de toutes les connexions réussies dans un point de terminaison SQL Server qui prend en charge la fonctionnalité. En cas d’échec de la résolution DNS lors d’une tentative de connexion, il tente d’établir une connexion à l’aide d’une adresse IP en cache, s’il en existe une pour ce serveur.

#### <a name="eventsource-tracing"></a>Suivi EventSource

Cette version introduit la prise en charge de la capture des journaux de suivi d’événements pour les applications de débogage. Pour capturer ces événements, les applications clientes doivent les détecter dans l’implémentation EventSource de SqlClient :

```
Microsoft.Data.SqlClient.EventSource
```

Pour plus d’informations, consultez [Guide pratique pour activer le suivi d’événements dans SqlClient](enable-eventsource-tracing.md).

#### <a name="enabling-managed-networking-on-windows"></a>Activation de la mise en réseau gérée sur Windows

Un nouveau commutateur AppContext, **« Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows »** , permet d’utiliser une implémentation SNI managée sur Windows à des fins de test et de débogage. Il modifie le comportement du pilote de façon à utiliser une indication SNI gérée dans les projets .NET Core 2.1 (et versions ultérieures) et .NET Standard 2.0 (et versions ultérieures) sur Windows, éliminant ainsi toutes les dépendances vis-à-vis de bibliothèques natives pour la bibliothèque Microsoft.Data.SqlClient.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Pour obtenir la liste complète des commutateurs disponibles dans le pilote, consultez [Commutateurs AppContext dans SqlClient](appcontext-switches.md).

#### <a name="enabling-decimal-truncation-behavior"></a>Activation du comportement de troncation décimale

L’échelle de données décimale est arrondie par défaut par le pilote, comme le fait SQL Server. À des fins de compatibilité descendante, vous pouvez définir le commutateur AppContext **Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal** sur **true**.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

#### <a name="new-connection-string-property-synonyms"></a>Nouveaux synonymes des propriétés de chaîne de connexion

De nouveaux synonymes ont été ajoutés pour les propriétés de chaîne de connexion existantes suivantes afin d’éviter toute confusion liée à l’espacement entre les propriétés comportant plusieurs mots. Les anciens noms des propriétés resteront pris en charge à des fins de compatibilité descendante, mais les nouvelles propriétés de chaîne de connexion seront désormais incluses lors de l’extraction de la chaîne de connexion auprès de [SqlConnectionStringBuilder](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder).

|Propriété de chaîne de connexion existante|Nouveau synonyme|
|-----------------------------------|-----------|
| ApplicationIntent | Intention de l'application |
| ConnectRetryCount | Nombre de nouvelles tentatives de connexion |
| ConnectRetryInterval | Intervalle avant nouvelle tentative de connexion |
| PoolBlockingPeriod | Période de blocage du pool |
| MultipleActiveResultSets | Jeux MARS (Multiple Active Result Set) |
| MultiSubnetFailover | Basculement de plusieurs sous-réseaux |
| TransparentNetworkIPResolution | Résolution transparente d’adresses IP réseau |
| TrustServerCertificate | Faire confiance au certificat de serveur |

#### <a name="sqlbulkcopy-rowscopied-property"></a>Propriété SqlBulkCopy RowsCopied

La propriété RowsCopied fournit un accès en lecture seule au nombre de lignes traitées dans l’opération de copie en bloc en cours. Cette valeur n’est pas nécessairement égale au nombre final de lignes ajoutées à la table de destination.

#### <a name="connection-open-overrides"></a>Remplacement de Connection.Open

Le comportement par défaut de SqlConnection.Open() peut être écrasé pour désactiver le délai de dix secondes et les tentatives de connexion automatiques déclenchées par des erreurs temporaires.

```csharp
using SqlConnection sqlConnection = new SqlConnection("Data Source=(local);Integrated Security=true;Initial Catalog=AdventureWorks;");
sqlConnection.Open(SqlConnectionOverrides.OpenWithoutRetry);
```

> [!NOTE]
> Notez que ce remplacement ne s’applique qu’à SqlConnection.Open() et non à SqlConnection.OpenAsync().

#### <a name="username-support-for-active-directory-interactive-mode"></a>Prise en charge du nom d’utilisateur pour le mode interactif Active Directory

Un nom d’utilisateur peut être spécifié dans la chaîne de connexion avec le mode d’authentification interactive Azure Active Directory pour .NET Framework et .NET Core.

Définissez un nom d’utilisateur à l’aide de la propriété de chaîne de connexion **User ID** ou **UID** :

```
"Server=<server name>; Database=<db name>; Authentication=Active Directory Interactive; User Id=<username>;"
```

#### <a name="order-hints-for-sqlbulkcopy"></a>Indicateurs d’ordre pour SqlBulkCopy

Des indicateurs d’ordre peuvent être spécifiés pour améliorer les performances des opérations de copie en bloc sur les tables avec index cluster. Pour plus d’informations, consultez la section [Opérations de copie en bloc](sql/bulk-copy-order-hints.md).

#### <a name="sni-dependency-changes"></a>Modification des dépendances SNI

Microsoft.Data.SqlClient (.NET Core et .NET Standard) sur Windows dépend maintenant de **Microsoft.Data.SqlClient.SNI.runtime**, et non plus de **runtime.native.System.Data.SqlClient.SNI**. La nouvelle dépendance ajoute la prise en charge de la plateforme ARM à celle des plateformes déjà prises en charge (ARM64, x64 et x86) sur Windows.

## <a name="release-notes-for-microsoftdatasqlclient-110"></a>Notes de publication de Microsoft.Data.SqlPackage 1.1.0

Les notes de publication sont également disponibles dans le référentiel GitHub : [1.1 Notes de publication](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

### <a name="new-features"></a>Nouvelles fonctionnalités

#### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted avec enclaves sécurisées

Always Encrypted est disponible à partir de Microsoft SQL Server 2016. Les enclaves sécurisées sont disponibles à partir de Microsoft SQL Server 2019. La fonctionnalité d’enclave ne peut être utilisée que si les chaînes de connexion incluent le protocole d’attestation et l’URL d’attestation requis. Par exemple :

```
Attestation Protocol=HGS;Enclave Attestation Url=<attestation_url_for_HGS>
```

Pour plus d'informations, consultez les pages suivantes :

- [Support SqlClient pour Always Encrypted](sql/sqlclient-support-always-encrypted.md)
- [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)

## <a name="release-notes-for-microsoftdatasqlclient-10"></a>Notes de publication pour Microsoft.Data.SqlPackage 1.0

La version initiale de l’espace de noms Microsoft.Data.SqlClient offre des fonctionnalités supplémentaires par rapport à l’espace de noms System.Data.SqlClient existant.
Les notes de publication sont également disponibles dans le référentiel GitHub : [Notes de publication 1.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.0).

### <a name="new-features"></a>Nouvelles fonctionnalités

#### <a name="new-features-over-net-framework-472-systemdatasqlclient"></a>Nouvelles fonctionnalités de .NET Framework 4.7.2 System.Data.SqlClient

- **Classification des données** : disponible dans Azure SQL Database et Microsoft SQL Server 2019.

- **Support UTF-8** : disponible dans Microsoft SQL Server 2019.

#### <a name="new-features-over-net-core-22-systemdatasqlclient"></a>Nouvelles fonctionnalités de .NET Core 2.2 System.Data.SqlClient

- **Classification des données** : disponible dans Azure SQL Database et Microsoft SQL Server 2019.

- **Support UTF-8** : disponible dans Microsoft SQL Server 2019.

- **Authentification** : mode d’authentification du mot de passe Active Directory.

### <a name="data-classification"></a>Classification des données

La classification des données apporte un nouvel ensemble d’API exposant la sensibilité des données en lecture seule et des informations de classification sur les objets récupérés par le biais de SqlDataReader lorsque la source sous-jacente prend en charge la fonctionnalité et contient des métadonnées sur la [sensibilité et la classification des données](../../relational-databases/security/sql-data-discovery-and-classification.md). Consultez l’exemple d’application dans [Découverte et classification des données dans SqlClient](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

```csharp
public class SqlDataReader
{
    public Microsoft.Data.SqlClient.DataClassification.SensitivityClassification SensitivityClassification
}

namespace Microsoft.Data.SqlClient.DataClassification
{
    public class ColumnSensitivity
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.SensitivityProperty> SensitivityProperties
    }
    public class InformationType
    {
        public string Id
        public string Name
    }
    public class Label
    {
        public string Id
        public string Name
    }
    public class SensitivityClassification
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.ColumnSensitivity> ColumnSensitivities
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.InformationType> InformationTypes
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.Label> Labels
    }
    public class SensitivityProperty
    {
        public Microsoft.Data.SqlClient.DataClassification.InformationType InformationType
        public Microsoft.Data.SqlClient.DataClassification.Label Label
    }
}
```

### <a name="utf-8-support"></a>Prise en charge d’UTF-8

La prise en charge d’UTF-8 ne nécessite aucun changement du code de l’application. Ces modifications SqlClient optimisent la communication client-serveur lorsque le serveur prend en charge l’encodage UTF-8 et que le classement de la colonne sous-jacente est en UTF-8. Consultez la section UTF-8 sous [Nouveautés de SQL Server 2019](../../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="always-encrypted-with-enclaves"></a>Always Encrypted avec enclaves

En général, la documentation existante qui utilise System.Data.SqlClient sur .NET Framework **et les fournisseurs de magasin de clés principales de colonne intégrés** doivent désormais fonctionner également avec .NET Core.

 [Développer à l’aide d’Always Encrypted avec le fournisseur de données .NET Framework](../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)

 [Always Encrypted : Protéger les données sensibles et stocker les clés de chiffrement dans le magasin de certificats Windows](/azure/sql-database/sql-database-always-encrypted)

### <a name="authentication"></a>Authentification

Vous pouvez spécifier différents modes d’authentification à l’aide de l’option de chaîne de connexion _Authentification_. Pour plus d’informations, consultez la [documentation de SqlAuthenticationMethod](/dotnet/api/system.data.sqlclient.sqlauthenticationmethod?view=netframework-4.7.2&preserve-view=true).

> [!NOTE]
> Les fournisseurs de magasins de clés personnalisés, comme le fournisseur d’Azure Key Vault, devront être mis à jour pour prendre en charge Microsoft.Data.SqlClient. De même, les fournisseurs d’enclaves devront également être mis à jour pour prendre en charge Microsoft.Data.SqlClient.
> Always Encrypted est pris en charge uniquement sur les cibles .NET Framework et .NET Core. Il n’est pas pris en charge sur .NET Standard dans la mesure où certaines dépendances de chiffrement manquent dans .NET Standard.