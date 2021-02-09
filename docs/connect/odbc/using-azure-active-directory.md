---
title: Utilisation d’Azure Active Directory avec ODBC Driver
description: Microsoft ODBC Driver for SQL Server permet aux applications ODBC de se connecter à une instance Azure SQL Database à l’aide d’Azure Active Directory.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 52205f03-ff29-4254-bfa8-07cced155c86
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2a6b27c8c14a92ed9f17c56cab1cf2285b2cfec9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198885"
---
# <a name="using-azure-active-directory-with-the-odbc-driver"></a>Utilisation d’Azure Active Directory avec ODBC Driver
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

## <a name="purpose"></a>Objectif

La version 13.1 et les versions ultérieures de Microsoft ODBC Driver for SQL Server permettent aux applications ODBC de se connecter à une instance d’Azure SQL Database à l’aide d’une identité fédérée dans Azure Active Directory avec un nom d’utilisateur/mot de passe, un jeton d’accès Azure Active Directory, une identité managée Azure Active Directory (17.3+) ou l’authentification intégrée Windows (17.6+ sur Linux/macOS). Dans la version 13.1 du pilote ODBC, l’authentification par jeton d’accès Azure Active Directory concerne _Windows uniquement_. La version 17 et les versions ultérieures du pilote ODBC prennent en charge cette authentification sur toutes les plateformes (Windows, Linux et macOS). Une nouvelle authentification interactive Azure Active Directory avec ID de connexion est introduite dans la version 17.1 du pilote ODBC pour Windows. Une nouvelle méthode d’authentification d’identité managée Azure Active Directory a été ajoutée à la version 17.3.1.1 du pilote ODBC pour les identités attribuées par le système et par l’utilisateur. Toutes ces méthodes utilisent de nouveaux mots clés de chaîne de connexion et de nom de source de données, ainsi que des attributs de connexion.

> [!NOTE]
> ODBC Driver sur Linux et macOS avant la version 17.6 prend en charge uniquement l’authentification Azure Active Directory directement auprès d’Azure Active Directory. Si vous utilisez l’authentification Azure Active Directory par nom d’utilisateur/mot de passe à partir d’un client Linux ou macOS et que votre configuration Active Directory exige l’authentification du client auprès d’un point de terminaison ADFS (Active Directory Federation Services), l’authentification risque d’échouer. À compter de la version du pilote 17.6, cette limitation a été supprimée.

## <a name="new-andor-modified-dsn-and-connection-string-keywords"></a>Nouveaux mots clés de chaîne de connexion et de nom de source de données et mots clés modifiés

Le mot clé `Authentication` peut être utilisé en cas de connexion avec un nom de source de données ou une chaîne de connexion pour contrôler le mode d’authentification. La valeur définie dans la chaîne de connexion remplace celle du nom de source de données, si elle existe. La _valeur préattribut_ du paramètre `Authentication` est la valeur calculée à partir des valeurs de chaîne de connexion et de nom de source de données.

