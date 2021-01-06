---
title: Octroi d’autorisations d’exécution de scripts Python et R
description: Découvrez comment autoriser les utilisateurs à exécuter des scripts Python et R externes dans SQL Server Machine Learning Services et accorder des autorisations de lecture, d’écriture ou de langage de définition de données (DDL) sur les bases de données.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019, contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 498021cc74f93ed216088ae1756f386604c07bb5
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878732"
---
# <a name="grant-users-permission-to-execute-python-and-r-scripts-with-sql-server-machine-learning-services"></a>Octroi d’autorisations d’exécution de scripts Python et R aux utilisateurs avec SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Découvrez comment autoriser les utilisateurs à exécuter des scripts Python et R externes dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) et accorder des autorisations de lecture, d’écriture ou de langage de définition de données (DDL) sur les bases de données.

Pour plus d’informations, consultez la section Autorisations dans la [vue d’ensemble de la sécurité du framework d’extensibilité](../../machine-learning/concepts/security.md#permissions).

<a name="permissions-external-script"></a>

## <a name="permission-to-run-scripts"></a>Autorisation d’exécuter des scripts

Vous devez autoriser chaque utilisateur qui exécute des scripts Python ou R avec SQL Server Machine Learning Services sans être administrateur à exécuter des scripts externes dans toutes les bases de données où le langage est utilisé.

Pour accorder l’autorisation d’exécuter un script externe, exécutez le script suivant :

```sql
USE <database_name>
GO
GRANT EXECUTE ANY EXTERNAL SCRIPT TO [UserName]
```

> [!NOTE]
> Les autorisations ne sont pas spécifiques au langage de script pris en charge. En d’autres termes, il n’existe pas de niveaux d’autorisation distincts pour les scripts R et Python.

<a name="permissions-db"></a>

## <a name="grant-databases-permissions"></a>Accorder des autorisations sur les bases de données

Quand un utilisateur exécute des scripts, il peut avoir besoin de lire les données d’autres bases de données. Il peut également être amené à créer des tables pour stocker les résultats et à écrire des données dans des tables.

Vérifiez que chaque compte d’utilisateur Windows ou connexion SQL qui exécute des scripts R ou Python dispose des autorisations appropriées sur la base de données en question : 

+ `db_datareader` pour lire des données.
+ `db_datawriter` pour enregistrer des objets dans la base de données.
+ `db_ddladmin` pour créer des objets comme des procédures stockées ou des tables contenant des données entraînées et sérialisées.

Par exemple, l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante donne à la connexion SQL *MySQLLogin* les droits nécessaires pour exécuter des requêtes T-SQL dans la base de données *ML_Samples*. Pour exécuter cette instruction, la connexion SQL doit déjà exister dans le contexte de sécurité du serveur.

```sql
USE ML_Samples
GO
EXEC sp_addrolemember 'db_datareader', 'MySQLLogin'
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les autorisations incluses dans chaque rôle, consultez [Rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md).
