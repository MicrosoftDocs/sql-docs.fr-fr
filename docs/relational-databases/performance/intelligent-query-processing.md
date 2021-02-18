---
title: Traitement de requêtes intelligent
description: Fonctionnalités de traitement de requêtes intelligent pour améliorer les performances des requêtes dans SQL Server et Azure SQL Database.
ms.custom: seo-dt-2019
ms.date: 11/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 40a2ba2ff9e3955c09a8d8529a67ec3275e17222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338959"
---
# <a name="intelligent-query-processing-in-sql-databases"></a>Traitement de requêtes intelligent dans les bases de données SQL

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

La famille de fonctionnalités de traitement de requêtes intelligent inclut des fonctionnalités qui améliorent les performances des charges de travail existantes avec un minimum d’effort d’implémentation à entreprendre. 

![Traitement de requêtes intelligent](./media/iqp-feature-family.png)

Regardez cette vidéo de 6 minutes pour obtenir un aperçu du traitement intelligent des requêtes :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Intelligent-Query-processing-in-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


Vous pouvez faire en sorte que les charges de travail soient automatiquement éligibles au traitement de requêtes intelligent en activant le niveau de compatibilité applicable pour la base de données. Vous pouvez définir cette option à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Par exemple :  

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

Le tableau suivant détaille toutes les fonctionnalités du traitement de requêtes intelligent ainsi que les exigences qui y sont associées pour le niveau de compatibilité de base de données.