|Nom|Valeurs|Default|Description|
|-|-|-|-|
|`Authentication`|(non défini), (chaîne vide), `SqlPassword`, `ActiveDirectoryPassword`, `ActiveDirectoryIntegrated`, `ActiveDirectoryInteractive`, `ActiveDirectoryMsi`, `ActiveDirectoryServicePrincipal` |(non défini)|Contrôle le mode d’authentification.<table><tr><th>Valeur<th>Description<tr><td>(non défini)<td>Mode d’authentification déterminé par d’autres mots clés (options de connexion héritées existantes).<tr><td>(chaîne vide)<td>(Chaîne de connexion uniquement.) Remplace et annule une valeur `Authentication` définie dans le nom de source de données.<tr><td>`SqlPassword`<td>Authentification directe auprès d’une instance SQL Server à l’aide d’un nom d’utilisateur et d’un mot de passe.<tr><td>`ActiveDirectoryPassword`<td>Authentification avec une identité Azure Active Directory à l’aide d’un nom d’utilisateur et d’un mot de passe.<tr><td>`ActiveDirectoryIntegrated`<td>_pilote Windows et Linux/Mac 17.6+ seulement_. Authentification avec une identité Azure Active Directory à l’aide de l’authentification intégrée.<tr><td>`ActiveDirectoryInteractive`<td>(_Pilote Windows uniquement_.) Authentification avec une identité Azure Active Directory à l’aide de l’authentification interactive.<tr><td>`ActiveDirectoryMsi`<td>Authentification avec une identité Azure Active Directory à l’aide de l’authentification d’identité managée. Pour l’identité attribuée par l’utilisateur, UID est défini sur l’ID d’objet de l’identité d’utilisateur.<tr><td>`ActiveDirectoryServicePrincipal`<td>(17.7+) Authentification avec une identité Azure Active Directory en utilisant l’authentification par principal de service.</table>|
|`Encrypt`|(non défini), `Yes`, `No`|(voir description)|Contrôle le chiffrement pour une connexion. Si la valeur préattribut du paramètre `Authentication` n’est pas _none_ dans le nom de source de données ou la chaîne de connexion, la valeur par défaut est `Yes`. Sinon, la valeur par défaut est `No`. Si l’attribut `SQL_COPT_SS_AUTHENTICATION` remplace la valeur préattribut de `Authentication`, définissez explicitement la valeur du chiffrement dans le nom de source de données, la chaîne de connexion ou l’attribut de connexion. La valeur préattribut du chiffrement est `Yes` si la valeur est définie sur `Yes` dans le nom de source de données ou la chaîne de connexion.|

## <a name="new-andor-modified-connection-attributes"></a>Nouveaux attributs de connexion et attributs modifiés

Les attributs de connexion préconnexion suivants ont été introduits ou modifiés de façon à prendre en charge l’authentification Azure Active Directory. Lorsqu’un attribut de connexion présente un mot clé de chaîne de connexion ou de nom de source de données correspondant et qu’il est défini, il est prioritaire.

|Attribut|Type|Valeurs|Default|Description|
|-|-|-|-|-|
|`SQL_COPT_SS_AUTHENTICATION`|`SQL_IS_INTEGER`|`SQL_AU_NONE`, `SQL_AU_PASSWORD`, `SQL_AU_AD_INTEGRATED`, `SQL_AU_AD_PASSWORD`, `SQL_AU_AD_INTERACTIVE`, `SQL_AU_AD_MSI`, `SQL_AU_AD_SPA`, `SQL_AU_RESET`|(non défini)|Voir la description du mot clé `Authentication` ci-dessus. `SQL_AU_NONE` est fourni pour remplacer explicitement une valeur `Authentication` définie dans le nom de source de données ou la chaîne de connexion, tandis que `SQL_AU_RESET` annule l’attribut si celui-ci a été défini, ce qui donne la priorité à la valeur de la chaîne de connexion ou du nom de source de données.|
|`SQL_COPT_SS_ACCESS_TOKEN`|`SQL_IS_POINTER`|Pointeur vers `ACCESSTOKEN` ou Null|NULL|Si la valeur est non Null, spécifie le jeton d’accès Azure AD à utiliser. Il n’est pas correct de spécifier un jeton d’accès ainsi que des mots clés de chaîne de connexion `UID`, `PWD`, `Trusted_Connection` ou `Authentication` ou leurs attributs équivalents. <br> **REMARQUE :** La version 13.1 du pilote ODBC ne le prend en charge que sur _Windows_.|
|`SQL_COPT_SS_ENCRYPT`|`SQL_IS_INTEGER`|`SQL_EN_OFF`, `SQL_EN_ON`|(voir description)|Contrôle le chiffrement pour une connexion. `SQL_EN_OFF` et `SQL_EN_ON` désactivent et activent respectivement le chiffrement. Si la valeur préattribut du paramètre `Authentication` n’est pas _none_ ou que `SQL_COPT_SS_ACCESS_TOKEN` est défini, et que `Encrypt` n’a pas été spécifié dans le nom de source de données ou la chaîne de connexion, la valeur par défaut est `SQL_EN_ON`. Sinon, la valeur par défaut est `SQL_EN_OFF`. Si l’attribut de connexion `SQL_COPT_SS_AUTHENTICATION` a la valeur _none_, définissez explicitement `SQL_COPT_SS_ENCRYPT` sur la valeur souhaitée si `Encrypt` n’a pas été spécifié dans le nom de source de données ou la chaîne de connexion. La valeur effective de cet attribut contrôle [si le chiffrement sera utilisé pour la connexion](../../relational-databases/native-client/features/using-encryption-without-validation.md).|
|`SQL_COPT_SS_OLDPWD`|\-|\-|\-|Non pris en charge avec Azure Active Directory, car les modifications de mot de passe des principaux Azure AD ne peuvent pas être effectuées par le biais d’une connexion ODBC. <br><br>L’expiration du mot de passe pour l’authentification SQL Server a été introduite dans SQL Server 2005. L’attribut `SQL_COPT_SS_OLDPWD` a été ajouté pour permettre au client de fournir à la fois l’ancien mot de passe et le nouveau dans le cadre de la connexion. Lorsque cette propriété est définie, le fournisseur n’utilisera pas le pool de connexions pour la première connexion ou pour les connexions suivantes, puisque la chaîne de connexion contiendra l’« ancien mot de passe » qui a maintenant changé.|
|`SQL_COPT_SS_INTEGRATED_SECURITY`|`SQL_IS_INTEGER`|`SQL_IS_OFF`,`SQL_IS_ON`|`SQL_IS_OFF`|_Déprécié_ ; utilisez `SQL_COPT_SS_AUTHENTICATION` défini sur `SQL_AU_AD_INTEGRATED` à la place. <br><br>Force l’utilisation de l’authentification Windows (Kerberos sur Linux et macOS) pour la validation de l’accès lors de la connexion au serveur. Lorsque l’authentification Windows est utilisée, le pilote ignore les valeurs d’identificateur d’utilisateur et de mot de passe fournies dans le cadre du traitement `SQLConnect`, `SQLDriverConnect` ou `SQLBrowseConnect`.|

