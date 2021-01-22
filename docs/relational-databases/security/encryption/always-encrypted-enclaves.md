---
title: Always Encrypted avec enclaves sécurisées
description: Découvrez la fonctionnalité Always Encrypted avec enclaves sécurisées pour SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: ed6a0a041cba407b06b26e8b1d800da1f47b2bbb
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534668"
---
# <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted avec enclaves sécurisées

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Always Encrypted avec enclaves sécurisées étend les fonctionnalités de calcul confidentiel d’[Always Encrypted](always-encrypted-database-engine.md) avec le chiffrement sur place et des requêtes confidentielles plus riches. Always Encrypted avec enclaves sécurisés est disponible dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] et dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] (en préversion).

Introduit dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] en 2015 et dans [!INCLUDE[sssql16](../../../includes/sssql16-md.md)], Always Encrypted protège la confidentialité des données sensibles contre les programmes malveillants et les utilisateurs *non autorisés* à privilèges élevés : administrateurs de bases de données (DBA), administrateurs d’ordinateurs, administrateurs de clouds ou toute autre personne ayant un accès légitime aux instances de serveur, au matériel, etc., mais ne devant pas avoir accès à tout ou partie des données réelles.  

Sans les améliorations abordées dans cet article, Always Encrypted protège les données en les chiffrant côté client, et en *n’autorisant jamais* l’affichage en texte clair des données ou des clés de chiffrement correspondantes dans le [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. Par conséquent, la fonctionnalité sur les colonnes chiffrées à l’intérieur de la base de données est extrêmement limitée. Les seules opérations que [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] peut effectuer sur les données chiffrées sont les comparaisons d’égalité (disponibles seulement avec le [chiffrement déterministe](always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption)). Toutes les autres opérations, notamment les opérations de chiffrement (chiffrement de données initial ou rotation de clés) et/ou les requêtes riches (par exemple, les critères spéciaux), ne sont pas prises en charge à l’intérieur de la base de données. Les utilisateurs doivent déplacer les données hors de la base de données pour effectuer ces opérations côté client.

Always Encrypted *avec enclaves sécurisées* résout ces limitations en autorisant des calculs sur les données de texte en clair à l’intérieur d’une enclave sécurisée côté serveur. Une enclave sécurisée est une région protégée de la mémoire au sein du processus [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. L’enclave sécurisée apparaît comme une zone opaque pour le reste de [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] et les autres processus de l’ordinateur d’hébergement. Il n’existe aucun moyen d’afficher les données ou le code à l’intérieur de l’enclave depuis l’extérieur, même avec un débogueur. Ces propriétés font de l’enclave sécurisée un *environnement d’exécution approuvé* capable d’accéder de manière sécurisée aux clés de chiffrement et aux données sensibles en clair, sans compromettre la confidentialité des données.

Always Encrypted utilise des enclaves sécurisées, comme illustré dans le diagramme suivant :

![flux de données](./media/always-encrypted-enclaves/ae-data-flow.png)

Lors de l’analyse d’une instruction Transact-SQL envoyée par une application, le [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] détermine si l’instruction contient des opérations sur des données chiffrées qui nécessitent l’utilisation de l’enclave sécurisée. Pour de telles instructions :

- Le pilote client envoie les clés de chiffrement de colonne requises pour les opérations à l’enclave sécurisée (sur un canal sécurisé) et envoie l’instruction pour exécution.

- Lors du traitement de l’instruction, le [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] délègue les opérations de chiffrement ou les calculs sur les colonnes chiffrées à l’enclave sécurisée. Si nécessaire, l’enclave déchiffre les données et effectue des calculs sur du texte en clair.

Pendant le traitement de l’instruction, les données et les clés de chiffrement de colonne ne sont pas exposées en clair dans le [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] en dehors de l’enclave sécurisée.

## <a name="supported-enclave-technologies"></a>Technologies d’enclave prises en charge

Dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], Always Encrypted avec enclaves sécurisées utilise des enclaves mémoire sécurisées de [sécurité basée sur la virtualisation (VBS)](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/), également appelées mode sécurisé virtuel ou enclaves VSM, dans Windows.

Dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], Always Encrypted avec enclaves sécurisées utilise des enclaves[Intel SGX (Software Guard Extensions)](https://itpeernetwork.intel.com/microsoft-azure-confidential-computing/). Intel SGX est une technologie d’environnement d’exécution de confiance basée sur le matériel qui est prise en charge dans les bases de données utilisant la configuration matérielle de la [série DC](https://docs.microsoft.com/azure/azure-sql/database/service-tiers-vcore?tabs=azure-portal#dc-series).

## <a name="secure-enclave-attestation"></a>Attestation d’enclave sécurisée

L’enclave sécurisée à l’intérieur du [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] peut accéder à des données sensibles et aux clés de chiffrement de colonne en clair. Avant d’envoyer une instruction impliquant des calculs d’enclave au [!INCLUDE[ssde-md](../../../includes/ssde-md.md)], le pilote client à l’intérieur de l’application doit donc vérifier que l’enclave sécurisée est une véritable enclave VBS ou SGX et que le code s’exécutant dans l’enclave sécurisée est la véritable bibliothèque Always Encrypted qui implémente les algorithmes de chiffrement Always Encrypted pour le chiffrement sur place et les opérations prise en charge dans les requêtes confidentielles.

Le processus de vérification de l’enclave, appelé **attestation d’enclave**, implique généralement un pilote client au sein de l’application et une communication entre [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] et un service d’attestation externe. Les spécificités du processus d’attestation dépendent du type de l’enclave (VBS ou SGX) et du service d’attestation.

Le processus d’attestation pour les enclaves sécurisées VBS dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] est l’[attestation de runtime Windows Defender System Guard](https://www.microsoft.com/security/blog/2018/06/05/virtualization-based-security-vbs-memory-enclaves-data-protection-through-isolation/), qui nécessite le service Guardian hôte (SGH) comme service d’attestation. 

L’attestation des enclaves Intel SGX dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] nécessite [Microsoft Azure Attestation](https://docs.microsoft.com/azure/attestation/overview).

> [!NOTE]
> [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] ne prend pas en charge Microsoft Azure Attestation. Le service Guardian hôte est la seule solution d’attestation prise en charge pour les enclaves VBS dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)].

