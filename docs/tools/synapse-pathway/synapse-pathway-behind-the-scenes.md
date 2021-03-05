---
title: Dans les coulisses d’Azure Synapse Pathway Preview.
description: Présentation technique approfondie de la façon dont Azure Synapse Pathway traduit votre code.
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-concept
ms.openlocfilehash: 9f23aa23ef40ee7df5ad601b73ad526df7bcf0da
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186299"
---
# <a name="azure-synapse-pathway-preview-behind-the-scenes"></a>Dans les coulisses d’Azure Synapse Pathway Preview
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

L’objectif d’Azure Synapse Pathway est de préserver l’intention fonctionnelle du code d’origine tout en effectuant une optimisation pour Synapse SQL. Synapse Pathway utilise un processus en trois phases pour traduire le code SQL d’un système source vers Azure Synapse SQL.

Chacune des phases préserve et augmente la connaissance de la source, y compris les métadonnées spécifiques à la source, pour garantir la meilleure qualité de la traduction.

 ![Azure Synapse Pathway.](./media/synapse-pathway-behind-the-scenes/behind-the-scene.png)

## <a name="stage-1--lexing-and-parsing"></a>Phase 1 : Segmentation et analyse

L’analyse du langage SQL est un problème qui a été résolu depuis longtemps. Il existe de nombreux analyseurs commerciaux et open source qui facilitent le processus sous-jacent consistant à prendre une instruction source, à la décomposer en jetons logiques, puis à exécuter un ensemble de règles d’analyse pour garantir la cohérence du langage. 

Synapse Pathway définit des grammaires sources qui permettent à l’outil d’identifier et de traiter l’entrée SQL en une arborescence de syntaxe abstraite (AST) augmentée, qui est utilisée dans un traitement ultérieur. 

## <a name="stage-2---augmented-abstract-syntax-tree-ast"></a>Phase 2 : Arborescence de syntaxe abstraite (AST) augmentée

Synapse Pathway définit une représentation commune de tous les objets dans une arborescence de syntaxe abstraite (AST) augmentée. L’AST Synapse Pathway comprend des instructions supplémentaires ou des métadonnées fragmentées qui sont utilisées pour prendre la décision appropriée lors de la traduction du code entre les deux systèmes.

En n’établissant pas simplement qu’un jeton est une fonction mais plutôt la spécification du type du système source, les composants de génération de script peuvent prendre des décisions plus intelligentes quant à la traduction vers Synapse SQL.

Par exemple, la fonction source pour la fonction « absolute » (absolu) est définie comme suit :

```sql  
ABS( float_expression ) 
```

Azure Synapse SQL définit la fonction « absolute » comme suit :

```sql  
ABS ( numeric_expression )  
```

Dans ce cas simple, Synapse Pathway comprend que la conversion en Synapse SQL de flottant en numérique est une [conversion](../../t-sql/functions/cast-and-convert-transact-sql.md?view=azure-sqldw-latest&preserve-view=true#implicit-conversions) implicite et ne nécessite aucun cast de type supplémentaire. Traduction du code simple, propre et efficace.

Conserver ces méta-informations sur les instructions et les fragments sources permet de résoudre les différences structurelles entre les plateformes, par exemple les conversions en logique de refus pour les prédicats des conditions de recherche dans une clause WHERE.

## <a name="stage-3---syntax-generation"></a>Phase 3 : Génération de la syntaxe

La dernière phase du processus est de générer une syntaxe pour Synapse SQL. En utilisant la structure AST générée à partir des fichiers sources, Synapse Pathway écrit chaque objet DDL dans un fichier individuel. Les générateurs de syntaxe utilisent une connaissance approfondie de la plateforme cible pour optimiser les instructions.

Par exemple, un modèle courant observé dans les scénarios de chargement de données est de d’abord supprimer tout le contenu d’une table intermédiaire, puis de charger les données à partir d’une autre table intermédiaire en mode INSERT/SELECT.

```sql  
DELETE staging.table1 ALL;
INSERT INTO staging.table1…
FROM staging.table2;
```

Synapse SQL a un chemin optimisé pour ce scénario : une instruction [CREATE TABLE AS SELECT](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-develop-ctas). L’instruction CTAS est une opération par lot avec une journalisation minimale, qui permet de hautes performances via l’utilisation de toute l’infrastructure de calcul disponible. Sans cette connaissance de Synapse SQL, les outils produisent souvent des instructions TRUNCATE et INSERT/SELECT.

```sql  
TRUNCATE TABLE staging.table1;
INSERT INTO staging.table1…
FROM staging.table2;
```

Bien qu’il ne soit pas incorrect, ce code peut être optimisé en instructions TABLE DROP et CTAS pour obtenir de meilleures performances.

```sql  
DROP TABLE staging.table1;
CREATE TABLE staging.table1
WITH
(
    -- Derived from the original table definition 
    DISTRIBUTION = HASH(column1),
    -- Derived from the original table definition
    CLUSTERED COLUMNSTORE INDEX
)
AS SELECT  * FROM staging.table2;
```

## <a name="next-steps"></a>Étapes suivantes

- [Télécharger Azure Synapse Pathway](synapse-pathway-download.md)
- [FORUM AUX QUESTIONS](pathway-faq.md)