## <a name="ui-additions-for-azure-active-directory-windows-driver-only"></a>Ajouts à l’interface utilisateur pour Azure Active Directory (pilote Windows uniquement)

Les interfaces utilisateur de configuration du nom de source de données et de connexion du pilote ont été améliorées de manière à intégrer les options supplémentaires nécessaires à l’utilisation de l’authentification avec Azure AD.

### <a name="creating-and-editing-dsns-in-the-ui"></a>Créer et modifier les noms de sources de données dans l’interface utilisateur

Il est possible d’utiliser les nouvelles options d’authentification Azure AD lors de la création ou de la modification d’un nom de source de données existant à l’aide de l’interface utilisateur de configuration du pilote :

`Authentication=ActiveDirectoryIntegrated` pour l’authentification intégrée Azure Active Directory sur Azure SQL Database

![L’écran de création et de modification du nom de source de données avec l’authentification intégrée Azure Active Directory sélectionnée.](windows/create-dsn-ad-integrated.png)

`Authentication=ActiveDirectoryPassword` pour l’authentification par nom d’utilisateur/mot de passe Azure Active Directory sur Azure SQL Database

![L’écran de création et de modification du nom de source de données avec l’authentification par mot de passe Azure Active Directory sélectionnée.](windows/create-dsn-ad-password.png)

`Authentication=ActiveDirectoryInteractive` pour l’authentification interactive Azure Active Directory sur Azure SQL Database

![L’écran de création et de modification du nom de source de données avec l’authentification interactive Azure Active Directory sélectionnée.](windows/create-dsn-ad-interactive.png)

`Authentication=SqlPassword` pour l’authentification par nom d’utilisateur/mot de passe pour SQL Server (Azure ou autre)

![L’écran de création et de modification du nom de source de données avec l’authentification SQL Server sélectionnée.](windows/create-dsn-ad-sql-server.png)

`Trusted_Connection=Yes` pour l’authentification intégrée Windows SSPI héritée

