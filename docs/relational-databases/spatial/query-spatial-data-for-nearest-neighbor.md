---
description: Interroger des données spatiales au sujet du plus proche voisin
title: Interroger des données spatiales au sujet du plus proche voisin | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 7af4ad5d-484e-45b4-aa16-83c33b358bb6
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e53b816b25c991e7dafb891a0006b256979cb298
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88464923"
---
# <a name="query-spatial-data-for-nearest-neighbor"></a>Interroger des données spatiales au sujet du plus proche voisin
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Une requête courante utilisée avec les données spatiales est la requête Plus proche voisin. Les requêtes Plus proche voisin sont utilisées pour trouver les objets spatiaux les plus proches d'un objet spatial spécifique. Par exemple, le localisateur de magasin d'un site Web doit souvent trouver les magasins les plus proches de l'emplacement d'un client.  
  
 Une requête Plus proche voisin peut être écrite dans divers formats de requête valides, mais pour qu'elle utilise un index spatial, la syntaxe suivante doit être utilisée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SELECT TOP ( number )  
        [ WITH TIES ]  
        [ * | expression ]   
        [, ...]  
    FROM spatial_table_reference, ...   
        [ WITH   
            (   
                [ INDEX ( index_ref ) ]   
                [ , SPATIAL_WINDOW_MAX_CELLS = <value>]   
                [ ,... ]   
            )   
        ]  
    WHERE   
        column_ref.STDistance ( @spatial_ object )   
            {   
                [ IS NOT NULL ] | [ < const ] | [ > const ]   
                | [ <= const ] | [ >= const ] | [ <> const ] ]   
            }  
            [ AND { other_predicate } ]   
    }  
    ORDER BY column_ref.STDistance ( @spatial_ object ) [ ,...n ]  
[ ; ]  
  
```  
  
## <a name="nearest-neighbor-query-and-spatial-indexes"></a>Requête Plus proche voisin et index spatiaux  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les clauses **TOP** et **ORDER BY** sont utilisées pour effectuer une requête Plus proche voisin sur les colonnes de données spatiales. La clause **ORDER BY** contient un appel à la méthode `STDistance()` pour le type de données de colonne spatiale. La clause **TOP** indique le nombre d'objets à retourner pour la requête.  
  
 Les conditions suivantes doivent être respectées pour qu'une requête Plus proche voisin utilise un index spatial :  
  
1.  Un index spatial doit être présent sur l’une des colonnes spatiales et la méthode `STDistance()` doit utiliser cette colonne dans les clauses **WHERE** et **ORDER BY** .  
  
2.  La clause **TOP** ne peut pas contenir d'instruction **PERCENT** .  
  
3.  La clause **WHERE** doit contenir une méthode `STDistance()` .  
  
4.  S’il existe plusieurs prédicats dans la clause **WHERE** , le prédicat qui contient la méthode `STDistance()` doit être connecté par une conjonction **AND** aux autres prédicats. La méthode `STDistance()` ne doit pas se trouver dans une partie facultative de la clause **WHERE** .  
  
5.  La première expression dans la clause **ORDER BY** doit utiliser la méthode `STDistance()` .  
  
6.  L’ordre de tri de la première expression `STDistance()` dans la clause **ORDER BY** doit être **ASC**.  
  
7.  Toutes les lignes pour lesquelles `STDistance` retourne **NULL** doivent être éliminées par filtrage.  
  
> [!WARNING]  
>   Les méthodes qui prennent les types de données **geography** ou **geometry** comme arguments retourneront **NULL** si les SRID ne sont pas les mêmes pour les types.  
  
 Il est recommandé d'utiliser les nouveaux pavages d'index spatial pour les index utilisés dans les requêtes Plus proche voisin. Pour plus d’informations sur les pavages d’index spatial, consultez [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md).  
  
## <a name="example"></a> Exemple  
 L'exemple de code suivant montre une requête Plus proche voisin qui peut utiliser un index spatial. L'exemple utilise la table `Person.Address` dans la base de données `AdventureWorks2016` .  
  
```sql  
USE AdventureWorks2016  
GO  
DECLARE @g geography = 'POINT(-121.626 47.8315)';  
SELECT TOP(7) SpatialLocation.ToString(), City FROM Person.Address  
WHERE SpatialLocation.STDistance(@g) IS NOT NULL  
ORDER BY SpatialLocation.STDistance(@g);  
```  
  
 Créez un index spatial sur la colonne SpatialLocation pour voir comment une requête Plus proche voisin utilise un index spatial. Pour plus d'informations sur la création d'index spatiaux, consultez [Create, Modify, and Drop Spatial Indexes](../../relational-databases/spatial/create-modify-and-drop-spatial-indexes.md).  
  
## <a name="example"></a> Exemple  
 L'exemple de code suivant montre une requête Plus proche voisin qui ne peut pas utiliser un index spatial.  
  
```sql  
USE AdventureWorks2016  
GO  
DECLARE @g geography = 'POINT(-121.626 47.8315)';  
SELECT TOP(7) SpatialLocation.ToString(), City FROM Person.Address  
ORDER BY SpatialLocation.STDistance(@g);  
```  
  
 La requête n’a pas de clause **WHERE** qui utilise `STDistance()` au format spécifié dans la section Syntaxe ; la requête ne peut donc pas utiliser d’index spatial.  
  
## <a name="see-also"></a>Voir aussi  
 [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
