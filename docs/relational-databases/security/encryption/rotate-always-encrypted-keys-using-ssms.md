---
title: Effectuer une rotation des clés Always Encrypted avec SQL Server Management Studio | Microsoft Docs
description: Découvrez la rotation des clés principales de colonne et des clés de chiffrement de colonne Always Encrypted avec SQL Server Management Studio.
ms.custom: ''
ms.date: 10/01/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
- sql13.SWB.COLUMNMASTERKEY.CLEANUP.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 249e0d2bc53083839787543723fc16dc11a4ff57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340136"
---
# <a name="rotate-always-encrypted-keys-using-sql-server-management-studio"></a>Effectuer une rotation des clés Always Encrypted avec SQL Server Management Studio
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Cet article décrit les tâches nécessaires pour la rotation des clés principales de colonne et des clés de chiffrement de colonne Always Encrypted avec [SQL Server Management Studio (SSMS)](../../../ssms/download-sql-server-management-studio-ssms.md).

Pour obtenir une vue d’ensemble de la gestion des clés Always Encrypted, notamment des recommandations sur les bonnes pratiques et des considérations importantes sur la sécurité, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).

<a name="rotatecmk"></a>
## <a name="rotate-column-master-keys"></a>Effectuer une rotation des clés principales de colonne 

La permutation d’une clé principale de colonne est le processus de remplacement d’une clé principale de colonne existante par une nouvelle clé principale de colonne. Vous devrez peut-être permuter une clé si elle a été compromise, ou pour vous conformer aux stratégies ou aux réglementations de conformité de votre organisation qui régissent la permutation périodique des clés de chiffrement. Une permutation des clés principales de colonne implique le déchiffrement de clés de chiffrement de colonne qui sont protégées avec la clé principale de colonne active, leur rechiffrement à l’aide de la nouvelle clé principale de colonne et la mise à jour des métadonnées de clé. 

### <a name="step-1-provision-a-new-column-master-key"></a>Étape 1 : Provisionner une nouvelle clé principale de colonne

Suivez les étapes décrites dans [Provisionner des clés principales de colonne avec la boîte de dialogue Nouvelle clé principale de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog).

### <a name="step-2-encrypt-column-encryption-keys-with-the-new-column-master-key"></a>Étape 2 : Chiffrer les clés de chiffrement de colonne avec la nouvelle clé principale de colonne

Une clé principale de colonne protège généralement une ou plusieurs clés de chiffrement de colonne. Chaque clé de chiffrement de colonne a une valeur chiffrée, stockée dans la base de données, qui est le produit du chiffrement de la clé de chiffrement de colonne avec la clé principale de colonne.
Dans cette étape, chiffrez chacune des clés de chiffrement de colonne qui sont protégées par la clé principale de colonne que vous permutez avec la nouvelle clé principale de colonne, puis stockez la nouvelle valeur chiffrée dans la base de données. Par conséquent, chaque clé de chiffrement de colonne affectée par la permutation comporte deux valeurs chiffrées : une valeur chiffrée avec la clé principale de colonne existante et une nouvelle valeur chiffrée avec la nouvelle clé principale de colonne.

1.  À l’aide de **Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted>Clés principales de colonne**, puis recherchez la clé principale de colonne que vous permutez.
2.  Cliquez avec le bouton droit sur la clé principale de colonne, puis sélectionnez **Permuter**.
3.  Dans la boîte de dialogue **Permutation de la clé principale de colonne** , sélectionnez le nom de la nouvelle clé principale de colonne que vous avez créée à l’étape 1, dans le champ **Cible** .
4.  Consultez la liste des clés de chiffrement de colonne protégées par les clés principales de colonne existantes. Ces clés sont affectées par la permutation.
5.  Cliquez sur **OK**.

