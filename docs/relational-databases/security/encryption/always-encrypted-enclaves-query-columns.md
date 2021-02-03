---
title: Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées
description: Exécuter des instructions DDL (Data Definition Language) pour configurer le chiffrement de votre colonne ou gérer des index sur des colonnes chiffrées, et interroger des colonnes chiffrées
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: bc92b0af972236b588369869afc5b023735ae699
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237125"
---
# <a name="run-transact-sql-statements-using-secure-enclaves"></a>Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md) permet à certaines instructions T-SQL (Transact-SQL) d’effectuer des calculs confidentiels sur des colonnes de base de données chiffrées dans une enclave sécurisée côté serveur.

## <a name="statements-using-secure-enclaves"></a>Instructions utilisant des enclaves sécurisées

Les types suivants d’instructions T-SQL utilisent des enclaves sécurisées.

### <a name="ddl-statements-using-secure-enclaves"></a>Instructions DDL utilisant des enclaves sécurisées

Les types suivants d’instructions [DDL (Data Definition Language)](../../../t-sql/statements/statements.md#data-definition-language) nécessitent des enclaves sécurisées.

- Instructions [ALTER TABLE column_definition (Transact-SQL)](../../../t-sql/statements/alter-table-column-definition-transact-sql.md) qui déclenchent des opérations de chiffrement sur place avec des clés prenant en charge les enclaves. Pour plus d’informations, consultez [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md).
- Instructions [CREATE INDEX (Transact-SQL)](../../../t-sql/statements/create-index-transact-sql.md) et [ALTER INDEX (Transact-SQL)](../../../t-sql/statements/alter-index-transact-sql.md) qui créent ou modifient des index sur des colonnes prenant en charge les enclaves avec un chiffrement aléatoire. Pour plus d’informations, consultez [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md).
  
### <a name="dml-statements-using-secure-enclaves"></a>Instructions DML utilisant des enclaves sécurisées

Les instructions ou requêtes [DML (Data Manipulation Language)](../../../t-sql/statements/statements.md#data-manipulation-language) suivantes sur des colonnes prenant en charge les enclaves avec un chiffrement aléatoire nécessitent des enclaves sécurisées :

- Requêtes utilisant un ou plusieurs des opérateurs Transact-SQL suivants pris en charge dans les enclaves sécurisées :
  - [Opérateurs de comparaison](../../../mdx/comparison-operators.md)
  - [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md)
  - [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md)
  - [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md)
  - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
  - [Jointures](../../performance/joins.md) - [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] prend en charge uniquement les jointures de boucles imbriquées. [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] prend en charge les jointures de boucles imbriquées, hachées et de fusion
  - [SELECT - Clause ORDER BY (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md). Prise en charge dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. Non pris en charge dans [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]
  - [SELECT - Clause ORDER BY (Transact-SQL)](../../../t-sql/queries/select-group-by-transact-sql.md). Prise en charge dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. Non pris en charge dans [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]
- Requêtes qui insèrent, mettent à jour ou suppriment des lignes, ce qui déclenche l’insertion et/ou la suppression d’une clé d’index dans un index sur une colonne prenant en charge les enclaves. Pour plus d’informations, consultez [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md).

> [!NOTE]
> Les opérations sur les index et les requêtes DML confidentielles utilisant des enclaves sont uniquement prises en charge sur les colonnes prenant en charge les enclaves qui utilisent le chiffrement aléatoire. Le chiffrement déterministe n’est pas pris en charge.
>
> Dans [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)], les requêtes confidentielles utilisant des enclaves sur des colonnes de chaînes de caractères (`char`, `nchar`) nécessitent un classement d’ordre de tri binary2 (BIN2) configuré pour la colonne. Dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], l’utilisation de classements BIN2 ou UTF-8 est obligatoire.

### <a name="dbcc-commands-using-secure-enclaves"></a>Commandes DBCC utilisant des enclaves sécurisées

Les commandes administratives [DBCC (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-transact-sql.md) qui impliquent la vérification de l’intégrité d’index peuvent également nécessiter des enclaves sécurisées si la base de données contient des index sur des colonnes prenant en charge les enclaves avec un chiffrement aléatoire. Par exemple, [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) et [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).

## <a name="prerequisites-for-running-statements-using-secure-enclaves"></a>Prérequis pour l’exécution d’instructions utilisant des enclaves sécurisées

Votre environnement doit répondre aux conditions suivantes pour prendre en charge l’exécution d’instructions utilisant une enclave sécurisée.

- Votre instance [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], ou votre base de données et votre serveur dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], doivent être correctement configurés pour prendre en charge les enclaves et l’attestation. Pour plus d’informations, consultez [Configurer l’enclave sécurisée et l’attestation](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Vous devez obtenir une URL d’attestation de votre environnement auprès de votre administrateur de service d’attestation.

  - Si vous utilisez [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et le service Guardian hôte (SGH), consultez [Déterminer et partager l’URL d’attestation SGH](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] et Microsoft Azure Attestation, consultez [Déterminer l’URL d’attestation de votre stratégie d’attestation](/sql/relational-databases/security/encryption/always-encrypted-enclaves?view=sql-server-ver15#secure-enclave-attestation).

- Si vous vous connectez à votre base de données à l’aide de votre application, celle-ci doit utiliser un pilote client qui prend en charge Always Encrypted avec enclaves sécurisées. Pour connecter l’application à la base de données, vous devez activer Always Encrypted pour la connexion de base de données et configurer correctement le protocole et l’URL d’attestation. Pour obtenir des informations détaillées, consultez [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md).
- Si vous utilisez SSMS (SQL Server Management Studio) ou Azure SQL Data Studio, vous devez activer Always Encrypted et configurer le protocole et l’URL d’attestation lors de la connexion à votre base de données. Pour plus d’informations, consultez les sections suivantes.

> [!NOTE]
> Pour les opérations suivantes, vous n’êtes pas obligé d’activer Always Encrypted et de configurer l’attestation pour vous connecter à la base de données si vous utilisez des clés de chiffrement de colonne mises en cache : Requêtes DDL qui créent ou modifient des index, requêtes DML qui mettent à jour des index et commandes DBCC qui vérifient l’intégrité d’index. Pour plus d’informations, consultez [Appeler des opérations d’indexation à l’aide de clés de chiffrement de colonne mises en cache](always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms"></a>Prérequis pour l’exécution d’instructions T-SQL utilisant des enclaves dans SSMS

Version minimale requise de SQL Server Management Studio :

- SSMS 18.3 avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SSMS 18.8 avec [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Veillez à exécuter vos instructions à partir d’une fenêtre de requête qui utilise une connexion sur laquelle vous avez activé Always Encrypted et correctement configuré l’URL d’attestation.

1. Dans la boîte de dialogue **Se connecter au serveur**, spécifiez le nom de votre serveur, sélectionnez une méthode d’authentification et spécifiez vos informations d’identification.
2. Cliquez sur **Options >>** et sélectionnez l’onglet **Always Encrypted**.
3. Cochez la case **Activer Always Encrypted (chiffrement de colonne)** et spécifiez votre URL d’attestation d’enclave. Par exemple, `https://hgs.bastion.local/Attestation` ou `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`.

    ![Se connecter au serveur avec une attestation à l’aide de SSMS](./media/always-encrypted-enclaves/column-encryption-setting.png)

4. Sélectionnez **Se connecter**.
5. Si vous êtes invité à activer les requêtes Paramétrage d’Always Encrypted, sélectionnez **Activer** si vous prévoyez d’exécuter des requêtes DML paramétrées. Pour plus d’informations, consultez [Paramétrage d’Always Encrypted](always-encrypted-query-columns-ssms.md#param).

Pour obtenir des informations supplémentaires, consultez [Activation et désactivation d’Always Encrypted pour une connexion de base de données](always-encrypted-query-columns-ssms.md#en-dis).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio"></a>Prérequis pour l’exécution d’instructions T-SQL utilisant des enclaves dans Azure Data Studio

Il est recommandé d’utiliser au minimum la version **1.23** ou une version ultérieure.

Veillez à exécuter vos instructions à partir d’une fenêtre de requête qui utilise une connexion sur laquelle vous avez activé Always Encrypted et correctement configuré le protocole et l’URL d’attestation.

1. Dans la boîte de dialogue **Connexion**, cliquez sur **Avancé...** .
2. Pour activer Always Encrypted pour la connexion, définissez le champ **Always Encrypted** sur **Activé**.
3. Spécifiez le protocole et l’URL d’attestation.
    - Si vous utilisez [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)], définissez le **Service Guardian hôte** comme **Protocole d’attestation** et entrez votre URL d’attestation du service Guardian hôte dans le champ **URL d’attestation de l’enclave**.
    - Si vous utilisez [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], définissez **Attestation Azure** comme **Protocole d’attestation** et entrez l’URL d’attestation faisant référence à votre stratégie dans Microsoft Azure Attestation dans le champ **URL d’attestation de l’enclave**.

    ![Se connecter au serveur avec une attestation à l’aide d’Azure Data Studio](./media/always-encrypted-enclaves/azure-data-studio-connect-with-enclaves.png)

4. Cliquez sur **OK** pour fermer les **Propriétés avancées**.

Pour obtenir des informations supplémentaires, consultez [Activation et désactivation d’Always Encrypted pour une connexion de base de données](always-encrypted-query-columns-ads.md#enabling-and-disabling-always-encrypted-for-a-database-connection).

Si vous prévoyez d’exécuter des requêtes DML paramétrées, vous devez également activer le [Paramétrage d’Always Encrypted](always-encrypted-query-columns-ads.md#parameterization-for-always-encrypted).

## <a name="examples"></a>Exemples

Cette section comprend des exemples de requêtes DML utilisant des enclaves. 

Les exemples utilisent le schéma ci-dessous.

```sql
CREATE SCHEMA [HR];
GO

CREATE TABLE [HR].[Jobs](
 [JobID] [int] IDENTITY(1,1) PRIMARY KEY,
 [JobTitle] [nvarchar](50) NOT NULL,
 [MinSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [MaxSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
);
GO

CREATE TABLE [HR].[Employees](
 [EmployeeID] [int] IDENTITY(1,1) PRIMARY KEY,
 [SSN] [char](11) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [FirstName] [nvarchar](50) NOT NULL,
 [LastName] [nvarchar](50) NOT NULL,
 [Salary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [JobID] [int] NULL,
 FOREIGN KEY (JobID) REFERENCES [HR].[Jobs] (JobID)
);
GO
```

### <a name="exact-match-search"></a>Recherche de correspondance exacte

La requête ci-dessous effectue une recherche de correspondance exacte sur la colonne de chaîne chiffrée `SSN`.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="pattern-matching-search"></a>Recherche avec des critères spéciaux

La requête ci-dessous effectue une recherche avec des critères spéciaux sur la colonne de chaîne chiffrée `SSN` pour trouver les employés dont les quatre derniers chiffres du numéro de sécurité sociale (USA) correspondent aux chiffres spécifiés.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="range-comparison"></a>Comparaison de plage

La requête ci-dessous effectue une comparaison de plage sur la colonne chiffrée `Salary` pour trouver les employés dont le salaire se trouve dans la plage spécifiée.

```sql
DECLARE @MinSalary money = 40000;
DECLARE @MaxSalary money = 45000;
SELECT * FROM [HR].[Employees] WHERE [Salary] > @MinSalary AND [Salary] < @MaxSalary;
GO
```

### <a name="joins"></a>Jointures

La requête ci-dessous effectue une jointure entre les tables `Employees` et `Jobs` à l’aide de la colonne `Salary` chiffrée. La requête récupère les employés dont le salaire est en dehors d’une plage de salaires pour le travail de l’employé.

```sql
SELECT * FROM [HR].[Employees] e
JOIN [HR].[Jobs] j
ON e.[JobID] = j.[JobID] AND e.[Salary] > j.[MaxSalary] OR e.[Salary] < j.[MinSalary];
GO
```

### <a name="sorting"></a>Tri

La requête ci-dessous trie les enregistrements d’employés en fonction de la colonne chiffrée `Salary` pour récupérer les 10 employés les mieux payés.
> [!NOTE]
> Le tri de colonnes chiffrées est pris en charge dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], mais pas dans [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

```sql
SELECT TOP(10) * FROM [HR].[Employees]
ORDER BY [Salary] DESC;
GO
```

## <a name="next-steps"></a>Étapes suivantes

- [Développer des applications en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Voir aussi

- [Résoudre les problèmes courants concernant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-troubleshooting.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Configurer le chiffrement de colonne sur place en utilisant Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-configure-encryption.md)
- [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md)
