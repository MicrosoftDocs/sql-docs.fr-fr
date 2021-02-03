---
description: Instructions pour les opérations d’index en ligne
title: Instructions pour les opérations d’index en ligne | Microsoft Docs
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- clustered indexes, online operations
- online index operations
- indexes [SQL Server], online operations
- disk space [SQL Server], indexes
- nonclustered indexes [SQL Server], online operations
- transaction logs [SQL Server], indexes
ms.assetid: d82942e0-4a86-4b34-a65f-9f143ebe85ce
author: MikeRayMSFT
ms.author: mikeray
ms.prod_service: table-view-index, sql-database
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9612070531dc13d9646d439501925f59f62c8da7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236907"
---
# <a name="guidelines-for-online-index-operations"></a>Instructions pour les opérations d’index en ligne

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Lorsque vous effectuez des opérations en ligne sur les index, les directives suivantes s'appliquent :  

- Les index cluster doivent être créés, reconstruits ou supprimés hors connexion quand la table sous-jacente contient les types de données LOB (Large OBject) suivants : **image**, **ntext** et **text**.  
- Les index non cluster non uniques peuvent être créés en ligne lorsque la table contient des types de données LOB, mais qu'aucune de ces colonnes n'est utilisée dans la définition de l'index en tant que colonne clé ou non-clé (incluse).  
- Les index de tables temporaires locales ne peuvent pas être créés, reconstruits ou supprimés en ligne. Cette restriction ne s'applique pas aux index des tables temporaires globales.
- L’exécution des index peut reprendre là où elle s’est arrêtée après une défaillance inattendue, un basculement de base de données ou une commande **PAUSE**. Consultez [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) et [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md).

> [!NOTE]  
> Les opérations d’index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions](../../sql-server/editions-and-components-of-sql-server-2016.md).  

Le tableau suivant présente les opérations d’index réalisables en ligne, les index qui sont exclus de ces opérations en ligne et les restrictions d’index pouvant être reprises. Des restrictions supplémentaires sont également incluses.  

| Opération en ligne sur l'index | Index exclus | Autres restrictions |  
|----------------------------|----------------------|------------------------|  
|ALTER INDEX REBUILD|Index cluster désactivé ou vue indexée désactivée<br /><br /> Index XML<br /><br />Index columnstore <br /><br /> Index de table temporaire locale|Si le mot clé ALL est spécifié, l'opération peut échouer lorsque la table contient un index exclu.<br /><br /> Des restrictions supplémentaires s'appliquent pour la reconstruction d'index désactivés. Pour plus d’informations, consultez [Désactiver les index et contraintes](../../relational-databases/indexes/disable-indexes-and-constraints.md).|  
|CREATE INDEX|Index XML<br /><br /> Index cluster unique de départ sur une vue<br /><br /> Index de table temporaire locale||  
|CREATE INDEX WITH DROP_EXISTING|Index cluster désactivé ou vue indexée désactivée<br /><br /> Index de table temporaire locale<br /><br /> Index XML||  
|DROP INDEX|Index désactivés.<br /><br /> Index XML<br /><br /> Index non cluster<br /><br /> Index de table temporaire locale|Il n'est pas possible de spécifier plusieurs index dans une même instruction.|  
|ALTER TABLE ADD CONSTRAINT (PRIMARY KEY ou UNIQUE)|Index de table temporaire locale<br /><br /> Index cluster|Une seule sous-clause est autorisée à la fois. Par exemple, vous ne pouvez ni ajouter ni supprimer les contraintes PRIMARY KEY ou UNIQUE dans la même instruction ALTER TABLE.|  
|ALTER TABLE DROP CONSTRAINT (PRIMARY KEY ou UNIQUE)|Index cluster||  
  
 La table sous-jacente ne peut pas être modifiée, tronquée ou supprimée tant qu'une opération en ligne sur l'index est en cours.  
  
 Le paramètre de l'option online (ON ou OFF) spécifié lors de la création ou de la suppression d'un index cluster est appliqué à tous les index non cluster qui doivent être reconstruits. Par exemple, si l'index cluster est construit en ligne à l'aide de CREATE INDEX WITH DROP_EXISTING, ONLINE=ON, tous les index non cluster associés sont également recréés en ligne.  
  
 Lors de la création ou de la reconstruction d'un index UNIQUE en ligne, le générateur d'index et une transaction utilisateur simultanée peuvent tenter d'insérer la même clé, enfreignant ainsi la condition d'unicité. Si une ligne entrée par un utilisateur est insérée dans le nouvel index (cible) avant le déplacement de la ligne d'origine de la table source dans le nouvel index, l'opération en ligne sur l'index échoue.  
  
 Même si ce cas de figure est rare, l'opération en ligne sur l'index peut provoquer un blocage lorsqu'elle interagit avec les mises à jour de base de données en raison d'activités d'un utilisateur ou d'une application. Dans ces rares cas, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] sélectionne l'activité de l'utilisateur ou de l'application comme victime du blocage.  
  
 Vous pouvez exécuter sur les index des opérations DDL simultanées en ligne sur la même table ou sur la même vue uniquement lors de la création de plusieurs nouveaux index non cluster ou de la réorganisation d'index non cluster. Toutes les autres opérations en ligne sur les index exécutées en même temps échouent. Par exemple, vous ne pouvez pas créer un nouvel index en ligne tout en reconstruisant en ligne un index sur la même table.  
  
 Une opération en ligne ne peut pas être effectuée lorsqu'un index contient une colonne du type d'objet volumineux et qu'il existe, dans la même transaction, des opérations de mise à jour avant cette opération en ligne. Pour contourner ce problème, placez l'opération en ligne en dehors de la transaction ou avant les éventuelles mises à jour dans la transaction.  
  
