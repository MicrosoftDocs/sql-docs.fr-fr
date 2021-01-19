---
description: Gérer les clés pour Always Encrypted avec enclaves sécurisées
title: Gérer les clés pour Always Encrypted avec enclaves sécurisées | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 68cc07fdf37dc358d0de024c3227f7225ebea27c
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534426"
---
# <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>Gérer les clés pour Always Encrypted avec enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md) étend la gestion des clés pour [Always Encrypted](always-encrypted-database-engine.md) en introduisant des clés activées pour les enclaves : 

- **Clé principale de colonne activée pour les enclaves** : une clé principale de colonne qui est créée avec la propriété `ENCLAVE_COMPUTATIONS` spécifiée dans l’objet de métadonnées de la clé principale de colonne à l’intérieur de la base de données. 
- **Clé de chiffrement de colonne prenant en charge l’enclave** : clé de chiffrement de colonne qui est chiffrée avec une clé principale de colonne prenant en charge l’enclave. Seules les clés de chiffrement de colonne activées pour les enclaves peuvent être utilisées pour les calculs à l’intérieur d’une enclave sécurisée côté serveur. 

Les instructions générales et les processus de [gestion des clés Always Encrypted](overview-of-key-management-for-always-encrypted.md) s’appliquent à la gestion des clés activées pour les enclaves. 

## <a name="managing-keys"></a>Gestion des clés

Les articles suivants traitent des aspects spécifiques à la gestion des clés activées pour les enclaves.

- [Provisionner des clés activées pour les enclaves](always-encrypted-enclaves-provision-keys.md)
- [Permuter des clés activées pour les enclaves](always-encrypted-enclaves-rotate-keys.md)

## <a name="next-steps"></a>Étapes suivantes
- [Provisionner des clés activées pour les enclaves](always-encrypted-enclaves-provision-keys.md)

## <a name="see-also"></a>Voir aussi  
- [Vue d’ensemble de la gestion des clés pour Always Encrypted](overview-of-key-management-for-always-encrypted.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
