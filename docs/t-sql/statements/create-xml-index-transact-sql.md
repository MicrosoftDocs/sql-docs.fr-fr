---
title: CREATE XML INDEX (Transact-SQL)
description: CREATE XML INDEX (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_XML_INDEX_TSQL
- XML INDEX
- CREATE_XML_TSQL
- XML
- CREATE XML
- CREATE XML INDEX
- XML_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- CREATE INDEX statement
- index creation [SQL Server], XML indexes
- XML indexes [SQL Server], creating
ms.assetid: c510cfbc-68be-4736-b3cc-dc5b7aa51f14
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
ms.custom: ''
ms.date: 08/10/2017
ms.openlocfilehash: 03295249061e1ed6210c8dfbc086d0326137ad26
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082233"
---
# <a name="create-xml-index-transact-sql"></a>CREATE XML INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée un index XML sur une table spécifiée. Un index peut être créé avant que la table soit remplie de données. Vous pouvez créer des index XML sur des tables dans une autre base de données en spécifiant un nom de base de données qualifié.  
  
> [!NOTE]  
>  Pour créer un index relationnel, consultez [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md). Pour plus d'informations sur la création d’un index spatial, consultez [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
--Create XML Index   
CREATE [ PRIMARY ] XML INDEX index_name   
    ON <object> ( xml_column_name )  
    [ USING XML INDEX xml_index_name   
        [ FOR { VALUE | PATH | PROPERTY } ] ]  
    [ WITH ( <xml_index_option> [ ,...n ] ) ]  
[ ; ]  
  
<object> ::=  
{ database_name.schema_name.table_name | schema_name.table_name | table_name }
  
<xml_index_option> ::=  
{   
    PAD_INDEX  = { ON | OFF }  
  | FILLFACTOR = fillfactor  
  | SORT_IN_TEMPDB = { ON | OFF }  
  | IGNORE_DUP_KEY = OFF  
  | DROP_EXISTING = { ON | OFF }  
  | ONLINE = OFF  
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS = { ON | OFF }  
  | MAXDOP = max_degree_of_parallelism  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 [PRIMARY] XML  
 Crée un index XML dans la colonne **xml** spécifiée. Lorsque PRIMARY est spécifié, un index cluster est créé avec la clé cluster créée à partir de la clé de clustering de la table utilisateur et d'un identificateur de nœud XML. Chaque table peut comporter jusqu'à 249 index XML. Tenez compte des points suivants lorsque vous créez un index XML :  
  
-   Un index cluster doit exister dans la clé primaire de la table utilisateur.  
  
-   La clé de clustering de la table utilisateur est limitée à 15 colonnes.  
  
-   Chaque colonne **xml** d’une table peut avoir un index XML primaire et plusieurs index XML secondaires.  
  
-   Un index XML primaire sur une colonne **xml** doit exister pour pouvoir créer un index XML secondaire sur la colonne.  
  
-   Un index XML peut être créé uniquement sur une seule colonne **xml**. Vous ne pouvez pas créer d’index XML sur une colonne non-**xml**, ni créer d’index relationnel sur une colonne **xml**.  
  
-   Vous ne pouvez pas créer d’index XML primaire ou secondaire sur une colonne **xml** d’une vue, sur une variable table avec des colonnes **xml** ou des variables de type **xml**.  
  
-   Vous ne pouvez pas créer d’index XML primaire sur une colonne **xml** calculée.  
  
-   Les valeurs de l'option SET doivent être identiques à celles requises pour les vues indexées et les index de colonne calculée. En particulier, l’option ARITHABORT doit être activée (ON) quand un index XML est créé et pendant l’insertion, la suppression ou la mise à jour des valeurs dans la colonne **xml**.  
  
 Pour plus d’informations, consultez [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
 *index_name*  
 Nom de l'index. Les noms d'index doivent être uniques dans une table, mais ne doivent pas être nécessairement uniques dans une base de données. Les noms d’index doivent se conformer aux règles régissant les [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 Les noms d’index XML primaires ne peuvent pas commencer par les caractères suivants : **#** , **##** , **@** ou **@@** .  
  
 *xml_column_name*  
 Colonne **xml** sur laquelle l’index est basé. Une seule colonne **xml** peut être spécifiée dans une même définition d’index XML. Toutefois, vous pouvez créer plusieurs index XML secondaires sur une colonne **xml**.  
  
 USING XML INDEX *xml_index_name*  
 Spécifie l'index XML primaire à utiliser pour créer un index XML secondaire.  
  
 FOR { VALUE | PATH | PROPERTY }  
 Spécifie le type d'index XML secondaire.  
  
 VALEUR  
 Crée un index XML secondaire sur les colonnes dans lesquelles les colonnes de clé (valeur et chemin de nœud) proviennent de l'index XML primaire.  
  
 PATH  
 Crée un index XML secondaire sur les colonnes créées sur des valeurs de chemin et des valeurs de nœud dans l'index XML primaire. Dans l'index secondaire PATH, les valeurs de chemin et de nœud sont des colonnes clés qui permettent de rechercher efficacement des chemins.  
  
 PROPERTY  
 Crée un index XML secondaire sur les colonnes (valeur PK, de chemin et de nœud) de l'index XML primaire où PK est la clé primaire de la table de base.  
  
 **\<object>::=**  
  
 Objet qualifié complet ou partiel à indexer.  
  
 *database_name*  
 Nom de la base de données.  
  
 *schema_name*  
 Nom du schéma auquel appartient la table.  
  
 *table_name*  
 Nom de la table à indexer.  
  
 **\<xml_index_option> ::=** 
  
 Spécifie les options à utiliser lorsque vous créez l'index.  
  
 PAD_INDEX **=** { ON | **OFF** }  
 Spécifie le remplissage de l'index. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Le pourcentage d’espace libre indiqué par *fillfactor* est appliqué aux pages de niveau intermédiaire de l’index.  
  
 OFF ou *fillfactor* n’est pas spécifié  
 Les pages de niveau intermédiaire sont presque entièrement remplies, ce qui laisse suffisamment d'espace libre pour au moins une ligne de la taille maximale permise par l'index, en prenant en compte l'ensemble de clés sur les pages intermédiaires.  
  
 L'option PAD_INDEX est utile seulement si FILLFACTOR est spécifié, car PAD_INDEX utilise le pourcentage spécifié par FILLFACTOR. Si le pourcentage défini pour FILLFACTOR n'est pas suffisamment élevé pour autoriser une ligne, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] remplace en interne le pourcentage de façon à ce qu'il autorise le minimum. Le nombre de lignes dans une page d’index intermédiaire n’est jamais inférieur à deux, quelle que soit la faiblesse de la valeur de *fillfactor*.  
  
 FILLFACTOR **=** _fillfactor_  
 Spécifie un pourcentage indiquant le taux de remplissage appliqué par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] au niveau feuille de chaque page d'index lors de la création ou de la reconstruction de l'index. *fillfactor* doit être une valeur entière comprise entre 1 et 100. La valeur par défaut est 0. Si *fillfactor* a pour valeur 100 ou 0, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] crée des index avec des pages de niveau feuille intégralement remplies.  
  
> [!NOTE]  
>  Les taux de remplissage 0 et 100 sont identiques en tous points.  
  
 La valeur FILLFACTOR s'applique uniquement lors de la création ou de la reconstruction de l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne conserve pas dynamiquement dans les pages le pourcentage d'espace libre défini. Pour afficher le facteur de remplissage, utilisez la vue de catalogue [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).  
  
> [!IMPORTANT]  
>  La création d'un index cluster avec un facteur de remplissage FILLFACTOR inférieur à 100 affecte la quantité d'espace de stockage qu'occupent les données, car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] redistribue les données lorsqu'il crée l'index cluster.  
  
 Pour plus d’informations, consultez [Spécifier un facteur de remplissage pour un index](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).  
  
 SORT_IN_TEMPDB **=** { ON | **OFF** }  
 Spécifie s’il faut stocker les résultats temporaires du tri dans **tempdb**. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 Les résultats intermédiaires du tri utilisés pour créer l’index sont stockés dans **tempdb**. Cela peut réduire le temps nécessaire pour créer un index si **tempdb** ne se trouve pas sur le même groupe de disques que la base de données utilisateur. Toutefois, une plus grande quantité d'espace disque est alors utilisée lors de la création de l'index.  
  
 OFF  
 Les résultats de tri intermédiaires sont stockés dans la même base de données que l'index.  
  
 En plus de l’espace nécessaire dans la base de données utilisateur pour créer l’index, **tempdb** doit disposer à peu près d’autant d’espace supplémentaire pour stocker les résultats intermédiaires du tri. Pour plus d’informations, consultez [Option SORT_IN_TEMPDB pour les index](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).  
  
 IGNORE_DUP_KEY **=OFF**  
 N'a aucun effet pour les index XML, car le type d'index n'est jamais unique. N'activez pas cette option (ON), sinon une erreur est déclenchée.  
  
 DROP_EXISTING **=** { ON | **OFF** }  
 Spécifie que l'index XML nommé préexistant est supprimé et reconstruit. La valeur par défaut est OFF.  
  
 ACTIVÉ  
 L'index existant est supprimé et recréé. Le nom d'index défini doit être identique à celui de l'index existant. Toutefois, la définition de l'index peut être modifiée. Par exemple, vous pouvez définir des colonnes, un ordre de tri, un schéma de partition ou des options d'indexation différentes.  
  
 OFF  
 Une erreur s'affiche si le nom d'index spécifié existe déjà.  
  
 Le type d'index ne peut pas être modifié à l'aide de DROP_EXISTING. En outre, un index primaire XML ne peut pas être redéfini comme index XML secondaire, et inversement.  
  
 ONLINE **=OFF**  
 Indique que les tables sous-jacentes et les index associés ne sont pas disponibles pour les requêtes et la modification de données pendant l'opération d'index. Dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les constructions d'index en ligne ne sont pas prises en charge pour les index XML. Si cette option est activée (ON) pour un index XML, une erreur est déclenchée. Omettez l'option ONLINE ou attribuez la valeur OFF à ONLINE.  
  
 Une opération d'index hors connexion qui crée, reconstruit ou supprime un index XML acquiert un verrou Sch-M (Modification du schéma) sur la table. Cela empêche tous les utilisateurs d'accéder à la table sous-jacente pendant la durée de l'opération.  
  
> [!NOTE]
>  Les opérations d’index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
 ALLOW_ROW_LOCKS **=** { **ON** | OFF }  
 Indique si les verrous de ligne sont autorisés ou non. La valeur par défaut est ON.  
  
 ACTIVÉ  
 Les verrous de ligne sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de ligne sont utilisés.  
  
 OFF  
 Les verrous de ligne ne sont pas utilisés.  
  
 ALLOW_PAGE_LOCKS **=** { **ON** | OFF }  
 Indique si les verrous de page sont autorisés. La valeur par défaut est ON.  
  
 ACTIVÉ  
 Les verrous de page sont autorisés lors de l'accès à l'index. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détermine le moment où les verrous de page sont utilisés.  
  
 OFF  
 Les verrous de page ne sont pas utilisés.  
  
 MAXDOP **=** _max_degree_of_parallelism_  
 Remplace l’option de configuration [Configurer l’option de configuration du serveur Degré maximal de parallélisme](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) pendant la durée de l’opération d’index. Utilisez MAXDOP pour limiter le nombre de processeurs utilisés dans une exécution de plan parallèle. Le nombre maximal de processeurs est égal à 64.  
  
> [!IMPORTANT]  
>  Bien que l'option MAXDOP soit prise en charge syntaxiquement pour tous les index XML, pour un index XML primaire, CREATE XML INDEX utilise uniquement un processeur unique.  
  
 *max_degree_of_parallelism* peut avoir la valeur :  
  
 1  
 Supprime la création de plans parallèles.  
  
 \>1  
 Limite le nombre maximal de processeurs utilisés dans l'indexation parallèle au nombre défini ou à un nombre inférieur en fonction de la charge de travail actuelle du système.  
  
 0 (valeur par défaut)  
 Utilise le nombre réel de processeurs ou un nombre de processeurs inférieur en fonction de la charge de travail actuelle du système.  
  
 Pour plus d’informations, consultez [Configurer des opérations d’index parallèles](../../relational-databases/indexes/configure-parallel-index-operations.md).  
  
> [!NOTE]
>  Les opérations d’index parallèles ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).  
  