## <a name="supported-client-drivers"></a>Pilotes clients pris en charge

Pour utiliser Always Encrypted avec enclaves sécurisées, une application doit utiliser un pilote client qui prend en charge la fonctionnalité. Configurez l’application et le pilote client pour autoriser les calculs d’enclave et l’attestation d’enclave. Pour plus d’informations, notamment la liste des pilotes clients pris en charge, consultez [Développer des applications à l’aide d’Always Encrypted](always-encrypted-client-development.md).

## <a name="terminology"></a>Terminologie

### <a name="enclave-enabled-keys"></a>Clés prenant en charge les enclaves

Always Encrypted avec enclaves sécurisées introduit le concept des clés prenant en charge l’enclave :

- **Clé principale de colonne prenant en charge les enclaves** : une clé principale de colonne avec la propriété `ENCLAVE_COMPUTATIONS` spécifiée dans l’objet de métadonnées de la clé principale de colonne à l’intérieur de la base de données. L’objet de métadonnées de la clé principale de colonne doit également contenir une signature valide des propriétés des métadonnées. Pour plus d’informations, consultez [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md).
- **Clé de chiffrement de colonne prenant en charge l’enclave** : clé de chiffrement de colonne qui est chiffrée avec une clé principale de colonne prenant en charge l’enclave. Seules les clés de chiffrement de colonne prenant en charge les enclaves peuvent être utilisées pour les calculs à l’intérieur de l’enclave sécurisée. 

Pour plus d’informations, consultez [Gérer des clés pour Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-manage-keys.md).

### <a name="enclave-enabled-columns"></a>Colonnes prenant en charge les enclaves

Une colonne prenant en charge l’enclave est une colonne de base de données qui est chiffrée avec une clé de chiffrement de colonne prenant en charge l’enclave.

## <a name="confidential-computing-capabilities-for-enclave-enabled-columns"></a>Fonctionnalités de calcul confidentiel pour les colonnes prenant en charge les enclaves

Les deux principaux avantages d’Always Encrypted avec enclaves sécurisées sont le chiffrement sur place et les requêtes confidentielles riches.

### <a name="in-place-encryption"></a>Chiffrement sur place

