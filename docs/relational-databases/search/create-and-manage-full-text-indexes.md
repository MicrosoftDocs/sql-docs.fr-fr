---
description: Créer et gérer des index de recherche en texte intégral
title: Créer et gérer des index de recherche en texte intégral | Microsoft Docs
ms.custom: ''
ms.date: 03/31/2020
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text indexes [SQL Server], about
ms.assetid: f8a98486-5438-44a8-b454-9e6ecbc74f83
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8a3c3792fa2aca009e02fc8fcdce6bf613b5e370
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341680"
---
# <a name="create-and-manage-full-text-indexes"></a>Créer et gérer des index de recherche en texte intégral
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Cette rubrique décrit comment créer, remplir et gérer des index de recherche en texte intégral dans SQL Server.
  
## <a name="prerequisite---create-a-full-text-catalog"></a>Prérequis - Créer un catalogue de texte intégral
Avant de pouvoir créer un index de recherche en texte intégral, vous devez disposer d’un catalogue de texte intégral. Le catalogue est un conteneur virtuel pour un ou plusieurs index de recherche en texte intégral. Pour plus d’informations, consultez [Créer et gérer des catalogues de texte intégral](../../relational-databases/search/create-and-manage-full-text-catalogs.md).
  
##  <a name="create-alter-or-drop-a-full-text-index"></a><a name="tasks"></a> Créer, modifier ou supprimer un index de recherche en texte intégral  
### <a name="create-a-full-text-index"></a>Créer un index de recherche en texte intégral  
  
-   [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)  
  
### <a name="alter-a-full-text-index"></a>Modifier un index de recherche en texte intégral
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
### <a name="drop-a-full-text-index"></a>Supprimer un index de recherche en texte intégral 
  
-   [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)

## <a name="populate-a-full-text-index"></a>Remplir un index de recherche en texte intégral
Le processus de création et de gestion d’un index de recherche en texte intégral est appelé *alimentation* (également appelé *analyse*). Il existe trois types de remplissage d’index de recherche en texte intégral :
-   Alimentation complète
-   Alimentation basée sur le suivi des modifications
-   Remplissage incrémentiel basé sur un horodatage.

Pour plus d’informations, consultez [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).

##  <a name="view-the-properties-of-a-full-text-index"></a><a name="view"></a> Afficher les propriétés d’un index de recherche en texte intégral
### <a name="view-the-properties-of-a-full-text-index-with-transact-sql"></a>Afficher les propriétés d’un index de recherche en texte intégral avec Transact-SQL

|Vue catalogue ou vue de gestion dynamique|Description|  
|----------------------------------------|-----------------|  
|[sys.fulltext_index_catalog_usages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)|Retourne une ligne pour chaque catalogue de texte intégral vers une référence d'index de recherche en texte intégral.|  
|[sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)|Contient une ligne pour chaque colonne qui fait partie d'un index de recherche en texte intégral.|  
|[sys.fulltext_index_fragments &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)|Un index de recherche en texte intégral utilise des tables internes appelées fragments d'index de recherche en texte intégral pour stocker les données d'index inversées. Cette vue permet d'interroger les métadonnées relatives à ces fragments. Cette vue contient une ligne pour chaque fragment d'index de recherche en texte intégral dans chaque table qui contient un index.|  
|[sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)|Contient une ligne par index de recherche en texte intégral d'un objet tabulaire.|  
|[sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)|Retourne des informations sur le contenu d'un index de recherche en texte intégral pour la table spécifiée.|  
|[sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)|Retourne des informations sur le contenu de niveau document d'un index de recherche en texte intégral pour la table spécifiée. Un mot clé donné peut apparaître dans plusieurs documents.|  
|[sys.dm_fts_index_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)|Retourne des informations sur les remplissages d'index de texte intégral actuellement en cours.|  
 
### <a name="view-the-properties-of-a-full-text-index-with-management-studio"></a>Afficher les propriétés d’un index de recherche en texte intégral avec Management Studio 
1.  Dans Management Studio, dans l’Explorateur d’objets, développez le serveur.  
  
2.  Développez **Bases de données**, puis la base de données qui contient l’index de recherche en texte intégral.  
  
3.  Développez **Tables**.  
  
4.  Cliquez avec le bouton droit sur la table sur laquelle l’index de recherche en texte intégral est défini, sélectionnez **Index de recherche en texte intégral** et, dans le menu contextuel **Index de recherche en texte intégral** , cliquez sur **Propriétés**. La boîte de dialogue **Propriétés d’index de recherche en texte intégral** s’affiche.  
  
