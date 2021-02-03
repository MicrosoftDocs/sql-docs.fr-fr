---
description: CREATE SPATIAL INDEX (Transact-SQL)
title: CREATE SPATIAL INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SPATIAL INDEX
- CREATE SPATIAL INDEX
- CREATE_SPATIAL_INDEX_TSQL
- SPATIAL_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- spatial indexes [SQL Server], creating
- index creation [SQL Server], spatial indexes
- CREATE SPATIAL INDEX statement
- CREATE INDEX statement
ms.assetid: ee6b9116-a7ff-463a-a9f0-b360804d8678
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 07f7bdead063167b789bd465fac6f66b653a0cbb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191029"
---
# <a name="create-spatial-index-transact-sql"></a>CREATE SPATIAL INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée un index spatial sur une table et une colonne spécifiées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un index peut être créé avant que la table soit remplie de données. Les index peuvent être créés sur des tables ou des vues d'une autre base de données en spécifiant un nom de base de données qualifié. Les index spatiaux exigent que la table ait une clé primaire cluster. Des index spatiaux ne peuvent pas être spécifiés sur des vues indexées. Pour plus d’informations sur les index spatiaux, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE SPATIAL INDEX index_name
  ON <object> ( spatial_column_name )  
    {  
       <geometry_tessellation> | <geography_tessellation>  
    }
  [ ON { filegroup_name | "default" } ]  
[;]
  
<object> ::=  
    { database_name.schema_name.table_name | schema_name.table_name | table_name }  
  
<geometry_tessellation> ::=  
{
  <geometry_automatic_grid_tessellation>
| <geometry_manual_grid_tessellation>
}  
  
<geometry_automatic_grid_tessellation> ::=  
{  
    [ USING GEOMETRY_AUTO_GRID ]  
          WITH  (  
        <bounding_box>  
            [ [,] <tessellation_cells_per_object> [ ,...n] ]  
            [ [,] <spatial_index_option> [ ,...n] ]  
                 )  
}  
  
<geometry_manual_grid_tessellation> ::=  
{  
       [ USING GEOMETRY_GRID ]  
         WITH (  
                    <bounding_box>  
                        [ [,]<tessellation_grid> [ ,...n] ]  
                        [ [,]<tessellation_cells_per_object> [ ,...n] ]  
                        [ [,]<spatial_index_option> [ ,...n] ]  
   )  
}
  
<geography_tessellation> ::=  
{  
      <geography_automatic_grid_tessellation> | <geography_manual_grid_tessellation>  
}  
  
<geography_automatic_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_AUTO_GRID ]  
    [ WITH (  
        [ [,] <tessellation_cells_per_object> [ ,...n] ]  
        [ [,] <spatial_index_option> ]  
     ) ]  
}  
  
<geography_manual_grid_tessellation> ::=  
{  
    [ USING GEOGRAPHY_GRID ]  
    [ WITH (  
                [ <tessellation_grid> [ ,...n] ]  
                [ [,] <tessellation_cells_per_object> [ ,...n] ]  
                [ [,] <spatial_index_option> [ ,...n] ]  
                ) ]  
}  
  
<bounding_box> ::=  
{  
      BOUNDING_BOX = ( {  
       xmin, ymin, xmax, ymax
       | <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>, <named_bb_coordinate>
  } )  
}  
  
<named_bb_coordinate> ::= { XMIN = xmin | YMIN = ymin | XMAX = xmax | YMAX=ymax }  
  
<tessellation_grid> ::=  
{
    GRIDS = ( { <grid_level> [ ,...n ] | <grid_size>, <grid_size>, <grid_size>, <grid_size>  }
        )  
}  
<tessellation_cells_per_object> ::=  
{
   CELLS_PER_OBJECT = n
}  
  
<grid_level> ::=  
{  
     LEVEL_1 = <grid_size>
  |  LEVEL_2 = <grid_size>
  |  LEVEL_3 = <grid_size>
  |  LEVEL_4 = <grid_size>
}  
  
<grid_size> ::= { LOW | MEDIUM | HIGH }  
  
