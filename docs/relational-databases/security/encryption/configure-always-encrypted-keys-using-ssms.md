---
title: Provisionner des clés Always Encrypted avec SQL Server Management Studio | Microsoft Docs
description: Découvrez comment approvisionner des clés principales de colonne et des clés de chiffrement de colonne Always Encrypted avec SQL Server Management Studio.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.PAGE.F1
- SQL13.SWB.COLUMNENCRYPTIONKEY.PAGE.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f31a17d3f8bf2285bc41c7462deec4e53008efd1
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237074"
---
# <a name="provision-always-encrypted-keys-using-sql-server-management-studio"></a>Provisionner des clés Always Encrypted avec SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article fournit les étapes nécessaires pour provisionner des clés principales de colonne et des clés de chiffrement de colonne Always Encrypted avec [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

Pour obtenir une vue d’ensemble de la gestion des clés Always Encrypted, notamment des recommandations sur les bonnes pratiques et des considérations importantes sur la sécurité, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

<a name="provisioncmk"></a>
## <a name="provision-column-master-keys-with-the-new-column-master-key-dialog"></a>Provisionner des clés principales de colonne avec la boîte de dialogue Nouvelle clé principale de colonne

La boîte de dialogue **Nouvelle clé principale de colonne** vous permet de générer une clé principale de colonne ou de choisir une clé existante dans un magasin de clés, puis de créer des métadonnées de clé principale de colonne pour la clé créée ou sélectionnée dans la base de données.

1.  À l’aide de **l’Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted** sous votre base de données.
2.  Cliquez avec le bouton droit sur le dossier **Clés principales de colonne** et sélectionnez **Nouvelle clé principale de colonne...** . 
3.  Dans la boîte de dialogue **Nouvelle clé principale de colonne** , entrez le nom de l’objet de métadonnées de clé principale de colonne.
4.  Sélectionnez un magasin de clés :
    - **Magasin de certificats - Utilisateur actuel** : indique l’emplacement du magasin de certificats de l’utilisateur actuel dans le magasin de certificats Windows, qui est votre magasin personnel. 
    - **Magasin de certificats - Ordinateur local** : indique l’emplacement du magasin de certificats de l’ordinateur local dans le magasin de certificats Windows. 
    - **Azure Key Vault** : Vous devez vous connecter à Azure (cliquez sur **Se connecter**). Une fois que vous êtes connecté, vous pouvez choisir un de vos abonnements Azure et un coffre de clés.
    - **Fournisseur du magasin de clés (KSP)**  : indique un magasin de clés accessible via un fournisseur du magasin de clés (KSP) qui implémente l’API de chiffrement nouvelle génération (CNG). En règle générale, ce type de magasin est un module de sécurité matériel. Après avoir sélectionné cette option, vous devez choisir un fournisseur KSP. Le **fournisseur de stockage de clés des logiciels Microsoft** est sélectionné par défaut. Si vous souhaitez utiliser une clé principale de colonne stockée dans un module de sécurité matériel, sélectionnez un fournisseur KSP pour votre appareil (il doit être installé et configuré sur l’ordinateur avant d’ouvrir la boîte de dialogue).
    -   **Fournisseur de services de chiffrement (CSP)** : un magasin de clés accessible via un fournisseur de services de chiffrement (CSP) qui implémente l’API de chiffrement (CAPI). En règle générale, un tel magasin est un module de sécurité matériel. Après avoir sélectionné cette option, vous devez choisir un fournisseur CSP.  Si vous souhaitez utiliser une clé principale de colonne stockée dans un module de sécurité matériel, sélectionnez un fournisseur CSP pour votre appareil (il doit être installé et configuré sur l’ordinateur avant d’ouvrir la boîte de dialogue).
    
    > [!NOTE]
    > CAPI étant une API déconseillée, l’option Fournisseur de services de chiffrement (CAPI) est désactivée par défaut. Vous pouvez l’activer en créant la valeur DWORD Fournisseur CAPI activé sous la clé **[HKEY_CURRENT_USER\Software\Microsoft\Microsoft SQL Server\sql13\Tools\Client\Always Encrypted]** dans le Registre Windows et en lui affectant la valeur 1. Vous devez utiliser CNG au lieu de CAPI, sauf si votre magasin de clés ne prend pas en charge CNG.
   
    Pour plus d’informations sur les magasins de clés ci-dessus, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

5. Si vous utilisez [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] et que votre instance SQL Server est configurée avec une enclave sécurisée, vous pouvez cocher la case **Autoriser les calculs d’enclave** pour rendre la clé principale activée pour les enclaves. Pour plus d’informations, consultez [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md). 

    > [!NOTE]
    > La case à cocher **Autoriser les calculs d’enclave** n’apparaît pas si votre instance SQL Server n’est pas configurée correctement avec une enclave sécurisée.

6.  Choisissez une clé existante dans votre magasin de clés, ou cliquez sur le bouton **Générer une clé** ou **Générer un certificat** pour créer une clé dans le magasin de clés. 
7.  Cliquez sur **OK** et la nouvelle clé apparaît dans la liste. 

