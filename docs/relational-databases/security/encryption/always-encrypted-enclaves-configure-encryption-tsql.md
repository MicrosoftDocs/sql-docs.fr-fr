---
description: Configurer le chiffrement de colonne sur place avec Transact-SQL
title: Configurer le chiffrement de colonne sur place avec Transact-SQL | Microsoft Docs
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
ms.openlocfilehash: ab59eec637bd5afc127227b09445417ffa1fe4eb
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534848"
---
# <a name="configure-column-encryption-in-place-with-transact-sql"></a>Configurer le chiffrement de colonne sur place avec Transact-SQL

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Cet article explique comment effectuer des opérations de chiffrement sur place sur des colonnes en utilisant Always Encrypted avec enclaves sécurisées, avec l’instruction [ALTER TABLE](../../../odbc/microsoft/alter-table-statement.md)/`ALTER COLUMN`. Pour obtenir des informations de base sur le chiffrement sur place et les prérequis généraux, consultez [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md).

Avec l’instruction `ALTER TABLE` ou `ALTER COLUMN`, vous pouvez définir la configuration de chiffrement cible pour une colonne. Quand vous exécutez l’instruction, l’enclave sécurisée côté serveur chiffre, rechiffre ou déchiffre les données stockées dans la colonne, en fonction de la configuration de chiffrement actuelle et cible spécifiée dans la définition de colonne de l’instruction. 
- Si la colonne n’est pas chiffrée actuellement, elle est chiffrée si vous spécifiez la clause `ENCRYPTED WITH` dans la définition de colonne.
- Si la colonne est actuellement chiffrée, elle est déchiffrée (convertie en colonne de texte en clair) si vous ne spécifiez pas la clause `ENCRYPTED WITH` dans la définition de colonne.
- Si la colonne est actuellement chiffrée, elle est rechiffrée si vous spécifiez la clause `ENCRYPTED WITH`, et si le type de chiffrement de colonne spécifié ou la clé de chiffrement de colonne spécifiée sont différents du type de chiffrement actuellement utilisé ou de la clé de chiffrement de colonne. 

> [!NOTE]
> Vous ne pouvez pas combiner des opérations de chiffrement avec d’autres modifications dans une même instruction `ALTER TABLE`/`ALTER COLUMN`, à l’exception du changement de la colonne en `NULL` ou en `NOT NULL`, ou du changement d’un classement. Par exemple, vous ne pouvez pas chiffrer une colonne ET changer le type de données de la colonne dans une même instruction Transact-SQL `ALTER TABLE`/`ALTER COLUMN`. Utilisez deux instructions distinctes.

Comme toute requête utilisant une enclave sécurisée côté serveur, une instruction `ALTER TABLE`/`ALTER COLUMN` qui déclenche un chiffrement sur place doit être envoyée sur une connexion avec Always Encrypted et les calculs d’enclave activés. 

Le reste de cet article décrit comment déclencher un chiffrement sur place avec l’instruction `ALTER TABLE`/`ALTER COLUMN` depuis SQL Server Management Studio. Vous pouvez également émettre l’instruction `ALTER TABLE`/`ALTER COLUMN` à partir d’Azure Data Studio ou de votre application. 

> [!NOTE]
> L’applet de commande [Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd) du module SqlServer PowerShell et [sqlcmd](../../../tools/sqlcmd-utility.md) ne prennent pas en charge l’utilisation de `ALTER TABLE`/`ALTER COLUMN` pour les opérations de chiffrement sur place.