Le chiffrement sur place autorise des opérations de chiffrement sur les colonnes de base de données à l’intérieur de l’enclave sécurisée, sans déplacer les données hors de la base de données. Le chiffrement sur place améliore les performances et la fiabilité du chiffrement. Vous pouvez effectuer un chiffrement sur place à l’aide de l’instruction [ALTER TABLE (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md). 

Les opérations de chiffrement sur place prises en charge sont les suivantes :

- Chiffrement d’une colonne de texte en clair avec une clé de chiffrement de colonne prenant en charge les enclaves.
- Rechiffrement d’une colonne chiffrée prenant en charge les enclaves pour :
  - Effectuer la rotation d’une clé de chiffrement de colonne (rechiffrer la colonne avec une nouvelle clé de chiffrement de colonne prenant en charge les enclaves).
  - Changer le type de chiffrement d’une colonne prenant en charge les enclaves, par exemple remplacer le chiffrement déterministe par le chiffrement aléatoire.
- Déchiffrement des données stockées dans une colonne prenant en charge les enclaves (conversion de la colonne en colonne de texte en clair).

Le chiffrement sur place est autorisé avec les chiffrements déterministe et aléatoire, à condition que les clés de chiffrement de colonne impliquées dans une opération de chiffrement prennent en charge les enclaves.

### <a name="confidential-queries"></a>Requêtes confidentielles

Les requêtes confidentielles sont des [requêtes DML](../../../t-sql/queries/queries.md) qui impliquent des opérations sur des colonnes prenant en charge les enclaves effectuées à l’intérieur de l’enclave sécurisée.

Les opérations prises en charge dans les enclaves sécurisées sont les suivantes :

| Opération| [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] | [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] |
|:---|:---|:---|
| [Opérateurs de comparaison](../../../mdx/comparison-operators.md) | Prise en charge | Prise en charge |
| [BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md) | Prise en charge | Prise en charge |
| [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md) | Prise en charge | Prise en charge |
| [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md) | Prise en charge | Prise en charge |
| [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select) | Prise en charge | Prise en charge |
| [Joins](../../performance/joins.md) | Seules les jointures de boucles imbriquées sont prises en charge | Prise en charge |
| [SELECT - Clause ORDER BY (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md) | Non pris en charge | Prise en charge |
| [SELECT - GROUP BY- Transact-SQL](../../../t-sql/queries/select-group-by-transact-sql.md) | Non pris en charge | Prise en charge |

> [!NOTE]
> Les opérations ci-dessus sont prises en charge dans les enclaves sécurisées uniquement sur des colonnes prenant en charge les enclaves avec le chiffrement **aléatoire** (pas le chiffrement déterministe). La comparaison d’égalité reste le seul calcul pris en charge sur des colonnes avec le chiffrement déterministe, et elle est effectuée en comparant le texte chiffré en dehors de l’enclave, peu importe si la colonne prend en charge les enclaves ou pas. Le chiffrement déterministe prend en charge les opérations suivantes impliquant des comparaisons d’égalité : 
> - [= (Égal à)](../../../t-sql/language-elements/equals-transact-sql.md) dans la recherche de points, les recherches et les jointures
> - [IN](../../../t-sql/language-elements/in-transact-sql.md)
> - [SELECT - GROUP BY](../../../t-sql/queries/select-group-by-transact-sql.md)
> - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
>
> Dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)], les requêtes confidentielles utilisant des enclaves sur une colonne de chaîne de caractères (`char`, `nchar`) nécessitent que la colonne utilise un classement d’ordre de tri binary2 (BIN2). Dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], les requêtes confidentielles sur les chaînes de caractères nécessitent un classement BIN2 ou UTF-8. 

### <a name="indexes-on-enclave-enabled-columns"></a>Index sur des colonnes prenant en charge les enclaves

Vous pouvez créer des index non cluster sur des colonnes prenant en charge les enclaves à l’aide du chiffrement aléatoire pour accélérer l’exécution des requêtes DML confidentielles utilisant l’enclave sécurisée.