SQL Server Management Studio obtient les métadonnées des clés de chiffrement de colonne qui sont protégées avec l’ancienne clé principale de colonne ainsi que les métadonnées de l’ancienne et de la nouvelle clés principales de colonne. SSMS utilise ensuite les métadonnées de clé principale de colonne pour accéder au magasin de clés contenant l’ancienne clé principale de colonne et déchiffre les clés de chiffrement de colonne. Par la suite, SSMS accède au magasin de clés contenant la nouvelle clé principale de colonne pour produire un nouvel ensemble de valeurs chiffrées des clés de chiffrement de colonne, puis ajoute les nouvelles valeurs aux métadonnées (en générant et en émettant des instructions [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) ).

> [!NOTE]
> Vérifiez qu’aucune des clés de chiffrement de colonne, chiffrées avec l’ancienne clé principale de colonne, n’est chiffrée avec toute autre clé principale de colonne. En d’autres termes, chaque clé de chiffrement de colonne affectée par la permutation doit avoir précisément une valeur chiffrée dans la base de données. Si une clé de chiffrement de colonne concernée a plusieurs valeurs chiffrées, vous devez supprimer les valeurs superflues avant de passer à la permutation (consultez *l’étape 4* sur la suppression d’une valeur chiffrée d’une clé de chiffrement de colonne).

### <a name="step-3-configure-your-applications-with-the-new-column-master-key"></a>Étape 3 : Configurer vos applications avec la nouvelle clé principale de colonne

Dans cette étape, vous devez vérifier que toutes vos applications clientes qui interrogent des colonnes de base de données protégées par la clé principale de colonne en rotation (c’est-à-dire les colonnes de base de données chiffrées avec une clé de chiffrement de colonne elle-même chiffrée avec la clé principale de colonne en rotation) peuvent accéder à la nouvelle clé principale de colonne. Cette étape dépend du type de magasin de clés dans lequel est stockée votre nouvelle clé principale de colonne. Par exemple :

- Si la nouvelle clé principale de colonne est un certificat stocké dans le magasin de certificats Windows, vous devez déployer le certificat au même emplacement de magasin de certificats (*Utilisateur actuel* ou *Ordinateur local*) en tant qu’emplacement spécifié dans le chemin d’accès à la clé de votre clé principale de colonne dans la base de données. L’application doit être en mesure d’accéder au certificat :
  - Si le certificat est stocké à l’emplacement du magasin de certificats *Utilisateur actuel*, il doit être importé dans le magasin de l’utilisateur actuel de l’identité Windows de l’application (utilisateur).
  - Si le certificat est stocké à l’emplacement du magasin de certificats *Ordinateur local*, l’identité Windows de l’application doit disposer d’une autorisation d’accès au certificat.
- Si la nouvelle clé principale de colonne est stockée dans Microsoft Azure Key Vault, l’application doit être implémentée afin de pouvoir s’authentifier auprès d’Azure et a l’autorisation d’accéder à la clé.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

> [!NOTE]
> À ce stade de la permutation, l’ancienne clé principale de colonne et la nouvelle clé principale de colonne sont valides et peuvent être utilisées pour accéder aux données.

### <a name="step-4-clean-up-column-encryption-key-values-encrypted-with-the-old-column-master-key"></a>Étape 4 : Nettoyer les valeurs des clés de chiffrement de colonne chiffrées avec l’ancienne clé principale de colonne

Après avoir configuré toutes vos applications pour utiliser la nouvelle clé principale de colonne, supprimez de la base de données les valeurs des clés de chiffrement de colonne chiffrées avec *l’ancienne* clé principale de colonne. La suppression des anciennes valeurs garantit que vous êtes prêt pour la rotation suivante (rappelez-vous que chaque clé de chiffrement de colonne, protégée par une clé principale de colonne à permuter, doit avoir précisément une valeur chiffrée).

