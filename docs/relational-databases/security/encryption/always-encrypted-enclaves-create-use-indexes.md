---
description: Créer et utiliser des index sur des colonnes en utilisant Always Encrypted avec enclaves sécurisées
title: Créer et utiliser des index sur des colonnes en utilisant Always Encrypted avec enclaves sécurisées | Microsoft Docs
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
ms.openlocfilehash: 4799cd725dce4eb8300717b8c89d601e9915f7d2
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534828"
---
# <a name="create-and-use-indexes-on-columns-using-always-encrypted-with-secure-enclaves"></a>Créer et utiliser des index sur des colonnes en utilisant Always Encrypted avec enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Cet article explique comment créer et utiliser des index sur des colonnes chiffrées en utilisant des clés de chiffrement de colonne activées pour les enclaves avec [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md).

Always Encrypted avec enclaves sécurisées prend en charge :
- Les index cluster et non-cluster sur les colonnes chiffrées en utilisant des clés de chiffrement déterministe et activées pour les enclaves.
  - Ces index sont triés en fonction du texte chiffré. Aucune considération particulière ne s’applique à ces index. Vous pouvez les gérer et les utiliser de la même façon que les index sur les colonnes chiffrées en utilisant un chiffrement déterministe et des clés qui ne sont pas activées pour les enclaves (comme avec Always Encrypted). 