## <a name="disk-space-considerations"></a>Considérations relatives à l’espace disque

Les opérations d’index en ligne nécessitent davantage d’espace disque que les opérations d’index hors connexion.

- Lors des opérations de création et de reconstruction d’index, de l’espace additionnel est requis pour pouvoir créer (ou reconstruire) l’index.
- De plus, de l’espace disque est requis pour l’index de mappage temporaire. Cet index temporaire est utilisé dans les opérations en ligne sur les index qui créent, reconstruisent ou suppriment un index cluster.
- La suppression d’un index cluster en ligne nécessite autant d’espace que la création (ou la reconstruction) d’un index cluster en ligne.

Pour plus d’informations, consultez [Disk Space Requirements for Index DDL Operations](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md).  
  
## <a name="performance-considerations"></a>Considérations relatives aux performances

Même si les opérations en ligne sur les index autorisent une activité de mise à jour utilisateur simultanée, elles durent plus longtemps si l'activité de mise à jour est très lourde. En général, les opérations en ligne sur les index sont plus lentes que leurs équivalents hors connexion, quel que soit le niveau d'activité de mise à jour.  
  
Comme les structures source et cible sont conservées pendant l'opération en ligne sur l'index, l'utilisation des ressources pour les transactions d'insertion, de mise à jour et de suppression peuvent augmenter jusqu'à doubler. Il pourrait s'ensuivre une dégradation des performances et une utilisation plus intense des ressources, en particulier du temps processeur, pendant l'opération d'index. Les opérations en ligne sur les index sont intégralement enregistrées dans le journal.  
  
Même si les opérations en ligne sont préférables, vous devez évaluer votre environnement et les conditions spécifiques requises. Il peut être plus approprié d'exécuter hors connexion des opérations sur les index. Ce faisant, les utilisateurs disposent d'un accès restreint aux données pendant l'opération, mais l'opération est réalisée plus vite et consomme moins de ressources.  
  
