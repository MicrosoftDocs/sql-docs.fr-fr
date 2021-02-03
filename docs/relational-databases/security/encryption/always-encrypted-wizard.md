---
title: Configurer le chiffrement de colonne à l’aide de l’Assistant Always Encrypted | Microsoft Docs
description: Découvrez comment configurer la fonctionnalité d’Always Encrypted pour les colonnes de base de données à l’aide de l’Assistant Always Encrypted dans SQL Server.
ms.custom: ''
ms.date: 10/30/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.alwaysencryptedwizard.encryption.f1
- sql13.swb.alwaysencryptedwizard.f1
- sql13.swb.alwaysencryptedwizard.masterkey.f1
helpviewer_keywords:
- Wizard, Always Encrypted
ms.assetid: 68daddc9-ce48-49aa-917f-6dec86ad5af5
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bedf502949e206b45bf7e1e332962c8c093d1bcb
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236698"
---
# <a name="configure-column-encryption-using-always-encrypted-wizard"></a>Configurer le chiffrement de colonne à l’aide de l’Assistant Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

L’Assistant Always Encrypted est un outil puissant qui vous permet de définir la configuration [Always Encrypted](always-encrypted-database-engine.md) souhaitée pour les colonnes de base de données sélectionnées. Selon la configuration actuelle et la configuration cible souhaitée, l’Assistant peut chiffrer une colonne, la déchiffrer (supprimer le chiffrement) ou la rechiffrer (par exemple, à l’aide d’une nouvelle clé de chiffrement de colonne ou d’un type de chiffrement différent du type actuel configuré pour la colonne). Plusieurs colonnes peuvent être configurées avec une seule exécution de l’Assistant.

L’Assistant vous permet de chiffrer des colonnes avec des clés de chiffrement de colonne existantes, ou vous pouvez choisir de générer une nouvelle clé de chiffrement de colonne, ou une nouvelle clé de chiffrement de colonne et une nouvelle clé principale de colonne. 

L’Assistant déplace les données hors de la base de données et en effectuant des opérations de chiffrement au sein du processus SSMS. L’Assistant crée une ou plusieurs tables avec la configuration de chiffrement souhaitée dans la base de données, charge toutes les données des tables d’origine, effectue les opérations de chiffrement demandées, charge les données dans les nouvelles tables, puis remplace les tables d’origine par les nouvelles tables.

> [!NOTE]
> L’exécution des opérations de chiffrement peut prendre beaucoup de temps. Pendant ce temps, votre base de données n’est pas disponible pour l’écriture de transactions. Nous recommandons d’utiliser PowerShell pour les opérations de chiffrement sur les tables plus grandes. Consultez [Configurer le chiffrement de colonne à l’aide d’Always Encrypted avec PowerShell](configure-column-encryption-using-powershell.md).

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Si vous utilisez [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] et que votre instance SQL Server est configurée avec une enclave sécurisée, vous pouvez exécuter des opérations de chiffrement sur place, sans déplacer les données en dehors de la base de données. Consultez [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md). Notez que l’Assistant ne prend pas en charge le chiffrement sur place.

::: moniker-end