![L’écran de création et de modification du nom de source de données avec l’authentification Windows intégrée sélectionnée.](windows/create-dsn-win-sspi.png)

`Authentication=ActiveDirectoryMsi` pour l’authentification Managed Identity Azure Active Directory

![L’écran de création et de modification du nom de source de données avec l’authentification Identités managées pour les ressources Azure sélectionnée.](windows/create-dsn-ad-msi.png)

`Authentication=ActiveDirectoryServicePrincipal` pour l’authentification par principal de service Azure Active Directory

![L’écran de création et de modification du nom de source de données avec l’authentification par principal de service Azure Active Directory sélectionnée.](windows/create-dsn-ad-spa.png)

Les six options correspondent respectivement à `Trusted_Connection=Yes` (authentification intégrée Windows SSPI héritée uniquement) et à `Authentication=` `ActiveDirectoryIntegrated`, `SqlPassword`, `ActiveDirectoryPassword`, `ActiveDirectoryInteractive`, `ActiveDirectoryMsi` et `ActiveDirectoryServicePrincipal`.

### <a name="sqldriverconnect-prompt-windows-driver-only"></a>Invite SQLDriverConnect (pilote Windows uniquement)

La boîte de dialogue d’invite affichée par SQLDriverConnect lorsqu’il demande les informations nécessaires pour établir la connexion contient quatre nouvelles options pour l’authentification Azure AD :

![La boîte de dialogue de connexion SQL Server affichée par SQLDriverConnect.](windows/server-login.png)

Ces options correspondent aux six mêmes options disponibles dans l’interface utilisateur de configuration du nom de source de données ci-dessus.

### <a name="example-connection-strings"></a>Exemples de chaîne de connexion
1. Authentification SQL Server : ancienne syntaxe. Le certificat de serveur n’est pas validé et le chiffrement n’est utilisé que si le serveur l’applique. Le nom d’utilisateur/mot de passe est transmis dans la chaîne de connexion.
`server=Server;database=Database;UID=UserName;PWD=Password;`
2. Authentification SQL : nouvelle syntaxe. Le client demande le chiffrement (la valeur par défaut de `Encrypt` est `true`) et le certificat de serveur est validé, quel que soit le paramètre de chiffrement (sauf si `TrustServerCertificate` a la valeur `true`). Le nom d’utilisateur/mot de passe est transmis dans la chaîne de connexion.
 `server=Server;database=Database;UID=UserName;PWD=Password;Authentication=SqlPassword;`
3. Authentification Windows intégrée (Kerberos sur Linux et macOS) avec SSPI (vers SQL Server ou SQL IaaS) : syntaxe actuelle. Le certificat de serveur n’est pas validé, sauf si le chiffrement est utilisé. 
`server=Server;database=Database;Trusted_Connection=yes;`
4. (_Pilote Windows uniquement_.) Authentification Windows intégrée avec SSPI (si la base de données cible se trouve dans SQL Server ou SQL IaaS) : nouvelle syntaxe. Le client demande le chiffrement (la valeur par défaut de `Encrypt` est `true`) et le certificat de serveur est validé, quel que soit le paramètre de chiffrement (sauf si `TrustServerCertificate` a la valeur `true`). 
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
5. Authentification par nom d’utilisateur/mot de passe Azure AD (si la base de données cible se trouve dans Azure SQL Database). Le certificat de serveur est validé, quel que soit le paramètre de chiffrement (sauf si `TrustServerCertificate` a la valeur `true`). Le nom d’utilisateur/mot de passe est transmis dans la chaîne de connexion. 
`server=Server;database=Database;UID=UserName;PWD=Password;Authentication=ActiveDirectoryPassword;`
6. (_pilote Windows et Linux/macOS 17.6+ seulement_.) Authentification Windows intégrée avec ADAL, qui implique l’acceptation d’informations d’identification de compte Windows contre un jeton d’accès émis par Azure AD, en supposant que la base de données cible se trouve dans Azure SQL Database. Le certificat de serveur est validé, quel que soit le paramètre de chiffrement (sauf si `TrustServerCertificate` a la valeur `true`). Sur Linux/macOS, un ticket Kerberos approprié doit être disponible. Pour plus d’informations, consultez la section ci-dessous sur les comptes fédérés et [Utilisation de l’authentification intégrée](linux-mac/using-integrated-authentication.md).
`server=Server;database=Database;Authentication=ActiveDirectoryIntegrated;`
7. (_Pilote Windows uniquement_.) L’authentification interactive Azure AD utilise la technologie authentification multifacteur Azure Active Directory pour configurer la connexion. Dans ce mode, lorsque l’ID de connexion est fourni, une boîte de dialogue d’authentification Azure est déclenchée, permettant ainsi à l’utilisateur d’entrer le mot de passe pour établir la connexion. Le nom d’utilisateur est transmis dans la chaîne de connexion.
`server=Server;database=Database;UID=UserName;Authentication=ActiveDirectoryInteractive;`