## <a name="perform-in-place-encryption-with-transact-sql-in-ssms"></a>Effectuer un chiffrement sur place avec Transact-SQL dans SSMS
### <a name="pre-requisites"></a>Conditions préalables
- Les prérequis décrits dans [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md).
- SQL Server Management Studio 18.3 ou ultérieur si vous utilisez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SQL Server Management Studio 18.8 ou ultérieur si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="steps"></a>Étapes
1. Ouvrez une fenêtre de requête avec Always Encrypted et les calculs d’enclave activés dans la connexion de base de données. Pour plus d’informations, consultez [Activation et désactivation d’Always Encrypted pour une connexion de base de données](always-encrypted-query-columns-ssms.md#en-dis).
2. Dans la fenêtre de requête, émettez l’instruction `ALTER TABLE`/`ALTER COLUMN`, en spécifiant la configuration de chiffrement cible pour la colonne que vous souhaitez chiffrer, déchiffrer ou rechiffrer. Si vous chiffrez ou rechiffrez la colonne à l’aide de la clause `ENCRYPTED WITH`. Si votre colonne est de type chaîne (par exemple `char`, `varchar`, `nchar`, `nvarchar`), il peut également être nécessaire de changer le classement en BIN2. 
    
    > [!NOTE]
    > Si votre clé principale de colonne est stockée dans Azure Key Vault, vous serez peut-être invité à vous connecter à Azure.

3. Effacez le cache du plan pour l’ensemble des lots et des procédures stockées qui accèdent à la table, afin d’actualiser les informations de chiffrement des paramètres. 
 
    ```sql
    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```
    > [!NOTE]
    > Si vous ne supprimez pas le plan de la requête affectée à partir du cache, la première exécution de la requête après le chiffrement peut échouer.

    > [!NOTE]
    > Utilisez `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE` ou `DBCC FREEPROCCACHE` pour effacer soigneusement le cache du plan, car cela peut entraîner une dégradation temporaire des performances de requête. Pour limiter l’impact négatif de l’effacement du cache, vous pouvez supprimer uniquement les plans sélectionnés des requêtes concernées.

4.  Appelez [sp_refresh_parameter_encryption](../../system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) afin de mettre à jour les métadonnées pour les paramètres de chaque module (procédure stockée, fonction, affichage, déclencheur) persistants dans [sys.parameters](../..//system-catalog-views/sys-parameters-transact-sql.md) et qui peuvent avoir été invalidés lors du chiffrement des colonnes.

### <a name="examples"></a>Exemples
#### <a name="encrypting-a-column-in-place"></a>Chiffrement d’une colonne sur place
L’exemple ci-dessous suppose que :
- `CEK1` est une clé de chiffrement de colonne activée pour les enclaves.
- La colonne `SSN` est en texte clair et elle utilise le classement de base de données par défaut Latin1 non-BIN2 (par exemple `Latin1_General_CI_AI_KS_WS`).

L’instruction chiffre la colonne `SSN` en utilisant un chiffrement aléatoire et la clé de chiffrement de colonne activée pour les enclaves sur place. Elle remplace également le classement de base de données par défaut avec le classement BIN2 correspondant (dans la même page de codes).

L’opération est effectuée en ligne (`ONLINE = ON`). Notez également l’appel à `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE`, qui recrée les plans des requêtes affectées par la modification de schéma de table.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char] COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
WITH
(ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-change-encryption-type"></a>Rechiffrer une colonne sur place pour changer le type de chiffrement
L’exemple ci-dessous suppose que :
- La colonne `SSN` est chiffrée avec un chiffrement déterministe et une clé de chiffrement de colonne activée pour les enclaves, `CEK1`.
- Le classement actuel, défini au niveau de la colonne, est `Latin1_General_BIN2`.

L’instruction ci-dessous rechiffre la colonne avec un chiffrement aléatoire et la même clé (`CEK1`)

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-rotate-a-column-encryption-key"></a>Rechiffrer une colonne sur place pour effectuer la rotation d’une clé de chiffrement de colonne
L’exemple ci-dessous suppose que :
- La colonne `SSN` est chiffrée avec un chiffrement aléatoire et une clé de chiffrement de colonne activée pour les enclaves, `CEK1`.
- `CEK2` est une clé de chiffrement de colonne activée pour les enclaves (à la différence de `CEK1`).
- Le classement actuel, défini au niveau de la colonne, est `Latin1_General_BIN2`.

L’instruction ci-dessous rechiffre la colonne avec `CEK2`.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK2]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```
#### <a name="decrypt-a-column-in-place"></a>Déchiffrer une colonne sur place
L’exemple ci-dessous suppose que :
- La colonne `SSN` est chiffrée avec une clé de chiffrement de colonne activée pour les enclaves.
- Le classement actuel, défini au niveau de la colonne, est `Latin1_General_BIN2`.

L’instruction ci-dessous déchiffre la colonne et laisse le classement inchangé. Vous pouvez également choisir de changer le classement. Par exemple, remplacez le classement par un classement non BIN2 dans la même instruction.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
WITH (ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

## <a name="next-steps"></a>Étapes suivantes
- [Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées](always-encrypted-enclaves-query-columns.md)
- [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md)
- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Voir aussi  
- [Résoudre les problèmes courants concernant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-troubleshooting.md)
- [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md)
- [Activer Always Encrypted avec enclaves sécurisées pour les colonnes chiffrées existantes](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)