<spatial_index_option> ::=  
{  
    PAD_INDEX = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | STATISTICS_NORECOMPUTE = { ON | OFF }  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
    | DATA_COMPRESSION = { NONE | ROW | PAGE }  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments

 *index_name*     
 Nom de l'index. Les noms d'index doivent être uniques dans une table, mais ne doivent pas être nécessairement uniques dans une base de données. Les noms d’index doivent se conformer aux règles régissant les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 ON \<object> ( *spatial_column_name* )     
 Spécifie l'objet (base de données, schéma ou table) sur lequel l'index doit être créé et le nom de la colonne spatiale.  
  
 *spatial_column_name* spécifie la colonne spatiale sur laquelle l’index est basé. Une seule colonne spatiale peut être spécifiée dans une définition d’index spatial unique ; toutefois, plusieurs index spatiaux peuvent être créés sur une colonne de type **geometry** ou **geography**.  
  
 USING      
 Indique le schéma de pavage pour l'index spatial. Ce paramètre utilise la valeur spécifique au type, affichée dans le tableau suivant :  
  
|Type de données de la colonne|Schéma de pavage|  
|-------------------------|-------------------------|  
|**geometry**|GEOMETRY_GRID|  
|**geometry**|GEOMETRY_AUTO_GRID|  
|**Geography**|GEOGRAPHY_GRID|  
|**Geography**|GEOGRAPHY_AUTO_GRID|  
  
 Un index spatial peut être créé uniquement sur une colonne de type **geometry** ou **geography**, sinon une erreur est générée. Si un paramètre non valide pour un type donné est passé, une erreur est générée.  
  
 Pour plus d’informations sur la façon dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implémente le pavage, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 ON *filegroup_name*      
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Crée l'index spécifié dans le groupe de fichiers spécifié. Si aucun emplacement n'est défini et que la table n'est pas partitionnée, l'index utilise le même groupe de fichiers que la table sous-jacente. Le groupe de fichiers doit déjà exister.  
  
 ON "default"     
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Crée l'index spécifié sur le groupe de fichiers par défaut.  
  
 Le terme « default », dans ce contexte, n'est pas un mot clé. Il s'agit de l'identificateur du groupe de fichiers par défaut et il doit être délimité, comme dans ON "default" ou ON [default]. Si "default" est spécifié, l'option QUOTED_IDENTIFIER doit être activée (ON) pour la session active. Il s'agit du paramètre par défaut. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../../t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **\<object>::=**     
 Objet qualifié complet ou partiellement qualifié à indexer.  
  
 *database_name*      
 Nom de la base de données.  
  
 *schema_name*     
 Nom du schéma auquel appartient la table.  
  
 *table_name*      
 Nom de la table à indexer.  
  
[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] prend en charge le format du nom en trois partie database_name.[schema_name].object_name lorsque le database_name est la base de données actuelle ou le database_name est tempdb et le object_name démarre par #.  
  
### <a name="using-options"></a>Options USING

 GEOMETRY_GRID      
 Spécifie le schéma de pavage de la grille **geometry** que vous utilisez. GEOMETRY_GRID peut être spécifié uniquement sur une colonne du type de données **geometry**.  GEOMETRY_GRID permet la mise au point manuelle du schéma de pavage.  
  
 GEOMETRY_AUTO_GRID      
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Peut être spécifié uniquement sur une colonne du type de données geometry. Il s'agit de l'option par défaut pour ce type de données et il n'est pas nécessaire de la spécifier.  
  
 GEOGRAPHY_GRID      
 Spécifie le schéma de pavage de la grille géographique. GEOGRAPHY_GRID peut être spécifié uniquement sur une colonne du type de données **geography**.  
  
 GEOGRAPHY_AUTO_GRID      
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Peut être spécifié uniquement sur une colonne du type de données geography.  Il s'agit de l'option par défaut pour ce type de données et il n'est pas nécessaire de la spécifier.  
  
### <a name="with-options"></a>Options WITH

BOUNDING_BOX      
Spécifie un jeu de quatre tuples numérique qui définit les quatre coordonnées du rectangle englobant : les coordonnées min. x et min. y de l'angle inférieur gauche, et les coordonnées max. x et max. y de l'angle supérieur droit.  
  
 *xmin*      
 Spécifie la coordonnée x de l'angle inférieur gauche du rectangle englobant.  
  
 *ymin*      
 Spécifie la coordonnée y de l'angle inférieur gauche du rectangle englobant.  
  
 *xmax*     
 Spécifie la coordonnée x de l'angle supérieur droit du rectangle englobant.  
  
 *ymax*     
 Spécifie la coordonnée y de l'angle supérieur droit du rectangle englobant.  
  
 XMIN = *xmin*     
 Spécifie le nom et la valeur de la propriété pour la coordonnée x de l'angle inférieur gauche du rectangle englobant.  
  
 YMIN =*ymin*      
 Spécifie le nom et la valeur de la propriété pour la coordonnée y de l'angle inférieur gauche du rectangle englobant.  
  
 XMAX =*xmax*      
 Spécifie le nom et la valeur de la propriété pour la coordonnée x de l'angle supérieur droit du rectangle englobant.  
  
 YMAX =*ymax*     
 Spécifie le nom et la valeur de la propriété pour la coordonnée y de l'angle supérieur droit du rectangle englobant.  
  
 > [!NOTE]
 > Les coordonnées du rectangle englobant s'appliquent uniquement dans une clause USING GEOMETRY_GRID.  
 >
 > *XMAX* doit être supérieur à *xmin* et *ymax* doit être supérieur à *ymin*. Vous pouvez spécifier toute représentation de valeur [float](../../t-sql/data-types/float-and-real-transact-sql.md) valide, en supposant que : *xmax* > *xmin* et *ymax* > *ymin*. Sinon, les erreurs appropriées sont générées.  
 >
 > Il n'y a pas de valeurs par défaut.  
 >
 > Les noms de propriété du rectangle englobant ne respectent pas la casse, indépendamment du classement de base de données.  
  
 Pour spécifier des noms de propriété, vous devez spécifier chacun d'entre eux une seule et unique fois. Vous pouvez les spécifier dans n'importe quel ordre. Par exemple, les clauses suivantes sont équivalentes :  
  
- BOUNDING_BOX =( XMIN =*xmin*, YMIN =*ymin*, XMAX =*xmax*, YMAX =*ymax* )  
  
- BOUNDING_BOX =( XMIN =*xmin*, XMAX =*xmax*, YMIN =*ymin*, YMAX =*ymax*)  
  
GRIDS      
Définit la densité de la grille à chaque niveau d'un schéma de pavage. Lorsque GEOMETRY_AUTO_GRID et GEOGRAPHY_AUTO_GRID sont sélectionnés, cette option est désactivée.  
  
 Pour plus d’informations sur le pavage, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
 Les paramètres GRIDS sont les suivants :  
  
 LEVEL_1     
 Spécifie la grille de premier niveau (haut).  
  
 LEVEL_2    
 Spécifie la grille de second niveau.  
  
 LEVEL_3    
 Spécifie la grille de troisième niveau.  
  
 LEVEL_4    
 Spécifie la grille de quatrième niveau.  
  
 LOW    
 Spécifie la densité la plus faible possible pour la grille à un niveau donné. LOW équivaut à 16 cellules (grille 4x4).  
  
 **MEDIUM**     
 Spécifie la densité moyenne pour la grille à un niveau donné. MEDIUM équivaut à 64 cellules (grille 8x8).  
  
 HIGH     
 Spécifie la densité la plus élevée possible pour la grille à un niveau donné. HIGH équivaut à 256 cellules (grille 16x16).  
  
> [!NOTE]
> Grâce aux noms de niveau, vous pouvez spécifier les niveaux dans n'importe quel ordre et omettre des niveaux. Si vous utilisez le nom d'un niveau, vous devez utiliser le nom des autres niveaux que vous spécifiez. Si vous omettez un niveau, sa densité prend MEDIUM comme valeur par défaut.  

> [!WARNING]
> Si une densité non valide est spécifiée, une erreur est générée.  
  
CELLS_PER_OBJECT =*n*     
Spécifie le nombre de cellules de pavage par objet pouvant être utilisées pour un objet spatial unique dans l'index par le processus de pavage. *n* peut être tout entier compris entre 1 et 8192 (inclus). Si un nombre non valide est passé ou que le nombre est supérieur au nombre maximal de cellules pour le pavage spécifié, une erreur est générée.  
  
 CELLS_PER_OBJECT a les valeurs par défaut suivantes :  
  
|Option USING|Cellules par objet par défaut|  
|------------------|------------------------------|  
|GEOMETRY_GRID|**16**|  
|GEOMETRY_AUTO_GRID|**8**|  
|GEOGRAPHY_GRID|**16**|  
|GEOGRAPHY_AUTO_GRID|**12**|  
  
Au niveau supérieur, si un objet couvre plus de cellules que le nombre spécifié par *n*, l’indexation utilise autant de cellules que nécessaire pour fournir un pavage de niveau supérieur complet. Dans de tels cas, un objet peut recevoir plus de cellules que le nombre spécifié. Dans ce cas, le nombre maximal est le nombre de cellules générées par la grille de niveau supérieur, qui dépend de la densité.  
  
La valeur CELLS_PER_OBJECT est utilisée par la règle de pavage de cellules par objet. Pour plus d’informations sur les règles de pavage, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
PAD_INDEX = { ON | **OFF** }     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
Spécifie le remplissage de l'index. La valeur par défaut est OFF.  
  
ACTIVÉ     
Indique que le pourcentage d’espace libre spécifié par *fillfactor* est appliqué aux pages du niveau intermédiaire de l’index.  
  
OFF ou *fillfactor* n’est pas spécifié     
Indique que les pages de niveau intermédiaire sont presque entièrement remplies, ce qui laisse suffisamment d'espace libre pour au moins une ligne de la taille maximale permise par l'index, en prenant en compte l'ensemble de clés sur les pages intermédiaires.  
  
L'option PAD_INDEX est utile seulement si FILLFACTOR est spécifié, car PAD_INDEX utilise le pourcentage spécifié par FILLFACTOR. Si le pourcentage défini pour FILLFACTOR n'est pas suffisamment élevé pour autoriser une ligne, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] remplace en interne le pourcentage de façon à ce qu'il autorise le minimum. Le nombre de lignes dans une page d’index intermédiaire n’est jamais inférieur à deux, quelle que soit la faiblesse de la valeur de *fillfactor*.  
  