![Interface utilisateur de l’authentification Windows Azure lors de l’utilisation de l’authentification interactive Active Directory.](windows/WindowsAzureAuth.png)

8. L’authentification Managed Identity Azure Active Directory utilise l’identité attribuée par le système ou par l’utilisateur comme authentification pour configurer la connexion. Pour l’identité attribuée par l’utilisateur, UID est défini sur l’ID d’objet de l’identité d’utilisateur.<br>
Pour l'identité affectée par le système,<br>
`server=Server;database=Database;Authentication=ActiveDirectoryMsi;`<br>
Pour l’identité affectée par l’utilisateur avec l’ID objet myObjectId,<br>
`server=Server;database=Database;UID=myObjectId;Authentication=ActiveDirectoryMsi;`

9. Authentification par principal de service Azure Active Directory `server=Server;databse=Database;UID=clientId;PWD=clientSecret;Authentication=ActiveDirectoryServicePrincipal;`

> [!NOTE]
>- Si vous utilisez les options Active Directory avec le pilote ODBC Windows **_antérieur à_* _ la version 17.4.2, vérifiez que la [Bibliothèque d’authentification Active Directory pour SQL Server](https://go.microsoft.com/fwlink/?LinkID=513072) a été installée. Si vous utilisez les pilotes Linux et macOS, vérifiez que `libcurl` est installé. Pour la version 17.2 et les versions ultérieures du pilote, il ne s’agit pas d’une dépendance explicite, car elle n’est pas requise pour les autres méthodes d’authentification ni les opérations ODBC.
>- Lorsque la configuration Azure Active Directory comprend des stratégies d’accès conditionnel et que le client est Windows 10 ou Server 2016 ou une version ultérieure, l’authentification via Integrated ou le nom d’utilisateur/mot de passe peut échouer. Les stratégies d’accès conditionnel nécessitent l’utilisation du Gestionnaire de comptes Windows (WAM), qui est pris en charge dans le pilote version 17.6 ou ultérieure pour Windows. Pour utiliser WAM, créez une nouvelle chaîne ou une valeur DWORD nommée `ADALuseWAM` dans `HKLM\Software\ODBC\ODBCINST.INI\ODBC Driver 17 for SQL Server`, `HKCU\Software\ODBC\ODBC.INI\<your-user-DSN-name>` ou `HKLM\Software\ODBC\ODBC.INI\<your-system-DSN-name>` pour la configuration globale, le nom de source de données de l’utilisateur ou la configuration de l’étendue du nom de source de données système et attribuez-lui la valeur 1. Notez que l’authentification avec WAM ne prend pas en charge l’exécution de l’application en tant qu’utilisateur différent avec `runas`. Les scénarios qui requièrent des stratégies d’accès conditionnel ne sont pas pris en charge pour Linux ou macOS.
>- Pour vous connecter avec un nom d’utilisateur de compte SQL Server et un mot de passe, vous pouvez maintenant utiliser la nouvelle option `SqlPassword`, tout particulièrement recommandée pour Azure SQL, car elle offre des paramètres de connexion plus sécurisés.
>- Pour vous connecter avec un nom d’utilisateur de compte Azure Active Directory et un mot de passe, spécifiez `Authentication=ActiveDirectoryPassword` dans la chaîne de connexion et les mots clés `UID` et `PWD` avec respectivement le nom d’utilisateur et le mot de passe.
>- Pour vous connecter avec l’authentification intégrée Windows ou Active Directory (pilote Windows et Linux/macOS 17.6+ seulement), spécifiez `Authentication=ActiveDirectoryIntegrated` dans la chaîne de connexion. Le pilote choisira automatiquement le mode d’authentification approprié. `UID` et `PWD` ne doivent pas être spécifiés.
>- Pour vous connecter avec l’authentification interactive Active Directory (pilote Windows uniquement), spécifiez `UID`.

## <a name="authenticating-with-an-access-token"></a>S’authentifier avec un jeton d’accès

L’attribut préconnexion `SQL_COPT_SS_ACCESS_TOKEN` permet d’utiliser un jeton d’accès obtenu auprès d’Azure AD pour l’authentification au lieu du nom d’utilisateur et du mot de passe, et de contourner la négociation et l’obtention d’un jeton d’accès par le pilote. Pour utiliser un jeton d’accès, définissez l’attribut de connexion `SQL_COPT_SS_ACCESS_TOKEN` sur un pointeur désignant une structure `ACCESSTOKEN` :

~~~
typedef struct AccessToken
{
    DWORD dataSize;
    BYTE data[];
} ACCESSTOKEN;
~~~

`ACCESSTOKEN` est une structure de longueur variable composée d’une _longueur_ de 4 octets suivie de _longueur_ octets de données opaques qui constituent le jeton d’accès. En raison de la façon dont SQL Server gère les jetons d’accès, un jeton d’accès obtenu au moyen d’une réponse JSON [OAuth 2.0](/azure/active-directory/develop/active-directory-authentication-scenarios) doit être développé afin que chaque octet soit suivi d’un octet de remplissage 0, comme une chaîne UCS-2 contenant uniquement des caractères ASCII. Toutefois, le jeton est une valeur opaque et la longueur spécifiée, en octets, ne doit inclure AUCUNE marque de fin Null. En raison de leurs contraintes de format et de longueur considérables, cette méthode d’authentification n’est disponible que programmatiquement, par le biais de l’attribut de connexion `SQL_COPT_SS_ACCESS_TOKEN` ; Il n’existe aucun mot clé de chaîne de connexion ou de nom de source de données correspondant. La chaîne de connexion ne doit pas contenir de mots clés `UID`, `PWD`, `Authentication` ni `Trusted_Connection`.

> [!NOTE]
> La version 13.1 du pilote ODBC ne prend en charge cette authentification que sur _Windows_.

## <a name="azure-active-directory-authentication-sample-code"></a>Exemple de code d’authentification Azure Active Directory

L’exemple suivant illustre le code nécessaire pour se connecter à SQL Server à l’aide d’Azure Active Directory avec les mots clés de connexion. Notez qu’il n’est pas nécessaire de modifier le code d’application lui-même ; la chaîne de connexion ou le nom de source de données s’il est utilisé, est la seule modification nécessaire pour avoir recours à l’authentification Azure AD :
~~~
    ...
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;PWD=myPass;Authentication=ActiveDirectoryPassword"
    ...
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);    
    ...