## <a name="remarks"></a>Notes  
 Les colonnes calculées dérivées des types de données **xml** peuvent être indexées comme des colonnes clés ou non-clés incluses dès lors que les types de données des colonnes calculées peuvent être utilisés comme des colonnes clés ou des colonnes non-clés d’index. Vous ne pouvez pas créer d’index XML primaire sur une colonne **xml** calculée.  
  
 Pour consulter des informations sur les index XML, utilisez la vue de catalogue [sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md).  
  
 Pour plus d’informations sur les index XML, consultez [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).  
  
## <a name="additional-remarks-on-index-creation"></a>Remarques supplémentaires sur la création d'index  
 Pour plus d’informations sur la création d’index, consultez la section « Notes » dans [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-primary-xml-index"></a>R. Création d'un index XML primaire  
 L'exemple suivant crée un index XML primaire sur la colonne `CatalogDescription` de la table `Production.ProductModel`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT * FROM sys.indexes  
            WHERE name = N'PXML_ProductModel_CatalogDescription')  
    DROP INDEX PXML_ProductModel_CatalogDescription   
        ON Production.ProductModel;  
GO  
CREATE PRIMARY XML INDEX PXML_ProductModel_CatalogDescription  
    ON Production.ProductModel (CatalogDescription);  
GO  
```  
  
### <a name="b-creating-a-secondary-xml-index"></a>B. Création d’un index XML secondaire  
 L'exemple suivant crée un index XML secondaire sur la colonne `CatalogDescription` de la table `Production.ProductModel`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.indexes  
            WHERE name = N'IXML_ProductModel_CatalogDescription_Path')  
    DROP INDEX IXML_ProductModel_CatalogDescription_Path  
        ON Production.ProductModel;  
GO  
CREATE XML INDEX IXML_ProductModel_CatalogDescription_Path   
    ON Production.ProductModel (CatalogDescription)  
    USING XML INDEX PXML_ProductModel_CatalogDescription FOR PATH ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [CREATE PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-function-transact-sql.md)   
 [CREATE PARTITION SCHEME &#40;Transact-SQL&#41;](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)   
 [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [sys.xml_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)  
  
