---
description: Tables temporelles avec version gérée par le système avec tables optimisées en mémoire
title: Tables temporelles avec version gérée par le système avec des tables optimisées en mémoire | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 23274522-e5cf-4095-bed8-bf986d6342e0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 05ed5ec0d34d2a1a612b76c0117d0bf66675ad7a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171891"
---
# <a name="system-versioned-temporal-tables-with-memory-optimized-tables"></a>Tables temporelles avec version gérée par le système avec tables optimisées en mémoire


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Les tables temporelles à système par version pour les [tables optimisées en mémoire](../in-memory-oltp/sample-database-for-in-memory-oltp.md) sont conçues pour fournir une solution rentable dans des scénarios où [l’audit de données et l’analyse à un moment donné](./temporal-table-usage-scenarios.md) sont nécessaires sur des données collectées à l’aide de charges de travail OLTP en mémoire. Elles offrent un débit transactionnel élevé et une simultanéité sans verrouillage, ainsi que la possibilité de stocker un grand nombre de données d’historique pouvant être interrogées facilement.

## <a name="overview"></a>Vue d’ensemble

Les tables temporelles à système par version conservent automatiquement un historique complet des modifications apportées aux données et exposent des extensions Transact-SQL utiles pour l’analyse à un moment donné. Dans un scénario classique, l’historique des données est conservé pendant très longtemps (plusieurs mois, voire années), même s’il n’est pas régulièrement interrogé.

L’audit de données et l’analyse à un moment donné peuvent être exigés dans différents environnements, notamment dans les systèmes OLTP qui traitent un grand nombre de requêtes et où la technologie OLTP en mémoire est mise en œuvre. Toutefois, l’utilisation de tables optimisées en mémoire dans des scénarios temporels est complexe, car une grande quantité de données d’historique générées excède généralement la limite de mémoire RAM disponible. Pourtant, stocker dans la RAM des données d’historique en lecture seule qui sont moins sollicitées avec le temps n’est pas une solution optimale.

Les tables temporelles à système par version des tables optimisées en mémoire fournissent un débit transactionnel élevé et une simultanéité sans verrouillage, ainsi que la possibilité de stocker un grand nombre de données d’historique en utilisant des tables en mémoire pour stocker les données actuelles (la table temporelle) et des tables sur disque pour les données d’historique. L’impact sur les opération DML est réduit au minimum grâce à l’utilisation d’un table de mise en lots interne optimisée en mémoire et générée automatiquement qui stocke l’historique récent et permet aux DML de s’exécuter à partir du code compilé en mode natif.

Le diagramme suivant illustre cette architecture. ![Architecture en mémoire temporelle](../../relational-databases/tables/media/temporal-in-memory-architecture.png "Architecture en mémoire temporelle")

## <a name="implementation-details"></a>Informations d’implémentation