~~~
L’exemple suivant illustre le code nécessaire pour se connecter à SQL Server à l’aide d’Azure Active Directory avec l’authentification par jeton d’accès. Dans ce cas, il est nécessaire de modifier le code de l’application pour traiter le jeton d’accès et définir l’attribut de connexion associé.
~~~
    SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server}"
    SQLCHAR accessToken[] = "eyJ0eXAiOi..."; // In the format extracted from an OAuth JSON response
    ...
    DWORD dataSize = 2 _ strlen(accessToken);
    ACCESSTOKEN *pAccToken = malloc(sizeof(ACCESSTOKEN) + dataSize);
    pAccToken->dataSize = dataSize;
    // Expand access token with padding bytes
    for(int i = 0, j = 0; i < dataSize; i += 2, j++) {
        pAccToken->data[i] = accessToken[j];
        pAccToken->data[i+1] = 0;
    }
    ...
    SQLSetConnectAttr(hDbc, SQL_COPT_SS_ACCESS_TOKEN, (SQLPOINTER)pAccToken, SQL_IS_POINTER);
    SQLDriverConnect(hDbc, NULL, connString, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);        
    ...
    free(pAccToken);
~~~
Voici un exemple de chaîne de connexion à utiliser avec l’authentification interactive Azure Active Directory. Il est à noter qu’il ne contient pas le champ PWD, car le mot de passe est entré sur l’écran Authentification Azure.
~~~
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myuser;Authentication=ActiveDirectoryInteractive"
~~~
Voici un exemple de chaîne de connexion à utiliser avec l’authentification Managed Identity Azure Active Directory. Notez que l’UID est défini sur l’ID d’objet de l’identité attribuée par d’utilisateur.
~~~
// For system-assigned identity,
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};Authentication=ActiveDirectoryMsi"
...
// For user-assigned identity with object ID equals to myObjectId
SQLCHAR connString[] = "Driver={ODBC Driver 17 for SQL Server};Server={server};UID=myObjectId;Authentication=ActiveDirectoryMsi"
~~~

