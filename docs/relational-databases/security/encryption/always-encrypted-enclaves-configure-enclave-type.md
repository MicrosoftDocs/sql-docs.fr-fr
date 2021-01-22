---
title: Configurer l’enclave sécurisée dans SQL Server
description: Configurez l’enclave sécurisée pour Always Encrypted avec des enclaves sécurisées dans SQL Server.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: f2aa24e3ebd335251ad2721444e9f9d8645ef221
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534879"
---
# <a name="configure-the-secure-enclave-in-sql-server"></a>Configurer l’enclave sécurisée dans SQL Server

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Avant de pouvoir utiliser [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md) dans SQL Server, vous devez configurer votre instance de manière à initialiser l’enclave sécurisée au moment du démarrage. Par défaut, SQL Server n’initialise pas l’enclave sécurisée. Vous pouvez changer cela en définissant l’option de configuration de serveur **Type d’enclave de chiffrement de colonne** sur la valeur qui représente un type d’enclave valide pour votre environnement.

> [!NOTE]
> Le rôle chargé de configurer l’enclave sécurisée est celui de l’administrateur de bases de données. Consultez [Rôles et responsabilités lors de la configuration de l’attestation avec SGH](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

Pour [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], le type d’enclave pris en charge est la sécurité basée sur la virtualisation (VBS). Avant de configurer le type d’enclave VBS, nous vous recommandons de configurer l’attestation avec le service Guardian hôte (SGH) pour l’ordinateur qui héberge votre instance. Pour commencer à utiliser SGH, consultez [Planifier l’attestation avec le Service Guardian hôte](always-encrypted-enclaves-host-guardian-service-plan.md). La configuration de l’attestation active également la sécurité basée sur la virtualisation, ce qui est nécessaire pour que l’enclave VBS soit correctement initialisée. Pour plus d’informations, consultez [Vérifier que la sécurité basée sur la virtualisation est en cours d’exécution](always-encrypted-enclaves-host-guardian-service-register.md#step-2-verify-virtualization-based-security-is-running).

Pour obtenir des instructions détaillées sur la configuration du type d’enclave, consultez [Configurer le type d’enclave pour l’option de configuration de serveur Always Encrypted](../../../database-engine/configure-windows/configure-column-encryption-enclave-type.md).

## <a name="next-steps"></a>Étapes suivantes

 [Gérer des clés pour Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-manage-keys.md)

## <a name="see-also"></a>Voir aussi  
 
 [Options de configuration du serveur (SQL Server)](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)