Une autre raison de nettoyer l’ancienne valeur avant l’archivage ou la suppression de l’ancienne clé principale de colonne a trait aux performances ; quand vous interrogez une colonne chiffrée, un pilote de client prenant en charge Always Encrypted risque de devoir tenter de déchiffrer deux valeurs : l’ancienne et la nouvelle. Le pilote ignore laquelle des deux clés principales de colonne est valide dans l’environnement de l’application et il récupère donc les deux valeurs chiffrées à partir du serveur. En cas d’échec du déchiffrement de l’une des valeurs, parce qu’elle est protégée par la clé principale de colonne non disponible (par exemple, si l’ancienne clé principale de colonne a été supprimée de magasin), le pilote tente de déchiffrer une autre valeur à l’aide de la nouvelle clé principale de colonne.

> [!WARNING]
> Si vous supprimez la valeur d’une clé de chiffrement de colonne avant que sa clé principale de colonne correspondante ne soit devenue disponible pour une application, l’application ne peut plus déchiffrer la colonne de base de données.

1.  Dans **l’Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted**, puis recherchez la clé principale de colonne existante à remplacer.
2.  Cliquez avec le bouton droit sur la clé principale de colonne existante, puis sélectionnez **Nettoyage**.
3.  Passez en revue la liste des valeurs clés de chiffrement de colonne à supprimer.
4.  Cliquez sur **OK**.

SQL Server Management Studio émet des instructions [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md) pour supprimer les valeurs chiffrées des clés de chiffrement de colonne chiffrées avec l’ancienne clé principale de colonne.

### <a name="step-5-delete-metadata-for-your-old-column-master-key"></a>Étape 5 : Supprimer les métadonnées pour votre ancienne clé principale de colonne

Si vous choisissez de supprimer la définition de l’ancienne clé principale de colonne de la base de données, procédez comme suit.

1. Dans **l’Explorateur d’objets**, accédez au dossier **Sécurité>Clés Always Encrypted>Clés principales de colonne**, puis recherchez l’ancienne clé principale de colonne à supprimer de la base de données.
2. Cliquez avec le bouton droit sur l’ancienne clé principale de colonne, puis sélectionnez **Supprimer**. (Cela génère et émet une instruction [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md) pour supprimer les métadonnées de clé principale de colonne.)
3. Cliquez sur **OK**.

> [!NOTE]
> Nous vous recommandons vivement de ne pas supprimer définitivement l’ancienne clé principale de colonne après la permutation. Au lieu de cela, laissez l’ancienne clé principale de colonne dans son magasin de clés actuel ou archivez-la dans un autre emplacement sécurisé. Si vous restaurez votre base de données à partir d’un fichier de sauvegarde à un point dans le temps avant la configuration de la nouvelle clé principale de colonne, vous aurez besoin de l’ancienne clé pour accéder aux données.

### <a name="permissions-for-rotating-column-master-key"></a>Autorisations pour la rotation de la clé principale de colonne

La permutation d’une clé principale de colonne nécessite les autorisations relatives à la base de données suivantes :

- **ALTER ANY COLUMN MASTER KEY** : obligatoire pour créer des métadonnées pour la nouvelle clé principale de colonne et supprimer les métadonnées pour l’ancienne clé principale de colonne.
- **ALTER ANY COLUMN ENCRYPTION KEY** : obligatoire pour modifier les métadonnées de clé de chiffrement de colonne (ajouter les nouvelles valeurs chiffrées).

Vous devez également être en mesure d’accéder à l’ancienne clé principale de colonne et à la nouvelle clé principale de colonne dans leurs magasins de clés. Pour accéder à un magasin de clés et utiliser une clé principale de colonne, vous pouvez avoir besoin d’autorisations sur le magasin de clés et/ou la clé :
- **Magasin de certificats - Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations *create*, *get*, *unwrapKey*, *wrapKey*, *sign* et *verify* sur le coffre contenant les clés principales de colonne.
- **Fournisseur du magasin de clés (CNG)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

<a name="rotatecek"></a> 
## <a name="rotate-column-encryption-keys"></a>Effectuer une rotation des clés de chiffrement de colonne

