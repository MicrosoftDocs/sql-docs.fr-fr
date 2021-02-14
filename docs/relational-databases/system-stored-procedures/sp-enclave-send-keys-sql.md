---
description: sp_enclave_send_keys (Transact-SQL)
title: sp_enclave_send_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/19/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sp_enclave_send_keys
- sp_enclave_send_keys_TSQL
- sys.sp_enclave_send_keys
- sys.sp_enclave_send_keys_TSQL
helpviewer_keywords:
- sp_enclave_send_keys
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: f7730c1f019c55cd307a5bb0be2c55580403457c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075020"
---
# <a name="sp_enclave_send_keys-transact-sql"></a>sp_enclave_send_keys (Transact-SQL)
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Envoie les clés de chiffrement des colonnes, définies dans la base de données, à l’enclave sécurisée côté serveur utilisée avec [Always Encrypted avec des enclaves sécurisées](../security/encryption/always-encrypted-enclaves.md).

`sp_enclave_send_keys` n’envoie que les clés pour lesquelles l’enclave est activée et les colonnes de chiffrement qui utilisent le chiffrement aléatoire et qui ont des index. Pour une requête utilisateur standard, un pilote client fournit à l’enclave les clés nécessaires pour les calculs dans la requête. `sp_enclave_send_keys` envoie toutes les clés de chiffrement de colonne définies dans la base de données et utilisées pour les colonnes chiffrées des index. 

`sp_enclave_send_keys` offre un moyen simple d’envoyer des clés à l’enclave et de remplir le cache de clé de chiffrement de colonne pour les opérations d’indexation ultérieures. Utilisez `sp_enclave_send_keys` pour activer :
- Un administrateur de base de données pour reconstruire ou modifier des index ou des statistiques sur des colonnes de base de données chiffrées, si l’administrateur de base de données n’a pas accès aux clés principales de colonne. Consultez [appeler des opérations d’indexation à l’aide de clés de chiffrement de colonne mises en cache](../security/encryption/always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).
- [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] pour terminer la récupération des index sur les colonnes chiffrées. Voir [récupération de base de données](../security/encryption/always-encrypted-enclaves.md#database-recovery).
- Une application qui utilise .NET Framework Fournisseur de données pour SQL Server de charger en masse des données dans des colonnes chiffrées.

Pour appeler correctement `sp_enclave_send_keys` , vous devez vous connecter à la base de données avec Always Encrypted et les calculs de l’enclave activés pour la connexion à la base de données. Vous devez également avoir accès aux clés principales de colonne, en protégeant les clés de chiffrement de colonne, que vous enverrez et que vous avez besoin d’autorisations pour accéder Always Encrypted métadonnées de clé dans la base de données. 

## <a name="syntax"></a>Syntaxe  
  
```
sp_enclave_send_keys
[ ;]  
```

## <a name="arguments"></a>Arguments

Cette procédure stockée n’a pas d’arguments.

## <a name="return-value"></a>Valeur de retour

Cette procédure stockée n’a pas de valeur de retour.
  
## <a name="result-sets"></a>Jeux de résultats

Cette procédure stockée n’a pas de jeu de résultats.
  
## <a name="permissions"></a>Autorisations

 Exiger les `VIEW ANY COLUMN ENCRYPTION KEY DEFINITION` `VIEW ANY COLUMN MASTER KEY DEFINITION` autorisations et dans la base de données.  
  
## <a name="examples"></a>Exemples  
  
```sql
EXEC sp_enclave_send_keys;  
```

## <a name="see-also"></a>Voir aussi
- [Always Encrypted avec enclaves sécurisées](../security/encryption/always-encrypted-enclaves.md) 
 
- [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](../security/encryption/always-encrypted-enclaves-create-use-indexes.md)

- [Tutoriel : Créer et utiliser des index sur des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire](../security/tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)