5.  Dans le volet **Sélectionner une page** , vous pouvez sélectionner l’une des pages suivantes :  
  
    |Page|Description|  
    |----------|-----------------|  
    |**Généralités**|Affiche les propriétés de base de l'index de recherche en texte intégral. Il s'agit de plusieurs propriétés modifiables et non modifiables telles que le nom de la base de données, le nom de la table et le nom de la colonne clé de recherche en texte intégral. Les propriétés modifiables sont les suivantes :<br /><br /> **Liste de mots vides de l’index de recherche en texte intégral**<br /><br /> **Indexation de texte intégral activée**<br /><br /> **Suivi des modifications**<br /><br /> **Liste de propriétés de recherche**|  
    |**Colonnes**|Affiche les colonnes de table qui sont disponibles pour l'indexation de texte intégral. La ou les colonnes sélectionnées sont indexées en texte intégral. Vous pouvez sélectionner autant de colonnes disponibles que vous souhaitez inclure dans l'index de recherche en texte intégral. Pour plus d’informations, consultez [Alimenter des index de recherche en texte intégral](populate-full-text-indexes.md).|
    |**Planifications**|Utilisez cette page afin de créer ou gérer des planifications pour un travail de l'Agent SQL Server qui démarre un remplissage incrémentiel de la table pour remplir l'index de recherche en texte intégral. Pour plus d’informations, consultez [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).<br /><br /> Remarque : Une fois que vous avez fermé la boîte de dialogue **Propriétés d’index de recherche en texte intégral**, la planification que vous venez de créer est associée à un travail de SQL Server Agent (démarrer le remplissage incrémentiel de la table sur *database_name*.*table_name*).|  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)] pour enregistrer vos modifications et fermer la boîte de dialogue **Propriétés d’index de recherche en texte intégral**.  
  
##  <a name="view-the-properties-of-indexed-tables-and-columns"></a><a name="props"></a> Afficher les propriétés des colonnes et tables indexées  
 Vous pouvez faire appel à plusieurs fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)], comme OBJECTPROPERTYEX, pour vous procurer la valeur de diverses propriétés d'indexation de texte intégral. Ces informations sont utiles pour administrer la recherche en texte intégral et résoudre les problèmes qui la concernent.  
  
 Le tableau ci-après recense les propriétés en texte intégral liées aux colonnes et tables indexées, ainsi que les fonctions [!INCLUDE[tsql](../../includes/tsql-md.md)] qui leur sont associées.  
  
|Propriété|Description|Fonction|  
|--------------|-----------------|--------------|  
|**FullTextTypeColumn**|TYPE COLUMN de la table qui contient les informations sur le type de document de la colonne.|[COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md)|  
|**IsFulltextIndexed**|Indique si une colonne a été activée pour l'indexation de texte intégral.|COLUMNPROPERTY|  
|**IsFulltextKey**|Indique si l'index représente la clé de texte intégral d'une table.|[INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md)|  
|**TableFulltextBackgroundUpdateIndexOn**|Indique si une table possède une indexation de mise à jour d'arrière-plan de texte intégral.|[OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md)|  
|**TableFulltextCatalogId**|ID du catalogue de texte intégral dans lequel résident les données d'indexation de texte intégral de la table.|OBJECTPROPERTYEX|  
|**TableFulltextChangeTrackingOn**|Indique si le suivi des modifications de texte intégral est activé pour la table.|OBJECTPROPERTYEX|  
|**TableFulltextDocsProcessed**|Nombre de lignes traitées depuis le démarrage de l'indexation de texte intégral.|OBJECTPROPERTYEX|  
|**TableFulltextFailCount**|Nombre de lignes que la recherche en texte intégral n'a pas indexées.|OBJECTPROPERTYEX|  
|**TableFulltextItemCount**|Nombre de lignes dont l'indexation de texte intégral a réussi.|OBJECTPROPERTYEX|  
|**TableFulltextKeyColumn**|ID de la colonne clé unique de texte intégral.|OBJECTPROPERTYEX|  
|**TableFullTextMergeStatus**|Indique s'il s'agit d'une table qui a un index de recherche en texte intégral qui est en cours de fusion.|OBJECTPROPERTYEX|  
|**TableFulltextPendingChanges**|Nombre d'entrées de suivi des modifications en attente de traitement.|OBJECTPROPERTYEX|  
|**TableFulltextPopulateStatus**|État de remplissage de la table de texte intégral.|OBJECTPROPERTYEX|  
|**TableHasActiveFulltextIndex**|Indique si une table possède un index de recherche en texte intégral actif.|OBJECTPROPERTYEX|  
  
