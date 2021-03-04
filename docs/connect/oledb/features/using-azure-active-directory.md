---
title: Utilisation d’Azure Active Directory
description: En savoir plus sur les méthodes d’authentification Azure Active Directory disponibles dans Microsoft OLE DB Driver pour SQL Server qui permettent de se connecter aux bases de données Azure SQL.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: f75cfc74616a58975bedf2dc7b2de342c0c57066
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837359"
---
# <a name="using-azure-active-directory"></a>Utilisation d’Azure Active Directory
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>Objectif


À partir de la version [18.2.1](../release-notes-for-oledb-driver-for-sql-server.md#1821), Microsoft OLE DB Driver pour SQL Server permet aux applications OLE DB de se connecter à une instance d’Azure SQL Database à l’aide d’une identité fédérée. Les nouvelles méthodes d’authentification sont les suivantes :
- ID de connexion et mot de passe Azure Active Directory
- Jeton d’accès Azure Active Directory
- Authentification intégrée à Azure Active Directory
- ID de connexion et mot de passe SQL


La version [18.3.0](../release-notes-for-oledb-driver-for-sql-server.md#1830) ajoute le support des méthodes d’authentification suivantes :
- Authentification interactive Azure Active Directory
- Authentification Managed Identity Azure Active Directory

La version [18.5.0](../release-notes-for-oledb-driver-for-sql-server.md#1850) ajoute le support des méthodes d’authentification suivantes :
- Authentification par principal de service Azure Active Directory

> [!NOTE]
> L’utilisation des modes d’authentification suivants avec la `DataTypeCompatibility` (ou la propriété correspondante) définie sur `80` n’est **pas** prise en charge :
> - Authentification Active Directory Azure avec l’ID de connexion et le mot de passe
> - Authentification Azure Active Directory avec le jeton d’accès
> - Authentification intégrée à Azure Active Directory
> - Authentification interactive Azure Active Directory
> - Authentification des identités managées Azure Active Directory
> - Authentification par principal de service Azure Active Directory

## <a name="connection-string-keywords-and-properties"></a>Mots clés et propriétés de chaîne de connexion
Les mots clés de chaîne de connexion suivants ont été introduits pour prendre en charge l’authentification Azure Active Directory :

|Mot clé de chaîne de connexion|Propriété de connexion|Description|
|---               |---                |---        |
|Jeton d'accès|SSPROP_AUTH_ACCESS_TOKEN|Spécifie un jeton d’accès pour s’authentifier auprès d’Azure Active Directory. |
|Authentification|SSPROP_AUTH_MODE|Spécifie la méthode d’authentification à utiliser.|

Pour plus d’informations sur les nouveaux mots clés/propriétés, consultez les pages suivantes :
- [Utilisation de mots clés de chaîne de connexion avec OLE DB Driver pour SQL Server](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [Propriétés d’initialisation et d’autorisation](../ole-db-data-source-objects/initialization-and-authorization-properties.md)

## <a name="encryption-and-certificate-validation"></a>Chiffrement et validation de certificat
Cette section traite des modifications apportées au chiffrement et au comportement de validation des certificats. Ces modifications sont **uniquement** effectives lors de l’utilisation de mots clés de chaîne de connexion de jeton d’accès ou d’authentification (ou leurs propriétés correspondantes).

### <a name="encryption"></a>Chiffrement
Pour améliorer la sécurité, lorsque les nouvelles propriétés/les nouveaux mots clés de connexion sont utilisés, le pilote remplace la valeur de chiffrement par défaut en la définissant sur `yes`. La substitution se produit au moment de l’initialisation de l’objet source de données. Si le chiffrement est défini avant l’initialisation par quelque moyen que ce soit, la valeur est respectée et n’est pas remplacée.

> [!NOTE]   
> Dans les applications ADO et dans les applications qui obtiennent l’interface `IDBInitialize` par le biais de `IDataInitialize::GetDataSource`, le composant principal qui implémente l’interface définit explicitement le chiffrement à sa valeur par défaut de `no`. Par conséquent, les nouvelles propriétés/nouveaux mots clés d’authentification respectent ce paramètre et la valeur de chiffrement **n’est pas** remplacée. Par conséquent, il est **recommandé** que ces applications définissent explicitement `Use Encryption for Data=true` pour remplacer la valeur par défaut.

### <a name="certificate-validation"></a>Validation du certificat
Pour améliorer la sécurité, les nouveaux mots clés/propriétés de connexion respectent le paramètre `TrustServerCertificate` (et les mots clés/propriétés de chaîne de connexion correspondants) **indépendamment du paramètre de chiffrement du client**. Par conséquent, le certificat de serveur est validé par défaut.

> [!NOTE]   
> La validation de certificat peut également être contrôlée via le champ `Value` de l’entrée de registre `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI18.0\GeneralFlags\Flag2`. Les valeurs valides sont `0` ou `1`. Le pilote OLE DB choisit l’option la plus sécurisée entre le registre et les paramètres de propriété/mot clé de la connexion. Autrement dit, le pilote validera le certificat de serveur tant qu’au moins un des paramètres du registre/de la connexion active la validation du certificat du serveur.

## <a name="gui-additions"></a>Ajouts d’interface graphique utilisateur
L’interface utilisateur graphique du pilote a été améliorée pour permettre l’authentification Azure Active Directory. Pour plus d'informations, consultez les pages suivantes :
- [Boîte de dialogue de connexion à SQL Server](../help-topics/sql-server-login-dialog.md)
- [Configuration UDL (Universal Data Link)](../help-topics/data-link-pages.md)

## <a name="example-connection-strings"></a>Exemples de chaîne de connexion
Cette section présente des exemples de mots clés de chaîne de connexion nouveaux et existants à utiliser avec `IDataInitialize::GetDataSource` et la propriété `DBPROP_INIT_PROVIDERSTRING`.

### <a name="sql-authentication"></a>Authentification SQL
- Utilisation de `IDataInitialize::GetDataSource`:
    - Nouveau :
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=SqlPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
    - Dépréciation :
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];User ID=[username];Password=[password];Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    - Nouveau :
        > Server=[server];Database=[database];**Authentication=SqlPassword**;UID=[username];PWD=[password];Encrypt=yes
    - Dépréciation :
        > Server=[server];Database=[database];UID=[username];PWD=[password];Encrypt=yes

### <a name="integrated-windows-authentication-using-security-support-provider-interface-sspi"></a>Authentification Windows intégrée à l’aide de l’interface SSPI (Security Support Provider Interface)

- Utilisation de `IDataInitialize::GetDataSource`:
    - Nouveau :
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
    - Dépréciation :
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Integrated Security=SSPI**;Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    - Nouveau :
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes
    - Dépréciation :
        > Server=[server];Database=[database];**Trusted_Connection=yes**;Encrypt=yes

### <a name="azure-active-directory-username-and-password-authentication"></a>Authentification Active Directory Azure avec nom d’utilisateur et mot de passe

- Utilisation de `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryPassword**;User ID=[username];Password=[password];Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryPassword**;UID=[username];PWD=[password];Encrypt=yes

### <a name="azure-active-directory-integrated-authentication"></a>Authentification intégrée à Azure Active Directory

- Utilisation de `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes

### <a name="azure-active-directory-authentication-using-an-access-token"></a>Authentification Azure Active Directory à l’aide d’un jeton d’accès

- Utilisation de `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Access Token=[access token]** ;Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    > La fourniture d’un jeton d’accès via `DBPROP_INIT_PROVIDERSTRING` n’est pas prise en charge

### <a name="azure-active-directory-interactive-authentication"></a>Authentification interactive Azure Active Directory

- Utilisation de `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryInteractive**;User ID=[username];Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryInteractive**;UID=[username];Encrypt=yes

### <a name="azure-active-directory-managed-identity-authentication"></a>Authentification Managed Identity Azure Active Directory

- Utilisation de `IDataInitialize::GetDataSource`:
    - Identité managée affectée par l’utilisateur :
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;User ID=[Object ID];Use Encryption for Data=true
    - Identité managée affectée par le système :
        > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryMSI**;Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    - Identité managée affectée par l’utilisateur :
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;UID=[Object ID];Encrypt=yes
    - Identité managée affectée par le système :
        > Server=[server];Database=[database];**Authentication=ActiveDirectoryMSI**;Encrypt=yes

### <a name="azure-active-directory-service-principal-authentication"></a>Authentification par principal de service Azure Active Directory

- Utilisation de `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[server];Initial Catalog=[database];**Authentication=ActiveDirectoryServicePrincipal**;User ID=[Application (client) ID];Password=[Application (client) secret];Use Encryption for Data=true
- Utilisation de `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[server];Database=[database];**Authentication=ActiveDirectoryServicePrincipal**;UID=[Application (client) ID];PWD=[Application (client) secret];Encrypt=yes

## <a name="code-samples"></a>Exemples de code

Les exemples suivants montrent le code requis pour se connecter à Azure Active Directory avec les mots clés de connexion. 

### <a name="access-token"></a>Jeton d'accès
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    wchar_t accessToken[] = L"eyJ0eXAiOi...";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Access Token=" + accessToken + L";Use Encryption for Data=true;";
    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```
### <a name="active-directory-integrated"></a>Intégration Active Directory
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Authentication=ActiveDirectoryIntegrated;Use Encryption for Data=true;";

    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr)) 
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```

## <a name="next-steps"></a>Étapes suivantes
- [Autoriser l’accès aux applications web Azure Active Directory à l’aide du flux d’octroi de code OAuth 2.0](/azure/active-directory/azuread-dev/v1-protocols-oauth-code).

- En savoir plus sur l’[authentification Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview) sur SQL Server.

- Configurez les connexions du pilote à l’aide des [mots clés de chaîne de connexion](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md) que le pilote OLE DB prend en charge.