Pour garantir qu’un index sur une colonne chiffrée à l’aide d’un chiffrement aléatoire n’entraîne pas la fuite de données sensibles, les valeurs de clés dans la structure des données d’index (B-tree) sont chiffrées et triées en fonction de leur valeurs de texte en clair. Le tri par la valeur de texte en clair est également utile pour traiter les requêtes à l’intérieur de l’enclave. Quand l’exécuteur de requête dans le [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] utilise un index sur une colonne chiffrée pour effectuer des calculs à l’intérieur de l’enclave, il parcourt l’index pour rechercher des valeurs spécifiques stockées dans la colonne. Chaque recherche peut impliquer plusieurs comparaisons. L’exécuteur de requête délègue chaque comparaison à l’enclave, qui déchiffre une valeur stockée dans la colonne et la valeur de clé de l’index chiffré à comparer, il effectue la comparaison sur du texte en clair et retourne le résultat de la comparaison à l’exécuteur.

La création d’index sur des colonnes qui utilisent le chiffrement aléatoire et ne prennent pas en charge les enclaves n’est toujours pas prise en charge.

Un index sur une colonne utilisant un chiffrement déterministe est trié en fonction du texte chiffré (pas du texte en clair), que la colonne prenne ou non en charge les enclaves.

Pour plus d’informations, consultez [Créer et utiliser des index sur des colonnes à l’aide d’Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-create-use-indexes.md). Pour obtenir des informations générales sur la façon dont fonctionne l’indexation dans [!INCLUDE[ssde-md](../../../includes/ssde-md.md)], consultez l’article [Description des index cluster et non cluster](../../indexes/clustered-and-nonclustered-indexes-described.md).

### <a name="database-recovery"></a>Récupération des bases de données