La permutation d’une clé de chiffrement de colonne implique le déchiffrement des données dans toutes les colonnes chiffrées avec la clé à permuter et le rechiffrement des données à l’aide de la nouvelle clé de chiffrement de colonne.

>[!NOTE]
> La permutation d’une clé de chiffrement de colonne peut prendre beaucoup de temps si les tables qui contiennent les colonnes chiffrées avec la clé soumise à la permutation sont volumineuses. Pendant le rechiffrement des données, vos applications ne peuvent pas écrire dans les tables concernées. Votre organisation doit donc apporter un soin particulier à la planification de la permutation des clés de chiffrement de colonne.
Pour permuter une clé de chiffrement de colonne, utilisez l’Assistant Always Encrypted.

1.  Ouvrez l’Assistant pour votre base de données : cliquez avec le bouton droit sur votre base de données, pointez sur **Tâches**, puis cliquez sur **Chiffrer les colonnes**.
2.  Examinez la page **Introduction** , puis cliquez sur **Suivant**.
3.  Dans la page **Sélection de la colonne** , développez les tables et recherchez toutes les colonnes à remplacer qui sont actuellement chiffrés avec l’ancienne clé de chiffrement de colonne.
4.  Pour chaque colonne chiffrée avec l’ancienne clé de chiffrement de colonne, définissez **Clé de chiffrement** sur une nouvelle clé générée automatiquement. **Remarque :** Vous pouvez aussi créer une clé de chiffrement de colonne avant d’exécuter l’Assistant : consultez [Provisionner des clés de chiffrement de colonne avec la boîte de dialogue Nouvelle clé de chiffrement de colonne](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog).
5.  Dans la page **Configuration de la clé principale** , sélectionnez un emplacement pour stocker la nouvelle clé et une source de clé principale, puis cliquez sur **Suivant**. **Remarque :** Si vous utilisez une clé de chiffrement de colonne existante (et non générée automatiquement), aucune action ne doit être effectuée dans cette page.
6.  Dans la page **Validation**, indiquez si vous souhaitez exécuter le script immédiatement ou créer un script PowerShell, puis cliquez sur **Suivant**.
7.  Dans la page **Résumé**, passez en revue les options que vous avez sélectionnées, puis cliquez sur **Terminer** et fermez l’Assistant à la fin.
8.  Dans **l’Explorateur d’objets**, accédez au dossier **Sécurité/Clés Always Encrypted/Clés de chiffrement de colonne** , puis recherchez l’ancienne clé de chiffrement de colonne à supprimer de la base de données. Cliquez avec le bouton droit sur la clé et sélectionnez **Supprimer**.

### <a name="permissions-for-rotating-column-encryption-keys"></a>Autorisations pour la rotation des clés de chiffrement de colonne

La permutation d’une clé de chiffrement de colonne nécessite les autorisations de base de données suivantes : **ALTER ANY COLUMN MASTER KEY** : obligatoire si vous utilisez une nouvelle clé de chiffrement de colonne générée automatiquement (une clé principale de colonne et ses nouvelles métadonnées sont également générées).
**ALTER ANY COLUMN ENCRYPTION KEY** : obligatoire pour ajouter des métadonnées pour la nouvelle clé de chiffrement de colonne.

Vous devez également être en mesure d’accéder aux clés principales de colonne pour l’ancienne et la nouvelle clés de chiffrement de colonne. Pour accéder à un magasin de clés et utiliser une clé principale de colonne, vous pouvez avoir besoin d’autorisations sur le magasin de clés et/ou la clé :
- **Magasin de certificats - Ordinateur local** : vous devez avoir l’accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations get, unwrapKey et verify sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne pour Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="next-steps"></a>Étapes suivantes
- [Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Voir aussi
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Configurer Always Encrypted à l’aide de SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
- [Configurer Always Encrypted à l’aide de PowerShell](configure-always-encrypted-using-powershell.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)