FILLFACTOR =*fillfactor*     
 **S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Spécifie un pourcentage indiquant le taux de remplissage appliqué par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au niveau feuille de chaque page d'index lors de la création ou de la reconstruction de l'index. *fillfactor* doit être une valeur entière comprise entre 1 et 100. La valeur par défaut est 0. Si *fillfactor* a pour valeur 100 ou 0, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des index avec des pages de niveau feuille intégralement remplies.  
  
> [!NOTE]  
> Les taux de remplissage 0 et 100 sont identiques en tous points.
  
 La valeur FILLFACTOR s'applique uniquement lors de la création ou de la reconstruction de l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne conserve pas dynamiquement dans les pages le pourcentage d'espace libre défini. Pour afficher le facteur de remplissage, utilisez la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
> [!IMPORTANT]  
> La création d'un index cluster avec un facteur de remplissage FILLFACTOR inférieur à 100 affecte la quantité d'espace de stockage qu'occupent les données, car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] redistribue les données lorsqu'il crée l'index cluster.  
  
 Pour plus d’informations, consultez [Spécifier un facteur de remplissage pour un index](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
SORT_IN_TEMPDB = { ON | **OFF** }       
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Spécifie si les résultats temporaires du tri doivent être stockés dans tempdb. La valeur par défaut est OFF.  
  
 ACTIVÉ     
 Les résultats de tri intermédiaires utilisés pour créer l'index sont stockés dans tempdb. Cela permet de réduire la durée de création d'un index si tempdb n'est pas sur le même groupe de disques que la base de données utilisateur. Toutefois, une plus grande quantité d'espace disque est alors utilisée lors de la création de l'index.  
  
 OFF     
 Les résultats de tri intermédiaires sont stockés dans la même base de données que l'index.  
  
 Outre l'espace nécessaire dans la base de données utilisateur pour créer l'index, tempdb doit disposer à peu près du même espace supplémentaire pour stocker les résultats de tri intermédiaires. Pour plus d’informations, consultez [Option SORT_IN_TEMPDB pour les index](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
IGNORE_DUP_KEY =**OFF**     
N'a aucun effet pour les index spatiaux, car le type d'index n'est jamais unique. N'activez pas cette option (ON), sinon une erreur est déclenchée.  
  
STATISTICS_NORECOMPUTE = { ON | **OFF**}     
Spécifie si les statistiques de distribution sont recalculées. La valeur par défaut est OFF.  
  
 ACTIVÉ    
 Les statistiques obsolètes ne sont pas recalculées automatiquement.  
  
 OFF    
 La mise à jour automatique des statistiques est activée.  
  
 Pour restaurer la mise à jour automatique des statistiques, affectez la valeur OFF à STATISTICS_NORECOMPUTE ou exécutez UPDATE STATISTICS sans la clause NORECOMPUTE.  
  
> [!IMPORTANT]  
> La désactivation du recalcul automatique des statistiques de distribution peut empêcher l'optimiseur de requête de sélectionner des plans d'exécution optimaux pour les requêtes qui impliquent la table.  
  
DROP_EXISTING = { ON | **OFF** }     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Spécifie que l'index spatial nommé préexistant est supprimé et reconstruit. La valeur par défaut est OFF.  
  
 ACTIVÉ     
 L'index existant est supprimé et recréé. Le nom d'index défini doit être identique à celui de l'index existant. Toutefois, la définition de l'index peut être modifiée. Par exemple, vous pouvez définir des colonnes, un ordre de tri, un schéma de partition ou des options d'indexation différentes.  
  
 OFF     
 Une erreur s'affiche si le nom d'index spécifié existe déjà.  
  
 Le type d'index ne peut pas être modifié à l'aide de DROP_EXISTING.  
  
ONLINE =**OFF**      
Indique que les tables sous-jacentes et les index associés ne sont pas disponibles pour les requêtes et la modification de données pendant l'opération d'index. Dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les constructions d'index en ligne ne sont pas prises en charge pour les index spatiaux. Si cette option a la valeur ON pour un index spatial, une erreur est générée. Omettez l'option ONLINE ou attribuez la valeur OFF à ONLINE.  
  
 Une opération d'index hors connexion qui crée, reconstruit ou supprime un index spatial acquiert un verrou Sch-M (Modification du schéma) sur la table. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération.  
  
> [!NOTE]  
> Les opérations d'index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
ALLOW_ROW_LOCKS = { **ON** | OFF }     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Indique si les verrous de ligne sont autorisés ou non. La valeur par défaut est ON.  
  
 ACTIVÉ     
 Les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés.  
  
 OFF     
 Les verrous de ligne ne sont pas utilisés.  
  
ALLOW_PAGE_LOCKS = { **ON** | OFF }     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 
  
 Indique si les verrous de page sont autorisés. La valeur par défaut est ON.  
  
 ACTIVÉ    
 Les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés.  
  
 OFF     
 Les verrous de page ne sont pas utilisés.  
  
MAXDOP =*max_degree_of_parallelism*      
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Remplace l'option de configuration `max degree of parallelism` pendant la durée de l'opération d'index. Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
> [!IMPORTANT]  
> Bien que l'option MAXDOP soit prise en charge syntaxiquement, CREATE SPATIAL INDEX n'utilise actuellement qu'un processeur unique.  
  
 *max_degree_of_parallelism* peut avoir la valeur :  
  
 1     
 Supprime la création de plans parallèles.  
  
 \>1    
 Limite le nombre maximal de processeurs utilisés dans l'indexation parallèle au nombre défini ou à un nombre inférieur en fonction de la charge de travail actuelle du système.  
  
 0 (valeur par défaut)    
 Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
 Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
> Les opérations d’index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
DATA_COMPRESSION = {NONE | ROW | PAGE}     
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures) et [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Détermine le niveau de compression de données utilisé par l'index.  
  
 Aucune     
 Aucune compression n'est utilisée sur les données selon l'index  
  
 ROW    
 Une compression de ligne est utilisée sur les données selon l'index  
  
 PAGE    
 Une compression de page est utilisée sur les données selon l'index  
  
## <a name="remarks"></a>Notes
Chaque option ne peut être spécifiée qu'une fois par instruction CREATE SPATIAL INDEX. La spécification d'un doublon de toute option génère une erreur.  
  
Vous pouvez créer jusqu'à 249 index spatiaux sur chaque colonne spatiale dans une table. Il peut être utile de créer plusieurs index spatiaux sur une colonne spatiale spécifique, par exemple pour indexer des paramètres de pavage différents dans une même colonne.  
  
> [!IMPORTANT]  
> Il existe plusieurs autres restrictions applicables à la création d'un index spatial. Pour plus d’informations, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
Une construction d'index ne peut pas utiliser le parallélisme de processus disponible.  
  
## <a name="methods-supported-on-spatial-indexes"></a>Méthodes prises en charge sur les index spatiaux
Dans certaines conditions, les index spatiaux prennent en charge plusieurs méthodes de géométrie basées sur les ensembles. Pour plus d’informations, consultez [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md).  
  
## <a name="spatial-indexes-and-partitioning"></a>Index spatiaux et partitionnement
Par défaut, si un index spatial est créé sur une table partitionnée, l'index est partitionné d'après le schéma de partition de la table. Ainsi, les données d'index et la ligne connexe sont stockées dans la même partition.  
  
Dans ce cas, pour modifier le schéma de partition de la table de base, il vous faudrait supprimer l'index spatial avant de pouvoir repartitionner la table de base. Pour éviter cette restriction, vous pouvez spécifier l'option « ON filegroup » lors de la création d'un index spatial. Pour plus d'informations, consultez « Index spatiaux et groupes de fichiers » plus loin dans cette rubrique.  
  
## <a name="spatial-indexes-and-filegroups"></a>Index spatiaux et groupes de fichiers
Par défaut, les index spatiaux sont partitionnés dans les mêmes groupes de fichiers que la table sur laquelle l'index est spécifié. Vous pouvez modifier ce comportement en spécifiant un groupe de fichiers :  
  
[ ON { *filegroup_name* | "default" } ]     
Si vous spécifiez un groupe de fichiers pour un index spatial, l'index est placé sur ce groupe de fichiers, quel que soit le schéma de partitionnement de la table.  
  
## <a name="catalog-views-for-spatial-indexes"></a>Affichages catalogue pour les index spatiaux

 Les affichages catalogue suivants sont propres aux index spatiaux :  
  
 [sys.spatial_indexes](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)  
 Représente les informations d'index principal des index spatiaux.  
  
 [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)  
 Représente les informations sur le schéma de pavage et les paramètres de chaque index spatial.  
  
## <a name="additional-remarks-about-creating-indexes"></a>Notes supplémentaires sur la création d’index

 Pour plus d’informations sur la création d’index, consultez la section « Notes » dans [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations
L'utilisateur doit disposer de l'autorisation `ALTER` sur la table ou l’affichage ou être membre du rôle de serveur fixe sysadmin ou des rôles de base de données fixes `db_ddladmin` et `db_owner`.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-spatial-index-on-a-geometry-column"></a>R. Création d'un index spatial sur une colonne de type geometry
L’exemple suivant crée une table nommée `SpatialTable` qui contient une colonne de type **geometry**, `geometry_col`. L'exemple crée ensuite un index spatial, `SIndx_SpatialTable_geometry_col1`, sur la table `geometry_col`. L'exemple utilise le schéma de pavage par défaut et spécifie le rectangle englobant.  
  
```sql  
CREATE TABLE SpatialTable(id int primary key, geometry_col geometry);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col1
   ON SpatialTable(geometry_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ) );  
```  
  
### <a name="b-creating-a-spatial-index-on-a-geometry-column"></a>B. Création d'un index spatial sur une colonne de type geometry
L'exemple suivant crée un deuxième index spatial, `SIndx_SpatialTable_geometry_col2`, sur `geometry_col` dans la table `SpatialTable`. L'exemple spécifie `GEOMETRY_GRID` comme schéma de pavage. L'exemple spécifie également le rectangle englobant, des densités différentes sur des niveaux de grille différents et 64 cellules par objet. L'exemple attribue aussi la valeur `ON` au remplissage d'index.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col2  
   ON SpatialTable(geometry_col)  
   USING GEOMETRY_GRID  
   WITH (  
    BOUNDING_BOX = ( xmin=0, ymin=0, xmax=500, ymax=200 ),  
    GRIDS = (LOW, LOW, MEDIUM, HIGH),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="c-creating-a-spatial-index-on-a-geometry-column"></a>C. Création d'un index spatial sur une colonne de type geometry
L'exemple suivant crée un troisième index spatial, `SIndx_SpatialTable_geometry_col3`, sur `geometry_col` dans la table `SpatialTable`. L'exemple utilise le schéma de pavage par défaut. L'exemple spécifie le rectangle englobant et utilise des densités de cellule différentes sur les troisième et quatrième niveaux, tout en utilisant le nombre par défaut de cellules par objet.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geometry_col3  
   ON SpatialTable(geometry_col)  
   WITH (  
    BOUNDING_BOX = ( 0, 0, 500, 200 ),  
    GRIDS = ( LEVEL_4 = HIGH, LEVEL_3 = MEDIUM ) );  
```  
  
### <a name="d-changing-an-option-that-is-specific-to-spatial-indexes"></a>D. Modification d'une option qui est spécifique aux index spatiaux
L'exemple suivant reconstruit l'index spatial créé dans l'exemple précédent, `SIndx_SpatialTable_geography_col3`, en spécifiant une nouvelle densité `LEVEL_3` avec DROP_EXISTING = ON.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable(geography_col)  
   WITH ( BOUNDING_BOX = ( 0, 0, 500, 200 ),  
        GRIDS = ( LEVEL_3 = LOW ),  
        DROP_EXISTING = ON );  
```  
  
### <a name="e-creating-a-spatial-index-on-a-geography-column"></a>E. Création d'un index spatial sur une colonne de type geography
L’exemple suivant crée une table nommée `SpatialTable2` qui contient une colonne de type **geography**, `geography_col`. L'exemple crée ensuite un index spatial, `SIndx_SpatialTable_geography_col1`, sur la table `geography_col`. L'exemple utilise les valeurs des paramètres par défaut du schéma de pavage GEOGRAPHY_AUTO_GRID.  
  
```sql  
CREATE TABLE SpatialTable2(id int primary key, object GEOGRAPHY);  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col1
   ON SpatialTable2(object);  
```  
  
> [!NOTE]  
> Pour les index de grille géographique, un rectangle englobant ne peut pas être spécifié.
  
### <a name="f-creating-a-spatial-index-on-a-geography-column"></a>F. Création d'un index spatial sur une colonne de type geography
L'exemple suivant crée un deuxième index spatial, `SIndx_SpatialTable_geography_col2`, sur `geography_col` dans la table `SpatialTable2`. L'exemple spécifie `GEOGRAPHY_GRID` comme schéma de pavage. L'exemple spécifie également des densités de grille différentes sur des niveaux différents et 64 cellules par objet. L'exemple attribue aussi la valeur `ON` au remplissage d'index.  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col2  
   ON SpatialTable2(object)  
   USING GEOGRAPHY_GRID  
   WITH (  
    GRIDS = (MEDIUM, LOW, MEDIUM, HIGH ),  
    CELLS_PER_OBJECT = 64,  
    PAD_INDEX  = ON );  
```  
  
### <a name="g-creating-a-spatial-index-on-a-geography-column"></a>G. Création d'un index spatial sur une colonne de type geography
L'exemple crée ensuite un troisième index spatial, `SIndx_SpatialTable_geography_col3`, sur `geography_col` dans la table `SpatialTable2`. L'exemple utilise le schéma de pavage par défaut, GEOGRAPHY_GRID et la valeur CELLS_PER_OBJECT par défaut (16).  
  
```sql  
CREATE SPATIAL INDEX SIndx_SpatialTable_geography_col3  
   ON SpatialTable2(object)  
   WITH ( GRIDS = ( LEVEL_3 = HIGH, LEVEL_2 = HIGH ) );  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)       
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)       
[CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)       
[CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)       
[CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)       
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)       
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)       
[DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)       
[DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)       
[EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)       
[sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)       
[sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)       
[sys.spatial_index_tessellations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md)       
[sys.spatial_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-spatial-indexes-transact-sql.md)       
[Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)       
