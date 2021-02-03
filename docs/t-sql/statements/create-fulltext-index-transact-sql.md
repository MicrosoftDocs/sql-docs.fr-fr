---
description: CREATE FULLTEXT INDEX (Transact-SQL)
title: CREATE FULLTEXT INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FULLTEXT_INDEX_TSQL
- CREATE_FULLTEXT_INDEX_TSQL
- CREATE FULLTEXT INDEX
- FULLTEXT INDEX
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], creating
- index creation [SQL Server], CREATE FULLTEXT INDEX statement
- CREATE FULLTEXT INDEX statement
ms.assetid: 8b80390f-5f8b-4e66-9bcc-cabd653c19fd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 780f0f1471f0ce7a03a4aa800e4e98cc6a533dbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179179"
---
# <a name="create-fulltext-index-transact-sql"></a>CREATE FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crée un index de recherche en texte intégral sur une table ou une vue indexée dans une base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un seul index de recherche en texte intégral est autorisé par table ou vue indexée et chaque index de recherche en texte intégral s'applique à une seule table ou vue indexée. Un index de recherche en texte intégral peut contenir jusqu'à 1 024 colonnes.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE FULLTEXT INDEX ON table_name  
   [ ( { column_name   
             [ TYPE COLUMN type_column_name ]  
             [ LANGUAGE language_term ]   
             [ STATISTICAL_SEMANTICS ]  
        } [ ,...n]   
      ) ]  
    KEY INDEX index_name   
    [ ON <catalog_filegroup_option> ]  
    [ WITH [ ( ] <with_option> [ ,...n] [ ) ] ]  
[;]  
  
<catalog_filegroup_option>::=  
 {  
    fulltext_catalog_name   
 | ( fulltext_catalog_name, FILEGROUP filegroup_name )  
 | ( FILEGROUP filegroup_name, fulltext_catalog_name )  
 | ( FILEGROUP filegroup_name )  
 }  
  
<with_option>::=  
 {  
   CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF [, NO POPULATION ] }   
 | STOPLIST [ = ] { OFF | SYSTEM | stoplist_name }  
 | SEARCH PROPERTY LIST [ = ] property_list_name   
 }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
*table_name*       
Nom de la table ou de la vue indexée qui contient la ou les colonnes incluses dans l'index de recherche en texte intégral.  
  
*column_name*       
Nom de la colonne incluse dans l'index de recherche en texte intégral. Seules les colonnes de type **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml** et **varbinary(max)** peuvent être indexées pour la recherche en texte intégral. Pour spécifier plusieurs colonnes, répétez la clause *column_name* comme suit :  
  
CREATE FULLTEXT INDEX ON *table_name* (*column_name1* [...], *column_name2* [...]) ...  
  
TYPE COLUMN *type_column_name*       
Spécifie le nom d’une colonne de table, *type_column_name*, utilisée pour contenir le type de document d’un document **varbinary(max)** ou **image**. Cette colonne,appelée colonne de type, contient une extension de fichier fourni par l'utilisateur (.doc, .pdf, .xls, et ainsi de suite). La colonne doit être de type **char**, **nchar**, **varchar** ou **nvarchar**.  
  
Spécifiez TYPE COLUMN *type_column_name* seulement si *column_name* spécifie une colonne **varbinary(max)** ou **image**, dans laquelle les données sont stockées sous forme de données binaires ; dans le cas contraire, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur.  
  
> [!NOTE]  
> Au moment de l’indexation, le moteur d’indexation et de recherche en texte intégral utilise l’abréviation figurant dans la colonne de type de chaque ligne de la table pour identifier le filtre de recherche en texte intégral à utiliser pour le document dans *column_name*. Le filtre charge le document en tant que flux binaire, supprime les informations de mise en forme et envoie le texte du document vers le composant d'analyseur lexical de texte intégral. Pour plus d’informations, consultez [Configurer et gérer les extensions analytiques avancées](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
LANGUAGE *language_term*       
Langue des données stockées dans *column_name*.  
  
*language_term* est facultatif et peut être spécifié sous la forme d’une chaîne, d’un entier ou d’une valeur hexadécimale correspondant à l’identificateur de paramètres régionaux (LCID) d’une langue. Si vous ne spécifiez aucune valeur, la langue par défaut de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisée.  
  
Si la valeur *language_term* est spécifié, la langue qu’elle représente est utilisée pour indexer les données stockées dans les colonnes **char**, **nchar**, **varchar**, **nvarchar**, **text** et **ntext**. Cette langue est la langue par défaut utilisée au moment de la requête si *language_term* n’est pas spécifié comme élément d’un prédicat de texte intégral sur la colonne.  
  