| **Fonctionnalité IQP** | **Pris en charge dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]** | **Pris en charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** |**Description** |
| ---------------- | ------- | ------- | ---------------- |
| [Jointures adaptatives (mode batch)](#batch-mode-adaptive-joins) | Oui, avec le niveau de compatibilité 140| Oui, à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] sous le niveau de compatibilité 140|Les jointures adaptatives sélectionnent dynamiquement un type de jointure lors de l’exécution en fonction des lignes d’entrée réelles.|
| [Nombre approximatif distinct](#approximate-query-processing) | Oui| Oui, à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|Fournit un comptage distinct (COUNT DISTINCT) approximatif pour les scénarios Big Data avec les avantages de performances élevées et d’une faible empreinte mémoire. |
| [Mode Batch sur Rowstore](#batch-mode-on-rowstore) | Oui, avec le niveau de compatibilité 150| Oui, à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] avec le niveau de compatibilité 150|Fournit un mode batch pour les charges de travail DW relationnelles utilisant le processeur de manière intensive sans nécessiter d’index columnstore.  | 
| [Exécution entrelacée](#interleaved-execution-for-mstvfs) | Oui, avec le niveau de compatibilité 140| Oui, à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] sous le niveau de compatibilité 140|Utilise la cardinalité réelle de la fonction table à instructions multiples rencontrée à la première compilation, au lieu d’une estimation fixe.|
| [Retour d’allocation de mémoire (mode batch)](#batch-mode-memory-grant-feedback) | Oui, avec le niveau de compatibilité 140| Oui, à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] sous le niveau de compatibilité 140|Si une requête en mode batch a des opérations débordant sur le disque, ajoutez de la mémoire pour les exécutions suivantes. Si une requête gaspille plus de 50 % de la mémoire qui lui est allouée, réduisez l’octroi de mémoire pour les exécutions suivantes.|
| [Retour d’allocation de mémoire (mode ligne)](#row-mode-memory-grant-feedback) | Oui, avec le niveau de compatibilité 150| Oui, à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] avec le niveau de compatibilité 150|Si une requête en mode ligne a des opérations débordant sur le disque, ajoutez de la mémoire pour les exécutions suivantes. Si une requête gaspille plus de 50 % de la mémoire qui lui est allouée, réduisez l’octroi de mémoire pour les exécutions suivantes.|
| [Incorporation (inlining) des fonctions UDF scalaires](#scalar-udf-inlining) | Non | Oui, à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] avec le niveau de compatibilité 150|Les fonctions UDF scalaires sont transformées en expressions relationnelles équivalentes qui sont incluses dans la requête appelante, ce qui entraîne souvent des gains de performances significatifs.|
| [Compilation différée de variable de table](#table-variable-deferred-compilation) | Oui, avec le niveau de compatibilité 150| Oui, à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] avec le niveau de compatibilité 150|Utilise la cardinalité réelle de la variable de table rencontrée à la première compilation, au lieu d’une estimation fixe.|

## <a name="batch-mode-adaptive-joins"></a>Jointures adaptatives en mode batch
La fonctionnalité des jointures adaptatives en mode batch, permet de choisir de différer une méthode [Jointure hachée ou jointure de boucles imbriquées](../../relational-databases/performance/joins.md) tant **que** la première entrée n’a pas été analysée, à l’aide d’un plan de mise en cache unique. L’opérateur de jointure adaptative définit un seuil qui sert à déterminer le moment où il faut basculer vers un plan de boucles imbriquées. Votre plan peut, par conséquent, passer dynamiquement à une meilleure stratégie de jointure pendant l’exécution.

Pour plus d’informations, notamment sur la désactivation des jointures adaptatives sans modifier le niveau de compatibilité, consultez [Comprendre les jointures adaptatives](../../relational-databases/performance/joins.md#adaptive).

## <a name="batch-mode-memory-grant-feedback"></a>Retour d’allocation de mémoire en mode batch
Le plan post-exécution d’une requête dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut la quantité minimale de mémoire nécessaire pour l’exécution et la taille d’allocation de mémoire idéale pour que toutes les lignes tiennent dans la mémoire. Les performances sont réduites quand les tailles d’allocation de mémoire ne sont pas dimensionnées correctement. Si l’allocation de mémoire est excessive, une certaine quantité de mémoire est inutilisée et l’accès concurrentiel est réduit. Si l’allocation de mémoire est insuffisante, il en résulte des dépassements de capacité coûteux sur le disque. En apportant une solution à la répétition des charges de travail, le retour d’allocation de mémoire en mode batch recalcule la quantité de mémoire réelle nécessaire pour une requête et met à jour la valeur d’allocation pour le plan mis en cache. Quand une instruction de requête identique est exécutée, la requête utilise la taille d’allocation de mémoire révisée, ce qui permet de réduire les allocations de mémoire excessives qui impactent l’accès concurrentiel et de corriger les allocations de mémoire sous-estimées qui provoquent des dépassements de capacité coûteux sur le disque.
Le graphe suivant montre un exemple d’utilisation du retour d’allocation de mémoire adaptative en mode batch. Pour la première exécution de la requête, la durée était de **88 secondes** en raison de dépassements de capacité importants :   

```sql
DECLARE @EndTime datetime = '2016-09-22 00:00:00.000';
DECLARE @StartTime datetime = '2016-09-15 00:00:00.000';
SELECT TOP 10 hash_unique_bigint_id
FROM dbo.TelemetryDS
WHERE Timestamp BETWEEN @StartTime and @EndTime
GROUP BY hash_unique_bigint_id
ORDER BY MAX(max_elapsed_time_microsec) DESC;
```

![Dépassements de capacité importants](./media/2_AQPGraphHighSpills.png)

Si le retour d’allocation de mémoire est activé, pour la deuxième exécution, la durée est de **1 seconde** (au lieu de 88 secondes), les dépassements de capacité sont entièrement supprimés et l’allocation est plus importante : 

![Aucun dépassement de capacité](./media/3_AQPGraphNoSpills.png)

### <a name="memory-grant-feedback-sizing"></a>Dimensionnement du retour d’allocation de mémoire
Dans le cas d’une allocation de mémoire excessive, si la mémoire allouée est plus de deux fois supérieure à la taille de la mémoire réelle utilisée, le retour d’allocation de mémoire recalcule l’allocation de mémoire et met à jour le plan mis en cache. Les plans dont les allocations de mémoire sont inférieures à 1 Mo ne sont pas recalculés par rapport à d’éventuels dépassements.
Dans le cas d’une allocation de mémoire dont la taille est insuffisante et qui entraîne un dépassement de capacité sur le disque pour les opérateurs en mode batch, le retour d’allocation de mémoire déclenche un nouveau calcul de l’allocation de mémoire. Les événements de dépassement de capacité sont signalés au retour d’allocation de mémoire et peuvent être exposés via l’événement xEvent *spilling_report_to_memory_grant_feedback*. Cet événement retourne l’ID de nœud du plan et la taille du dépassement de données de ce nœud.

### <a name="memory-grant-feedback-and-parameter-sensitive-scenarios"></a>Retour d’allocation de mémoire et scénarios sensibles aux paramètres
Différentes valeurs de paramètre peuvent également nécessiter différents plans de requête pour maintenir une situation optimale. Ce type de requête est défini comme « sensible aux paramètres ». Pour les plans sensibles aux paramètres, le retour d’allocation de mémoire se désactive sur une requête si la mémoire requise est instable. Le plan est désactivé après plusieurs exécutions répétées de la requête et ce comportement peut être observé en surveillant l’événement xEvent *memory_grant_feedback_loop_disabled* XEvent. Pour plus d’informations sur la détection et la sensibilité des paramètres, reportez-vous au [Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing).

### <a name="memory-grant-feedback-caching"></a>Mise en cache du retour d’allocation de mémoire
Le retour peut être stocké dans le plan mis en cache pour une seule exécution. Toutefois, ce sont les exécutions consécutives de cette instruction qui bénéficient des ajustements du retour d’allocation de mémoire. Cette fonctionnalité s’applique à l’exécution répétée d’instructions. Le retour d’allocation de mémoire modifie uniquement le plan mis en cache. Actuellement, les modifications ne sont pas capturées dans le Magasin des requêtes.
Le retour n’est pas persistant si le plan est supprimé du cache. Le retour est également perdu en cas de basculement. Une instruction qui utilise `OPTION (RECOMPILE)` crée un plan et ne le met pas en cache. Parce qu’il n’est pas mis en cache, aucun retour d’allocation de mémoire n’est généré, et il n’est pas stocké pour cette compilation et l’exécution. Toutefois, si une instruction équivalente (autrement dit, avec le même hachage de requête) qui n’utilise **pas**`OPTION (RECOMPILE)` a été mise en cache, puis réexécutée, l’instruction consécutive peut bénéficier du retour d’allocation de mémoire.

### <a name="tracking-memory-grant-feedback-activity"></a>Suivi de l’activité du retour d’allocation de mémoire
Vous pouvez suivre les événements du retour d’allocation de mémoire à l’aide de l’événement xEvent *memory_grant_updated_by_feedback*. Cet événement effectue le suivi de l’historique du nombre d’exécutions actuel, du nombre de fois que le plan a été mis à jour par le retour d’allocation de mémoire, de l’allocation de mémoire supplémentaire idéale avant modification et l’allocation de mémoire supplémentaire idéale après que le retour d’allocation de mémoire a modifié le plan mis en cache.

### <a name="memory-grant-feedback-resource-governor-and-query-hints"></a>Retour d’allocation de mémoire, resource governor et indicateurs de requête
La mémoire réelle allouée respecte la limite de mémoire de requête déterminée par l’indicateur de requête ou resource governor.

### <a name="disabling-batch-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Désactivation du retour d’allocation de mémoire en mode batch sans modification du niveau de compatibilité
La rétroaction d’allocation de mémoire peut être désactivée dans l’étendue de la base de données ou de l’instruction tout en maintenant le niveau de compatibilité de base de données 140 et au-delà. Pour désactiver la rétroaction d’allocation de mémoire en mode batch pour toutes les exécutions de requête en provenance de la base de données, exécutez ce qui suit dans le contexte de la base de données applicable :

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Quand il est activé, ce paramètre apparaît comme étant activé dans [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).

Pour réactiver la rétroaction d’allocation de mémoire en mode batch pour toutes les exécutions de requête en provenance de la base de données, exécutez ce qui suit dans le contexte de la base de données applicable :

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

Vous pouvez aussi désactiver la rétroaction d’allocation de mémoire en mode batch pour une requête spécifique en désignant `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` en tant qu’[indicateur de requête USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Par exemple :

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK')); 
```

Un indicateur de requête USE HINT est prioritaire par rapport à une configuration incluse dans l’étendue d’une base de données ou à un paramètre d’indicateur de trace.

## <a name="row-mode-memory-grant-feedback"></a>Rétroaction d’allocation de mémoire en mode ligne

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

La rétroaction d’allocation de mémoire en mode ligne étend la fonctionnalité de rétroaction d’allocation de mémoire en mode batch en ajustant les tailles d’allocation de mémoire pour les opérateurs du mode batch et du mode ligne.  

Pour activer la rétroaction d’allocation de mémoire en mode ligne dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], fixez le niveau de compatibilité à 150 pour la base de données à laquelle vous vous connectez lors de l’exécution de la requête.

L’activité de la rétroaction d’allocation de mémoire en mode ligne sera visible par le biais du XEvent **memory_grant_updated_by_feedback**. 

Avec la rétroaction d’allocation de mémoire en mode ligne, deux nouveaux attributs de plan de requête apparaissent pour les plans réels après exécution : **IsMemoryGrantFeedbackAdjusted** et _*_LastRequestedMemory_*_, qui sont ajoutés à l’élément XML du plan de requête _MemoryGrantInfo*. 

*LastRequestedMemory* indique la mémoire allouée en kilo-octets (Ko) à partir de l’exécution de la requête précédente. L’attribut *IsMemoryGrantFeedbackAdjusted* permet de vérifier l’état de la rétroaction d’allocation de mémoire de l’instruction au sein d’un plan d’exécution de requête réel. Les valeurs affichées dans cet attribut sont les suivantes :

| Valeur IsMemoryGrantFeedbackAdjusted | Description |
|---|---|
| Non : première exécution | La rétroaction d’allocation de mémoire n’ajuste pas la mémoire pour la première compilation et l’exécution associée.  |
| Non : allocation précise | S’il n’y a pas de dépassement sur disque et que l’instruction utilise au moins 50 % de la mémoire allouée, la rétroaction d’allocation de mémoire n’est pas déclenchée. |
| Non : rétroaction désactivée | Si la rétroaction d’allocation de mémoire est déclenchée en permanence et varie entre des opérations d’augmentation et de diminution de la mémoire, nous la désactivons pour l’instruction. |
| Oui : ajustement | La rétroaction d’allocation de mémoire a été appliquée et peut encore être ajustée pour l’exécution suivante. |
| Oui : Stable | La rétroaction d’allocation de mémoire a été appliquée et la mémoire allouée est maintenant stable ; en d’autres termes, ce qui a été alloué pour l’exécution précédente est identique à ce qui a été alloué pour l’exécution actuelle. |

### <a name="disabling-row-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Désactivation du retour d’allocation de mémoire en mode ligne sans modification du niveau de compatibilité
Le retour d’allocation de mémoire en mode ligne peut être désactivé dans l’étendue de la base de données ou de l’instruction tout en maintenant le niveau de compatibilité de la base de données à au moins 150. Pour le retour d’allocation de mémoire en mode ligne pour toutes les exécutions de requêtes provenant de la base de données, exécutez ce qui suit dans le contexte de la base de données applicable :

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Pour réactiver le retour d’allocation de mémoire en mode ligne pour toutes les exécutions de requêtes provenant de la base de données, exécutez ce qui suit dans le contexte de la base de données applicable :

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

Vous pouvez aussi désactiver la rétroaction d’allocation de mémoire en mode ligne pour une requête spécifique en désignant `DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK` en tant qu’[indicateur de requête USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Par exemple :

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK')); 
```

Un indicateur de requête USE HINT est prioritaire par rapport à une configuration incluse dans l’étendue d’une base de données ou à un paramètre d’indicateur de trace.

## <a name="interleaved-execution-for-mstvfs"></a>Exécution entrelacée pour les fonctions table à instructions multiples (MSTVF)
Avec l’exécution entrelacée, le nombre réel de lignes de la fonction est utilisé pour prendre des décisions de plan de requête en aval plus avisées. Pour plus d’informations sur les fonctions table à instructions multiples (MSTVF), consultez [Fonctions table](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

L’exécution entrelacée change la limite unidirectionnelle entre les phases d’exécution et d’optimisation pour l’exécution d’une seule requête, et permet d’adapter les plans selon les estimations de cardinalité révisées. Pendant l’optimisation, si nous rencontrons un candidat pour l’exécution entrelacée, c’est-à-dire des **fonctions table à instructions multiples (MSTVF)** , nous suspendons l’optimisation, exécutons la sous-arborescence applicable, capturons des estimations de cardinalité précises, puis reprenons l’optimisation pour les opérations en aval.   

Les fonctions table à instructions multiples ont une estimation de cardinalité fixe égale à 100 à compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], et égale à 1 pour les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’exécution entrelacée entraîne des problèmes de performance des charges de travail qui sont liés à ces estimations de cardinalité fixes associées aux MSTVF. Pour plus d’informations sur les MSTVF, consultez [Créer des fonctions définies par l’utilisateur &#40;moteur de base de données&#41;](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

L’image suivante illustre une sortie de [Statistiques des requêtes actives](../../relational-databases/performance/live-query-statistics.md), un sous-ensemble d’un plan d’exécution global qui montre l’impact des estimations de cardinalité fixes des MSTVF. Vous pouvez comparer le flux de lignes réel et les lignes estimées. Il y a trois zones notables dans le plan (le flux va de droite à gauche) :
1. L’analyse de table MSTVF utilise une estimation fixe de 100 lignes. Dans cet exemple, toutefois, l’analyse de table présente un flux de 527 597 lignes, comme indiqué dans les statistiques des requêtes actives par *527597 sur 100* (nombre réel sur nombre estimé). L’estimation fixe est donc considérablement biaisée.
1. Pour l’opération de boucles imbriquées, seules 100 lignes sont supposées être retournées par le côté extérieur de la jointure. Étant donné le nombre élevé de lignes retournées par la MSTVF, vous avez tout intérêt à utiliser un autre algorithme de jointure.
1. Pour l’opération de correspondance de hachage, notez le petit symbole d’avertissement qui indique dans ce cas un dépassement de capacité sur le disque.

![Flux de lignes par rapport aux lignes estimées](./media/7_AQPFlowThreeAreas.png)

Comparez le plan précédent au plan réel généré avec l’exécution entrelacée :

![Plan entrelacé](./media/8_AQPInterleavedEnabledPlan.png)

1. Notez que l’analyse de table MSTVF reflète maintenant une estimation de cardinalité précise. Notez également la réorganisation de cette analyse de table et des autres opérations.
1. Concernant les algorithmes de jointure, nous sommes passés d’une opération de boucles imbriquées à une opération de correspondance de hachage, qui est plus adaptée étant donné le nombre important de lignes concernées.
1. Notez également que nous n’avons plus d’avertissements de dépassement de capacité, car nous allouons davantage de mémoire en fonction du nombre de lignes réel de l’analyse de table MSTVF.

### <a name="interleaved-execution-eligible-statements"></a>Instructions éligibles pour l’exécution entrelacée
Les instructions de référence MSTVF dans l’exécution entrelacée doivent être en lecture seule et ne pas faire partie d’une opération de modification de données. De plus, les MSTVF ne sont pas éligibles pour l’exécution entrelacée si elles n’utilisent pas de constantes d’exécution.

### <a name="interleaved-execution-benefits"></a>Avantages de l’exécution entrelacée
En règle générale, plus le décalage est important entre le nombre de lignes réel et estimé, associé au nombre d’opérations de plan en aval, plus l’impact sur les performances est élevé.
En général, l’exécution entrelacée bénéficie aux requêtes dans les situations suivantes :
1. Il y a un décalage important entre le nombre réel et le nombre estimé de lignes pour le jeu de résultats intermédiaire (dans ce cas, la MSTVF).
1. En outre, la requête globale est sensible à un changement de taille du résultat intermédiaire. Cela se produit généralement quand une arborescence complexe englobe la sous-arborescence dans le plan de requête.
Une simple instruction `SELECT *` d’une MSTVF ne tire aucun bénéfice de l’exécution entrelacée.

### <a name="interleaved-execution-overhead"></a>Surcharge de l’exécution entrelacée
La surcharge doit être minime, voire nulle. Les MSTVF ont déjà été matérialisées avant l’introduction de l’exécution entrelacée, mais cette fois nous autorisons une optimisation différée et nous tirons parti de l’estimation de cardinalité de l’ensemble des lignes matérialisées.
Comme c’est le cas avec n’importe quel changement affectant le plan, certains plans peuvent changer à un point tel que l’amélioration de la cardinalité de la sous-arborescence entraîne un plan complètement inadapté pour la requête globale. Pour atténuer cet effet, vous pouvez rétablir le niveau de compatibilité ou utiliser le Magasin des requêtes pour forcer l’utilisation de la version non régressée du plan.

### <a name="interleaved-execution-and-consecutive-executions"></a>Exécution entrelacée et exécutions consécutives
Une fois qu’un plan d’exécution entrelacée est mis en cache, le plan avec les estimations révisées issues de la première exécution est utilisé pour les exécutions consécutives sans nouvelle instanciation de l’exécution entrelacée.

### <a name="tracking-interleaved-execution-activity"></a>Suivi de l’activité d’exécution entrelacée
Vous pouvez voir les attributs d’utilisation dans le plan d’exécution de requête réel :

| Attribut de plan d'exécution | Description |
| --- | --- |
| ContainsInterleavedExecutionCandidates | S’applique au nœud *QueryPlan*. Quand la valeur est *true*, cela signifie que le plan contient des candidats pour l’exécution entrelacée. |
| IsInterleavedExecuted | Attribut de l’élément *RuntimeInformation* sous le RelOp pour le nœud TVF. Quand la valeur est *true*, cela signifie que l’opération a été matérialisée dans le cadre d’une opération d’exécution entrelacée. |

Vous pouvez également suivre les occurrences d’exécution entrelacée via les événements xEvent suivants :

| Événement xEvent | Description |
| ---- | --- |
| interleaved_exec_status | Cet événement se déclenche quand l’exécution entrelacée se produit. |
| interleaved_exec_stats_update | Cet événement décrit les estimations de cardinalité mises à jour par l’exécution entrelacée. |
| Interleaved_exec_disabled_reason | Cet événement se déclenche quand une requête avec un candidat possible pour l’exécution entrelacée n’obtient pas l’exécution entrelacée. |

Une requête doit être exécutée pour permettre à l’exécution entrelacée de réviser les estimations de cardinalité MSTVF. Toutefois, le plan d’exécution estimé montre toujours s’il y a des candidats pour l’exécution entrelacée via l’attribut de plan d’exécution de requêtes `ContainsInterleavedExecutionCandidates`.    

### <a name="interleaved-execution-caching"></a>Mise en cache de l’exécution entrelacée
Si un plan est effacé ou supprimé du cache, à l’exécution de la requête c’est une nouvelle compilation qui utilise l’exécution entrelacée.
Une instruction qui utilise `OPTION (RECOMPILE)` crée un plan à l’aide de l’exécution entrelacée et ne le met pas en cache.     

### <a name="interleaved-execution-and-query-store-interoperability"></a>Interopérabilité de l’exécution entrelacée et du Magasin des requêtes
L’utilisation des plans qui utilisent l’exécution entrelacée peut être forcée. Le plan est la version dont les estimations de cardinalité ont été corrigées en fonction de l’exécution initiale.    

### <a name="disabling-interleaved-execution-without-changing-the-compatibility-level"></a>Désactivation de l’exécution entrelacée sans modifier le niveau de compatibilité
L’exécution entrelacée peut être désactivée dans l’étendue de la base de données ou de l’instruction tout en maintenant le niveau de compatibilité de base de données 140 et au-delà.  Pour désactiver l’exécution entrelacée pour toutes les exécutions de requête en provenance de la base de données, exécutez ce qui suit dans le contexte de la base de données applicable :

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = OFF;
```

Quand il est activé, ce paramètre apparaît comme étant activé dans [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).
Pour réactiver l’exécution entrelacée pour toutes les exécutions de requête en provenance de la base de données, exécutez ce qui suit dans le contexte de la base de données applicable :

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = OFF;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = ON;
```

Vous pouvez aussi désactiver l’exécution entrelacée pour une requête spécifique en désignant `DISABLE_INTERLEAVED_EXECUTION_TVF` en tant qu’[indicateur de requête USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Par exemple :

```sql
SELECT [fo].[Order Key], [fo].[Quantity], [foo].[OutlierEventQuantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Fact].[WhatIfOutlierEventQuantity]('Mild Recession',
                            '1-01-2013',
                            '10-15-2014') AS [foo] ON [fo].[Order Key] = [foo].[Order Key]
                            AND [fo].[City Key] = [foo].[City Key]
                            AND [fo].[Customer Key] = [foo].[Customer Key]
                            AND [fo].[Stock Item Key] = [foo].[Stock Item Key]
                            AND [fo].[Order Date Key] = [foo].[Order Date Key]
                            AND [fo].[Picked Date Key] = [foo].[Picked Date Key]
                            AND [fo].[Salesperson Key] = [foo].[Salesperson Key]
                            AND [fo].[Picker Key] = [foo].[Picker Key]
OPTION (USE HINT('DISABLE_INTERLEAVED_EXECUTION_TVF'));
```

Un indicateur de requête USE HINT est prioritaire par rapport à une configuration incluse dans l’étendue d’une base de données ou à un paramètre d’indicateur de trace.

## <a name="table-variable-deferred-compilation"></a>Compilation différée de variable de table

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

La **compilation différée de variable de table** améliore la qualité du plan et les performances globales pour les requêtes faisant référence à des variables de table. Pendant l’optimisation et la compilation de plans initiale, cette fonctionnalité va propager les estimations de cardinalité basées sur le nombre réel de lignes de la variable de table. Le nombre exact de lignes sera ensuite utilisé pour optimiser les opérations de plan en aval.

Avec la compilation différée de la variable de table, la compilation d’une instruction qui fait référence à une variable de table est différée jusqu'à la première exécution réelle de l’instruction. Ce comportement de compilation différée est identique à celui des tables temporaires. Cette modification entraîne l’utilisation de la cardinalité réelle au lieu de l’estimation d’origine sur une ligne. 

Pour permettre la compilation différée des variables de table, définissez le niveau de compatibilité de la base de données à 150 pour la base de données à laquelle vous êtes connecté durant l’exécution de la requête.

La compilation différée de variables de table **ne** modifie aucune autre caractéristique des variables de table. Par exemple, cette fonctionnalité n’ajoute pas de statistiques de colonnes aux variables de table.

La compilation différée de variables de table **n’augmente pas la fréquence des recompilations**. Au lieu de cela, elle se positionne là où la compilation initiale se produit. Le plan mis en cache obtenu est généré en fonction du nombre de lignes de variable de table dans la compilation différée initiale. Le plan mis en cache est réutilisé par des requêtes consécutives. Et ce, jusqu’à ce que le plan soit supprimé ou recompilé. 

Le nombre de lignes de variable de table utilisé pour la compilation du plan initial représente une valeur type très différente d’une estimation du nombre de lignes fixe. S’il est différent, les opérations en aval en bénéficieront. Si le nombre de lignes de variable de table varie de manière significative entre les exécutions, il est possible que cette fonctionnalité n’améliore pas les performances.

### <a name="disabling-table-variable-deferred-compilation-without-changing-the-compatibility-level"></a>Désactivation de la compilation différée de variables de table sans changer le niveau de compatibilité
Désactivez la compilation différée des variables de table au niveau de la base de données ou de l’instruction, tout en maintenant un niveau de compatibilité de la base de données supérieur ou égal à 150. Pour désactiver la compilation différée des variables de table dans toutes les exécutions de requête provenant de la base de données, exécutez l’exemple suivant dans le contexte de la base de données applicable :

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = OFF;
```

Pour réactiver la compilation différée des variables de table dans toutes les exécutions de requête provenant de la base de données, exécutez l’exemple suivant dans le contexte de la base de données applicable :

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = ON;
```

Vous pouvez également désactiver la compilation différée des variables de table pour une requête spécifique en désignant DISABLE_DEFERRED_COMPILATION_TV comme un indicateur de requête USE HINT.  Par exemple :

```sql
DECLARE @LINEITEMS TABLE 
    (L_OrderKey INT NOT NULL,
     L_Quantity INT NOT NULL
    );

INSERT @LINEITEMS
SELECT L_OrderKey, L_Quantity
FROM dbo.lineitem
WHERE L_Quantity = 5;

SELECT O_OrderKey,
    O_CustKey,
    O_OrderStatus,
    L_QUANTITY
FROM    
    ORDERS,
    @LINEITEMS
WHERE    O_ORDERKEY    =    L_ORDERKEY
    AND O_OrderStatus = 'O'
OPTION (USE HINT('DISABLE_DEFERRED_COMPILATION_TV'));
```

## <a name="scalar-udf-inlining"></a>Incorporation (inlining) des fonctions UDF scalaires

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

La fonctionnalité d’incorporation des fonctions UDF scalaires transforme automatiquement les [fonctions UDF scalaires](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#Scalar) en expressions relationnelles. Elle les incorpore dans la requête SQL appelante. Cette transformation améliore les performances des charges de travail qui tirent parti des fonctions UDF scalaires. La fonctionnalité d’incorporation des fonctions UDF scalaires facilite l’optimisation basée sur le coût des opérations à l’intérieur des fonctions UDF. Les résultats sont des plans efficaces, axés sur les ensembles et parallèles au lieu de plans d’exécution inefficaces, itératifs, en série. Cette fonctionnalité est activée par défaut sous le niveau de compatibilité de base de données 150.

Pour plus d’informations, consultez [Incorporation des fonctions UDF scalaires](../user-defined-functions/scalar-udf-inlining.md).

## <a name="approximate-query-processing"></a>Traitement des requêtes approximatif

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Le traitement des requêtes approximatif est une nouvelle famille de fonctionnalités. Il fournit des agrégations dans de vastes jeux de données où la réactivité est plus importante que la précision absolue. Un exemple est le calcul d’un **COUNT(DISTINCT())** dans 10 milliards de lignes pour l’affichage sur un tableau de bord. Dans ce cas, la précision absolue n’est pas importante, mais la réactivité est essentielle. La nouvelle fonction d’agrégation **APPROX_COUNT_DISTINCT** retourne le nombre approximatif de valeurs non null uniques dans un groupe.

Pour plus d’informations, consultez [APPROX_COUNT_DISTINCT (Transact-SQL)](../../t-sql/functions/approx-count-distinct-transact-sql.md).

## <a name="batch-mode-on-rowstore"></a>Mode Batch sur rowstore 

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Le mode batch sur rowstore permet l’exécution en mode batch des charges de travail analytiques sans avoir besoin d’index columnstore.  Cette fonctionnalité prend en charge les filtres bitmap et l’exécution du mode batch des segments de mémoire sur disque et des index B-tree. Le mode batch sur rowstore permet de prendre en charge tous les opérateurs existants compatibles avec le mode batch.

### <a name="background"></a>Arrière-plan
[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a introduit une nouvelle fonctionnalité pour accélérer les charges de travail analytiques : les index columnstore. Nous avons étendu les cas d’utilisation et amélioré les performances des index columnstore dans chaque version suivante. Jusqu’à présent, nous avons présenté et documenté toutes ces fonctionnalités sous la forme d’une fonctionnalité unique. Vous créez des index columnstore sur vos tables. Et votre charge de travail analytique s’exécute plus rapidement. Toutefois, il existe deux jeux de technologies connexes mais distincts :
- Les index **ColumnStore** permettent aux requêtes analytiques d’accéder uniquement aux données dans les colonnes nécessaires. La compression de page au format columnstore est aussi plus efficace que la compression dans des index **rowstore** traditionnels. 
- Avec le traitement **en mode batch**, les opérateurs de requête traitent les données plus efficacement. Ils travaillent sur un lot de lignes au lieu d’une ligne à la fois. Plusieurs autres améliorations d’évolutivité sont liées au traitement en mode Batch. Pour plus d’informations sur le mode batch, consultez [Modes d’exécution](../../relational-databases/query-processing-architecture-guide.md#execution-modes).

Les deux ensembles de fonctionnalités interagissent pour améliorer les E/S (entrées/sorties) et l’utilisation du processeur :
- En utilisant des index columnstore, une plus grande quantité de vos données s’intègre dans la mémoire. Cela réduit la charge de travail d’E/S.
- Le traitement en mode Batch utilise plus efficacement le processeur.

Dès que possible, les deux technologies tirent parti l’une de l’autre. Par exemple, les agrégats en mode Batch peuvent être évalués dans le cadre d’une analyse d’index columnstore. Les données columnstore compressées sont également traitées à l’aide de l’encodage de la longueur d’exécution de manière beaucoup plus efficace avec les jointures en mode batch et les agrégats en mode batch. 
 
Il est toutefois important de comprendre que les deux fonctionnalités sont indépendantes :
* Vous pouvez obtenir des plans en mode ligne qui utilisent des index columnstore.
* Vous pouvez obtenir des plans en mode batch qui utilisent uniquement des index rowstore. 

Vous obtenez généralement les meilleurs résultats lorsque vous utilisez les deux fonctionnalités ensemble. Donc, jusqu’à présent, l’optimiseur de requête SQL Server ne prenait en compte le traitement en mode batch que pour les requêtes impliquant au moins une table avec un index columnstore.

Les index columnstore ne sont peut-être pas appropriés pour certaines applications. Une application peut utiliser une autre fonctionnalité qui n’est pas pris en charge avec des index columnstore. Par exemple, les modifications sur place ne sont pas compatibles avec la compression columnstore. Les déclencheurs ne sont donc pas pris en charge sur les tables avec des index columnstore en cluster. Plus important encore, les index columnstore ajoutent une surcharge aux instructions **DELETE** et **UPDATE**. 

Pour certaines charges de travail transactionnelles-analytiques hybrides, la surcharge d’une charge de travail transactionnelle l’emporte sur les avantages liés à l’utilisation des index columnstore. De tels scénarios peuvent bénéficier d’une utilisation améliorée du processeur via un traitement en mode batch uniquement. C’est la raison pour laquelle la fonctionnalité Mode batch sur rowstore prend en compte le mode batch pour toutes les requêtes, quel que soit le type d’index concerné.

### <a name="workloads-that-might-benefit-from-batch-mode-on-rowstore"></a>Charges de travail pouvant tirer parti du mode Batch sur rowstore
Les charges de travail suivantes peuvent tirer parti du mode Batch sur rowstore :
* Une partie importante de la charge de travail se compose de requêtes analytiques. En règle générale, ces requêtes utilisent des opérateurs tels que des jointures ou des agrégats qui traitent des centaines de milliers de lignes ou plus.
* La charge de travail est liée à l’UC. Si le goulot d’étranglement se situe au niveau des E/S, il est toujours recommandé d’envisager l’utilisation d’un index columnstore, si possible.
* La création d’un index columnstore ajoute trop de surcharge à la partie transactionnelle de votre charge de travail. Ou bien, la création d’un index columnstore est impossible, car votre application dépend d’une fonctionnalité qui n’est pas encore prise en charge avec les index columnstore.


> [!NOTE]
> Le mode Batch sur rowstore ne peut aider qu’en réduisant la consommation de l’UC. Si votre goulot d’étranglement est lié aux E/S et si les données ne sont pas déjà mises en cache (cache « à froid »), le mode batch sur rowstore n’améliore pas le temps écoulé des requêtes. De même, si la mémoire de la machine est insuffisante pour mettre en cache toutes les données, une amélioration des performances est peu probable.

### <a name="what-changes-with-batch-mode-on-rowstore"></a>Ce qui change avec le mode Batch sur rowstore

Définissez le niveau de compatibilité de la base de données à 150. Aucun autre changement n’est nécessaire.

Même si une requête n’accède à aucune table avec des index columnstore, le processeur de requêtes détermine l’utilisation ou non du mode batch à l’aide d’informations heuristiques. Les données heuristiques sont constituées de ces vérifications :
1. Une vérification initiale des tailles de la table, des opérateurs utilisés et des cardinalités estimées dans la requête d’entrée.
2. Des points de contrôle supplémentaires, lorsque l’optimiseur détecte des plans nouveaux et à moindre coût pour la requête. Si ces autres plans n’utilisent pas le mode Batch de manière significative, l’optimiseur cesse d’explorer les alternatives du mode Batch.

Si le mode Batch sur rowstore est utilisé, vous voyez le mode d’exécution réel en tant que **mode Batch** dans le plan de requête. L’opérateur d’analyse utilise le mode Batch pour les segments de mémoire sur disque et les index B-tree. Cette analyse en mode Batch peut évaluer les filtres de bitmap du mode Batch. Vous pouvez également voir d’autres opérateurs en mode Batch dans le plan. Par exemple, les jonctions de hachage, les agrégats basés sur le hachage, les tris, les agrégats de fenêtre, les filtres, la concaténation et les opérateurs scalaires de calcul.

### <a name="remarks"></a>Notes

Les plans de requête n’utilisent pas toujours le mode Batch. L’optimiseur de requête peut décider que le mode Batch n’est pas utile pour la requête. 

L’espace de recherche de l’optimiseur de requête change. Par conséquent, si vous obtenez un plan en mode ligne, il peut être différent du plan que vous obtenez dans un niveau de compatibilité inférieur. Et si vous obtenez un plan en mode Batch, il peut être différent du plan que vous obtenez avec un index columstore. 

Les plans peuvent également changer pour les requêtes qui combinent des index columnstore et rowstore, en raison de la nouvelle analyse rowstore du mode Batch.

Voici les limitations actuelles pour la nouvelle analyse du mode Batch sur rowstore : 
- Elle n’intervient pas pour les tables OLTP en mémoire, ou pour tout index autre que des segments de mémoire sur disque et des arborescences B-tree. 
- Elle n’intervient pas non plus si une colonne LOB est récupérée ou filtrée. Cette limitation inclut les jeux de colonnes éparses et les colonnes XML.

Il existe des requêtes pour lesquelles le mode Batch n’est pas utilisé même avec des index columnstore. Les requêtes qui impliquent des curseurs en sont des exemples. Ces mêmes exclusions s’étendent également au mode Batch sur rowstore.

### <a name="configure-batch-mode-on-rowstore"></a>Configurer le mode Batch sur rowstore
La configuration au niveau de la base de données **BATCH_MODE_ON_ROWSTORE** est activée par défaut. Elle désactive le mode Batch sur rowstore sans nécessiter de changement du niveau de compatibilité de la base de données :

```sql
-- Disabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = OFF;

-- Enabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = ON;
```

Vous pouvez désactiver le mode Batch sur rowstore via la configuration au niveau de la base de données. Mais vous pouvez toujours remplacer le paramètre au niveau de la requête à l’aide de l’indicateur de requête **ALLOW_BATCH_MODE**. L’exemple suivant active le mode Batch sur rowstore même avec la fonctionnalité désactivée via la configuration limitée à la base de données :

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('ALLOW_BATCH_MODE'));
```

Vous pouvez aussi désactiver le mode Batch sur rowstore pour une requête spécifique à l’aide de l’indicateur de requête **DISALLOW_BATCH_MODE**. Voir l’exemple suivant :

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('DISALLOW_BATCH_MODE'));
```

## <a name="see-also"></a>Voir aussi
[Centre de performances pour le moteur de base de données SQL Server et Azure SQL Database](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)     
[Guide d’architecture de traitement des requêtes](../../relational-databases/query-processing-architecture-guide.md)    
[Guide de référence des opérateurs Showplan logiques et physiques](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
[Jointures](../../relational-databases/performance/joins.md)       
[Illustration du traitement de requêtes intelligent](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/intelligent-query-processing)   
