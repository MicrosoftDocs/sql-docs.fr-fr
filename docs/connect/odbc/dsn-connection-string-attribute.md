---
title: ODBC DSN et mots clés de chaîne de connexion
description: Cette page liste les mots clés des chaînes de connexion et des noms de source de données pour SQLSetConnectAttr et SQLGetConnectAttr, disponibles dans le pilote ODBC pour SQL Server.
ms.custom: ''
ms.date: 02/04/2019
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-chojas
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: d905ac9c7f2ea02bfac04b54e247d833ae337b96
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837384"
---
# <a name="dsn-and-connection-string-keywords-and-attributes"></a>Attributs et mots clés de chaîne de connexion et DSN

Cette page liste les mots clés des chaînes de connexion et des noms de source de données pour SQLSetConnectAttr et SQLGetConnectAttr, disponibles dans le pilote ODBC pour SQL Server.

## <a name="supported-dsnconnection-string-keywords-and-connection-attributes"></a>Attributs de connexion et mots clés de chaîne de connexion et de nom de source de données pris en charge

Le tableau suivant liste les mots clés disponibles et les attributs pour chaque plateforme (L : Linux, M : macOS, W : Windows). Pour plus d’informations, cliquez sur le mot clé ou l’attribut.

| Mot clé de chaîne de connexion / DSN | Attribut de connexion | Plateforme |
|-|-|-|
| [Addr](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Adresse](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [AnsiNPW](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ANSI_NPW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssansinpw) | LMW |
| [APP](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ApplicationIntent](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_APPLICATION_INTENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssapplicationintent) | LMW |
| [AttachDBFileName](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ATTACHDBFILENAME](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssattachdbfilename) | LMW |
| [Authentification](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | [SQL_COPT_SS_AUTHENTICATION](../../connect/odbc/dsn-connection-string-attribute.md#authentication---sql_copt_ss_authentication) | LMW |
| [AutoTranslate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRANSLATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstranslate) | LMW |
| [ColumnEncryption](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | [SQL_COPT_SS_COLUMN_ENCRYPTION](../../connect/odbc/dsn-connection-string-attribute.md#columnencryption---sql_copt_ss_column_encryption) | LMW |
| [ConnectRetryCount](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_COUNT](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [ConnectRetryInterval](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | [SQL_COPT_SS_CONNECT_RETRY_INTERVAL](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) | W |
| [Sauvegarde de la base de données](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_ATTR_CURRENT_CATALOG](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| [Description](../../connect/odbc/dsn-connection-string-attribute.md#description) | | LMW |
| [Driver](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [DSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Encrypt](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_ENCRYPT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssencrypt) | LMW |
| [Failover_Partner](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssfailoverpartner) | W |
| [FailoverPartnerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_FAILOVER_PARTNER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | W |
| [FileDSN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [KeepAlive](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (v17.4+, DSN uniquement)| | LMW |
| [KeepAliveInterval](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md) (v17.4+, DSN uniquement) | | LMW |
| [KeystoreAuthentication](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystorePrincipalId](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [KeystoreSecret](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#connection-string-keywords) | | LMW |
| [Langage](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [MARS_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MARS_ENABLED](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmarsenabled) | LMW |
| [MultiSubnetFailover](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_MULTISUBNET_FAILOVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssmultisubnetfailover) | LMW |
| [Net](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Réseau](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [PWD](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [QueryLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquery) | W |
| [QueryLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfquerylog) | W |
| [QueryLogTIme](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_QUERY_INTERVAL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfqueryinterval) | W |
| [QuotedId](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_QUOTED_IDENT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssquotedident) | LMW |
| [Regional](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [SaveFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [Serveur](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [ServerSPN](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_SERVER_SPN](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| [StatsLog_On](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdata) | W |
| [StatsLogFile](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_PERF_DATA_LOG](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalog) | W |
| [TransparentNetworkIPResolution](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | [SQL_COPT_SS_TNIR](../../connect/odbc/dsn-connection-string-attribute.md#transparentnetworkipresolution---sql_copt_ss_tnir) | LMW |
| [Trusted_Connection](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_INTEGRATED_SECURITY](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssintegratedsecurity) | LMW |
| [TrustServerCertificate](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | [SQL_COPT_SS_TRUST_SERVER_CERTIFICATE](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstrustservercertificate) | LMW |
| [UID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| [UseFMTONLY](../../connect/odbc/dsn-connection-string-attribute.md#usefmtonly) | | LMW |
| [WSID](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) | | LMW |
| | [SQL_ATTR_ACCESS_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ACCESS_MODE) | LMW |
| | [SQL_ATTR_ASYNC_DBC_EVENT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCALLBACK](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_DBC_PCONTEXT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_ASYNC_ENABLE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | W |
| | [SQL_ATTR_AUTO_IPD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_AUTOCOMMIT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_AUTOCOMMIT) | LMW |
| | [SQL_ATTR_CONNECTION_DEAD](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_CONNECTION_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_DBC_INFO_TOKEN](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_LOGIN_TIMEOUT](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_LOGIN_TIMEOUT) | LMW |
| | [SQL_ATTR_METADATA_ID](../../odbc/reference/syntax/sqlsetconnectattr-function.md) | LMW |
| | [SQL_ATTR_ODBC_CURSORS](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_ODBC_CURSORS) | LMW |
| | [SQL_ATTR_PACKET_SIZE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_PACKET_SIZE) | LMW |
| | [SQL_ATTR_QUIET_MODE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_QUIET_MODE) | LMW |
| | [SQL_ATTR_RESET_CONNECTION](../../odbc/reference/develop-driver/upgrading-a-3-5-driver-to-a-3-8-driver.md#connection-pooling) <br> (SQL_COPT_SS_RESET_CONNECTION) | LMW |  
| | [SQL_ATTR_TRACE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACE) | LMW |
| | [SQL_ATTR_TRACEFILE](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_OPT_TRACEFILE) | LMW |
| | [SQL_ATTR_TRANSLATE_LIB](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_DLL) | LMW |
| | [SQL_ATTR_TRANSLATE_OPTION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TRANSLATE_OPTION) | LMW |
| | [SQL_ATTR_TXN_ISOLATION](../../odbc/reference/syntax/sqlsetconnectattr-function.md) <br> (SQL_TXN_ISOLATION) | LMW |
| | [SQL_COPT_SS_ACCESS_TOKEN](dsn-connection-string-attribute.md#sql_copt_ss_access_token) | LMW |
| | [SQL_COPT_SS_ANSI_OEM](dsn-connection-string-attribute.md#sql_copt_ss_ansi_oem)| W |
| | [SQL_COPT_SS_AUTOBEGINTXN](dsn-connection-string-attribute.md#sql_copt_ss_autobegintxn)| LMW |
| | [SQL_COPT_SS_BCP](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbcp) | LMW |
| | [SQL_COPT_SS_BROWSE_CACHE_DATA](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) | LMW |
| | [SQL_COPT_SS_BROWSE_CONNECT](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseconnect) | LMW |
| | [SQL_COPT_SS_BROWSE_SERVER](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssbrowseserver) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREDATA](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoredata) | LMW |
| | [SQL_COPT_SS_CEKEYSTOREPROVIDER](dsn-connection-string-attribute.md#sql_copt_ss_cekeystoreprovider) | LMW |
| | [SQL_COPT_SS_CLIENT_CONNECTION_ID](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) | LMW |
| | [SQL_COPT_SS_CONCAT_NULL](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconcatnull) | LMW |
| | [SQL_COPT_SS_CONNECTION_DEAD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssconnectiondead) | LMW |
| | [SQL_COPT_SS_ENLIST_IN_DTC](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssenlistindtc) | W |
| | [SQL_COPT_SS_ENLIST_IN_XA](dsn-connection-string-attribute.md#sql_copt_ss_enlist_in_xa) | LMW |
| | [SQL_COPT_SS_FALLBACK_CONNECT](dsn-connection-string-attribute.md#sql_copt_ss_fallback_connect) | LMW |
| | [SQL_COPT_SS_INTEGRATED_AUTHENTICATION_METHOD](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_MUTUALLY_AUTHENTICATED](../../relational-databases/native-client/odbc/service-principal-names-spns-in-client-connections-odbc.md) | LMW |
| | [SQL_COPT_SS_OLDPWD](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssoldpwd) | LMW |
| | [SQL_COPT_SS_PERF_DATA_LOG_NOW](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssperfdatalognow) | W |
| | [SQL_COPT_SS_PRESERVE_CURSORS](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsspreservecursors) | LMW |
| | [SQL_COPT_SS_SPID](../../connect/odbc/dsn-connection-string-attribute.md#sql_copt_ss_spid) (v17.5+) | LMW |
| | [SQL_COPT_SS_TXN_ISOLATION](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsstxnisolation) | LMW |
| | [SQL_COPT_SS_USER_DATA](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptssuserdata) | LMW |
| | [SQL_COPT_SS_WARN_ON_CP_ERROR](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md#sqlcoptsswarnoncperror) | LMW |
| [ClientCertificate](../../connect/odbc/dsn-connection-string-attribute.md#clientcertificate) | | LMW | 
| [ClientKey](../../connect/odbc/dsn-connection-string-attribute.md#clientkey) | | LMW | 


Voici quelques mots clés de chaîne de connexion et attributs de connexion qui ne sont pas documentés dans [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md), [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) et [Fonction SQLSetConnectAttr](../../odbc/reference/syntax/sqlsetconnectattr-function.md).

### <a name="description"></a>Description

Utilisé pour décrire la source de données.

### <a name="sql_copt_ss_ansi_oem"></a>SQL_COPT_SS_ANSI_OEM

Contrôle la conversion ANSI vers OEM des données. 

| Valeur d'attribut | Description |
|-|-|
| SQL_AO_OFF | (Valeur par défaut) Aucune conversion n’est effectuée. |
| SQL_AO_ON | La conversion est effectuée. |

### <a name="sql_copt_ss_autobegintxn"></a>SQL_COPT_SS_AUTOBEGINTXN

Version 17.6+ alors que la validation automatique est désactivée, contrôle la BEGIN TRANSACTION automatique après ROLLBACK ou COMMIT.

| Valeur d'attribut | Description |
|-|-|
| SQL_AUTOBEGINTXN_ON | (Par défaut) BEGIN TRANSACTION automatique après ROLLBACK ou COMMIT. |
| SQL_AUTOBEGINTXN_OFF | Aucun BEGIN TRANSACTION automatique après ROLLBACK ou COMMIT. |

### <a name="sql_copt_ss_fallback_connect"></a>SQL_COPT_SS_FALLBACK_CONNECT

Contrôle l’utilisation de connexions de secours SQL Server. Cet attribut n’est plus pris en charge.

| Valeur d'attribut | Description |
|-|-|
| SQL_FB_OFF | (Valeur par défaut) Les connexions de secours sont désactivées. |
| SQL_FB_ON | Les connexions de secours sont activées. |



## <a name="new-connection-string-keywords-and-connection-attributes"></a>Nouveaux attributs de connexion et mots clés de chaîne de connexion

###  <a name="authentication---sql_copt_ss_authentication"></a>Authentification - SQL_COPT_SS_AUTHENTICATION

Définit le mode d’authentification à utiliser lors de la connexion à SQL Server. Pour plus d’informations, consultez [Utilisation d’Azure Active Directory](using-azure-active-directory.md).

| Valeur de mot clé | Valeur d'attribut | Description |
|-|-|-|
| |SQL_AU_NONE|(Valeur par défaut) Non défini. La combinaison des autres attributs détermine le mode d’authentification.|
|SqlPassword|SQL_AU_PASSWORD|Authentification SQL Server avec nom d’utilisateur et mot de passe.|
|ActiveDirectoryIntegrated|SQL_AU_AD_INTEGRATED|Authentification intégrée à Azure Active Directory.|
|ActiveDirectoryPassword|SQL_AU_AD_PASSWORD|Authentification par mot de passe Azure Active Directory.|
|ActiveDirectoryInteractive|SQL_AU_AD_INTERACTIVE|Authentification interactive Azure Active Directory.|
|ActiveDirectoryMsi|SQL_AU_AD_MSI|Authentification Managed Identity Azure Active Directory Pour l’identité attribuée par l’utilisateur, UID est défini sur l’ID d’objet de l’identité d’utilisateur. |
| |SQL_AU_RESET|Non défini. Remplace tout nom de source de données ou paramètre de chaîne de connexion.|

> [!NOTE]
> Quand vous utilisez le mot clé ou l’attribut `Authentication`, vous devez explicitement définir le paramètre `Encrypt` sur la valeur souhaitée dans la chaîne de connexion / le nom de source de données / l’attribut de connexion. Pour plus d’informations, consultez [Utilisation de mots clés de chaîne de connexion avec SQL Server Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).

### <a name="columnencryption---sql_copt_ss_column_encryption"></a>ColumnEncryption - SQL_COPT_SS_COLUMN_ENCRYPTION

Contrôle le chiffrement transparent des colonnes (Always Encrypted). Pour plus d’informations, consultez [Utilisation d’Always Encrypted (ODBC)](using-always-encrypted-with-the-odbc-driver.md).

| Valeur de mot clé | Valeur d'attribut | Description |
|-|-|-|
|activé|SQL_CE_ENABLED|Active Always Encrypted.|
|Désactivé|SQL_CE_DISABLED|(Valeur par défaut) Désactive Always Encrypted.|
| |SQL_CE_RESULTSETONLY|Active le déchiffrement uniquement (résultats et valeurs de retour).|

### <a name="transparentnetworkipresolution---sql_copt_ss_tnir"></a>TransparentNetworkIPResolution - SQL_COPT_SS_TNIR

Contrôle la fonctionnalité Résolution d’adresses IP réseau transparente, qui interagit avec MultiSubnetFailover pour autoriser les tentatives de reconnexion plus rapides. Pour plus d’informations, consultez [Utilisation de la résolution d’adresses IP réseau transparente](using-transparent-network-ip-resolution.md).

| Valeur de mot clé | Valeur d'attribut| Description |
|-|-|-|
|activé|SQL_IS_ON|(Par défaut) Active la résolution transparente d’adresses IP réseau.|
|Désactivé|SQL_IS_OFF|Désactive la résolution d’adresses IP réseau transparente.|

### <a name="usefmtonly"></a>UseFMTONLY

Contrôle l’utilisation de SET FMTONLY pour les métadonnées lors de la connexion à SQL Server versions 2012 et ultérieures.

| Valeur de mot clé | Description |
|-|-|
|Non|(Valeur par défaut) Utiliser sp_describe_first_result_set pour les métadonnées si elles sont disponibles. |
|Oui| Utiliser SET FMTONLY pour les métadonnées. |


## <a name="clientcertificate"></a>ClientCertificate

Spécifie le certificat à utiliser pour l’authentification. Les options sont : 

| Valeur d’option | Description |
|-|-|
| sha1:`<hash_value>` | le pilote ODBC utilise le hachage SHA1 pour localiser un certificat dans Windows Certificate Store |
| subject:`<subject>` | le pilote ODBC utilise l’objet pour localiser un certificat dans Windows Certificate Store |
| file:`<file_location>`[,password:`<password>`] | le pilote ODBC utilise un fichier de certificat. |

Si le certificat est au format PFX et que la clé privée dans le certificat PFX est protégée par un mot de passe, le mot de passe est requis. Pour les certificats aux formats PEM et DER, l’attribut ClientKey est requis


## <a name="clientkey"></a>ClientKey

Spécifie un emplacement de fichier de la clé privée pour les certificats PEM ou DER spécifiés par l’attribut ClientCertificate. Format: 

| Valeur d’option | Description |
|-|-|
| file:`<file_location>`[,password:`<password>`] | Spécifie l’emplacement du fichier de la clé privée. |

Si le fichier de la clé privée est protégé par un mot de passe, le mot de passe est requis. Si le mot de passe contient des caractères « , », un caractère « , » supplémentaire est ajouté immédiatement après chacun d’eux. Par exemple, si le mot de passe est « a, b, c », le mot de passe placé dans une séquence d’échappement présent dans la chaîne de connexion est « a,, b,, c ». 
    

### <a name="sql_copt_ss_access_token"></a>SQL_COPT_SS_ACCESS_TOKEN

Autorise l’utilisation d’un jeton d’accès Azure Active Directory pour l’authentification. Pour plus d’informations, consultez [Utilisation d’Azure Active Directory](using-azure-active-directory.md).

| Valeur d'attribut | Description |
|-|-|
| NULL | (Valeur par défaut) Aucun jeton d’accès n’est fourni. |
| ACCESSTOKEN* | Pointeur vers un jeton d’accès. |

### <a name="sql_copt_ss_cekeystoredata"></a>SQL_COPT_SS_CEKEYSTOREDATA

Communique avec une bibliothèque de fournisseur de magasins de clés chargée. Consultez « Contrôle le chiffrement transparent des colonnes (Always Encrypted) ». Cet attribut n’a aucune valeur par défaut. Pour plus d’informations, consultez [Fournisseurs de magasins de clés personnalisés](custom-keystore-providers.md).

| Valeur d'attribut | Description |
|-|-|
| CEKEYSTOREDATA * | Structure des données de communication pour la bibliothèque du fournisseur de magasins de clés |

### <a name="sql_copt_ss_cekeystoreprovider"></a>SQL_COPT_SS_CEKEYSTOREPROVIDER

Charge une bibliothèque de fournisseur de magasins de clés pour Always Encrypted, ou récupère les noms des bibliothèques de fournisseur de magasins de clés chargées. Pour plus d’informations, consultez [Fournisseurs de magasins de clés personnalisés](custom-keystore-providers.md). Cet attribut n’a aucune valeur par défaut.

| Valeur d'attribut | Description |
|-|-|
| char * | Chemin d’une bibliothèque de fournisseur de magasins de clés |

### <a name="sql_copt_ss_enlist_in_xa"></a>SQL_COPT_SS_ENLIST_IN_XA

Pour activer les transactions XA avec un processeur de transaction (TP) compatible XA, l’application doit appeler **SQLSetConnectAttr** avec SQL_COPT_SS_ENLIST_IN_XA et un pointeur vers un objet `XACALLPARAM`. Cette option est prise en charge sur Windows, Linux (versions 17.3 et ultérieures) et macOS.
```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, param, SQL_IS_POINTER);  // XACALLPARAM *param
``` 
 Pour associer une transaction XA à une connexion ODBC uniquement, fournissez TRUE ou FALSE avec SQL_COPT_SS_ENLIST_IN_XA au lieu du pointeur lors de l’appel de **SQLSetConnectAttr**. Cette méthode est seulement valide sur Windows et ne peut pas être utilisée pour spécifier des opérations XA par le biais d’une application cliente. 
 ```
SQLSetConnectAttr(hdbc, SQL_COPT_SS_ENLIST_IN_XA, (SQLPOINTER)TRUE, 0);
``` 

|Valeur|Description|Plateformes|  
|-----------|-----------------|-----------------|  
|Objet XACALLPARAM*|Pointeur vers un objet `XACALLPARAM`.|Windows, Linux et macOS|
|TRUE|Associe la transaction XA à la connexion ODBC. Toutes les activités de base de données connexes seront effectuées sous la protection de la transaction XA.|Windows|  
|FALSE|Dissocie la transaction de la connexion ODBC.|Windows|

 Pour plus d’informations sur les transactions XA, consultez [Utilisation de transactions XA](../../connect/odbc/use-xa-with-dtc.md).

### <a name="sql_copt_ss_spid"></a>SQL_COPT_SS_SPID

Récupère l’ID du processus serveur de la connexion. Il équivaut à la variable T-SQL [@@SPID](../../t-sql/functions/spid-transact-sql.md), mais n'entraîne aucun aller-retour supplémentaire au serveur.

| Valeur d'attribut | Description |
|-|-|
| DWORD | SPID |