Quand il est spécifié sous forme de chaîne, *language_term* correspond à la valeur de la colonne alias dans la table système syslanguages. La chaîne doit être placée entre guillemets simples, comme dans **'** _language\_term_ **'** . Quand il est spécifié sous la forme d’un entier, *language_term* est le LCID qui identifie la langue. Quand il est spécifié sous la forme d’une valeur hexadécimale, *language_term* est 0x suivi de la valeur hexadécimale du LCID. La valeur hexadécimale peut comporter un maximum de huit chiffres, zéros non significatifs inclus.  
  
Si la valeur est au format DBCS (jeu de caractères codés sur deux octets), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la convertit en Unicode.  
  
Les ressources, telles que les analyseurs lexicaux et les générateurs de formes dérivées, doivent être activées pour la langue spécifiée comme *language_term*. Si ces ressources ne prennent pas en charge la langue spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur.  
  
Utilisez la procédure stockée sp_configure pour accéder aux informations concernant la langue de texte intégral par défaut de l’instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
Pour les colonnes de type non-BLOB et non-XML contenant des données texte dans plusieurs langues, ou lorsque la langue du texte stocké dans la colonne est inconnue, vous pouvez envisager d'utiliser la ressource de langue neutre (0x0). Toutefois, vous devez d'abord comprendre les conséquences possibles de l'utilisation de la ressource de la langue neutre (0x0). Pour obtenir des informations sur les solutions possibles et les conséquences de l’utilisation de la ressource de langue neutre (0x0), consultez [Choisir une langue lors de la création d’un index de recherche en texte intégral](../../relational-databases/search/choose-a-language-when-creating-a-full-text-index.md).  
  
Pour les documents stockés dans des colonnes de type XML ou BLOB, l'encodage linguistique du document est utilisé lors de l'indexation. Par exemple, dans les colonnes XML, l’attribut **xml:lang** dans les documents XML identifie la langue. Au moment de la requête, la valeur précédemment spécifiée dans *language_term* devient la langue par défaut utilisée pour les requêtes de texte intégral, sauf si *language_term* est spécifié dans le cadre d’une requête de texte intégral.  
  
STATISTICAL_SEMANTICS       
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures) 
  
Crée les index de ressemblance de documents et d'expressions clés supplémentaires qui font partie de l'indexation sémantique statistique. Pour plus d’informations, consultez [Recherche sémantique &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
KEY INDEX *index_name*       
Nom de l’index de clé unique sur *table_name*. KEY INDEX doit être une colonne unique, de clé unique et ne pas accepter les valeurs NULL. Sélectionnez le plus petit index de clé unique comme clé unique de texte intégral.  Pour obtenir les meilleures performances, nous recommandons un type de données Integer pour la clé de texte intégral.  
  
*fulltext_catalog_name*       
Catalogue de texte intégral utilisé pour l'index de recherche en texte intégral. Le catalogue doit déjà exister dans la base de données. Cette clause est facultative. En l'absence de toute spécification, le catalogue par défaut est utilisé. S'il n'existe aucun catalogue par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur.  
  
FILEGROUP *filegroup_name*       
Crée l'index de recherche en texte intégral spécifié dans le groupe de fichiers spécifié. Le groupe de fichiers doit déjà exister. Si la clause FILEGROUP n'est pas spécifiée, l'index de recherche en texte intégral est placé dans le même groupe de fichiers comme table ou vue de base pour une table non partitionnée ou dans le groupe de fichiers principal pour une table partitionnée.  
  
 CHANGE_TRACKING [ = ] { MANUAL | **AUTO** | OFF [ , NO POPULATION ] }       
 Spécifie si les changements (mises à jour, suppressions ou insertions) apportées aux colonnes de table couverts par l’index de recherche en texte intégral seront propagés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à cet index. Les modifications apportées aux données via WRITETEXT et UPDATETEXT ne sont pas répercutées dans l'index de recherche en texte intégral et ne sont pas prises en compte par le suivi des modifications.  
  
MANUAL       
Indique que les modifications suivies doivent être propagées manuellement en appelant l’instruction ALTER FULLTEXT INDEX ... START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] (*alimentation manuelle*). Vous pouvez utiliser l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour appeler régulièrement cette instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
**AUTO**       
Indique que les modifications suivies seront propagées automatiquement à mesure que les données seront modifiées dans la table de base (*alimentation automatique*). Même si les modifications sont propagées automatiquement, elles n'apparaissent pas nécessairement immédiatement dans l'index de recherche en texte intégral. AUTO est la valeur par défaut.  
  