##  <a name="get-info-about-the-full-text-key-column"></a><a name="key"></a> Obtenir des informations sur la colonne de clés de texte intégral  
 En général, le résultat des fonctions d'ensemble de lignes CONTAINSTABLE ou FREETEXTTABLE doit être joint avec la table de base. Dans ce cas-là, vous devez connaître le nom de la colonne clé unique. Vous pouvez déterminer si un index unique donné est utilisé comme clé de texte intégral et obtenir l'identificateur de la colonne clés de texte intégral.  
  
### <a name="determine-whether-a-given-unique-index-is-used-as-the-full-text-key-column"></a>Déterminer si un index unique donné est utilisé comme colonne de clés de texte intégral  
  
Utilisez une instruction [SELECT](../../t-sql/queries/select-transact-sql.md) pour appeler la fonction [INDEXPROPERTY](../../t-sql/functions/indexproperty-transact-sql.md). Lors de l’appel de fonction, utilisez la fonction OBJECT_ID pour convertir le nom de la table (*nom_table*) en ID de table, spécifiez le nom d’un index unique pour la table et indiquez la propriété d’index **IsFulltextKey** , comme suit :  
  
```sql  
SELECT INDEXPROPERTY( OBJECT_ID('table_name'), 'index_name',  'IsFulltextKey' );  
```  
  
 L'instruction retourne la valeur 1 si l'index est utilisé pour garantir l'unicité de la colonne clé de texte intégral, et la valeur 0 dans le cas contraire.  
  
 **Exemple**  
  
 L'exemple suivant permet de déterminer si l'index `PK_Document_DocumentNode` est utilisé pour garantir l'unicité de la colonne clé de texte intégral, comme suit :  
  
```sql  
USE AdventureWorks  
GO  
SELECT INDEXPROPERTY ( OBJECT_ID('Production.Document'), 'PK_Document_DocumentNode',  'IsFulltextKey' )  
```  
  
 Cet exemple retourne la valeur 1 si l'index `PK_Document_DocumentNode` est utilisé pour garantir l'unicité de la colonne clés de texte intégral. Si tel n'est pas le cas, la valeur 0 ou une valeur Null est retournée. La valeur Null signifie que vous utilisez un nom d'index non valide, que le nom d'index ne correspond pas à la table, que la table n'existe pas, etc.  
  
### <a name="find-the-identifier-of-the-full-text-key-column"></a>Rechercher l’identificateur de la colonne de clés de texte intégral  
  
Chaque table activée pour la recherche en texte intégral comporte une colonne qui est utilisée pour garantir l’unicité des lignes de la table (*colonne de clés**unique*). La propriété **TableFulltextKeyColumn**, obtenue à l’aide de la fonction OBJECTPROPERTYEX, contient l’ID de la colonne clé unique.  
 
Pour obtenir cet identificateur, vous pouvez utiliser une instruction SELECT afin d'appeler la fonction OBJECTPROPERTYEX. Utilisez la fonction OBJECT_ID pour convertir le nom de la table (*nom_table*) en ID de table et spécifiez la propriété **TableFulltextKeyColumn** , comme suit :  
  
```sql  
SELECT OBJECTPROPERTYEX(OBJECT_ID( 'table_name'), 'TableFulltextKeyColumn' ) AS 'Column Identifier';  
```  
  
 **Exemples**  
  
 L'exemple ci-après retourne l'identificateur de la colonne clé de texte intégral ou une valeur Null. La valeur Null signifie que vous utilisez un nom d'index non valide, que le nom d'index ne correspond pas à la table, que la table n'existe pas, etc.  
  
```sql
USE AdventureWorks;  
GO  
SELECT OBJECTPROPERTYEX(OBJECT_ID('Production.Document'), 'TableFulltextKeyColumn');  
GO  
```  
  
 L'exemple ci-après explique comment utiliser l'identificateur de la colonne clé unique pour obtenir le nom de la colonne.  
  
```sql  
USE AdventureWorks;  
GO  
DECLARE @key_column sysname  
SET @key_column = Col_Name(Object_Id('Production.Document'),  
ObjectProperty(Object_id('Production.Document'),  
'TableFulltextKeyColumn')   
)  
SELECT @key_column AS 'Unique Key Column';  
GO  
```  
  
 Cet exemple retourne une colonne de jeu de résultats appelée `Unique Key Column`, qui contient une seule ligne indiquant le nom de la colonne clé unique de la table Document, DocumentNode. Notez que, si cette requête contenait un nom d'index non valide, si le nom d'index ne correspondait pas à la table, si la table n'existait pas, etc., une valeur NULL serait retournée.  