Utilisez PowerShell. 

 - Pour une procédure détaillée montrant comment configurer Always Encrypted avec l’Assistant et comment l’utiliser dans une application cliente, consultez les tutoriels Azure SQL Database suivants :
    - [Protéger les données sensibles dans Azure SQL Database avec Always Encrypted et des clés principales de colonne dans le magasin de certificats Windows](/azure/azure-sql/database/always-encrypted-certificate-store-configure)
    - [Protéger les données sensibles dans Azure SQL Database avec Always Encrypted et des clés principales de colonne dans Azure Key Vault](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

 - Consultez [Prise en main d’Always Encrypted avec SSMS](https://channel9.msdn.com/events/DataDriven/SQLServer2016/AlwaysEncrypted)pour regarder une vidéo qui traite de l’utilisation de l’Assistant. Consultez également le blog de l’équipe de sécurité [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][Assistant de chiffrement SSMS - Activer Always Encrypted en quelques étapes](https://techcommunity.microsoft.com/t5/SQL-Server/SSMS-Encryption-Wizard-Enabling-Always-Encrypted-in-a-Few-Easy/ba-p/384545).  
 - Pour plus d’informations sur les clés Always Encrypted, consultez [Vue d’ensemble de la gestion de clés pour Always Encrypted](overview-of-key-management-for-always-encrypted.md).
 - Pour plus d’informations sur les types de chiffrement pris en charge dans Always Encrypted, consultez [Sélection du chiffrement déterministe ou aléatoire](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).
 
 ## <a name="permissions"></a>Autorisations
Pour effectuer des opérations de chiffrement avec l’Assistant, vous devez avoir les autorisations **VIEW ANY COLUMN MASTER KEY DEFINITION** et **VIEW ANY COLUMN ENCRYPTION KEY DEFINITION**. Vous devez également avoir l’autorisation d’accéder aux clés principales de colonne que vous utilisez dans les magasins de clés qui les contiennent :
- **Magasin de certificats - Ordinateur local** : vous devez avoir l’accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations get, unwrapKey et verify sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Par ailleurs, si vous créez des clés à l’aide de l’Assistant, vous devez avoir des autorisations supplémentaires listées dans [Provisionner des clés principales de colonne dans la boîte de dialogue Nouvelle clé principale de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) et [Provisionner des clés de chiffrement de colonne dans la boîte de dialogue Nouvelle clé de chiffrement de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog).

## <a name="open-the-always-encrypted-wizard"></a>Ouvrir l’Assistant Always Encrypted
Vous pouvez lancer l’Assistant à trois niveaux différents : 
- Au niveau de la base de données, si vous voulez chiffrer plusieurs colonnes de tables différentes.
- Au niveau de la table, si vous voulez chiffrer plusieurs colonnes de la même table.
- Au niveau de la colonne, si vous voulez chiffrer une colonne spécifique.
 
 1. Connectez-vous à votre [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec le composant Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
   
 2. Pour chiffrer :
     1. Plusieurs colonnes dans des tables différentes d’une base de données, cliquez avec le bouton droit sur votre base de données, pointez sur **Tâches**, puis sélectionnez **Chiffrer les colonnes**.
     1. Plusieurs colonnes de la même table, accédez à la table, cliquez dessus avec le bouton droit, puis sélectionnez **Chiffrer les colonnes**.
     1. Une colonne individuelle, accédez à la colonne, cliquez dessus avec le bouton droit, puis sélectionnez **Chiffrer les colonnes**.


   
 ## <a name="column-selection-page"></a>Page de sélection de la colonne
Dans cette page, vous sélectionnez les colonnes à chiffrer, rechiffrer ou déchiffrer, et définissez la configuration du chiffrement cible pour les colonnes sélectionnées.

Pour chiffrer une colonne en texte clair (colonne non chiffrée), sélectionnez un type de chiffrement (**déterministe** ou **aléatoire**) et une clé de chiffrement pour la colonne. 

Pour changer le type de chiffrement ou pour permuter (changer) la clé de chiffrement de colonne d’une colonne déjà chiffrée, sélectionnez le type de chiffrement et la clé souhaités. 

Si vous voulez que l’Assistant chiffre ou rechiffre une ou plusieurs colonnes à l’aide d’une nouvelle clé de chiffrement de colonne, sélectionnez une clé contenant **(New)** dans son nom. L’Assistant génère la clé.

Pour déchiffrer une colonne chiffrée, sélectionnez **Texte en clair** comme type de chiffrement.


> [!NOTE]
> L’Assistant ne prend pas en charge les opérations de chiffrement sur les tables temporelles et en mémoire. Vous pouvez créer des tables temporelles ou en mémoire vides à l’aide de Transact-SQL et insérer des données à l’aide de votre application.

## <a name="master-key-configuration-page"></a>Page de configuration de la clé principale
Si vous avez sélectionné une clé de chiffrement de colonne générée automatiquement pour une colonne de la page précédente, dans cette page vous devez sélectionner une clé principale de colonne existante ou configurer une nouvelle clé principale de colonne qui chiffre la clé de chiffrement de colonne. 

Quand vous configurez une nouvelle clé principale de colonne, vous pouvez choisir une clé existante dans le magasin de certificats Windows ou dans Azure Key Vault, et demander à l’Assistant de créer simplement un objet de métadonnées pour la clé dans la base de données, ou vous pouvez choisir de générer la clé et l’objet de métadonnées décrivant la clé dans la base de données. 

Pour plus d’informations sur la création et le stockage des clés principales de colonne dans le magasin de certificats Windows, Azure Key Vault ou d’autres magasins de clés, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!TIP]
> L’Assistant vous permet de parcourir et créer des clés uniquement dans le magasin de certificats Windows et Azure Key Vault. Par ailleurs, il génère automatiquement les noms des nouvelles clés et des nouveaux objets de métadonnées de base de données décrivant les clés. Si vous avez besoin de contrôler davantage le provisionnement de vos clés (et d’avoir plus de choix pour le magasin de clés contenant votre clé principale de colonne), vous pouvez utiliser les boîtes de dialogue **Nouvelle clé principale de colonne** et **Nouvelle clé de chiffrement de colonne** pour d’abord créer les clés, puis exécuter l’Assistant et choisir les clés que vous avez créées. Consultez [Provisionner des clés principales de colonne dans la boîte de dialogue Nouvelle clé principale de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog) et [Provisionner des clés de chiffrement de colonne dans la boîte de dialogue Nouvelle clé de chiffrement de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog). 

## <a name="next-steps"></a>Étapes suivantes
- [Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Voir aussi  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Vue d’ensemble de la gestion des clés pour Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Configurer Always Encrypted à l’aide de SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Provisionner des clés Always Encrypted en utilisant PowerShell](configure-always-encrypted-keys-using-powershell.md)
 - [Configurer le chiffrement de colonne avec Always Encrypted en utilisant PowerShell](configure-column-encryption-using-powershell.md)
 - [Configurer le chiffrement de colonne en utilisant Always Encrypted avec un package DAC](configure-always-encrypted-using-dacpac.md)