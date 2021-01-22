---
description: Approvisionner des clés prenant en charge les enclaves
title: Provisionner des clés activées pour les enclaves | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: e28b6d18b5fe466aa239164b18ebdfe5fef0895c
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534758"
---
# <a name="provision-enclave-enabled-keys"></a>Approvisionner des clés prenant en charge les enclaves

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Cet article explique comment provisionner des clés activées pour les enclaves qui prennent en charge les calculs à l’intérieur des enclaves sécurisées côté serveur utilisées pour [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md). 

Les instructions générales et les processus de [gestion des clés Always Encrypted](overview-of-key-management-for-always-encrypted.md) s’appliquent quand vous provisionnez des clés activées pour les enclaves. Cet article traite des détails spécifiques à Always Encrypted avec enclaves sécurisées.

Pour provisionner une clé principale de colonne activée pour les enclaves en utilisant SQL Server Management Studio ou PowerShell, vérifiez que la nouvelle clé prend en charge les calculs d’enclave. L’outil (SSMS ou PowerShell) génère alors l’instruction `CREATE COLUMN MASTER KEY`, qui définit `ENCLAVE_COMPUTATIONS` dans les métadonnées de clé principale des colonnes dans la base de données. Pour plus d’informations, consultez [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md). 

L’outil signe également numériquement les propriétés principales de colonne avec la clé principale de colonne et stocke la signature dans les métadonnées de la base de données. La signature empêche la falsification malveillante avec le paramètre `ENCLAVE_COMPUTATIONS`. Les pilotes du client SQL vérifient les signatures avant d’autoriser l’utilisation de l’enclave. Cela permet aux administrateurs de sécurité de contrôler des données de la colonne pouvant être calculées à l’intérieur de l’enclave.

`ENCLAVE_COMPUTATIONS` est non modifiable, ce qui signifie que vous ne pouvez pas le changer une fois que vous avez défini la clé principale de colonne dans les métadonnées. Pour activer les calculs d’enclave en utilisant une clé de chiffrement de colonne chiffrée par une clé principale de colonne donnée, vous devez effectuer une rotation de la clé principale de colonne et la remplacer par une clé principale de colonne activée pour les enclaves. Consultez [Effectuer une rotation de clés activées pour les enclaves](always-encrypted-enclaves-rotate-keys.md).

> [!NOTE]
> Actuellement, SSMS et PowerShell prennent en charge les clés principales de colonne activées pour les enclaves stockées dans Azure Key Vault ou dans le magasin de certificats Windows. Les modules de sécurité matériels (avec CNG ou CAPI) ne sont pas pris en charge.

Pour créer une clé de chiffrement de colonne activée pour les enclaves, vous devez veiller à sélectionner une clé principale de colonne activée pour les enclaves pour chiffrer la nouvelle clé. 

Les sections suivantes fournissent plus de détails sur le provisionnement des clés activées pour les enclaves avec SSMS et PowerShell.

## <a name="provision-enclave-enabled-keys-using-sql-server-management-studio"></a>Provisionner des clés activées pour les enclaves avec SQL Server Management Studio
Dans SQL Server Management Studio, vous pouvez provisionner :
- Une clé principale de colonne activée pour les enclaves en utilisant la boîte de dialogue **Nouvelle clé principale de colonne**.
- Une clé principale de colonne activée pour les enclaves en utilisant la boîte de dialogue **Nouvelle clé de chiffrement de colonne**.

> [!NOTE]
> Actuellement, l’[Assistant Always Encrypted](always-encrypted-wizard.md) ne prend pas en charge la génération de clés activées pour les enclaves. Vous pouvez cependant créer des clés activées pour les enclaves en utilisant les boîtes de dialogue ci-dessus puis, quand vous exécutez l’Assistant, sélectionner un chiffrement de colonne activée pour les enclaves déjà existant pour les colonnes que vous voulez chiffrer.

Version minimale requise de SSMS :

- SSMS 18.3 avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SSMS 18.8 avec [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="provision-enclave-enabled-column-master-keys-with-the-new-column-master-key-dialog"></a>Provisionner des clés principales de colonne activées pour les enclaves en utilisant la boîte de dialogue Nouvelle clé principale de colonne
Pour provisionner une clé principale de colonne activée pour les enclaves, suivez les étapes décrites dans [Provisionner des clés principales de colonne avec la boîte de dialogue Nouvelle clé principale de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog). Veillez à sélectionner **Autoriser les calculs d’enclave**. Consultez la capture d’écran ci-dessous :

![Autoriser les calculs d’enclave](./media/always-encrypted-enclaves/allow-enclave-computations.png)

> [!NOTE]
> La case à cocher **Autoriser les calculs d’enclave** apparaît uniquement si une enclave sécurisée est configurée pour votre base de données. Si vous utilisez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Configurer l’enclave sécurisée dans SQL Server](always-encrypted-enclaves-configure-enclave-type.md). Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], consultez [Activer Intel SGX pour votre base de données Azure SQL](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).

> [!TIP]
> Pour vérifier si une clé principale de colonne est activée pour les enclaves, cliquez sur celle-ci avec le bouton droit dans l’Explorateur d’objets, puis sélectionnez **Propriétés**. Si la clé est activée pour les enclaves, **Calculs d’enclave : Autorisés** apparaît dans la fenêtre montrant les propriétés de la clé. Vous pouvez aussi utiliser la vue [sys.column_master_keys (Transact-SQL)](../../system-catalog-views/sys-column-master-keys-transact-sql.md).