## <a name="index-varbinarymax-and-xml-columns"></a>Colonnes varbinary(max) et xml d’index  
 Si une colonne **varbinary(max)** , **varbinary** ou **xml** est indexée en texte intégral, elle peut faire l’objet d’une requête à l’aide des prédicats de texte intégral (CONTAINS et FREETEXT) et des fonctions de texte intégral (CONTAINSTABLE et FREETEXTTABLE), au même titre que n’importe quelle autre colonne indexée en texte intégral.
   
### <a name="index-varbinarymax-or-varbinary-data"></a>Données varbinary(max) ou varbinary d’index  
 Une seule colonne **varbinary(max)** ou **varbinary** peut stocker de nombreux types de documents. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge tout type de document pour lequel un filtre est installé et disponible dans le système d'exploitation. Le type d'un document est identifié par l'extension de fichier de celui-ci. Par exemple, pour une extension de fichier .doc, la recherche en texte intégral utilise le filtre qui prend en charge les documents Microsoft Word. Pour obtenir la liste des types de documents disponibles, interrogez l’affichage catalogue [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) .  
  
Notez que le Moteur d'indexation et de recherche en texte intégral peut bénéficier des filtres installés dans le système d'exploitation. Avant de pouvoir utiliser des filtres de système d'exploitation, des analyseurs lexicaux et des générateurs de formes dérivées, vous devez les charger dans l'instance de serveur, comme suit :  
  
```sql  
EXEC sp_fulltext_service @action='load_os_resources', @value=1  
```  
  
Pour créer un index de recherche en texte intégral sur une colonne **varbinary(max)** , le moteur d’indexation et de recherche en texte intégral a besoin d’accéder aux extensions de fichier des documents dans la colonne **varbinary(max)** . Ces informations doivent être stockées dans une colonne de table, appelée colonne de type, qui doit être associée à la colonne **varbinary(max)** dans l’index de recherche en texte intégral. Lors de l'indexation d'un document, le Moteur d'indexation et de recherche en texte intégral utilise l'extension de fichier indiquée dans la colonne de type pour identifier le filtre à utiliser.  
   
### <a name="index-xml-data"></a>Données xml d’index  
 Une colonne de type de données **xml** stocke uniquement des documents et des fragments XML, et seul le filtre XML est utilisé pour les documents. Par conséquent, une colonne de type est inutile. Sur les colonnes **xml** , l’index de recherche en texte intégral indexe le contenu des éléments XML, sans prendre en compte les balises XML. Les valeurs d'attributs sont indexées en texte intégral, à moins qu'il ne s'agisse de valeurs numériques. Des balises d'éléments sont utilisées comme limites de jeton. Les fragments et les documents XML ou HTML correctement formés contenant plusieurs langues sont pris en charge.  
  
 Pour plus d’informations sur l’indexation et l’interrogation sur une colonne **xml**, consultez [Utiliser la recherche en texte intégral avec des colonnes XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
##  <a name="disable-or-re-enable-full-text-indexing-for-a-table"></a><a name="disable"></a> Désactiver ou réactiver l’indexation de texte intégral pour une table   
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par défaut, toutes les bases de données créées par les utilisateurs sont activées pour la recherche en texte intégral. De plus, une table individuelle est automatiquement activée pour l'indexation de texte intégral dès qu'un index de recherche en texte intégral est créé sur cette table et qu'une colonne est ajoutée à l'index. Une table est automatiquement désactivée pour l'indexation de recherche en texte intégral lorsque la dernière colonne est supprimée de son index de texte intégral.  
  
 Dans une table à index de recherche en texte intégral, vous pouvez désactiver ou réactiver manuellement une table pour l'indexation de recherche en texte intégral en utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

1.  Développez le groupe de serveurs, **Bases de données**, puis la base de données qui contient la table à activer pour l’indexation de texte intégral.  
  
2.  Développez **Tables** et cliquez avec le bouton droit sur la table que vous souhaitez désactiver ou réactiver pour l’indexation de texte intégral.  
  
3.  Sélectionnez **Index de recherche en texte intégral**, puis cliquez sur **Disable Full-Text index** (Désactiver l’index de recherche en texte intégral) ou **Enable Full-Text index**(Activer l’index de recherche en texte intégral).  
  
##  <a name="remove-a-full-text-index-from-a-table"></a><a name="remove"></a> Supprimer un index de recherche en texte intégral d’une table  
  
1.  Dans l'Explorateur d'objets, cliquez avec le bouton droit sur la table dotée de l'index de recherche en texte intégral à supprimer.  
  
2.  Sélectionnez **Supprimer l’index de recherche en texte intégral**.  
  
3.  Quand le système vous y invite, cliquez sur **OK** pour confirmer la suppression de l’index de recherche en texte intégral.  
  
  
