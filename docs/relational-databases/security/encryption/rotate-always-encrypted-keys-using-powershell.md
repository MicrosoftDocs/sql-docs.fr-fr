---
description: Permuter des clés Always Encrypted à l’aide de PowerShell
title: Permuter des clés Always Encrypted à l’aide de PowerShell | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 5117b4fd-c8d3-48d5-87c9-756800769f31
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5d12d2e93c7580e3a70f7f63af085af335bbc5d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340151"
---
# <a name="rotate-always-encrypted-keys-using-powershell"></a>Permuter des clés Always Encrypted à l’aide de PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article fournit les étapes permettant de permuter des clés Always Encrypted à l’aide du module SqlServer PowerShell. Pour plus d’informations sur l’utilisation du module SqlServer PowerShell pour Always Encrypted, consultez [Configurer Always Encrypted à l’aide de PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

La permutation de clés Always Encrypted consiste à remplacer une clé existante par une nouvelle. Vous devrez peut-être permuter une clé si elle a été compromise, ou pour vous conformer aux stratégies ou aux réglementations de conformité de votre organisation qui régissent la permutation périodique des clés de chiffrement. 

Always Encrypted utilisant deux types de clés, il existe deux principaux flux de travail de permutation des clés : la permutation des clés principales de colonne et la permutation de clés de chiffrement de colonne.

* **Permutation des clés de chiffrement de colonne** : elle implique le déchiffrement des données chiffrées avec la clé actuelle, et le rechiffrement des données à l’aide de la nouvelle clé de chiffrement de colonne. La permutation d’une clé de chiffrement de colonne nécessitant l’accès aux clés et à la base de données, la permutation des clés de chiffrement de colonne ne peut être effectuée que sans séparation des rôles.
* **Permutation des clés principales de colonne** : elle implique le déchiffrement de clés de chiffrement de colonne qui sont protégées avec la clé principale de colonne active, leur rechiffrement à l’aide de la nouvelle clé principale de colonne et la mise à jour des métadonnées pour les deux types de clés. La permutation des clés principales de colonne peut être effectuée avec ou sans séparation des rôles (lors de l’utilisation du module SqlServer PowerShell).

## <a name="column-master-key-rotation-without-role-separation"></a>Permutation des clés principales de colonne sans séparation des rôles

La méthode de permutation d’une clé principale de colonne décrite dans cette section ne prend pas en charge la séparation des rôles entre un administrateur de sécurité et un administrateur de base de données. Certaines des étapes ci-dessous combinent des opérations sur les clés physiques avec des opérations sur les métadonnées de clés. Ce flux de travail est donc recommandé pour les organisations qui utilisent le modèle DevOps, ou quand votre base de données est hébergée dans le cloud et que le principal objectif est de restreindre l’accès des administrateurs du cloud (mais pas des administrateurs de base de données) aux données sensibles. Elle n’est pas recommandée si les rivaux potentiels incluent des administrateurs de bases de données, ou si ceux-ci ne doivent pas avoir accès aux données sensibles.  