### <a name="provision-enclave-enabled-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Provisionner des clés principales de colonne activées pour les enclaves avec la boîte de dialogue Nouvelle clé de chiffrement de colonne
Pour provisionner une clé de chiffrement de colonne activée pour les enclaves, suivez les étapes décrites dans [Provisionner des clés de chiffrement de colonne avec la boîte de dialogue Nouvelle clé de chiffrement de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog). Quand vous sélectionnez une clé principale de colonne, vérifiez qu’elle est activée pour les enclaves.

> [!TIP]
> Pour vérifier si une clé de chiffrement de colonne est activée pour les enclaves, cliquez sur celle-ci avec le bouton droit dans l’Explorateur d’objets, puis sélectionnez **Propriétés**. Si la clé est activée pour les enclaves, **Calculs d’enclave : Autorisés** apparaît dans la fenêtre montrant les propriétés de la clé.

## <a name="provision-enclave-enabled-keys-using-powershell"></a>Approvisionner des clés prenant en charge les enclaves avec PowerShell
Pour provisionner des clés prenant en charge les enclaves avec PowerShell, vous avez besoin du module PowerShell SqlServer version 21.1.18179 ou ultérieure.

En général, les workflows de provisionnement de clés PowerShell (avec et sans séparation des rôles) pour Always Encrypted, décrits dans [Provisionner des clés Always Encrypted en utilisant PowerShell](configure-always-encrypted-keys-using-powershell.md) s’appliquent également aux clés activées pour les enclaves. Cette section décrit les détails spécifiques aux clés activées pour les enclaves.

Le module SqlServer PowerShell étend les applets de commande [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) et [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings) avec le paramètre `-AllowEnclaveComputations` pour vous permettre de spécifier une clé principale de colonne qui est activée pour les enclaves pendant le processus de provisionnement. Les deux applets de commande créent un objet local contenant les propriétés d’une clé principale de colonne (stockée dans Azure Key Vault ou dans le magasin de certificats Windows). Si ce paramètre est spécifié, la propriété `-AllowEnclaveComputations` marque la clé comme étant activée pour les enclaves dans l’objet local. Elle a aussi pour effet que l’applet de commande accède à la clé principale de colonne référencée (dans Azure Key Vault ou dans le magasin de certificats Windows) pour signer numériquement les propriétés de la clé. Une fois que vous avez créé un objet de paramètres pour une nouvelle clé principale de colonne activée pour les enclaves, vous pouvez l’utiliser dans un appel ultérieur de l’applet de commande [**New-SqlColumnMasterKey**](/powershell/module/sqlserver/new-sqlcolumnmasterkey) pour créer un objet de métadonnées décrivant la nouvelle clé dans la base de données.

Le provisionnement des clés de chiffrement de colonne activées pour les enclaves n’est pas différent de celui des clés de chiffrement de colonne qui ne sont pas activées pour les enclaves. Vous devez simplement vérifier qu’une clé principale de colonne utilisée pour chiffrer la nouvelle clé de chiffrement de colonne est activée pour les enclaves.

> [!NOTE]
> Le module SqlServer PowerShell ne prend actuellement pas en charge le provisionnement de clés activées pour les enclaves stockées dans des modules de sécurité matériels (avec CNG ou CAPI).

### <a name="example---provision-enclave-enabled-keys-using-windows-certificate-store"></a>Exemple - Provisionner des clés activées pour les enclaves en utilisant le magasin de certificats Windows
L’exemple complet ci-dessous montre comment configurer des clés activées pour les enclaves, en stockant la clé principale de colonne dans le magasin de certificats Windows. Le script est basé sur l’exemple de [Magasin de certificats Windows sans séparation des rôles (exemple)](configure-always-encrypted-keys-using-powershell.md#windows-certificate-store-without-role-separation-example). Il est important de noter que l’utilisation du paramètre `-AllowEnclaveComputations` dans le l’applet de commande [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) est la seule différence entre les workflows dans les deux exemples.

```powershell
# Create a column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key
# using the -AllowEnclaveComputations parameter.
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

### <a name="example---provision-enclave-enabled-keys-using-azure-key-vault"></a>Exemple - Provisionner des clés activées pour les enclaves en utilisant Azure Key Vault
L’exemple complet ci-dessous montre comment configurer des clés activées pour les enclaves, en stockant la clé principale de colonne dans Azure Key Vault. Le script est basé sur l’exemple de [Azure Key Vault sans séparation des rôles (exemple)](configure-always-encrypted-keys-using-powershell.md#azure-key-vault-without-role-separation-example). Il est important de noter deux différences entre le workflow pour les clés activées pour les enclaves et celui pour les clés qui ne sont pas activées pour les enclaves. 
- Dans le script ci-dessous, l’applet de commande [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) utilise le paramètre `-AllowEnclaveComputations` pour rendre la nouvelle clé principale de colonne activée pour les enclaves. 
- Le script ci-dessous appelle l’applet de commande [**Add-SqlAzureAuthenticationContext**](/powershell/module/sqlserver/add-sqlazureauthenticationcontext) pour se connecter à Azure avant d’appeler l’applet de commande [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings). Vous devez d’abord vous connecter à Azure, car le paramètre `-AllowEnclaveComputations` fait que l’applet de commande **New-SqlAzureKeyVaultColumnMasterKeySettings** appelle Azure Key Vault pour signer les propriétés de la clé principale de colonne.

```powershell
# Create a column master key in Azure Key Vault.
Import-Module Az
Connect-AzAccount
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"
$azureCtx = Set-AzConteXt -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if your desired group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Authenticate to Azure - it is required before calling the next cmdlet.
Add-SqlAzureAuthenticationContext -Interactive

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="next-steps"></a>Étapes suivantes
- [Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées](always-encrypted-enclaves-query-columns.md)
- [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md)
- [Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Voir aussi  
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Gérer des clés pour Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-manage-keys.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)