OFF [ `,` NO POPULATION]       
Spécifie que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne conserve aucune liste des modifications apportées aux données indexées. Si l'option NO POPULATION est spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] remplit l'index une fois qu'il est créé.  
  
L'option NO POPULATION peut être utilisée uniquement lorsque la valeur de CHANGE_TRACKING est OFF. Si l'option NO POPULATION est spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne remplit pas l'index une fois qu'il est créé. L'index n'est rempli qu'une fois que l'utilisateur a exécuté la commande ALTER FULLTEXT INDEX avec la clause START FULL POPULATION ou START INCREMENTAL POPULATION.  
  
STOPLIST [ = ] { OFF | **SYSTEM** | *stoplist_name* }       
Associe une liste de mots vides de texte intégral à l'index. L'index ne contient aucun jeton répertorié dans la liste de mots vides spécifiée. Si STOPLIST n'est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associe la liste de mots vides de texte intégral système à l'index.  
  
OFF       
Indique qu'aucune liste de mots vides ne doit être associée à l'index de recherche en texte intégral.  
  
**SYSTEM**       
Indique que la liste de mots vides de texte intégral système par défaut doit être utilisée pour cet index de recherche en texte intégral.  
  
*stoplist_name*       
Spécifie le nom de la liste de mots vides à associer à l'index de recherche en texte intégral.  
  
SEARCH PROPERTY LIST [ = ] *property_list_name*       
**S’applique à** : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures)  
  
Associe une liste de propriétés de recherche à l'index.  
 
OFF       
Indique qu'aucune liste de propriétés ne doit être associée à l'index de recherche en texte intégral.  
  
*property_list_name*       
Spécifie le nom de la liste de propriétés de recherche à associer à l'index de recherche en texte intégral.  
  
## <a name="remarks"></a>Notes  
Pour plus d’informations, consultez [Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md).  
  
Sur les colonnes **xml**, vous pouvez créer un index de recherche en texte intégral qui indexe le contenu des éléments XML tout en ignorant le balisage XML. Les valeurs d'attributs sont indexées en texte intégral, à moins qu'il ne s'agisse de valeurs numériques. Des balises d'éléments sont utilisées comme limites de jeton. Les fragments et les documents XML ou HTML correctement formés contenant plusieurs langues sont pris en charge. Pour plus d’informations, consultez [Utiliser la recherche en texte intégral avec des colonnes XML](../../relational-databases/xml/use-full-text-search-with-xml-columns.md).  
  
Nous recommandons que la colonne de clé d'index soit un type de données integer. Cela permet d'optimiser au moment de l'exécution de la requête.  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a>Interactions entre le suivi des modifications et le paramètre NO POPULATION  
 Le remplissage de l'index de recherche en texte intégral dépend de l'activation du suivi des modifications et varie selon que WITH NO POPULATION est spécifié ou non dans l'instruction ALTER FULLTEXT INDEX. Le tableau suivant résume le résultat de leur interaction.  
  
|Suivi des modifications|WITH NO POPULATION|Résultats|  
|---------------------|------------------------|------------|  
|Non activé|Non spécifié|Un remplissage complet est effectué sur l'index.|  
|Non activé|Spécifié|Le remplissage de l'index n'a pas lieu tant qu'une instruction ALTER FULLTEXT INDEX...START POPULATION n'a pas été émise.|  
|activé|Spécifié|Une erreur est levée et l'index n'est pas modifié.|  
|activé|Non spécifié|Un remplissage complet est effectué sur l'index.|  
  
 Pour plus d’informations sur l’alimentation d’index les index de recherche en texte intégral, consultez [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).  
  
## <a name="permissions"></a>Autorisations  
 L'utilisateur doit disposer de l'autorisation `REFERENCES` sur le catalogue de texte intégral et de l'autorisation `ALTER` sur la table ou la vue indexée ou il doit être membre du rôle serveur fixe `sysadmin` ou `db_owner` ou des rôles de base de données fixe `db_ddladmin`.  
  
Si `SET STOPLIST` est spécifié, l’utilisateur doit disposer de l’autorisation REFERENCES de la liste de mots vides spécifiée. Le propriétaire de la liste de mots vides peut accorder cette autorisation.  
  
