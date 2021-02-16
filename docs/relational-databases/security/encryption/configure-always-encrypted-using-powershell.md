---
title: Configurer Always Encrypted à l’aide de PowerShell | Microsoft Docs
description: Découvrez comment importer et utiliser le module SqlServer PowerShell qui fournit des applets de commande pour configurer Always Encrypted dans Azure SQL Database et SQL Server.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 12f2bde5-e100-41fa-b474-2d2332fc7650
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c5768357f83252c4d71ac1aeff064364a63367b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345401"
---
# <a name="configure-always-encrypted-using-powershell"></a>Configurer Always Encrypted à l’aide de PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Le module SqlServer PowerShell fournit des applets de commande pour configurer [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) dans [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)] ou [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

## <a name="security-considerations-when-using-powershell-to-configure-always-encrypted"></a>Considérations de sécurité sur l’utilisation de PowerShell pour configurer Always Encrypted

L’objectif principal d’Always Encrypted étant de garantir la sécurité des données sensibles chiffrées même si le système de base de données est compromis, l’exécution d’un script PowerShell qui traite des clés ou des données sensibles sur l’ordinateur SQL Server peut réduire ou annuler les avantages de la fonctionnalité. Pour obtenir des recommandations supplémentaires relatives à la sécurité, consultez [Considérations en matière de sécurité pour la gestion des clés](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

Vous pouvez utiliser PowerShell pour gérer des clés Always Encrypted avec et sans séparation des rôles, ce qui permet de contrôler qui a accès aux clés de chiffrement réelles du magasin de clés et qui a accès à la base de données.

 Pour obtenir des recommandations supplémentaires, consultez [Considérations en matière de sécurité pour la gestion des clés](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

## <a name="prerequisites"></a>Prérequis

Installez le [module SqlServer](/powershell/sqlserver/sqlserver/vlatest/sqlserver) sur un ordinateur sécurisé qui n’est PAS un ordinateur qui héberge votre instance de SQL Server. Vous pouvez installer le module directement à partir de PowerShell Gallery.  Pour plus d’informations, consultez les instructions de [téléchargement](../../../powershell/download-sql-server-ps-module.md).


## <a name="importing-the-sqlserver-module"></a><a name="importsqlservermodule"></a> Importation du module SqlServer 

Pour charger le module SqlServer

1.  Utilisez l’applet de commande **Set-ExecutionPolicy** pour définir la stratégie d’exécution de scripts appropriée.
2.  Utilisez l’applet de commande **Import-Module** pour importer le module SqlServer.

Cet exemple charge le module SQL Server.

```
# Import the SQL Server Module.  
Import-Module "SqlServer" 
```

## <a name="connecting-to-a-database"></a><a name="connectingtodatabase"></a> Connexion à une base de données

Certaines des applets de commande Always Encrypted fonctionnent avec des données ou des métadonnées dans la base de données et nécessitent que vous vous connectiez d’abord à la base de données. Il existe deux méthodes recommandées pour se connecter à une base de données lors de la configuration d’Always Encrypted à l’aide du module SqlServer : 
1. Se connecter à l’aide de l’applet de commande **Get-SqlDatabase**.
2. Se connecter à l’aide d’un fournisseur SQL Server PowerShell.

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="using-get-sqldatabase"></a>Utilisation de Get-SqlDatabase
L’applet de commande **Get-SqlDatabase** vous permet de vous connecter à une base de données dans SQL Server ou Azure SQL Database. Elle retourne un objet de base de données, que vous pouvez ensuite passer à l’aide du paramètre **InputObject** d’une applet de commande qui se connecte à la base de données. 

### <a name="using-sql-server-powershell"></a>Utilisation de SQL Server PowerShell

```
# Import the SqlServer module
Import-Module "SqlServer"  

# Connect to your database
# Set the valid server name, database name and authentication keywords in the connection string
$serverName = "<Azure SQL server name>.database.windows.net"
$databaseName = "<database name>"
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Authentication = Active Directory Integrated"
$database = Get-SqlDatabase -ConnectionString $connStr

# List column master keys for the specified database.
Get-SqlColumnMasterKey -InputObject $database
```

Vous pouvez également utiliser un canal :


```
$database | Get-SqlColumnMasterKey
```

### <a name="using-sql-server-powershell-provider"></a>Utilisation du fournisseur SQL Server PowerShell
Le [fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md) présente la hiérarchie des objets SQL Server dans des chemins semblables aux chemins de système de fichiers. Avec SQL Server PowerShell, vous pouvez parcourir les chemins à l’aide d’alias Windows PowerShell semblables aux commandes que vous utilisez généralement pour parcourir les chemins du système de fichiers. Une fois que vous accédez à l’instance cible et à la base de données, les applets de commande suivantes ciblent cette base de données, comme indiqué dans l’exemple suivant. 

> [!NOTE]
> Cette méthode de connexion à une base de données fonctionne uniquement pour SQL Server (elle n’est pas prise en charge dans Azure SQL Database).

```
# Import the SqlServer module.
Import-Module "SqlServer"
# Navigate to the database in the remote instance.
cd SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
# List column master keys in the above database.
Get-SqlColumnMasterKey
```

 
Vous pouvez également spécifier un chemin de base de données à l’aide du paramètre générique **Path** , au lieu de naviguer dans la base de données.


```
# Import the SqlServer module.
Import-Module "SqlServer" 
# List column master keys for the specified database.
Get-SqlColumnMasterKey -Path SQLSERVER:\SQL\servercomputer\DEFAULT\Databases\yourdatabase
```
 
## <a name="always-encrypted-tasks-using-powershell"></a>Tâches Always Encrypted à l’aide de PowerShell

- [Provisionner des clés Always Encrypted à l’aide de PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [Permuter des clés Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell.md)
- [Chiffrer, rechiffrer ou déchiffrer des colonnes avec Always Encrypted à l’aide de PowerShell](configure-column-encryption-using-powershell.md)


##  <a name="always-encrypted-cmdlet-reference"></a><a name="aecmdletreference"></a> Référence des applets de commande Always Encrypted

Les applets de commande PowerShell suivantes sont disponibles pour Always Encrypted :

|APPLET DE COMMANDE |Description
|:---|:---
|**[Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext)** |Effectue une authentification dans Azure et acquiert un jeton d’authentification.
|**[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)** |Ajoute une nouvelle valeur chiffrée pour un objet de clé de chiffrement de colonne existant dans la base de données.
|**[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)** |Met fin à la permutation d’une clé principale de colonne
|**[Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)**   |Retourne tous les objets de clés de chiffrement de colonne définis dans la base de données, ou retourne un objet de clé de chiffrement de colonne portant le nom spécifié.
|**[Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey)**   |Retourne les objets de clés principales de colonne définis dans la base de données, ou retourne un objet de clé principale de colonne portant le nom spécifié.
|**[Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation)** |Lance la permutation d’une clé principale de colonne.
|**[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)** |Crée un objet SqlColumnMasterKeySettings décrivant une clé asymétrique stockée dans Azure Key Vault.
|**[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)** |Crée un objet SqlColumnMasterKeySettings décrivant une clé asymétrique stockée dans un magasin de clés prenant en charge l’API CNG (Cryptography Next Generation).
|**[New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)**   |Crée un objet clé de chiffrement de colonne dans la base de données.
|**[New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)**   |Génère une valeur chiffrée d’une clé de chiffrement de colonne.
|**[New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings)** |Crée un objet SqlColumnEncryptionSettings qui encapsule des informations sur le chiffrement d’une colonne unique, notamment le type de chiffrement et la clé de chiffrement de colonne.
|**[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)**   |Crée un objet clé principale de colonne dans la base de données.
|**[New-SqlColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkeysettings)**|Crée un objet SqlColumnMasterKeySettings pour une clé principale de colonne avec le fournisseur et le chemin d’accès à la clé spécifiés.
|**[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)** |Crée un objet SqlColumnMasterKeySettings décrivant une clé asymétrique stockée dans un magasin de clés avec un fournisseur de services de chiffrement prenant en charge l’API CAPI (Cryptography API).
|**[Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey)** |Supprime l’objet de clé de chiffrement de colonne de la base de données.
|**[Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue)**   |Supprime une valeur chiffrée d’un objet de clé de chiffrement de colonne existant dans la base de données.
|**[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)** |Supprime l’objet de clé principale de colonne de la base de données.
|**[Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)** |Chiffre, déchiffre ou rechiffre les colonnes spécifiées dans la base de données.



## <a name="see-also"></a>Voir aussi

- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](../../../relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio.md)
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)