Si une instance de SQL Server échoue, ses bases de données peuvent être laissées dans un état où les fichiers de données peuvent contenir des modifications résultant de transactions incomplètes. Lorsque l’instance est démarrée, elle exécute un processus appelé [récupération de base de données](../../logs/the-transaction-log-sql-server.md#recovery-of-all-incomplete-transactions-when--is-started), qui implique la restauration de chaque transaction incomplète trouvée dans le journal des transactions pour garantir que l’intégrité de la base de données est préservée. Si une transaction incomplète a apporté des modifications à un index, ces modifications doivent également être annulées. Par exemple, certaines valeurs de clés dans l’index peuvent devoir être supprimées ou réinsérées.

> [!IMPORTANT]
> Microsoft recommande fortement d'activer la [Récupération de base de données accélérée (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) pour votre base de données **avant** de créer le premier index sur une colonne prenant en charge les enclaves avec un chiffrement aléatoire. ADR est activé par défaut dans [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], mais pas dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)].

Avec le [processus de récupération de base de données traditionnel](/azure/sql-database/sql-database-accelerated-database-recovery#the-current-database-recovery-process) (qui suit le modèle de récupération [ARIES](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf)), pour annuler une modification apportée à un index, SQL Server doit attendre qu’une application fournisse la clé de chiffrement de colonne pour la colonne de l’enclave, ce qui peut prendre un certain temps. La récupération de base de données accélérée (ADR) réduit notablement le nombre d’opérations d’annulation qui doivent être reportées parce qu’une clé de chiffrement de colonne n’est pas disponible dans le cache au sein de l’enclave. Par conséquent, elle augmente sensiblement la disponibilité de la base de données en réduisant au minimum le risque de blocage d’une nouvelle transaction. Avec ADR activée, SQL Server peut toujours avoir besoin d’une clé de chiffrement de colonne pour effectuer le nettoyage d’anciennes versions de données. Cependant, cette tâche d’arrière-plan n’affecte pas la disponibilité des transactions de la base de données ou des utilisateurs. Toutefois, vous pouvez voir les messages d’erreur dans le journal des erreurs, qui indiquent des opérations de nettoyage ayant échoué parce qu’il manquait une clé de chiffrement de colonne.

## <a name="security-considerations"></a>Considérations relatives à la sécurité

Les considérations de sécurité suivantes s’appliquent à Always Encrypted avec enclaves sécurisés.

- La sécurité de vos données au sein de l’enclave dépend d’un protocole et d’un service d’attestation. Par conséquent, vous devrez vérifier que le service et les stratégies d’attestation mis en œuvre par le service d’attestation sont bien gérés par un administrateur approuvé. De plus, les services d’attestation prennent généralement en charge des stratégies et des protocoles d’attestation différents, dont certains procèdent à une vérification minimale de l’enclave et de son environnement et sont conçus pour les tests et le développement. Respectez scrupuleusement les instructions spécifiques à votre service d’attestation pour vérifier que vous utilisez les configurations et les stratégies recommandées pour vos déploiements de production. 
- Le chiffrement d’une colonne à l’aide d’un chiffrement aléatoire avec une clé de chiffrement prenant en charge les enclaves peut entraîner une fuite de l’ordre des données stockées dans la colonne, puisque ces colonnes prennent en charge des comparaisons de plages. Par exemple, si une colonne chiffrée contenant les salaires des employés a un index, un administrateur de base de données malveillant pourrait analyser l’index pour trouver la valeur de salaire chiffrée maximale et identifier une personne touchant le salaire maximal (en supposant que le nom de la personne n’est pas chiffré). 
- Si vous utilisez Always Encrypted pour protéger des données sensibles contre un accès non autorisé des administrateurs de bases de données, ne partagez pas les clés principales des colonnes ou les clés de chiffrement des colonnes avec les administrateurs de bases de données. Un administrateur de base de données peut gérer des index sans avoir directement accès aux clés, en tirant parti du cache des clés de chiffrement de colonne au sein de l’enclave.

## <a name="considerations-for-business-continuity-disaster-recovery-and-data-migration"></a><a name="anchorname-1-considerations-availability-groups-db-migration"></a> Considérations relatives à la continuité d’activité, à la reprise d’activité après sinistre et à la migration des données

Lors de la configuration d’une solution à haute disponibilité ou de reprise d’activité après sinistre pour une base de données à l’aide d’Always Encrypted avec enclaves sécurisées, vérifiez que tous les réplicas de base de données peuvent utiliser une enclave sécurisée. Si une enclave est disponible pour le réplica principal, mais pas pour le réplica secondaire, toute instruction qui tente d’utiliser la fonctionnalité d’Always Encrypted avec enclaves sécurisées échoue après le basculement.

Quand vous copiez ou migrez une base de données à l’aide d’Always Encrypted avec enclaves sécurisées, vérifiez que l’environnement cible prend toujours en charge les enclaves. Sinon, les instructions qui utilisent des enclaves ne fonctionneront pas sur la copie ou la base de données migrée.

Voici les considérations spécifiques à prendre en compte :

- **SQL Server**
  - Quand vous configurez un [groupe de disponibilité Always On](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md), vous devez vérifier que chaque instance de SQL Server qui héberge une base de données dans le groupe de disponibilité prend en charge Always Encrypted avec enclaves sécurisées et a une enclave et une attestation configurées.
  - Lors de la restauration d’un fichier de sauvegarde d’une base de données qui utilise la fonctionnalité d’Always Encrypted avec enclaves sécurisées sur une instance de SQL Server qui n’a pas d’enclave configurée, l’opération de restauration réussit et toutes les fonctionnalités qui ne reposent pas sur l’enclave sont disponibles. Toutefois, toutes les instructions suivantes utilisant la fonctionnalité de l’enclave échouent, et les index sur des colonnes prenant en charge des enclaves à l’aide d’un chiffrement aléatoire ne sont plus valides. La même chose est valable lors de l’attachement d’une base de données utilisant Always Encrypted avec enclaves sécurisées sur l’instance pour laquelle l’enclave n’est pas configurée.
  - Si votre base de données contient des index sur des colonnes prenant en charge les enclaves avec un chiffrement aléatoire, veillez à activer la [récupération de base de données accélérée (ADR)](../../backup-restore/restore-and-recovery-overview-sql-server.md#adr) dans la base de données avant de créer une sauvegarde de base de données. ADR garantit que la base de données, notamment les index, est disponible immédiatement après la restauration de la base de données. Pour plus d’informations, consultez [Récupération de la base de données](#database-recovery).
  
- **Azure SQL Database**
  - Lors de la configuration de la [géoréplication active](https://docs.microsoft.com/azure/azure-sql/database/active-geo-replication-overview), vérifiez qu’une base de données secondaire prend en charge les enclaves sécurisées si la base de données primaire les prend en charge.

Dans SQL Server et Azure SQL Database, quand vous migrez votre base de données à l’aide d’un fichier bacpac, veillez à supprimer tous les index des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire avant de créer le fichier bacpac.

## <a name="known-limitations"></a>Limitations connues

Always Encrypted avec enclaves sécurisées résout certaines limitations d’Always Encrypted en prenant en charge le chiffrement sur place et les requêtes confidentielles riches avec index, comme cela est expliqué dans [Fonctionnalités de calcul confidentiel pour les colonnes prenant en charge les enclaves](#confidential-computing-capabilities-for-enclave-enabled-columns).

Toutes les autres limitations d’Always Encrypted listées dans [Détails de la fonctionnalité](always-encrypted-database-engine.md#feature-details) s’appliquent également à Always Encrypted avec enclaves sécurisées.

Les limitations suivantes sont spécifiques à Always Encrypted avec enclaves sécurisées :

- Il est impossible de créer des index cluster sur des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire.
- Les colonnes prenant en charge les enclaves utilisant un chiffrement aléatoire ne peuvent pas être des colonnes de clé primaire, ni être référencées par des contraintes de clé étrangères ou des contraintes de clé unique.
- Dans [!INCLUDE[sql-server-2019](../../../includes/sssqlv15-md.md)] (cette limitation ne s’applique pas à [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]), seules les jointures de boucles imbriquées (avec des index, le cas échéant) sont prises en charge sur les colonnes compatibles avec les enclaves avec un chiffrement aléatoire. Pour plus d’informations sur les différences entre les produits, consultez [Requêtes confidentielles](#confidential-queries).
- Les opérations de chiffrement sur place ne peuvent pas être combinées avec d’autres modifications des métadonnées de la colonne, à l’exception des modifications d’un classement au sein de la même page de codes et possibilité de valeur null. Par exemple, vous ne pouvez pas chiffrer, rechiffrer ou déchiffrer une colonne ET changer un type de données de la colonne dans une seule instruction Transact-SQL `ALTER TABLE`/`ALTER COLUMN`. Utilisez deux instructions distinctes.
- L’utilisation de clés prenant en charge les enclaves pour les colonnes dans des tables en mémoire n’est pas prise en charge.
- Les expressions qui définissent des colonnes calculées ne peuvent pas effectuer de calculs sur des colonnes prenant en charge les enclaves avec un chiffrement aléatoire (même si les calculs font partie des opérations prises en charge listées dans les [requêtes confidentielles](#confidential-queries)).
- Les caractères d’échappement ne sont pas pris en charge dans les paramètres de l’opérateur LIKE sur les colonnes avec enclave utilisant un chiffrement aléatoire.
- Les requêtes avec l’opérateur LIKE ou un opérateur de comparaison avec un paramètre de requête utilisant l’un des types de données suivants (qui deviennent des objets volumineux après chiffrement) ignorent les index et effectuent des analyses de table.
  - `nchar[n]` et `nvarchar[n]`, si n est supérieur à 3967.
  - `char[n]`, `varchar[n]`, `binary[n]`, `varbinary[n]`, si n est supérieur à 7935.
- Limitations des outils :
  - Les seuls magasins de clés pris en charge pour le stockage des clés principales de colonne prenant en charge l’enclave sont le Magasin de certificats Windows et Azure Key Vault.
  - L’importation/exportation de bases de données contenant des clés activées pour les enclaves n’est pas prise en charge.
  - Pour déclencher une opération de chiffrement sur place via `ALTER TABLE`/`ALTER COLUMN`, vous devez émettre l’instruction à l’aide d’une fenêtre de requête dans SSMS ou vous pouvez écrire votre propre programme qui émet l’instruction. Actuellement, l’applet de commande Set-SqlColumnEncryption dans le module PowerShell SqlServer et l’Assistant Always Encrypted dans SQL Server Management Studio ne prennent pas en charge le chiffrement sur place : ils déplacent les données en dehors de la base de données pour les opérations de chiffrement, même si les clés de chiffrement de colonne utilisées pour les opérations prennent en charge l’enclave.

## <a name="next-steps"></a>Étapes suivantes

- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Configurer et utiliser Always Encrypted avec enclaves sécurisées](configure-always-encrypted-enclaves.md)

## <a name="see-also"></a>Voir aussi

- [Gérer des clés pour Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves-manage-keys.md)