Sur les ordinateurs multiprocesseurs qui exécutent SQL Server 2016, les instructions d’index peuvent, à l’instar d’autres requêtes, utiliser davantage de processeurs pour réaliser les opérations d’analyse et de tri associées à l’instruction d’index. Vous pouvez utiliser l'option d'index MAXDOP pour contrôler le nombre de processeurs dédiés à l'opération d'index en ligne. De cette manière, vous pouvez équilibrer les ressources utilisées par l'opération d'index avec celles des utilisateurs simultanés. Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md). Pour plus d’informations sur les éditions de SQL Server qui prennent en charge les opérations d’index parallèles, consultez [Fonctionnalités prises en charge par les éditions](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
Du fait qu'un verrou S ou Sch-M lock est conservé dans la phase finale de l'opération sur un index, soyez prudent lorsque vous exécutez une opération en ligne sur un index dans une transaction utilisateur explicite, telle qu'un blocage BEGIN TRANSACTION...COMMIT. Cette action maintient le verrou jusqu'à la fin de la transaction, gênant ainsi l'accès concurrentiel des utilisateurs.  
  
La reconstruction d'index en ligne peut augmenter la fragmentation lorsqu'elle est autorisée à s'exécuter avec les options `MAX DOP > 1` et `ALLOW_PAGE_LOCKS = OFF` . Pour plus d’informations, consultez [Fonctionnement : La reconstruction d’index en ligne peut entraîner une fragmentation accrue](/archive/blogs/psssql/how-it-works-online-index-rebuild-can-cause-increased-fragmentation).  
  
## <a name="transaction-log-considerations"></a>Considérations relatives au journal des transactions

Les opérations d'index à grande échelle, réalisées hors connexion ou en ligne, peuvent générer de fortes charges de données susceptibles de remplir rapidement le journal de transactions. Pour garantir que l'opération d'index peut être restaurée, le journal des transactions ne doit pas être tronqué tant que l'opération d'index n'est pas terminée ; toutefois, le journal peut être sauvegardé pendant l'opération d'index. Par conséquent, le journal des transactions doit disposer d'un espace suffisant pour stocker les transactions des opérations d'index et les éventuelles transactions utilisateur simultanées pendant la durée de l'opération d'index. Pour plus d’informations, consultez [Espace disque du journal des transactions pour les opérations d’index](../../relational-databases/indexes/transaction-log-disk-space-for-index-operations.md).  

## <a name="resumable-index-considerations"></a>Considérations relatives aux index pouvant être repris

> [!NOTE]
> L’option d’index pouvant être repris pour la création et la regénération d’index s’applique à SQL Server (regénération d’index à partir de SQL Server 2017, avec création d’index également prise en charge dans SQL Server 2019) et SQL Database. Consultez [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) et [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md).

Lors de la création ou de la reconstruction d’un index en ligne pouvant être repris, les recommandations suivantes s’appliquent :

- Gestion, planification et extension des fenêtres de maintenance d’index. Vous pouvez suspendre et redémarrer une opération de création ou de reconstruction d’index à plusieurs reprises en fonction de vos fenêtres de maintenance.
- Récupération après des échecs de création ou de reconstruction d’index (par exemple, un basculement de la base de données ou un manque d’espace disque).
- Quand une opération d’index est en pause, l’index d’origine et celui qui vient d’être créé nécessitent de l’espace disque et doivent être mis à jour durant les opérations DML.
- Activation de la troncation des journaux des transactions au cours d’une opération de création ou de reconstruction d’index.
- L’option SORT_IN_TEMPDB=ON n’est pas prise en charge

> [!IMPORTANT]
> Dans la mesure où la création ou la reconstruction peut être reprise, il n’est pas nécessaire de maintenir ouverte une transaction de longue durée, ce qui permet la troncation des journaux au cours de cette opération et une meilleure gestion de l’espace des journaux. Avec la nouvelle conception, nous avons réussi à conserver les données nécessaires dans une base de données, ainsi que toutes les références indispensables au redémarrage de l’opération pouvant être reprise.

En règle générale, il n’existe aucune différence de performances entre la regénération d’index en ligne avec reprise et sans reprise. La création d’index pouvant être repris impose une surcharge constante qui entraîne un petit écart de performances par rapport aux autres types d’index. D’une manière générale, cette différence n’est sensible que pour les petites tables.

Lors de la mise à jour d’un index pouvant être repris alors qu’une opération d’index est en pause :

- Pour les charges de travail de lecture principalement, l’impact sur les performances est insignifiant.
- Pour les grosses charges de travail de mise à jour, vous risquez de faire face à une dégradation du débit (nos tests montrent une dégradation inférieure à 10 %).

En règle générale, il n’existe aucune différence de qualité de défragmentation entre la création ou la reconstruction d’index en ligne avec et sans reprise.

## <a name="online-default-options"></a>Options par défaut d’exécution en ligne

> [!IMPORTANT]
> Ces options sont en préversion publique pour [!INCLUDE[ssNoVersion](../../includes/sssql19-md.md)].

Vous pouvez définir des options par défaut pour l’exécution en ligne (« online ») ou pouvant être reprise (« resumable ») à un niveau de base de données en définissant les options de configuration étendues à la base de données ELEVATE_ONLINE ou ELEVATE_RESUMABLE. Avec ces options par défaut, vous pouvez éviter l’exécution accidentelle d’une opération qui met votre table de base de données en mode hors connexion. Les deux options forcent le moteur à élever automatiquement certaines opérations à une exécution en ligne (« online) ou à une exécution pouvant être reprise (« resumable »).  
Vous pouvez définir l’option comme FAIL_UNSUPPORTED, WHEN_SUPPORTED ou OFF en utilisant la commande [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Vous pouvez attribuer différentes valeurs aux options d’exécution en ligne (« online ») et d’exécution pouvant être reprise (« resumable »).

ELEVATE_ONLINE et ELEVATE_RESUMABLE s’appliquent uniquement aux instructions DDL qui prennent en charge la syntaxe online et resumable, respectivement. Par exemple, si vous tentez de créer un index XML avec ELEVATE_ONLINE=FAIL_UNSUPORTED, l’opération s’exécute en mode hors connexion, car les index XML ne prennent pas en charge la syntaxe ONLINE=. Les options n’ont d’effet que sur les instructions DDL qui sont soumises sans spécifier d’option ONLINE ou RESUMABLE. Par exemple, en soumettant une instruction avec l’option ONLINE=OFF ou RESUMABLE=OFF, l’utilisateur peut remplacer un paramètre FAIL_UNSUPPORTED et exécuter une instruction en mode hors connexion et/ou sans qu’elle puisse être reprise.

> [!NOTE]
> ELEVATE_ONLINE et ELEVATE_RESUMABLE ne s’appliquent pas aux opérations d’index XML.

## <a name="related-content"></a>Contenu connexe

- [Fonctionnement des opérations d'index en ligne](../../relational-databases/indexes/how-online-index-operations-work.md)  
- [Exécuter des opérations en ligne sur les index](../../relational-databases/indexes/perform-index-operations-online.md)  
- [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)  
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)