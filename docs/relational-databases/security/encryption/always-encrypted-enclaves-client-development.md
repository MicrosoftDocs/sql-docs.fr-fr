---
description: Développer des applications avec Always Encrypted avec enclaves sécurisées
title: Développer des applications avec Always Encrypted avec enclaves sécurisées | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
dev_langs:
- CSharp
ms.assetid: 9595eb66-284c-4474-828f-8961a05ce989
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64c628172ba95ff9de546c018ea00a9ba63943c9
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075601"
---
# <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Développer des applications avec Always Encrypted avec enclaves sécurisées
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md) étend [Always Encrypted](always-encrypted-database-engine.md) pour permettre des fonctionnalités plus riches dans les requêtes des applications sur des colonnes de base de données sensibles chiffrées. Elle tire parti des technologies des enclaves sécurisées pour permettre à l’exécuteur d’une requête dans [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] de déléguer des calculs sur des colonnes chiffrées à une enclave sécurisée à l’intérieur du processus [!INCLUDE[ssde-md](../../../includes/ssde-md.md)].

## <a name="prerequisites"></a>Prérequis

- Votre instance [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], ou votre base de données et votre serveur dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], doivent être correctement configurés pour prendre en charge les enclaves et l’attestation. Pour plus d’informations, consultez [Configurer l’enclave sécurisée et l’attestation](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Vous devez obtenir une URL d’attestation pour votre environnement auprès de votre administrateur de service d’attestation.

  - Si vous utilisez [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), consultez [Déterminer et partager l’URL d’attestation SGH](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, consultez [Déterminer l’URL d’attestation de votre de stratégie d’attestation](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation).

- Votre application doit utiliser une version de pilote client SQL qui prend en charge les enclaves sécurisées. Pour plus d’informations, consultez les sections ci-dessous.

- Vous devez configurer un protocole d’attestation et une URL d’attestation pour une connexion de base de données. La manière exacte de configurer le protocole d’attestation et l’URL d’attestation dépend du pilote client que vous utilisez.

## <a name="client-drivers-for-always-encrypted-with-secure-enclaves"></a>Pilotes clients pour Always Encrypted avec enclaves sécurisées

Pour développer des applications en utilisant Always Encrypted avec enclaves sécurisées, vous avez besoin d’une version du pilote client SQL qui prend en charge les enclaves sécurisées. Le pilote client joue le rôle clé suivant :

- Avant de soumettre pour exécution une requête qui utilise une enclave sécurisée à [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], le pilote lance l’attestation d’enclave, pour vérifier que l’enclave sécurisée est fiable et peut être utilisée de façon sécurisée pour traiter des données sensibles. Pour plus d’informations sur l’attestation, consultez [Attestation des enclaves sécurisées](always-encrypted-enclaves.md#secure-enclave-attestation).
- Une fois l’attestation réussie, le pilote client établit une session sécurisée avec l’enclave en négociant un secret partagé.
- Le pilote utilise le secret partagé pour chiffrer les clés de chiffrement de colonne dont l’enclave aura besoin pour traiter la requête, puis envoie les clés à [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], qui les transfère à l’enclave sécurisée qui déchiffre les clés. 
- Enfin, le pilote envoie la requête pour exécution, ce qui déclenche les calculs à l’intérieur de l’enclave sécurisée.

## <a name="next-steps"></a>Étapes suivantes

Les pilotes clients suivants prennent en charge Always Encrypted avec enclaves sécurisées :

- Fournisseur de données Microsoft .NET pour SQL Server dans .NET Framework 4.6 ou ultérieur et .NET Core 2.1 ou ultérieur. 
    - Pour plus d’informations, consultez [Utilisation d’Always Encrypted avec le fournisseur de données Microsoft .NET pour SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md).
    - Pour obtenir un tutoriel pas à pas, consultez [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](../../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md).
    - Consultez également [Exemple illustrant l’utilisation du fournisseur Azure Key Vault et d’Always Encrypted avec enclaves sécurisées](../../../connect/ado-net/sql/azure-key-vault-enclave-example.md).
- Microsoft ODBC Driver for SQL Server, version 17.4 ou ultérieure. 
    - Pour plus d’informations, consultez [Utilisation d’Always Encrypted avec ODBC Driver](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md). 
    - Pour plus d’informations sur la façon d’activer les calculs d’enclave pour une connexion de base de données en utilisant ODBC, consultez la section [Activation d’Always Encrypted avec enclaves sécurisées](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves).
- Microsoft JDBC Driver pour SQL Server, version 8.2 ou ultérieure.
    - Pour plus d’informations, consultez [Utilisation d’Always Encrypted avec enclaves sécurisées avec le pilote JDBC](../../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md).
- Fournisseur de données .NET Framework pour SQL Server dans .NET Framework 4.7.2 ou supérieur. 
    - Pour plus d’informations, consultez [Utilisation d’Always Encrypted avec le fournisseur de données .NET Framework pour SQL Server](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md).
    - Pour obtenir un tutoriel pas à pas, consultez [Tutoriel : Développer une application .NET Framework avec Always Encrypted avec enclaves sécurisées](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>Voir aussi

- [Résoudre les problèmes courants concernant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-troubleshooting.md)