| Tâche | Article | Accède au magasin de clés/aux clés en texte clair| Accède à la base de données
|:---|:---|:---|:---
|Étape 1. Créer une clé principale de colonne dans un magasin de clés.<br><br>**Remarque :** Le module SqlServer PowerShell ne prend pas en charge cette étape. Pour accomplir cette tâche à partir de la ligne de commande, vous devez utiliser des outils propres à votre magasin de clés. | [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Oui | Non
|Étape 2. Démarrer un environnement PowerShell et importer le module SQL Server. | [Importer le module SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Non | Non
|Étape 3. Se connecter à votre serveur et à la base de données. | [Connexion à une base de données](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Non | Oui
|Étape 4. Créer un objet SqlColumnMasterKeySettings qui contient des informations sur l’emplacement de votre nouvelle clé principale de colonne. SqlColumnMasterKeySettings est un objet qui existe en mémoire (dans PowerShell). Pour le créer, utilisez l’applet de commande propre à votre magasin de clés. |[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)<br> | Non | Non
|Étape 5. Créer les métadonnées relatives à votre nouvelle clé principale de colonne dans votre base de données. | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Remarque :** En arrière-plan, cette applet de commande exécute l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) pour créer des métadonnées de clés. | Non | Oui
|Étape 6. S’authentifier auprès d’Azure, si votre clé principale de colonne actuelle ou nouvelle est stockée dans Azure Key Vault. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Oui | Non
|Étape 7. Démarrer la permutation, en chiffrant chacune des clés de chiffrement de colonne, qui sont actuellement protégées avec l’ancienne clé principale de colonne, à l’aide de la nouvelle clé principale de colonne. Après cette étape, chaque clé de chiffrement de colonne concernée (associée à l’ancienne clé principale de colonne à permuter) est chiffrée avec l’ancienne et la nouvelle clé principale de colonne, et a deux valeurs chiffrées dans les métadonnées de base de données.| [Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation) | Oui | Oui
|Étape 8 : Assurer la coordination avec les administrateurs de toutes les applications qui interrogent des colonnes chiffrées dans la base de données (et qui sont protégées avec l’ancienne clé principale de colonne), pour qu’ils puissent garantir que les applications peuvent accéder à la nouvelle clé principale de colonne.| [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Oui | Non
|Étape 9. Terminer la permutation.<br><br>**Remarque :** Avant d’exécuter cette étape, vérifiez que toutes les applications qui interrogent des colonnes chiffrées protégées avec l’ancienne clé principale de colonne ont été configurées pour utiliser la nouvelle clé principale de colonne. Si vous effectuez cette étape prématurément, certaines de ces applications risquent de ne pas pouvoir déchiffrer les données. Terminez la permutation en supprimant les valeurs chiffrées de la base de données qui ont été créées avec l’ancienne clé principale de colonne. Cette opération supprime l’association entre l’ancienne clé principale de colonne et les clés de chiffrement de colonne qu’elle protège. |[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)| Non | Oui
|Étape 10. Supprimer les métadonnées de l’ancienne clé principale de colonne. |[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| Non | Oui

> [!NOTE]
> Nous vous recommandons vivement de ne pas supprimer définitivement l’ancienne clé principale de colonne après la permutation. Au lieu de cela, laissez l’ancienne clé principale de colonne dans son magasin de clés actuel ou archivez-la dans un autre emplacement sécurisé. Si vous restaurez votre base de données à partir d’un fichier de sauvegarde à un point dans le temps *avant* la configuration de la nouvelle clé principale de colonne, vous aurez besoin de l’ancienne clé pour accéder aux données.

### <a name="rotating-a-column-master-key-without-role-separation-windows-certificate-example"></a>Permutation d’une clé principale de colonne sans séparation des rôles (exemple avec certificat Windows)

Le script ci-dessous est un exemple de bout en bout qui remplace une clé principale de colonne existante (CMK1) par une nouvelle clé principale de colonne (CMK2).

```powershell
# Create a new column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create metadata for your new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings

# Initiate the rotation from the current column master key to the new column master key.
$oldCmkName = "CMK1"
Invoke-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName -TargetColumnMasterKeyName $newCmkName -InputObject $database

# Complete the rotation of the old column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key metadata.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="column-master-key-rotation-with-role-separation"></a>Permutation des clés principales de colonne avec séparation des rôles

Le flux de travail de permutation de clé principale de colonne décrit dans cette section garantit la séparation entre un administrateur de la sécurité et un administrateur de base de données.

> [!IMPORTANT]
> Avant d’exécuter des étapes où *Accède au magasin de clés/aux clés en texte clair*=**Oui** dans le tableau ci-dessous (étapes qui accèdent à des clés en texte clair ou au magasin de clés), vérifiez que l’environnement PowerShell s’exécute sur un ordinateur sécurisé différent de celui qui héberge votre base de données. Pour plus d’informations, consultez [Considérations en matière de sécurité pour la gestion des clés](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management).

### <a name="part-1-dba"></a>Partie 1 : Administrateur de base de données

Un administrateur de base de données récupère les métadonnées concernant la clé principale de colonne à permuter, et concernant les clés de chiffrement de colonne affectées, qui sont associées à la clé principale de colonne active. L’administrateur de base de données partage toutes ces informations avec un administrateur de la sécurité.


| Tâche | Article | Accède au magasin de clés/aux clés en texte clair| Accède à la base de données
|:---|:---|:---|:---
|Étape 1. Démarrer un environnement PowerShell et importer le module SqlServer. | [Importer le module SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Non | None
|Étape 2. Se connecter à votre serveur et à une base de données. | [Se connecter à une base de données](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Non | Oui
|Étape 3. Récupérer les métadonnées relatives à l’ancienne clé principale de colonne.| [Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey) | Non | Oui
|Étape 4. Récupérer les métadonnées relatives aux clés de chiffrement de colonne, protégées avec l’ancienne clé principale de colonne, notamment leurs valeurs chiffrées. | [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey) | Non | Oui
|Étape 5. Partager l’emplacement de la clé principale de colonne (nom du fournisseur et chemin d’accès à la clé principale de colonne) et les valeurs chiffrées des clés de chiffrement de colonne correspondantes, protégées avec l’ancienne clé principale de colonne.| Consultez les exemples ci-dessous. | Non | Non

### <a name="part-2-security-administrator"></a>Partie 2 : Administrateur de la sécurité

L’administrateur de la sécurité génère une nouvelle clé principale de colonne, rechiffre les clés de chiffrement de colonne affectées avec la nouvelle clé principale de colonne, puis partage avec l’administrateur les informations relatives à la nouvelle clé principale de colonne ainsi que l’ensemble des nouvelles valeurs chiffrées pour les clés de chiffrement de colonne affectées.

| Tâche | Article | Accède aux clés en texte clair/au magasin de clés| Accède à la base de données
|:---|:---|:---|:---
|Étape 1. Obtenir auprès de votre administrateur de base de données l’emplacement de l’ancienne clé principale de colonne et les valeurs chiffrées des clés de chiffrement de colonne correspondantes, protégées avec l’ancienne clé principale de colonne.|N/A<br>Consultez les exemples ci-dessous.|Non| Non
|Étape 2. Créer une clé principale de colonne dans un magasin de clés.<br><br>**Remarque :** Le module SqlServer ne prend pas en charge cette étape. Pour accomplir cette tâche à partir d’une ligne de commande, vous devez utiliser les outils propres au type de votre magasin de clés.|[Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Oui | Non
|Étape 3. Démarrer un environnement PowerShell et importer le module SqlServer. | [Importer le module SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Non | Non
|Étape 4. Créer un objet SqlColumnMasterKeySettings qui contient des informations sur l’emplacement de votre **ancienne** clé principale de colonne. SqlColumnMasterKeySettings est un objet qui existe en mémoire (dans PowerShell). |New-SqlColumnMasterKeySettings| Non | Non
|Étape 5. Créer un objet SqlColumnMasterKeySettings qui contient des informations sur l’emplacement de votre **nouvelle** clé principale de colonne. SqlColumnMasterKeySettings est un objet qui existe en mémoire (dans PowerShell). Pour le créer, utilisez l’applet de commande propre à votre magasin de clés. | [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)| Non | Non
|Étape 6. S’authentifier auprès d’Azure, si votre ancienne clé principale de colonne (actuelle) ou votre nouvelle clé principale de colonne est stockée dans Azure Key Vault. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Oui | Non
|Étape 7. Rechiffrer chaque valeur de la clé de chiffrement de colonne, qui est actuellement protégée avec l’ancienne clé principale de colonne, à l’aide de la nouvelle clé principale de colonne. | [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)<br><br>**Remarque :** Quand vous appelez cette applet de commande, passez les objets SqlColumnMasterKeySettings pour l’ancienne et la nouvelle clé de principale de colonne à chiffrer, ainsi qu’une valeur de la clé de chiffrement de colonne.|Oui|Non
|Étape 8 : Partager avec votre administrateur de base de données l’emplacement de la nouvelle clé principale de colonne (nom du fournisseur et chemin d’accès à la clé principale de colonne) et l’ensemble des nouvelles valeurs chiffrées des clés de chiffrement de colonne.| Consultez les exemples ci-dessous. | Non | Non

> [!NOTE]
> Nous vous recommandons vivement de ne pas supprimer définitivement l’ancienne clé principale de colonne après la permutation. Au lieu de cela, laissez l’ancienne clé principale de colonne dans son magasin de clés actuel ou archivez-la dans un autre emplacement sécurisé. Si vous restaurez votre base de données à partir d’un fichier de sauvegarde à un point dans le temps *avant* la configuration de la nouvelle clé principale de colonne, vous aurez besoin de l’ancienne clé pour accéder aux données.

### <a name="part-3-dba"></a>Partie 3 : Administrateur de base de données

L’administrateur de base de données crée des métadonnées pour la nouvelle clé principale de colonne et met à jour les métadonnées des clés de chiffrement de colonne concernées, pour ajouter le nouvel ensemble de valeurs chiffrées. Lors de cette étape, l’administrateur de base de données travaille également en coordination avec les administrateurs des applications qui interrogent des colonnes de chiffrement, qui garantissent que l’application peut accéder à la nouvelle clé principale de colonne. Une fois que toutes les applications sont configurées pour utiliser la nouvelle clé principale de colonne, l’administrateur de base de données supprime l’ancien ensemble de valeurs chiffrées et les anciennes métadonnées de clés principales de colonne.

| Tâche | Article | Accède aux clés en texte clair/au magasin de clés| Accède à la base de données
|:---|:---|:---|:---
|Étape 1. Obtenir auprès de votre administrateur de la sécurité l’emplacement de la nouvelle clé principale de colonne et le nouvel ensemble de valeurs chiffrées des clés de chiffrement de colonne correspondantes, protégées avec l’ancienne clé principale de colonne.| Consultez les exemples ci-dessous. | Non | Non
|Étape 2. Démarrer un environnement PowerShell et importer le module SqlServer. | [Importer le module SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Non | Non
|Étape 3. Se connecter à votre serveur et à une base de données. | [Connexion à une base de données](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Non | Oui
|Étape 4. Créer un objet SqlColumnMasterKeySettings qui contient des informations sur l’emplacement de votre nouvelle clé principale de colonne. SqlColumnMasterKeySettings est un objet qui existe en mémoire (dans PowerShell). |New-SqlColumnMasterKeySettings| Non| Non
|Étape 5. Créer les métadonnées relatives à votre nouvelle clé principale de colonne dans votre base de données.|[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Remarque :** en arrière-plan, cette cmdlet émet l’instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) pour créer des métadonnées de clés. | Non | Oui
|Étape 6. Récupérer les métadonnées relatives aux clés de chiffrement de colonne, protégées avec l’ancienne clé principale de colonne.| [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)| Non | Oui
|Étape 7. Ajouter une nouvelle valeur chiffrée (générée à l’aide de la nouvelle clé principale de colonne) aux métadonnées pour chaque clé de chiffrement de colonne concernée.|[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)|Non|Oui
|Étape 8 : Assurer la coordination avec les administrateurs de toutes les applications qui interrogent des colonnes chiffrées dans la base de données (et qui sont protégées avec l’ancienne clé principale de colonne), pour qu’ils puissent garantir que les applications peuvent accéder à la nouvelle clé principale de colonne.|[Création et stockage des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Non|Non
|Étape 9. Terminer la permutation en supprimant de la base de données les valeurs chiffrées associées à l’ancienne clé principale de colonne.<br><br>**Remarque :** avant d’effectuer cette étape, vérifiez que toutes les applications qui interrogent des colonnes chiffrées protégées avec l’ancienne clé principale de colonne ont été configurées pour utiliser la nouvelle clé principale de colonne. Si vous effectuez cette étape prématurément, certaines de ces applications risquent de ne pas pouvoir déchiffrer les données.<br><br>Cette étape supprime une association entre l’ancienne clé principale de colonne et les clés de chiffrement de colonne qu’elle protège. | [Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)<br><br>Vous pouvez également utiliser [Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue) | Non|Oui
|Étape 10. Supprimer de la base de données les métadonnées de l’ancienne clé principale de colonne.| [Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| Non|Oui

### <a name="rotating-a-column-master-key-with-role-separation-windows-certificate-example"></a>Permutation d’une clé principale de colonne avec séparation des rôles (exemple avec certificat Windows)

Le script ci-dessous est un exemple de bout en bout qui permet de générer une nouvelle clé principale de colonne qui est un certificat dans le Magasin de certificats Windows, afin de remplacer une clé principale de colonne existante (active) par la nouvelle clé principale de colonne. Le script part du principe que la base de données cible contient la clé principale de colonne, nommée CMK1 (à permuter), qui chiffre certaines clés de chiffrement de colonne.

Partie 1 : Administrateur de base de données

```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Retrieve the data about the old column master key, which needs to be rotated.
$oldCmkName = "CMK1"
$oldCmk = Get-SqlColumnMasterKey -Name $oldCmkName -InputObject $database


# Share the location of the old column master key with a Security Administrator, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $oldCmkDataFile
$oldCmk.KeyStoreProviderName +", " + $oldCmk.KeyPath >> $oldCmkDataFile


# Find column encryption keys associated with the old column master key and provide the encrypted values of column encryption keys to the Security Administrator, via a CSV file on a share drive.
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
$oldCekValuesFile = "Z:\oldcekvalues.txt"
"CEKName, CEKEncryptedValue" > $oldCekValuesFile 
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {
        # This column encryption has 2 encrypted values - let's check, if it is associated with the old column master key.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {# This column encryption key has 1 encrypted value that was produced using the old column master key
        # Save the name and the encrypted value of the column encryption key in the file.
        $encryptedValue =  "0x" + -join ($ceks[$i].ColumnEncryptionKeyValues[0].EncryptedValue |  foreach {$_.ToString("X2") } )
        $ceks[$i].Name + "," + $encryptedValue >> $oldCekValuesFile
    }
} 
```


Partie 2 : Administrateur de la sécurité

```powershell
# Obtain the location of the old column master key and the encrypted values of the corresponding column encryption keys, from your DBA, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
$oldCmkData = Import-Csv $oldCmkDataFile
$oldCekValuesFile = "Z:\oldcekvalues.txt"
$oldCekValues = @(Import-Csv $oldCekValuesFile)

# Create a new column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:\" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your old column master key. 
$oldCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $oldCmkData.KeyStoreProviderName -KeyPath $oldCmkData.KeyPath

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation $storeLocation -Thumbprint $cert.Thumbprint


# Prepare a CSV file, you will use to share the encrypted values of column encryption keys, produced using the new column master key.
$newCekValuesFile = "Z:\newcekvalues.txt"
"CEKName, CEKEncryptedValue" > $newCekValuesFile

# Re-encrypt each value with the new column master key and save the new encrypted value in the file.
for($i=0; $i -lt $oldCekValues.Count; $i++){
    # Re-encrypt each value with the new CMK
    $newValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $newCmkSettings -ColumnMasterKeySettings $oldCmkSettings -EncryptedValue $oldCekValues[$i].CEKEncryptedValue
    $oldCekValues[$i].CEKName + ", " + $newValue >> $newCekValuesFile
}

# Share the new column master key data with your DBA, via a CSV file.
$newCmkDataFile = $home + "\newcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $newCmkDataFile
$newCmkSettings.KeyStoreProviderName +", " + $newCmkSettings.KeyPath >> $newCmkDataFile
```


Partie 3 : Administrateur de base de données

```powershell
# Obtain the location of the new column master key and the new encrypted values of the corresponding column encryption keys, from your Security Administrator, via a CSV file on a share drive.
$newCmkDataFile = "Z:\newcmkdata.txt"
$newCmkData = Import-Csv $newCmkDataFile
$newCekValuesFile = "Z:\newcekvalues.txt"
$newCekValues = @(Import-Csv $newCekValuesFile)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $newCmkData.KeyStoreProviderName -KeyPath $newCmkData.KeyPath
# Create metadata for the new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings


# Get all CEK objects
$oldCmkName = "CMK1"
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {# This column encryption key has 2 encrypted values. Let's check, if it is associated with the old CMK.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {
        # Find the corresponding new encrypted value, received from the Security Administrator.
        $newValueRow = ($newCekValues| Where-Object {$_.CEKName -eq $ceks[$i].Name }[0])
        # Update the column encryption key metadata object by adding the new encrypted value
        Add-SqlColumnEncryptionKeyValue -ColumnMasterKeyName $newCmkName -Name $ceks[$i].Name -EncryptedValue $newValueRow.CEKEncryptedValue -InputObject $database 
    }
}

# Complete the rotation of the current column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="rotating-a-column-encryption-key"></a>Permutation d’une clé de chiffrement de colonne

La permutation d’une clé de chiffrement de colonne implique le déchiffrement des données dans toutes les colonnes, chiffrées avec la clé à permuter, et le rechiffrement des données à l’aide de la nouvelle clé de chiffrement de colonne. Ce flux de travail de permutation nécessite l’accès aux clés et à la base de données. Il ne peut donc pas être effectué avec la séparation des rôles. La permutation d’une clé de chiffrement de colonne peut prendre beaucoup de temps si les tables qui contiennent les colonnes chiffrées avec la clé soumise à la permutation sont volumineuses. Votre organisation doit donc apporter un soin particulier à la planification de la permutation des clés de chiffrement de colonne.

Vous pouvez faire pivoter une clé de chiffrement de colonne à l’aide d’une approche hors ligne ou en ligne. La première méthode est susceptible d’être plus rapide, mais vos applications ne peuvent pas écrire dans les tables concernées. La deuxième approche est susceptible de prendre plus de temps, mais vous pouvez limiter l’intervalle de temps pendant lequel les tables concernées sont indisponibles pour les applications. Consultez [Configurer le chiffrement de colonne à l’aide d’Always Encrypted avec PowerShell](configure-column-encryption-using-powershell.md) et [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption/) pour plus de détails.

| Tâche | Article | Accède au magasin de clés/aux clés en texte clair| Accède à la base de données
|:---|:---|:---|:---
|Étape 1. Démarrer un environnement PowerShell et importer le module SqlServer. | [Importer le module SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Non | Non
|Étape 2. Se connecter à votre serveur et à une base de données. | [Connexion à une base de données](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Non | Oui
|Étape 3. S’authentifier auprès d’Azure, si votre clé principale de colonne (qui protège la clé de chiffrement de colonne, soumise à permutation) est stockée dans Azure Key Vault. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Oui | Non
|Étape 4. Générer une clé de chiffrement de colonne, la chiffrer avec la clé principale de colonne et créer les métadonnées de clé de chiffrement de colonne dans la base de données.  | [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Remarque :** Utilisez une variation de l’applet de commande qui génère et chiffre en interne une clé de chiffrement de colonne.<br>En arrière-plan, l’applet de commande exécute l’instruction [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) pour créer les métadonnées de clé. | Oui | Oui
|Étape 5. Rechercher toutes les colonnes chiffrées avec l’ancienne clé de chiffrement de colonne. | [Guide de programmation SMO (SQL Server Management Objects)](../../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) | Non | Oui
|Étape 6. Créer un objet *SqlColumnEncryptionSettings* pour chaque colonne concernée.  SqlColumnMasterKeySettings est un objet qui existe en mémoire (dans PowerShell). Il spécifie le schéma de chiffrement cible pour une colonne. Dans ce cas, l’objet doit spécifier que la colonne concernée doit être chiffrée à l’aide de la nouvelle clé de chiffrement de colonne. | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | Non | Non
|Étape 7. Rechiffrer les colonnes identifiées à l’étape 5 à l’aide de la nouvelle clé de chiffrement de colonne. | [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Remarque :** cette étape peut prendre longtemps. Vos applications ne peuvent pas accéder aux tables pendant toute la durée de l’opération ou pendant une partie de l’opération, en fonction de l’approche sélectionnée (en ligne ou hors ligne). | Oui | Oui
|Étape 8 : Supprimer les métadonnées de l’ancienne clé de chiffrement de colonne. | [Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey) | Non | Oui

### <a name="example---rotating-a-column-encryption-key"></a>Exemple : permutation d’une clé de chiffrement de colonne

Le script ci-dessous illustre la permutation d’une clé de chiffrement de colonne.  Ce script part du principe que la base de données cible contient certaines colonnes chiffrés avec une clé de chiffrement de colonne, nommée CEK1 (à permuter), qui est protégée à l’aide d’une clé principale de colonne, nommée CMK1 (la clé principale de colonne n’est pas stockée dans Azure Key Vault). 


```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Generate a new column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cmkName = "CMK1"
$newCekName = "CEK2"
New-SqlColumnEncryptionKey -Name $newCekName -InputObject $database -ColumnMasterKey $cmkName 


# Find all columns encrypted with the old column encryption key, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$oldCekName = "CEK1"
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted -and $columns[$j].ColumnEncryptionKeyName -eq $oldCekName) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType $columns[$j].EncryptionType -EncryptionKey $newCekName
        }
     }
}

# Re-encrypt all columns, currently encrypted with the old column encryption key, using the new column encryption key.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -UseOnlineApproach -MaxDowntimeInSeconds 120 -LogFileDirectory .

# Remove the old column encryption key metadata.
Remove-SqlColumnEncryptionKey -Name $oldCekName -InputObject $database
```

## <a name="next-steps"></a>Étapes suivantes
- [Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)
  
## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Configurer Always Encrypted à l’aide de PowerShell](configure-always-encrypted-using-powershell.md)
- [Effectuer une rotation des clés Always Encrypted avec SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)