---
title: Configurer et utiliser Always Encrypted avec enclaves sécurisées | Microsoft Docs
description: Découvrez comment configurer et utiliser Always Encrypted avec enclaves sécurisées dans SQL Server et Azure SQL Database, afin d’utiliser des fonctionnalités plus riches pour les données sensibles.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 481493a50fdefc22f6eb4d77feb13cfc4848388d
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534648"
---
# <a name="configure-and-use-always-encrypted-with-secure-enclaves"></a>Configurer et utiliser Always Encrypted avec enclaves sécurisées 

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted avec enclaves sécurisés](always-encrypted-enclaves.md) étend la fonctionnalité [Always Encrypted](always-encrypted-database-engine.md) existante pour activer des fonctionnalités plus complexes sur les données sensibles tout en préservant la confidentialité des données. Cet article liste les tâches courantes de configuration et d’utilisation de la fonctionnalité.

Pour consulter des tutoriels montrant comment se mettre rapidement à Always Encrypted avec enclaves sécurisées, consultez :

- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="set-up-the-secure-enclave-and-attestation"></a>Configurer l’enclave sécurisée et l’attestation

Avant de pouvoir utiliser Always Encrypted avec enclaves sécurisées, vous devez configurer votre environnement de sorte que l’enclave sécurisée soit disponible pour la base de données. Vous devez également configurer l’[attestation d’enclave](always-encrypted-enclaves.md#secure-enclave-attestation). 

Le processus de configuration de votre environnement varie selon que vous utilisez [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] ou [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="set-up-the-secure-enclave-and-attestation-in-ssnoversion-md"></a>Configurer l’enclave sécurisée et l’attestation dans [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

Pour plus de détails, consultez les articles suivants :
- [Planifier l’attestation avec le Service Guardian hôte](./always-encrypted-enclaves-host-guardian-service-plan.md)
- [Déployer le Service Guardian hôte pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md)
- [Inscrire votre ordinateur auprès du Service Guardian hôte](./always-encrypted-enclaves-host-guardian-service-register.md)
- [Configurer l’enclave sécurisée dans SQL Server](always-encrypted-enclaves-configure-enclave-type.md)

### <a name="set-up-the-secure-enclave-and-attestation-in-sssdsfull"></a>Configurer l’enclave sécurisée et l’attestation dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]

Pour plus de détails, consultez les articles suivants :
- [Planifier les enclaves et l’attestation Intel SGX dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-plan)
- [Activer Intel SGX pour votre instance Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-enable-sgx)
- [Configurer Azure Attestation pour votre serveur logique Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-sqldbmi-configure-attestation)

## <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>Gérer les clés pour Always Encrypted avec enclaves sécurisées
Consultez les articles suivants pour plus de détails :
- [Gérer des clés pour Always Encrypted avec enclaves sécurisées - vue d’ensemble](always-encrypted-enclaves-manage-keys.md)
- [Provisionner des clés activées pour les enclaves](always-encrypted-enclaves-provision-keys.md)
- [Permuter des clés activées pour les enclaves](always-encrypted-enclaves-rotate-keys.md)

## <a name="configure-columns-with-always-encrypted-with-secure-enclaves"></a>Configurer des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées
Consultez les articles suivants pour plus de détails :
- [Configurer le chiffrement de colonne sur place à l’aide d’Always Encrypted avec enclaves sécurisées - vue d’ensemble](always-encrypted-enclaves-configure-encryption.md)
- [Configurer le chiffrement de colonne sur place avec Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md)
- [Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes](always-encrypted-enclaves-enable-for-encrypted-columns.md)

## <a name="run-transact-sql-statements-using-secure-enclaves"></a>Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées
Consultez les articles suivants pour plus de détails :
- [Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées](always-encrypted-enclaves-query-columns.md)
- [Résoudre les problèmes courants concernant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-troubleshooting.md)

## <a name="create-and-use-indexes-on-enclave-enabled-columns"></a>Créer et utiliser des index sur des colonnes avec enclave
Consultez les articles suivants pour plus de détails :
- [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md)
  
## <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Développer des applications avec Always Encrypted avec enclaves sécurisées
Consultez les articles suivants pour plus de détails :
- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md)