## <a name="considerations-for-using-adfs-federated-accounts-on-linuxmacos"></a>Considérations relatives à l’utilisation des comptes fédérés ADFS sur Linux/macOS

À partir de la version 17.6, les pilotes pour Linux et macOS prennent en charge l’authentification à l’aide des comptes fédérés par ADFS Azure Active Directory à l’aide d’un nom d’utilisateur/mot de passe (`ActiveDirectoryPassword`) ou de Kerberos (`ActiveDirectoryIntegrated`). Certaines limitations dépendent de la plateforme lorsque vous utilisez le mode intégré.

Lors de l’authentification auprès d’un utilisateur dont le suffixe UPN est différent du domaine Kerberos, par exemple, un autre suffixe UPN est utilisé, il est nécessaire d’utiliser l’option Principal de l’entreprise (utilisez l’option `-E` avec `kinit` et fournissez le nom du principal sous la forme `user@federated-domain`) lors de l’obtention des tickets Kerberos. Cela permet au pilote de déterminer correctement le domaine fédéré et le domaine Kerberos.

Vous pouvez vérifier qu’un ticket Kerberos approprié est disponible en inspectant la sortie de la commande `klist`. Si le domaine fédéré est le même que le domaine Kerberos et le suffixe UPN, le nom du principal sera sous la forme `user@realm`. S’il est différent, le nom du principal doit se présenter sous la forme `user@federated-domain@realm`.

### <a name="linux"></a>Linux

Sur SuSE 11, la version de la bibliothèque Kerberos par défaut de 1.6.x ne prend pas en charge l’option Principal d’entreprise nécessaire pour utiliser d’autres suffixes UPN. Pour utiliser d’autres suffixes UPN avec l’authentification intégrée Azure AD, mettez à niveau la bibliothèque Kerberos vers 1.7 ou une version plus récente.

Sur Alpine Linux, le `libcurl` par défaut ne prend pas en charge l’authentification SPNEGO/Kerberos requise pour l’authentification intégrée Azure AD.

### <a name="macos"></a>macOS

La bibliothèque Kerberos système `kinit` prend en charge le Principal d’entreprise avec l’option `--enterprise`, mais effectue également implicitement une canonicalisation des noms, ce qui empêche l’utilisation d’autres suffixes UPN. Pour utiliser d’autres suffixes UPN avec l’authentification intégrée Azure AD, installez une bibliothèque Kerberos plus récente via `brew install krb5` et utilisez son `kinit` avec l’option `-E` comme décrit ci-dessus.

## <a name="see-also"></a>Voir aussi

[L’authentification basée sur le jeton prend en charge Azure SQL Database à l’aide de l’authentification Azure AD](/archive/blogs/sqlsecurity/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth)

[Utilisation de l’authentification intégrée](linux-mac/using-integrated-authentication.md)