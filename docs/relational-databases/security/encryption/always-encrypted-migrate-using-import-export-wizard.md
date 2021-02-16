---
description: Migrer des données à partir ou à destination de colonnes à l’aide d’Always Encrypted avec l’Assistant Importation et exportation SQL Server
title: Migrer des données à partir ou à destination de colonnes à l’aide d’Always Encrypted avec l’Assistant Importation et exportation SQL Server| Microsoft Docs
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- SQL13.SWB.COLUMNMASTERKEY.ROTATION.F1
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 234dabee531f85b4e3f8a687067dee9f91c96b33
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345488"
---
# <a name="migrate-data-to-or-from-columns-using-always-encrypted-with-sql-server-import-and-export-wizard"></a>Migrer des données à partir ou à destination de colonnes à l’aide d’Always Encrypted avec l’Assistant Importation et exportation SQL Server 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

L’[Assistant Importation et Exportation SQL Server](../../../integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard.md) est un outil qui vous permet de copier des données depuis une source vers une destination. Ce document explique comment utiliser l’Assistant Importation et exportation SQL Server si une source et/ou une destination est une base de données SQL Server contenant des colonnes protégées avec [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md).

## <a name="migration-scenarios"></a>Scénarios de migration
Avec l’Assistant Importation et exportation SQL Server, vous pouvez implémenter les scénarios suivants pour migrer des données vers ou depuis des colonnes chiffrées.

### <a name="encrypt-plaintext-data-on-migration"></a>Chiffrer des données texte en clair lors de la migration
Si votre source de données contient des données texte en clair et que votre destination est une base de données SQL Server contenant des colonnes chiffrées, vous pouvez utiliser l’Assistant Importation et exportation SQL Server pour récupérer les données texte en clair à partir de la source, les chiffrer et copier les données chiffrées (texte chiffré) vers les colonnes chiffrées dans la base de données de destination. Pour ce scénario de migration, la source de données peut être n’importe quel magasin de données pris en charge par l’Assistant Importation et exportation SQL Server. Par exemple, un fichier, une base de données SQL Server ou une base de données dans un autre système de base de données.

