---
title: Créer une clé principale de base de données | Microsoft Docs
description: Créez une clé principale de base de données dans SQL Server à l’aide de Transact-SQL. Vérifiez que vous disposez des autorisations requises.
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], creating
ms.assetid: 8cb24263-e97d-4e4d-9429-6cf494a4d5eb
author: jaszymas
ms.author: jaszymas
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f97dbd4425b990cd2ec433afa817081683b38f45
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251414"
---
# <a name="create-a-database-master-key"></a>Créer une clé principale de base de données

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Cette rubrique explique comment créer une clé principale de base de données dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)].

## <a name="security"></a>Sécurité

### <a name="permissions"></a>Autorisations

Requiert l'autorisation CONTROL sur la base de données.

## <a name="using-transact-sql"></a>Utilisation de Transact-SQL

### <a name="to-create-a-database-master-key"></a>Pour créer une clé principale de base de données

1. Choisissez un mot de passe pour chiffrer la copie de la clé principale qui sera stockée dans la base de données.
2. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].
3. Développez **Bases de données système**, cliquez avec le bouton droit sur `master`, puis cliquez sur **Nouvelle requête**.
4. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.

   ```sql
     -- Creates the master key.
     -- The key is encrypted using the password "23987hxJ#KL95234nl0zBe".  
     CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';  

   ```

Pour plus d’informations, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-master-key-transact-sql.md).
