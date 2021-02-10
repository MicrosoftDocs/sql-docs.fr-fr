---
title: Sous-expression commune
description: Affiche un exemple d’amélioration de requête introduit dans Analytics Platform System CU 7.3
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: d1d60b85bec6ec810cc506e1002dbc8ecc60ed50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052790"
---
# <a name="common-subexpression-elimination-explained"></a>Explication de l’élimination des sous-expressions communes

APS CU 7.3 améliore les performances des requêtes avec l’élimination de sous-expression commune dans l’optimiseur de requête SQL. L’amélioration améliore les requêtes de deux manières. Le premier avantage est la possibilité d’identifier et d’éliminer ces expressions pour réduire le temps de compilation SQL. Le deuxième avantage et le plus important est que les opérations de déplacement de données pour ces sous-expressions redondantes sont éliminées, de sorte que la durée d’exécution des requêtes est plus rapide.

```sql
select top 100 asceding.rnk, i1.i_product_name best_performing, i2.i_product_name worst_performing
  from(select *
       from (select item_sk,rank() over (order by rank_col asc) rnk
             from (select ss_item_sk item_sk,avg(ss_net_profit) rank_col
                   from store_sales ss1
                   where ss_store_sk = 8
                   group by ss_item_sk
                   having avg(ss_net_profit) > 0.9*(select avg(ss_net_profit) rank_col
                                                    from store_sales
                                                    where ss_store_sk = 8
                                                      and ss_hdemo_sk is null
                                                    group by ss_store_sk))V1)V11
       where rnk  < 11) asceding,
      (select *
       from (select item_sk,rank() over (order by rank_col desc) rnk
             from (select ss_item_sk item_sk,avg(ss_net_profit) rank_col
                   from store_sales ss1
                   where ss_store_sk = 8
                   group by ss_item_sk
                   having avg(ss_net_profit) > 0.9*(select avg(ss_net_profit) rank_col
                                                    from store_sales
                                                    where ss_store_sk = 8
                                                      and ss_hdemo_sk is null
                                                    group by ss_store_sk))V2)V21
       where rnk  < 11) descending,
  item i1,
  item i2
  where asceding.rnk = descending.rnk
    and i1.i_item_sk=asceding.item_sk
    and i2.i_item_sk=descending.item_sk
  order by asceding.rnk
  ;
```
Examinez la requête ci-dessus à partir des outils de test TPC-DS.  Dans la requête ci-dessus, la sous-requête est la même, mais la clause ORDER BY avec Rank () sur Function est triée de deux façons différentes. Avant CU 7.3, cette sous-requête sera évaluée et exécutée deux fois, une fois pour l’ordre croissant et une fois pour l’ordre décroissant, à savoir deux opérations de déplacement de données. Après l’installation d’APS CU 7.3, la partie de sous-requête est évaluée une seule fois, réduisant ainsi le déplacement des données et la fin plus rapide de la requête.

Nous avons introduit un [commutateur de fonctionnalité](appliance-feature-switch.md) appelé « OptimizeCommonSubExpressions » qui vous permet de tester la fonctionnalité, même après la mise à niveau vers APS cu 7.3. La fonctionnalité est activée par défaut, mais peut être désactivée. 

> [!NOTE] 
> Les modifications apportées aux valeurs de commutateur de fonctionnalité requièrent un redémarrage du service.

Vous pouvez essayer l’exemple de requête en créant les tables suivantes dans votre environnement de test et en évaluant le plan d’explication pour la requête mentionnée ci-dessus. 

```sql
CREATE TABLE [dbo].[store_sales] (
    [ss_sold_date_sk] int NULL, 
    [ss_sold_time_sk] int NULL, 
    [ss_item_sk] int NOT NULL, 
    [ss_customer_sk] int NULL, 
    [ss_cdemo_sk] int NULL, 
    [ss_hdemo_sk] int NULL, 
    [ss_addr_sk] int NULL, 
    [ss_store_sk] int NULL, 
    [ss_promo_sk] int NULL, 
    [ss_ticket_number] int NOT NULL, 
    [ss_quantity] int NULL, 
    [ss_wholesale_cost] decimal(7, 2) NULL, 
    [ss_list_price] decimal(7, 2) NULL, 
    [ss_sales_price] decimal(7, 2) NULL, 
    [ss_ext_discount_amt] decimal(7, 2) NULL, 
    [ss_ext_sales_price] decimal(7, 2) NULL, 
    [ss_ext_wholesale_cost] decimal(7, 2) NULL, 
    [ss_ext_list_price] decimal(7, 2) NULL, 
    [ss_ext_tax] decimal(7, 2) NULL, 
    [ss_coupon_amt] decimal(7, 2) NULL, 
    [ss_net_paid] decimal(7, 2) NULL, 
    [ss_net_paid_inc_tax] decimal(7, 2) NULL, 
    [ss_net_profit] decimal(7, 2) NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([ss_item_sk]),  PARTITION ([ss_sold_date_sk] RANGE RIGHT FOR VALUES (2450815, 2451180, 2451545, 2451911, 2452276, 2452641, 2453006)));

CREATE TABLE [dbo].[item] (
    [i_item_sk] int NOT NULL, 
    [i_item_id] char(16) COLLATE Latin1_General_100_CI_AS_KS_WS NOT NULL, 
    [i_rec_start_date] date NULL, 
    [i_rec_end_date] date NULL, 
    [i_item_desc] varchar(200) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_current_price] decimal(7, 2) NULL, 
    [i_wholesale_cost] decimal(7, 2) NULL, 
    [i_brand_id] int NULL, 
    [i_brand] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_class_id] int NULL, 
    [i_class] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_category_id] int NULL, 
    [i_category] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_manufact_id] int NULL, 
    [i_manufact] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_size] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_formulation] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_color] char(20) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_units] char(10) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_container] char(10) COLLATE Latin1_General_100_CI_AS_KS_WS NULL, 
    [i_manager_id] int NULL, 
    [i_product_name] char(50) COLLATE Latin1_General_100_CI_AS_KS_WS NULL
)
WITH (CLUSTERED INDEX ( [i_item_sk] ASC ), DISTRIBUTION = REPLICATE);
```
Si vous examinez le plan d’explication de la requête, vous verrez qu’avant CU 7.3 (ou lorsque le commutateur de fonctionnalité est désactivé), la requête a 17 nombre total d’opérations et après CU 7.3 (ou avec le commutateur de fonctionnalité activé) la même requête affiche 9 nombre total d’opérations. Si vous comptez uniquement les opérations de déplacement de données, vous verrez que le plan précédent a quatre opérations de déplacement et deux opérations de déplacement dans le nouveau plan. Le nouvel optimiseur de requête a pu réduire deux opérations de déplacement de données en réutilisant la table temporaire qu’il a déjà créée avec le nouveau plan, réduisant ainsi le runtime de requête. 