Pour garantir que l’Assistant Importation et exportation SQL Server peut chiffrer les données, vous devez activer Always Encrypted pour la connexion de la base de données de destination et vous devez avoir accès aux clés protégeant les données dans les colonnes de la base de données de destination. Pour plus d’informations, consultez [Activer et désactiver Always Encrypted pour une connexion de base de données](#enable-and-disable-always-encrypted-for-a-database-connection) et [Autorisations pour le chiffrement ou le déchiffrement des données lors de la migration](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="decrypt-encrypted-data-on-migration"></a>Déchiffrer des données chiffrées lors de la migration
Si vous migrez des données stockées dans des colonnes de base de données chiffrées dans une base de données SQL Server, vous pouvez configurer l’Assistant Importation et exportation SQL Server pour déchiffrer les données et copier les données déchiffrées (texte en clair) vers une destination, qui peut être n’importe quel magasin de données pris en charge par l’Assistant Importation et exportation SQL Server, par exemple un fichier, une base de données SQL Server ou une base de données dans un autre système de base de données.

Pour garantir que l’Assistant Importation et exportation SQL Server peut déchiffrer les données, vous devez activer Always Encrypted pour la connexion de la base de données source et vous devez avoir accès aux clés protégeant les données dans les colonnes de la base de données source. Pour plus d’informations, consultez [Activer et désactiver Always Encrypted pour une connexion de base de données](#enable-and-disable-always-encrypted-for-a-database-connection) et [Autorisations pour le chiffrement ou le déchiffrement des données lors de la migration](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="re-encrypt-data-on-migration"></a>Rechiffrer des données lors de la migration
Si vous copiez les données à partir de colonnes chiffrées dans une base de données SQL Server source vers des colonnes chiffrées dans la même base de données ou dans une autre base de données SQL Server, vous pouvez configurer l’Assistant Importation et exportation SQL Server pour déchiffrer les données après les avoir récupérées auprès de la source et pour les rechiffrer avant de les insérer dans les colonnes chiffrées de la base de données de destination. Utilisez cette méthode si le schéma des colonnes cibles (par exemple les types de données des colonnes, les types de chiffrement et les clés de chiffrement de colonne) est différent du schéma des colonnes sources.

Pour garantir que l’Assistant Importation et exportation SQL Server peut chiffrer et déchiffrer les données, vous devez activer Always Encrypted à la fois pour la connexion de la base de données source et de la base de données de destination, et vous devez avoir accès aux clés protégeant les données dans les colonnes de la base de données source et de la base de données de destination. Pour plus d’informations, consultez [Activer et désactiver Always Encrypted pour une connexion de base de données](#enable-and-disable-always-encrypted-for-a-database-connection) et [Autorisations pour le chiffrement ou le déchiffrement des données lors de la migration](#permissions-for-encrypting-or-decrypting-data-during-migration).

### <a name="keep-data-encrypted-during-migration"></a>Conserver les données chiffrées lors de la migration
Si vous copiez les données depuis des colonnes chiffrées dans une base de données SQL Server source vers des colonnes chiffrées dans la même base de données ou dans une autre base de données SQL Server, et que les colonnes cibles utilisent **exactement** le même schéma (notamment les mêmes types de données, les mêmes types de chiffrement et les mêmes clés de chiffrement de colonne) que les colonnes sources, vous pouvez configurer l’Assistant Importation et exportation SQL Server pour extraire le texte chiffré des colonnes sources et pour insérer les données chiffrées (texte chiffré) dans les colonnes chiffrées de la base de données SQL Server cible. 

Pour ce scénario, vous pouvez utiliser n’importe quel fournisseur de données prenant en charge SQL Server pour la connexion à la base de données SQL Server source ou de destination. Si vous utilisez un fournisseur qui prend en charge Always Encrypted pour la connexion à la base de données de destination, vous devez vérifier qu’Always Encrypted est désactivé pour la connexion de base de données. Pour plus d’informations, consultez [Activer et désactiver Always Encrypted pour une connexion de base de données](#enable-and-disable-always-encrypted-for-a-database-connection).

Vous devez également vérifier que le principal de la base de données (l’utilisateur) utilisé par l’Assistant Importation et exportation SQL Server pour se connecter à la base de données de destination est configuré avec l’option `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` définie sur `ON`. Cette option supprime les contrôles des métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc, ce qui permet à l’Assistant d’insérer en bloc les données chiffrées dans la base de données de destination sans déchiffrer les données. Pour plus d’informations, consultez [Charger en bloc des données chiffrées dans des colonnes protégées par Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="enable-and-disable-always-encrypted-for-a-database-connection"></a>Activer et désactiver Always Encrypted pour une connexion de base de données
Si votre scénario de migration nécessite que l’Assistant Importation et exportation SQL Server puisse chiffrer et/ou déchiffrer des données, vous devez configurer la connexion de base de données SQL Server source et/ou de destination avec un fournisseur de données qui prend en charge Always Encrypted. Vous devez également activer Always Encrypted pour la connexion de base de données source et/ou de destination.

Vous pouvez utiliser n’importe quel fournisseur de données pour une connexion si vous n’avez pas besoin que l’Assistant chiffre ou déchiffre des données sur cette connexion.

Les fournisseurs de données suivants de l’Assistant Importation et exportation SQL Server prennent en charge Always Encrypted.

- Fournisseur de données .NET Framework pour SQL Server
  - Vérifiez que la machine où s’exécute l’Assistant utilise .NET Framework 4.6.1 ou ultérieur.
  - Pour activer Always Encrypted pour une connexion, définissez `Column Encryption Setting` sur `Enabled` dans les propriétés de la connexion. Pour désactiver Always Encrypted, définissez `Column Encryption Setting` sur `Disabled`. Pour plus d’informations, consultez [Se connecter à SQL Server avec le fournisseur de données .NET Framework pour SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-net-framework-data-provider-for-sql-server) et [Activation d’Always Encrypted pour les requêtes d’application](develop-using-always-encrypted-with-net-framework-data-provider.md#enabling-always-encrypted-for-application-queries).
- Fournisseur de données .NET Framework pour ODBC.
  - Installez Microsoft ODBC Driver 13.1 ou ultérieur.
    - Pour activer Always Encrypted pour une connexion, définissez `Column Encryption` sur `Enabled` dans les propriétés de la connexion. Pour désactiver Always Encrypted, définissez `Column Encryption` sur `Disabled`. Pour plus d’informations, consultez [Se connecter à SQL Server avec le pilote ODBC pour SQL Server](../../../integration-services/import-export-data/connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard.md#connect-to-sql-server-with-the-odbc-driver-for-sql-server) et [Activation d’Always Encrypted pour une application ODBC](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-in-an-odbc-application).

## <a name="permissions-for-encrypting-or-decrypting-data-during-migration"></a>Autorisations pour le chiffrement ou le déchiffrement des données lors de la migration

Pour chiffrer ou déchiffrer des données stockées dans une base de données SQL Server source ou de destination, vous avez besoin des autorisations *VIEW ANY COLUMN MASTER KEY DEFINITION* et *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* dans la base de données source.

Vous devez également pouvoir accéder aux clés principales de colonne configurées pour les colonnes qui stockent les données que vous chiffrez ou que vous déchiffrez :

- **Magasin de certificats - Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations _get_, _unwrapKey_ et _verify_ sur le coffre contenant la clé principale de colonne.
- **Fournisseur de magasin de clés (CNG)**  : vous pouvez être invité à fournir les informations d’identification et les autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)**  : vous pouvez être invité à fournir les informations d’identification et les autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.
Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md).

## <a name="next-steps"></a>Étapes suivantes
- [Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>Voir aussi
- [Always Encrypted](always-encrypted-database-engine.md)
- [Exporter et importer des bases de données avec Always Encrypted](always-encrypted-migrate-using-bacpac.md)
- [Sauvegarder et restaurer des bases de données avec Always Encrypted](always-encrypted-migrate-using-backup-restore.md)
- [Charger en bloc des données chiffrées dans des colonnes avec Always Encrypted](migrate-sensitive-data-protected-by-always-encrypted.md)