Une fois que vous en avez terminé avec la boîte de dialogue, SQL Server Management Studio crée des métadonnées pour votre clé principale de colonne dans la base de données. La boîte de dialogue effectue cette opération en générant et en émettant une instruction [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) .

::: moniker range=">=sql-server-ver15"

Si vous configurez une clé principale de colonne activée pour les enclaves, SSMS signe également les métadonnées en utilisant la clé principale de colonne. 

::: moniker-end

### <a name="permissions-for-provisioning-a-column-master-key"></a>Autorisations pour le provisionnement d’une clé principale de colonne

Vous avez besoin de l’autorisation de base de données *ALTER ANY COLUMN MASTER KEY* dans la base de données pour que la boîte de dialogue crée une clé principale de colonne. Pour utiliser la boîte de dialogue afin de créer une clé principale de colonne ou d’utiliser une clé existante d’un magasin de clés, vous pouvez avoir besoin d’autorisations sur le magasin de clés et/ou sur la clé :
- **Magasin de certificats - Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : Vous avez besoin des autorisations *obtenir* et *lister* pour sélectionner et utiliser une clé, et de l’autorisation *créer* pour créer une clé. Pour configurer une clé principale de colonne activée pour les enclaves, vous avez également besoin de l’autorisation *signer* pour générer une signature des métadonnées de la clé.
- **Fournisseur du magasin de clés (CNG)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="provisioncek"></a> 
## <a name="provision-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Provisionner des clés de chiffrement de colonne avec la boîte de dialogue Nouvelle clé de chiffrement de colonne

La boîte de dialogue **Nouvelle clé de chiffrement de colonne** vous permet de générer une clé de chiffrement de colonne, de la chiffrer avec une clé principale de colonne et de créer des métadonnées de clé de chiffrement de colonne dans la base de données.

1.  À l’aide de **l’Explorateur d’objets**, accédez au dossier **Sécurité/Clés Always Encrypted** sous votre base de données.
2.  Cliquez avec le bouton droit sur le dossier **Clés de chiffrement de colonne** et sélectionnez **Nouvelle clé de chiffrement de colonne…** . 
3.  Dans la boîte de dialogue **Nouvelle clé de chiffrement de colonne** , entrez le nom de l’objet de métadonnées de clé de chiffrement de colonne.
4.  Sélectionnez un objet de métadonnées qui représente votre clé principale de colonne dans la base de données.
5.  Cliquez sur **OK**. 

Une fois que vous en avez terminé avec la boîte de dialogue, SQL Server Management Studio génère une clé de chiffrement de colonne, puis récupère les métadonnées pour la clé principale de colonne que vous avez sélectionnée dans la base de données. SSMS utilise ensuite les métadonnées de la clé principale de colonne pour contacter le magasin de clés qui contient votre clé principale de colonne et pour chiffrer la clé de chiffrement de colonne. Enfin, SSMS crée les données des métadonnées pour le chiffrement de la nouvelle colonne dans la base de données en générant et en émettant une instruction [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md).

### <a name="permissions-for-provisioning-a-column-encryption-key"></a>Autorisations pour l’approvisionnement d’une clé de chiffrement de colonne

Vous avez besoin des autorisations de base de données *ALTER ANY COLUMN ENCRYPTION KEY* et *VIEW ANY COLUMN MASTER KEY DEFINITION* dans la base de données pour que la boîte de dialogue crée les métadonnées de la clé de chiffrement de colonne et accède aux métadonnées de la clé principale de colonne.
Pour accéder à un magasin de clés et utiliser la clé principale de colonne, vous pouvez avoir besoin d’autorisations sur le magasin de clés et/ou la clé :
- **Magasin de certificats - Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations *get*, *unwrapKey*, *wrapKey*, *sign* et *verify* sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="provision-always-encrypted-keys-using-the-always-encrypted-wizard"></a>Provisionner des clés Always Encrypted avec l’Assistant Always Encrypted

L’[Assistant Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-wizard.md) est un outil de chiffrement, de déchiffrement et de rechiffrement de colonnes de base de données sélectionnées. Bien qu’il puisse utiliser des clés déjà configurées, il vous permet également de générer une nouvelle clé principale de colonne et un nouveau chiffrement de colonne. 

## <a name="next-steps"></a>Étapes suivantes
- [Configurer le chiffrement de colonne à l’aide de l’Assistant Always Encrypted](always-encrypted-wizard.md)
- [Configurer le chiffrement de colonne en utilisant Always Encrypted avec un package DAC](configure-always-encrypted-using-dacpac.md)
- [Effectuer une rotation des clés Always Encrypted avec SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)
- [Migrer des données à partir ou à destination de colonnes à l’aide d’Always Encrypted avec l’Assistant Importation et exportation SQL Server](always-encrypted-migrate-using-import-export-wizard.md)

## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
- [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)
- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Provisionner des clés Always Encrypted en utilisant PowerShell](configure-always-encrypted-keys-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