> [!NOTE]  
> L'autorisation REFERENCE est accordée au public sur la liste de mots vides par défaut qui accompagne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-unique-index-a-full-text-catalog-and-a-full-text-index"></a>R. Création d'un index unique, d'un catalogue de texte intégral et d'un index de recherche en texte intégral  
 L'exemple ci-après crée un index unique sur la colonne `JobCandidateID` de la table `HumanResources.JobCandidate` de l'exemple de base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. L'exemple crée ensuite un catalogue de texte intégral par défaut, `ft`. Enfin, l'exemple crée un index de recherche en texte intégral sur la colonne `Resume`, à l'aide du catalogue `ft` et de la liste de mots vides système.  
  
```sql  
CREATE UNIQUE INDEX ui_ukJobCand ON HumanResources.JobCandidate(JobCandidateID);  
CREATE FULLTEXT CATALOG ft AS DEFAULT;  
CREATE FULLTEXT INDEX ON HumanResources.JobCandidate(Resume)   
   KEY INDEX ui_ukJobCand   
   WITH STOPLIST = SYSTEM;  
GO  
```  
  
### <a name="b-creating-a-full-text-index-on-several-table-columns"></a>B. Création d'un index de recherche en texte intégral sur plusieurs colonnes de table  
 L'exemple suivant crée un catalogue de texte intégral, `production_catalog`, dans l'exemple de base de données `AdventureWorks`. L'exemple crée ensuite un index de recherche en texte intégral qui utilise ce nouveau catalogue. L'index de recherche en texte intégral est sur les colonnes `ReviewerName`, `EmailAddress` et `Comments` de la table `Production.ProductReview`. Pour chaque colonne, l'exemple spécifie le LCID de l'anglais, `1033`, qui est la langue des données dans les colonnes. Cet index de recherche en texte intégral utilise un index de clé unique existant, `PK_ProductReview_ProductReviewID`. Comme recommandé, cette clé d'index est sur une colonne d'entiers, `ProductReviewID`.  
  
```sql  
CREATE FULLTEXT CATALOG production_catalog;  
GO  
CREATE FULLTEXT INDEX ON Production.ProductReview  
 (   
  ReviewerName  
     Language 1033,  
  EmailAddress  
     Language 1033,  
  Comments   
     Language 1033       
 )   
  KEY INDEX PK_ProductReview_ProductReviewID   
      ON production_catalog;   
GO  
```  
  
### <a name="c-creating-a-full-text-index-with-a-search-property-list-without-populating-it"></a>C. Création d'un index de recherche en texte intégral avec une liste de propriétés de recherche sans le remplir  
 L'exemple suivant crée un index de recherche en texte intégral sur les colonnes `Title`, `DocumentSummary` et `Document` de la table `Production.Document`. L'exemple spécifie le LCID de l'anglais, `1033`, qui est la langue des données dans les colonnes. Cet index de recherche en texte intégral utilise le catalogue de texte intégral par défaut et un index de clé unique existant, `PK_Document_DocumentID`. Comme recommandé, cette clé d'index est sur une colonne d'entiers, `DocumentID`.  
  
 L'exemple spécifie la liste de mots vides SYSTEM. Il spécifie également une liste de propriétés de recherche, `DocumentPropertyList` ; pour obtenir un exemple qui crée cette liste de propriétés, consultez [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
 L'exemple spécifie que le suivi des modifications est désactivé sans remplissage. Plus tard, durant les heures creuses, cet exemple utilise une instruction ALTER FULLTEXT INDEX pour démarrer un remplissage complet sur le nouvel index et activer le suivi des modifications automatique.  
  
```sql  
CREATE FULLTEXT INDEX ON Production.Document  
  (   
  Title  
      Language 1033,   
  DocumentSummary  
      Language 1033,   
  Document   
      TYPE COLUMN FileExtension  
      Language 1033   
  )  
  KEY INDEX PK_Document_DocumentID  
          WITH STOPLIST = SYSTEM, SEARCH PROPERTY LIST = DocumentPropertyList, CHANGE_TRACKING OFF, NO POPULATION;  
   GO  
```  
  
 Plus tard, pendant une heure creuse, l'index est rempli :  
  
```sql  
ALTER FULLTEXT INDEX ON Production.Document SET CHANGE_TRACKING AUTO;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[Créer et gérer des index de recherche en texte intégral](../../relational-databases/search/create-and-manage-full-text-indexes.md)       
[ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)       
[DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)       
[Recherche en texte intégral](../../relational-databases/search/full-text-search.md)       
[GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)       
[sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)       
[Rechercher les propriétés du document à l’aide des listes des propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)       
  
  
