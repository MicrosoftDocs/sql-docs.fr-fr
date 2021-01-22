---
description: Effectuer une rotation de clés activées pour les enclaves
title: Effectuer une rotation | Microsoft Docs
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
ms.openlocfilehash: 8ef5e2e3cbf4e00619aeb4b0d1643f0d07100aad
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534316"
---
# <a name="rotate-enclave-enabled-keys"></a>Effectuer une rotation de clés activées pour les enclaves

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Dans Always Encrypted, une rotation de clé est le processus de remplacement d’une clé principale de colonne existante ou d’une clé de chiffrement de colonne par une nouvelle clé. Cet article décrit les cas d’utilisation et les considérations relatives à la rotation de clés spécifique à [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md) quand la clé initiale et/ou la (nouvelle) clé cible est une clé activée pour les enclaves. Pour obtenir des instructions générales et des processus de gestion des clés Always Encrypted, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](overview-of-key-management-for-always-encrypted.md). 

Il peut être nécessaire d’effectuer la rotation d’une clé pour des raisons de sécurité ou de conformité. Par exemple, si une clé a été compromise ou si les stratégies de votre organisation vous obligent à remplacer les clés régulièrement. En outre, Always Encrypted utilisant la rotation de clés avec des enclaves sécurisées permet d’activer ou de désactiver les fonctionnalités de l’enclave sécurisée côté serveur pour vos colonnes chiffrées.

- Quand vous remplacez une clé qui n’est pas activée pour les enclaves par une clé qui l’est, vous déverrouillez la fonctionnalité de l’enclave sécurisée qui permet d’interroger les colonnes qui sont protégées par la clé. Pour plus d’informations, consultez [Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- Quand vous remplacez une clé qui est activée pour les enclaves par une clé qui ne l’est pas, vous désactivez la fonctionnalité de l’enclave sécurisée qui permet d’interroger les colonnes qui sont protégées par la clé.

Si vous effectuez la rotation d’une clé uniquement pour des raisons de sécurité et/ou de conformité, et non pas pour activer ou désactiver les calculs d’enclaves pour vos colonnes, vérifiez que la clé cible a la même configuration que la clé source concernant les enclaves. Par exemple, si la clé source est activée pour les enclaves, la clé cible doit également être activée pour les enclaves.

Les étapes ci-dessous incluent des liens vers des articles détaillés, selon votre scénario de rotation :

1. Provisionnez une nouvelle clé (une clé principale de colonne ou une clé de chiffrement de colonne).
    - Pour approvisionner une nouvelle clé activée pour les enclaves, consultez [Provisionner des clés activées pour les enclaves](always-encrypted-enclaves-provision-keys.md).
    - Pour provisionner une clé qui n’est pas activée pour les enclaves, consultez [Provisionner des clés Always Encrypted avec SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md) et [Provisionner des clés Always Encrypted avec PowerShell](configure-always-encrypted-keys-using-powershell.md).
2. Remplacez une clé existante par la nouvelle clé.
    - Si vous effectuez la rotation d’une clé de chiffrement de colonne, et que la clé source et la clé cible sont toutes deux activées pour les enclaves, vous pouvez effectuer la rotation (qui implique le rechiffrement de vos données) sur place. Pour plus d’informations, consultez [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md).
    - Pour obtenir des instructions détaillées sur la rotation des clés, consultez [Effectuer la rotation de clés Always Encrypted avec SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) et [Effectuer la rotation de clés Always Encrypted avec PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Étapes suivantes

- [Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées](always-encrypted-enclaves-query-columns.md)
- [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md)
- [Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md)  

## <a name="see-also"></a>Voir aussi  
- [Gérer des clés pour Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-manage-keys.md)