Les points suivants sur les tables temporelles à système par version avec tables optimisées en mémoire sont des considérations à connaître lors de la création d’une table à système par version à mémoire optimisée. Pour connaître les options de syntaxe et un exemple, consultez [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

- Seules les tables durables optimisées en mémoire peuvent être des tables à système par version (**DURABILITY = SCHEMA_AND_DATA**).
- La table d’historique de la table à système par version optimisée en mémoire doit être sur disque, qu’elle soit créée par l’utilisateur final ou le système.
- Les requêtes qui affectent uniquement la table active (en mémoire) peuvent être utilisées dans des [modules T-SQL compilés en mode natif](../in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md). Les requêtes temporelles utilisant la clause FOR SYSTEM TIME ne sont pas prises en charge dans les modules compilés en mode natif. L’utilisation de la clause FOR SYSTEM TIME avec les tables optimisées en mémoire dans les requêtes ad hoc et les modules non natifs est prise en charge.
- Quand **SYSTEM_VERSIONING = ON**, une table de mise en lots interne optimisée en mémoire est automatiquement créée pour accepter les dernières modifications apportées à la table à système par version qui résultent d’opérations de mise à jour et de suppression sur la table optimisée en mémoire actuelle.
- Les données de la table de mise en lots interne optimisée en mémoire sont régulièrement déplacées vers la table d’historique sur disque par la tâche de vidage de données asynchrones. Ce mécanisme de vidage des données vise à maintenir les mémoires tampons internes à moins de 10 % de la consommation de mémoire de leurs objets parents. Vous pouvez suivre la consommation de mémoire totale de la table temporelle à système par version optimisée en mémoire en interrogeant [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md) et en résumant les données de la table de mise en lots interne optimisée en mémoire et de la table temporelle actuelle.
- Vous pouvez appliquer un vidage des données en appelant [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md).
- Quand **SYSTEM_VERSIONING = OFF** ou quand le schéma de la table à système par version est modifié par l’ajout, la suppression ou le changement de colonnes, tout le contenu de la mémoire tampon de mise en lots interne est déplacé vers la table d’historique sur disque.
- L’interrogation des données d’historique est en réalité sous le niveau d’isolement de capture instantanée et renvoie toujours une union entre la mémoire tampon de mise en lots en mémoire et la table sur disque sans doublons.
- Les opérations **ALTER TABLE** qui modifient le schéma de la table en interne doivent effectuer un vidage de données, ce qui peut allonger la durée de l’opération.

## <a name="the-internal-memory-optimized-staging-table"></a>La table de mise en lots interne optimisée en mémoire

La table de mise en lots interne optimisée en mémoire est un objet interne créé par le système pour optimiser les opérations DML.

- Le nom de la table est généré au format suivant : **Memory_Optimized_History_Table_<ID_objet>** , où *<ID_objet>* est l’identificateur de la table temporelle actuelle.
- La table réplique le schéma de la table temporelle actuelle et une colonne BIGINT. Cette colonne supplémentaire garantit l’unicité des lignes déplacées vers la mémoire tampon de l’historique interne.
- La colonne supplémentaire est au format suivant : **Change_ID[_<suffixe>]** , où *_\<suffix>* est éventuellement ajouté dans les cas où la table contient déjà une colonne *Change_ID*.
- La taille de ligne maximale pour une table à système par version optimisée en mémoire est réduite de 8 octets en raison de la colonne BIGINT supplémentaire dans la table de mise en lots. La nouvelle valeur maximale est désormais 8 052 octets.
- La table de mise en lots interne optimisée en mémoire n’est pas représentée dans l’Explorateur d’objets de SQL Server Management Studio.
- Les métadonnées relatives à cette table, ainsi que sa connexion à la table temporelle actuelle, sont disponibles dans [sys.internal_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md).

## <a name="the-data-flush-task"></a>Tâche de vidage de données

Le vidage des données est une tâche régulièrement activée qui recherche si une table optimisée en mémoire répond à une condition basée sur la taille de mémoire pour le déplacement des données. Le déplacement des données commence lorsque la consommation de mémoire de la table de mise en lots interne atteint 8 % de la consommation de mémoire de la table temporelle actuelle.

La tâche de vidage de données est activée régulièrement selon une planification qui varie en fonction de la charge de travail existante. Sa fréquence peut aller de toutes les minutes pour une charge de travail légère à toutes les 5 secondes pour une charge de travail lourde. Un thread est généré pour chaque table de mise en lots interne optimisée en mémoire qui doit être nettoyée.

Le vidage de données supprime tous les enregistrements de la mémoire tampon interne en mémoire antérieurs à la plus ancienne des transactions en cours d’exécution et les déplace vers la table d’historique sur disque.

Vous pouvez appliquer un vidage des données en appelant [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md) et en spécifiant le nom du schéma et de la table : **sys.sp_xtp_flush_temporal_history \@schema_name, \@object_name**. Cette commande exécutée par l’utilisateur fait appel au même processus de déplacement de données que lorsque la tâche de vidage de données est appelée par le système selon la planification interne.

## <a name="see-also"></a>Voir aussi

- [Tables temporelles](../../relational-databases/tables/temporal-tables.md)
- [Prise en main des tables temporelles avec versions gérées par le système](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Scénarios d’utilisation de table temporelle](../../relational-databases/tables/temporal-table-usage-scenarios.md)
- [Vérifications de cohérence système des tables temporelles](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Partitionnement des tables temporelles](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Considérations et limitations liées aux tables temporelles](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Sécurité de la table temporelle](../../relational-databases/tables/temporal-table-security.md)
- [Gérer la rétention des données d’historique dans les tables temporelles avec contrôle de version par le système](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Vues et fonctions de métadonnées de table temporelle](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
