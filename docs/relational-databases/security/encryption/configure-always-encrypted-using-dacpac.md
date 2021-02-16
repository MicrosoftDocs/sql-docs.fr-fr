---
description: Configurer le chiffrement de colonne en utilisant Always Encrypted avec un package DAC
title: Configurer le chiffrement de colonne en utilisant Always Encrypted avec un package DAC | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, configure with SSMS
ms.assetid: 29816a41-f105-4414-8be1-070675d62e84
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: cae5c4c74f5fc4f6df9b62b1ad985b469aa528ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345420"
---
# <a name="configure-column-encryption-using-always-encrypted-with-a-dac-package"></a>Configurer le chiffrement de colonne en utilisant Always Encrypted avec un package DAC 
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

Un [package d’application de la couche Données (DAC)](../../data-tier-applications/data-tier-applications.md), également appelé DACPAC, est une unité portable de déploiement de base de données SQL Server qui définit tous les objets SQL Server, notamment les tables et les colonnes à l’intérieur des tables. Quand vous publiez un DACPAC sur une base de données (quand vous mettez à niveau une base de données avec un DACPAC), le schéma de la base de données cible est mis à jour pour correspondre au schéma dans le DACPAC. Vous pouvez publier un DACPAC en utilisant l’[Assistant Mise à niveau d’une application de la couche Données](../../data-tier-applications/upgrade-a-data-tier-application.md#UsingDACUpgradeWizard) dans SQL Server Management Studio, [PowerShell](../../data-tier-applications/upgrade-a-data-tier-application.md#UpgradeDACPowerShell) ou [sqlpackage](../../../tools/sqlpackage/sqlpackage-publish.md).

Cet article traite des considérations spéciales relatives à la mise à niveau d’une base de données quand le DACPAC et/ou la base de données cible contient des colonnes protégées avec [Always Encrypted](always-encrypted-database-engine.md). Si le schéma de chiffrement d’une colonne dans le DACPAC diffère du schéma de chiffrement pour une colonne existante dans la base de données cible, la publication du DACPAC entraîne le chiffrement, le déchiffrement ou le rechiffrement des données stockées dans la colonne. Consultez la table ci-dessous pour plus d’informations.

| Condition|Action|
|:---|:---|
|La colonne est chiffrée dans le fichier DACPAC et n’est pas chiffrée dans la base de données.| Les données de la colonne sont chiffrées.|
|La colonne n’est pas chiffrée dans le fichier DACPAC et est chiffrée dans la base de données.| Les données de la colonne sont déchiffrées (le chiffrement est supprimé de la colonne).|
| La colonne est chiffrée à la fois dans le fichier DACPAC et la base de données, mais la colonne dans le fichier DACPAC utilise un type de chiffrement et/ou une clé de chiffrement de colonne autre que la colonne correspondante dans la base de données.|Les données de la colonne sont déchiffrées, puis rechiffrées pour correspondre à la configuration de chiffrement dans le fichier DACPAC.|

Le déploiement d’un package DAC peut également entraîner la création ou la suppression d’objets de métadonnées pour des clés principales de colonne ou des clés de chiffrement de colonne pour Always Encrypted.

## <a name="performance-considerations"></a>Considérations relatives aux performances
Pour effectuer des opérations de chiffrement, l’outil que vous utilisez pour déployer un DACPAC doit déplacer les données en dehors de la base de données. L’outil crée une ou plusieurs tables avec la configuration de chiffrement souhaitée dans la base de données, charge toutes les données des tables d’origine, effectue les opérations de chiffrement demandées, charge les données dans les nouvelles tables, puis remplace les tables d’origine par les nouvelles tables. L’exécution des opérations de chiffrement peut prendre beaucoup de temps. Pendant ce temps, votre base de données n’est pas disponible pour l’écriture de transactions. 

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Si vous utilisez [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] et que votre instance SQL Server est configurée avec une enclave sécurisée, vous pouvez exécuter des opérations de chiffrement sur place, sans déplacer les données en dehors de la base de données. Consultez [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md). Notez que le chiffrement sur place n’est pas disponible pour les déploiements DACPAC.

::: moniker-end

## <a name="permissions-for-publishing-a-dac-package-if-always-encrypted-is-set-up"></a>Autorisations pour la publication d’un package DAC si Always Encrypted est configuré

Pour publier un package DAC si Always Encrypted est configuré dans le DACPAC et/ou dans la base de données cible, vous pouvez avoir besoin de tout ou partie des autorisations ci-dessous, en fonction des différences entre le schéma du DACPAC et le schéma de la base de données cible.

*ALTER ANY COLUMN MASTER KEY*, *ALTER ANY COLUMN ENCRYPTION KEY*, *VIEW ANY COLUMN MASTER KEY DEFINITION*, *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION*

Si l’opération de mise à niveau déclenche une opération de chiffrement de données, vous devez également être en mesure d’accéder aux clés principales de colonne configurées pour les colonnes concernées :

- **Magasin de certificats - Ordinateur local** : vous devez avoir un accès en lecture au certificat utilisé comme clé principale de colonne, ou être l’administrateur de l’ordinateur.
- **Azure Key Vault** : vous avez besoin des autorisations *create*, *get*, *unwrapKey*, *wrapKey*, *sign* et *verify* sur le coffre contenant la clé principale de colonne.
- **Fournisseur du magasin de clés (CNG)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur KSP.
- **Fournisseur de services de chiffrement (CAPI)**  : vous pouvez être invité à fournir les informations d’identification et autorisations nécessaires quand vous utilisez un magasin de clés ou une clé, selon le magasin et la configuration du fournisseur CSP.

Pour plus d’informations, consultez [Créer et stocker des clés principales de colonne (Always Encrypted)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md). 

 
## <a name="next-steps"></a>Étapes suivantes
- [Développer des applications avec Always Encrypted](always-encrypted-client-development.md)
- [Interroger des colonnes en utilisant Always Encrypted avec SQL Server Management Studio](always-encrypted-query-columns-ssms.md)

## <a name="see-also"></a>Voir aussi  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Vue d’ensemble de la gestion des clés pour Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Configurer Always Encrypted à l’aide de SQL Server Management Studio](configure-always-encrypted-using-sql-server-management-studio.md)
 - [Configurer le chiffrement de colonne à l’aide de l’Assistant Always Encrypted](always-encrypted-wizard.md)
 - [Configurer le chiffrement de colonne avec Always Encrypted en utilisant PowerShell](configure-column-encryption-using-powershell.md)
 