- Les index non-cluster sur les colonnes chiffrées avec des clés de chiffrement aléatoire et activées pour les enclaves.
  - Les valeurs de clé dans la structure de données d’index (arbre B) sont chiffrées et triées en fonction de leurs valeurs en texte clair. Pour plus d’informations, consultez [Index sur des colonnes prenant en charge les enclaves](always-encrypted-enclaves.md#indexes-on-enclave-enabled-columns).

> [!NOTE]
> Le reste de cet article traite des index non-cluster sur les colonnes chiffrées avec des clés de chiffrement aléatoire et activées pour les enclaves.

Comme un index sur une colonne utilisant un chiffrement aléatoire et une clé de chiffrement de colonne activée pour les enclaves contient des données chiffrées (texte chiffré) triées en fonction du texte en clair, le moteur SQL Server doit utiliser l’enclave pour toute opération qui implique la création, la mise à jour ou la recherche dans un index, notamment :

- Création ou régénération d'un index.
- Insertion, mise à jour ou suppression d’une ligne dans une table (contenant une colonne indexée/chiffrée), qui déclenche l’insertion et/ou la suppression d’une clé d’index dans l’index.
- Exécution de commandes `DBCC` qui impliquent la vérification de l’intégrité des index, par exemple [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) ou [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).
- Récupération de base de données (par exemple après un échec et le redémarrage de SQL Server), si SQL Server doit annuler des modifications apportées à l’index (plus de détails ci-dessous).

Toutes les opérations ci-dessus nécessitent que l’enclave ait la clé de chiffrement de colonne pour la colonne indexée. La clé est nécessaire pour déchiffrer les clés d’index. En règle générale, l’enclave peut obtenir une clé de chiffrement de colonne dans une des deux façons suivantes :
- Directement depuis l’application cliente.
- À partir du cache de clés de chiffrement de colonne.

## <a name="invoke-indexing-operations-with-column-encryption-keys-provided-directly-by-the-client"></a>Appeler des opérations d’indexation avec des clés de chiffrement de colonne fournies directement par le client
Pour que cette méthode d’appel d’opérations d’indexation fonctionne, l’application (incluant un outil comme SQL Server Management Studio (SSMS)) émettant une requête qui déclenche une opération sur un index doit :

- Se connecter à la base de données avec Always Encrypted et des calculs d’enclave activés pour la connexion de base de données.
- L’application doit avoir accès à la clé principale de colonne protégeant la clé de chiffrement de colonne pour la colonne indexée.

Une fois que le moteur SQL Server analyse la requête de l’application et détermine qu’elle doit mettre à jour un index sur une colonne chiffrée pour exécuter la requête, il demande au pilote du client d’émettre la clé de chiffrement de colonne nécessaire à l’enclave via un canal sécurisé. C’est exactement le même mécanisme qui est utilisé pour fournir à l’enclave les clés de chiffrement de colonne pour le traitement de toutes les autres requêtes qui n’utilisent pas d’index. Par exemple, un chiffrement sur place ou des requêtes utilisant une correspondance de modèle et des comparaisons de plages.

Cette méthode est pratique pour vérifier que la présence d’index sur des colonnes chiffrées est transparente pour les applications déjà connectées à la base de données avec Always Encrypted et activées pour les calculs d’enclave. La connexion de l’application peut utiliser l’enclave pour le traitement des requêtes. Une fois que vous créez un index sur une colonne, le pilote à l’intérieur de votre application fournira en toute transparence les clés de chiffrement de colonne à l’enclave pour les opérations d’indexation. La création d’index peut augmenter le nombre de requêtes qui nécessitent que l’application envoie les clés de chiffrement de la colonne à l’enclave.

Pour utiliser cette méthode, suivez les instructions générales concernant l’exécution des instructions avec une enclave sécurisée qui sont fournies dans [Exécuter des instructions Transact-SQL utilisant des enclaves sécurisées](always-encrypted-enclaves-query-columns.md).

Pour obtenir des instructions pas à pas sur la façon d’utiliser cette méthode, consultez [Didacticiel : Création et utilisation des index sur des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).

## <a name="invoke-indexing-operations-using-cached-column-encryption-keys"></a>Appeler des opérations d’indexation à l’aide de clés de chiffrement de colonne mises en cache

Une fois qu’une application cliente envoie une clé de chiffrement de colonne à l’enclave pour traiter n’importe quelle requête nécessitant des calculs d’enclave, l’enclave met en cache la clé de chiffrement de colonne dans un cache interne. Ce cache se trouve à l’intérieur de l’enclave et est inaccessible à partir de l’extérieur.

Si cette même application ou une autre application cliente utilisée par ce même utilisateur ou un autre utilisateur déclenche une opération sur un index sans fournir directement le chiffrement de colonne nécessaire, l’enclave recherche la clé de chiffrement de colonne dans le cache. Par conséquent, l’opération sur l’index réussit, bien que l’application cliente n’a pas fourni la clé.

Pour cette méthode d’appel au fonctionnement d’opérations d’indexation, l’application doit se connecter à la base de données sans Always Encrypted activé pour la connexion et la clé de chiffrement de colonne requise doit être disponible dans le cache à l’intérieur de l’enclave.

Cette méthode d’appel des opérations est prise en charge seulement pour les requêtes qui ne nécessitent pas de clés de chiffrement de colonne pour les autres opérations non liées à des index. Par exemple, une application insérant une ligne avec une instruction `INSERT` dans une table qui contient une colonne chiffrée doit se connecter à la base de données avec Always Encrypted activé dans la chaîne de connexion, et elle doit avoir accès aux clés, que la colonne chiffrée ait ou non un index.

Cette méthode est précise pour :
 - Vérifier que la présence d’index sur des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire est transparente pour les applications et les utilisateurs n’ayant pas accès aux clés et aux données en texte en clair. 
 - Elle garantit que la création d’un index sur une colonne chiffrée ne ralentit pas les requêtes existantes. Si une application émet une requête sur une table contenant des colonnes chiffrées sans nécessiter l’accès aux clés, l’application peut continuer à s’exécuter sans avoir accès aux clés après la création d’un index par un administrateur de base de données. Par exemple, considérez une application qui exécute la requête ci-dessous sur la table **Employees**, qui contient des colonnes chiffrées. L’administrateur de base de données n’a créé d’index sur aucune des colonnes chiffrées.

   ```sql
   DELETE FROM [dbo].[Employees] WHERE [EmployeeID] = 1;
   GO
   ```

   Si l’application envoie la requête via une connexion sans Always Encrypted et les calculs d’enclave activés, la requête réussit. La requête ne déclenche aucun calcul sur les colonnes chiffrées. Une fois qu’un administrateur de base de données a créé un index sur des colonnes chiffrées, la requête déclenche la suppression des clés d’index dans les index. Dans ce cas, l’enclave a besoin des clés de chiffrement de colonne. Toutefois, l’application sera en mesure de continuer à exécuter cette requête sur la même connexion tant qu’un propriétaire des données a fourni les clés de chiffrement de colonne à l’enclave.

 - Pour obtenir la séparation des rôles lors de la gestion des index, car elle permet aux DBA de créer et modifier des index sur des colonnes chiffrées, sans avoir accès aux données sensibles. 

> [!TIP] 
> [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md) vous permet d’envoyer facilement à l’enclave toutes les clés de chiffrement de colonne activées pour les enclaves utilisées pour les index et de remplir le cache de clés.

Pour obtenir des instructions pas à pas sur la façon d’utiliser cette méthode, consultez [Didacticiel : Création et utilisation des index sur des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md). 

## <a name="next-steps"></a>Étapes suivantes
- [Exécuter des instructions Transact-SQL à l’aide d’enclaves sécurisées](always-encrypted-enclaves-query-columns.md)

## <a name="see-also"></a>Voir aussi  
- [Tutoriel : Création et utilisation des index sur des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).
- [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md)
