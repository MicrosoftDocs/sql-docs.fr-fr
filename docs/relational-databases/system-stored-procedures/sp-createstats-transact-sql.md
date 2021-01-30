---
description: sp_createstats (Transact-SQL)
title: sp_createstats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_createstats_TSQL
- sp_createstats
dev_langs:
- TSQL
helpviewer_keywords:
- sp_createstats
ms.assetid: 8204f6f2-5704-40a7-8d51-43fc832eeb54
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a0fb35bd43125370dc4123425cc919be3033951f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205206"
---
# <a name="sp_createstats-transact-sql"></a>sp_createstats (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Appelle l’instruction [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) pour créer des statistiques à une seule colonne sur les colonnes qui ne sont pas déjà la première colonne d’un objet de statistiques. La création de statistiques à une seule colonne augmente le nombre d’histogrammes, ce qui peut améliorer les estimations de cardinalité, les plans de requête et les performances des requêtes. La première colonne d'un objet de statistiques comporte un histogramme ; les autres colonnes n'ont pas d'histogramme.  
  
 sp_createstats est utile aux applications de tests des performances, par exemple, pour lesquelles les délais d'exécution des requêtes sont critiques, car il n'est pas possible d'attendre que l'optimiseur de requête génère des statistiques de colonnes uniques. Dans la plupart des cas, il n’est pas nécessaire d’utiliser sp_createstats ; l’optimiseur de requête génère des statistiques de colonnes uniques selon les besoins afin d’améliorer les plans de requête lorsque l’option **AUTO_CREATE_STATISTICS** est activée.  
  
 Pour plus d’informations sur les statistiques, consultez [Statistiques](../../relational-databases/statistics/statistics.md). Pour plus d’informations sur la génération de statistiques de colonnes uniques, consultez l’option **AUTO_CREATE_STATISTICS** dans les [options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_createstats   
    [   [ @indexonly =   ] { 'indexonly'   | 'NO' } ]   
    [ , [ @fullscan =    ] { 'fullscan'    | 'NO' } ]   
    [ , [ @norecompute = ] { 'norecompute' | 'NO' } ]  
    [ , [ @incremental = ] { 'incremental' | 'NO' } ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @indexonly = ] 'indexonly'` Crée des statistiques uniquement sur les colonnes qui se trouvent dans un index existant et qui ne constituent pas la première colonne d’une définition d’index. **indexonly** est **de type char (9)**. La valeur par défaut est NO.  
  
`[ @fullscan = ] 'fullscan'` Utilise l’instruction [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) avec l’option **FULLSCAN** . **FULLSCAN** est **de type char (9)**.  La valeur par défaut est NO.  
  
`[ @norecompute = ] 'norecompute'` Utilise l’instruction [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) avec l’option **NORECOMPUTE** . **NORECOMPUTE** est **de type char (12)**.  La valeur par défaut est NO.  
  
`[ @incremental = ] 'incremental'` Utilise l’instruction [Create Statistics](../../t-sql/statements/create-statistics-transact-sql.md) avec l’option **INCREMENTAL = on** . Le type **incrémentiel** est **char (12)**.  La valeur par défaut est NO.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Chaque nouvel objet de statistiques porte le même nom que la colonne sur laquelle il est créé.  
  
## <a name="remarks"></a>Notes  
 sp_createstats ne crée pas ou ne met pas à jour les statistiques sur les colonnes qui sont la première colonne d’un objet de statistiques existant ;  Cela comprend la première colonne de statistiques créées pour les index, les colonnes avec des statistiques à une seule colonne générées avec l’option AUTO_CREATE_STATISTICS et la première colonne de statistiques créée avec l’instruction CREATe STATISTICs. sp_createstats ne crée pas de statistiques sur les premières colonnes des index désactivés, sauf si cette colonne est utilisée dans un autre index activé. sp_createstats ne crée pas de statistiques sur les tables avec un index cluster désactivé.  
  
 Lorsque la table contient un jeu de colonnes, sp_createstats ne crée pas de statistiques sur les colonnes éparses. Pour plus d’informations sur les jeux de colonnes et les colonnes éparses, consultez [utiliser des jeux de colonnes](../../relational-databases/tables/use-column-sets.md) et utiliser des [colonnes éparses](../../relational-databases/tables/use-sparse-columns.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle de base de données fixe db_owner.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-create-single-column-statistics-on-all-eligible-columns"></a>R. Créer des statistiques de colonnes uniques sur toutes les colonnes appropriées  
 L'exemple suivant crée des statistiques de colonnes uniques pour toutes les colonnes appropriées de la base de données active.  
  
```  
EXEC sp_createstats;  
GO  
```  
  
### <a name="b-create-single-column-statistics-on-all-eligible-index-columns"></a>B. Créer des statistiques de colonnes uniques sur toutes les colonnes d'index appropriées  
 L'exemple suivant crée des statistiques de colonnes uniques pour toutes les colonnes appropriées qui sont déjà dans un index et qui ne constituent pas la première colonne de cet index.  
  
```  
EXEC sp_createstats 'indexonly';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Statistiques](../../relational-databases/statistics/statistics.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [Moteur de base de données des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
