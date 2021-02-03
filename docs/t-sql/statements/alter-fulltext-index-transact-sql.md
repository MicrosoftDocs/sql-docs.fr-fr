---
description: ALTER FULLTEXT INDEX (Transact-SQL)
title: ALTER FULLTEXT INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER FULLTEXT INDEX
- ALTER_FULLTEXT_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- modifying full-text indexes
- full-text indexes [SQL Server], modifying
- search property lists [SQL Server], associating with full-text indexes
- ALTER FULLTEXT INDEX statement
ms.assetid: b6fbe9e6-3033-4d1b-b6bf-1437baeefec3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 17981c3c04c17b0c0291714a245dfa1856a69af7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204229"
---
# <a name="alter-fulltext-index-transact-sql"></a>ALTER FULLTEXT INDEX (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Modifie les propriétés d’un index de recherche en texte intégral dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER FULLTEXT INDEX ON table_name  
   { ENABLE   
   | DISABLE  
   | SET CHANGE_TRACKING [ = ] { MANUAL | AUTO | OFF }  
   | ADD ( column_name   
           [ TYPE COLUMN type_column_name ]   
           [ LANGUAGE language_term ]  
           [ STATISTICAL_SEMANTICS ]  
           [,...n]   
         )  
     [ WITH NO POPULATION ]  
   | ALTER COLUMN column_name  
     { ADD | DROP } STATISTICAL_SEMANTICS  
     [ WITH NO POPULATION ]  
   | DROP ( column_name [,...n] )  
     [ WITH NO POPULATION ]   
   | START { FULL | INCREMENTAL | UPDATE } POPULATION  
   | {STOP | PAUSE | RESUME } POPULATION   
   | SET STOPLIST [ = ] { OFF| SYSTEM | stoplist_name }  
     [ WITH NO POPULATION ]   
   | SET SEARCH PROPERTY LIST [ = ] { OFF | property_list_name }  
     [ WITH NO POPULATION ]   
   }  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *table_name*  
 Nom de la table ou de la vue indexée qui contient la ou les colonnes incluses dans l'index de recherche en texte intégral. La spécification des noms de la base de données et du propriétaire de la table est facultative.  
  
 ENABLE | DISABLE  
 Indique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de collecter des données d’index de recherche en texte intégral pour *table_name*. ENABLE active l'index de texte intégral et DISABLE le désactive. La table ne prend pas en charge les requêtes de texte intégral pendant que l'index est désactivé.  
  
 La désactivation d'un index de recherche en texte intégral vous permet de désactiver le suivi des modifications tout en conservant l'index de recherche en texte intégral, que vous pouvez réactiver n'importe quand à l'aide de l'option ENABLE. Lorsque l'index de recherche en texte intégral est désactivé, ses métadonnées restent dans les tables système. Si CHANGE_TRACKING est à l'état activé (mise à jour manuelle ou automatique) quand l'index de recherche en texte intégral est désactivé, l'état de l'index est figé ; toute analyse en cours est arrêtée et les nouvelles modifications apportées aux données de la table ne sont pas suivies ni propagées dans l'index.  
  
 SET CHANGE_TRACKING {MANUAL | AUTO | OFF}  
 Spécifie si les modifications (mises à jour, suppressions ou insertions) apportées aux colonnes de table couvertes par l’index de recherche en texte intégral seront propagées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’index de recherche en texte intégral. Les modifications apportées aux données via WRITETEXT et UPDATETEXT ne sont pas répercutées dans l'index de recherche en texte intégral et ne sont pas prises en compte par le suivi des modifications.  
  
> [!NOTE]  
>  Pour plus d’informations, consultez [Interactions entre le suivi des modifications et le paramètre NO POPULATION](#change-tracking-no-population).
  
 MANUAL  
 Spécifie que les modifications suivies sont propagées manuellement en appelant l’instruction ALTER FULLTEXT INDEX ... START UPDATE POPULATION [!INCLUDE[tsql](../../includes/tsql-md.md)] (*alimentation manuelle*). Vous pouvez utiliser l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour appeler régulièrement cette instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 AUTO  
 Indique que les modifications suivies seront propagées automatiquement à mesure que les données seront modifiées dans la table de base (*alimentation automatique*). Même si les modifications sont propagées automatiquement, elles n'apparaissent pas nécessairement immédiatement dans l'index de recherche en texte intégral. AUTO est la valeur par défaut.  
  
 OFF  
 Spécifie que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne conservera pas de liste des modifications apportées aux données indexées.  
  
 ADD | DROP *column_name*  
 Spécifie les colonnes à ajouter ou à supprimer de l'index de texte intégral. Les colonnes doivent être de type **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** ou **varbinary(max)**.  
  
 Utilisez la clause DROP uniquement sur les colonnes pour lesquelles l'indexation de texte intégral a été activée.  
  
 Utilisez TYPE COLUMN et LANGUAGE avec la clause ADD pour définir ces propriétés sur *column_name*. Lorsqu'une colonne est ajoutée, l'index de texte intégral de la table doit être à nouveau rempli pour que les requêtes de texte intégral fonctionnent dans la colonne en question.  
  
> [!NOTE]  
>  Le remplissage de l'index de recherche en texte intégral après l'ajout ou la suppression d'une colonne dans un index de recherche en texte intégral dépend de l'activation du suivi des modifications et si WITH NO POPULATION est spécifié. Pour plus d’informations, consultez [Interactions entre le suivi des modifications et le paramètre NO POPULATION](#change-tracking-no-population).
  
 TYPE COLUMN *type_column_name*  
 Spécifie le nom d’une colonne de table, *type_column_name*, utilisée pour contenir le type de document d’un document **varbinary**, **varbinary(max)** ou **image**. Cette colonne,appelée colonne de type, contient une extension de fichier fourni par l'utilisateur (.doc, .pdf, .xls, et ainsi de suite). La colonne doit être de type **char**, **nchar**, **varchar** ou **nvarchar**.  
  
 Spécifiez TYPE COLUMN *type_column_name* seulement si *column_name* spécifie une colonne **varbinary**, **varbinary(max)** ou **image**, dans laquelle les données sont stockées sous forme de données binaires ; dans le cas contraire, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur.  
  
> [!NOTE]  
>  Au moment de l’indexation, le moteur d’indexation et de recherche en texte intégral utilise l’abréviation figurant dans la colonne de type de chaque ligne de la table pour identifier le filtre de recherche en texte intégral à utiliser pour le document dans *column_name*. Le filtre charge le document en tant que flux binaire, supprime les informations de mise en forme et envoie le texte du document vers le composant d'analyseur lexical de texte intégral. Pour plus d’informations, consultez [Configurer et gérer les extensions analytiques avancées](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
 LANGUAGE *language_term*  
 Langue des données stockées dans **column_name**.  
  
 *language_term* est facultatif et peut être spécifié sous la forme d’une chaîne, d’un entier ou d’une valeur hexadécimale correspondant à l’identificateur de paramètres régionaux (LCID) d’une langue. Si une langue est définie avec *language_term*, elle est appliquée à tous les éléments de la condition de recherche. Si vous ne spécifiez aucune valeur, le système utilise la langue de texte intégral par défaut de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Utilisez la procédure stockée **sp_configure** pour accéder aux informations sur la langue de texte intégral par défaut de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Quand il est spécifié sous forme de chaîne, *language_term* correspond à la valeur de la colonne **alias** dans la table système **syslanguages**. La chaîne doit être placée entre guillemets simples, comme dans '*language_term*'. Quand il est spécifié sous la forme d’un entier, *language_term* est le LCID qui identifie la langue. Quand il est spécifié sous la forme d’une valeur hexadécimale, *language_term* est 0x suivi de la valeur hexadécimale du LCID. La valeur hexadécimale peut comporter un maximum de huit chiffres, zéros non significatifs inclus.  
  
 Si la valeur est au format DBCS (jeu de caractères codés sur deux octets), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la convertit en Unicode.  
  
 Les ressources, telles que les analyseurs lexicaux et les générateurs de formes dérivées, doivent être activées pour la langue spécifiée comme *language_term*. Si ces ressources ne prennent pas en charge la langue spécifiée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne une erreur.  
  
 Pour les colonnes de type non-BLOB et non-XML contenant des données texte dans plusieurs langues, ou lorsque la langue du texte stocké dans la colonne est inconnue, utilisez la ressource de langue neutre (0x0). Pour les documents stockés dans des colonnes de type XML ou BLOB, l'encodage linguistique du document est utilisé lors de l'indexation. Par exemple, dans les colonnes de type XML, l'attribut xml:lang des documents XML identifie la langue. Au moment de la requête, la valeur précédemment spécifiée dans *language_term* devient la langue par défaut utilisée pour les requêtes de texte intégral, sauf si *language_term* est spécifié dans le cadre d’une requête de texte intégral.  
  
 STATISTICAL_SEMANTICS  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Crée les index de ressemblance de documents et d'expressions clés supplémentaires qui font partie de l'indexation sémantique statistique. Pour plus d’informations, consultez [Recherche sémantique &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 [ **,**_...n_]  
 Indique que plusieurs colonnes peuvent être spécifiées pour les clauses ADD, ALTER ou DROP. Si vous spécifiez plusieurs colonnes, séparez-les par des virgules.  
  
 WITH NO POPULATION  
 Indique que l'index de texte intégral ne sera pas rempli après une opération d'ajout (ADD) ou de suppression (DROP) de colonne ni après une opération SET STOPLIST. L'index ne sera rempli que si l'utilisateur exécute une commande START...POPULATION.  
  
 Lorsque NO POPULATION est spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne remplit pas l'index. L'index est rempli uniquement lorsque l'utilisateur lance une commande ALTER FULLTEXT INDEX...START POPULATION. Lorsque NO POPULATION n'est pas spécifié, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] remplit l'index.  
  
 Si vous spécifiez WITH NO POPULATION alors que CHANGE_TRACKING est activé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] renvoie une erreur. Si vous ne spécifiez pas WITH NO POPULATION et que CHANGE_TRACKING est activé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] procède à un remplissage complet de l’index.  
  
> [!NOTE]  
>  Pour plus d’informations, consultez [Interactions entre le suivi des modifications et le paramètre NO POPULATION](#change-tracking-no-population).
  
 {ADD | DROP } STATISTICAL_SEMANTICS  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Active ou désactive l'indexation sémantique statistique pour les colonnes spécifiées. Pour plus d’informations, consultez [Recherche sémantique &#40;SQL Server&#41;](../../relational-databases/search/semantic-search-sql-server.md).  
  
 START {FULL|INCREMENTAL|UPDATE} POPULATION  
 Indique à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de démarrer le remplissage de l’index de recherche en texte intégral de *table_name*. Si un remplissage de l’index de texte intégral est déjà en cours, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retourne un avertissement et ne démarre pas un nouveau remplissage.  
  
 FULL  
 Spécifie que chaque ligne de la table est extraite pour l'indexation de texte intégral même si ces lignes ont déjà été indexées.  
  
 INCREMENTAL  
 Spécifie que seules les lignes modifiées depuis le dernier remplissage sont récupérées pour l'indexation de texte intégral. INCREMENTAL peut être appliqué seulement si la table contient une colonne de type **timestamp**. Si aucune colonne de type **timestamp** n’est présente dans le catalogue de texte intégral, la table fait l’objet d’un remplissage FULL.  
  
 UPDATE  
 Spécifie le traitement de toutes les insertions, mises à jour ou suppressions depuis la dernière mise à jour de l'index de suivi des modifications. Le remplissage du suivi des modifications doit être activé sur une table, mais l'index de mise à jour en arrière-plan ou le suivi automatique des modifications doivent être désactivés.  
  
 {STOP | PAUSE | RESUME } POPULATION  
 Arrête ou suspend tout remplissage en cours ; ou arrête ou reprend tout remplissage suspendu.  
  
 STOP POPULATION n'arrête pas le suivi automatique des modifications ni l'index de mise à jour en arrière-plan. Pour arrêter le suivi des modifications, utilisez SET CHANGE_TRACKING OFF.  
  
 Il est possible d'utiliser PAUSE POPULATION et RESUME POPULATION uniquement pour les remplissages complets ; en effet, les autres types de remplissage reprennent les analyses au point où l'analyse a été arrêtée.  
  
 SET STOPLIST { OFF| SYSTEM | *stoplist_name* }  
 Modifie la liste de mots vides de texte intégral associée à l'index, le cas échéant.  
  
 OFF  
 Indique qu'aucune liste de mots vides ne doit être associée à l'index de recherche en texte intégral.  
  
 SYSTEM  
 Indique que la liste de mots vides de texte intégral système par défaut doit être utilisée pour cet index de recherche en texte intégral.  
  
 *stoplist_name*  
 Spécifie le nom de la liste de mots vides à associer à l'index de recherche en texte intégral.  
  
 Pour plus d’informations, consultez [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md).  
  
 SET SEARCH PROPERTY LIST { OFF | *property_list_name* } [ WITH NO POPULATION ]  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 Modifie la liste de propriétés de recherche associée à l'index, le cas échéant.  
  
 OFF  
 Indique qu'aucune liste de propriétés ne doit être associée à l'index de recherche en texte intégral. Quand vous désactivez la liste des propriétés de recherche d’un index de recherche en texte intégral (ALTER FULLTEXT INDEX ... SET SEARCH PROPERTY LIST OFF), la recherche de propriétés sur la table de base n’est plus possible.  
  
 Par défaut, lorsque vous désactivez une liste des propriétés de recherche existante, l'index de recherche en texte intégral est rempli à nouveau automatiquement. Si vous spécifiez WITH NO POPULATION lorsque vous désactivez la liste des propriétés de recherche, le remplissage automatique n'a pas lieu. Toutefois, nous vous recommandons d'exécuter finalement un remplissage complet sur cet index de recherche en texte intégral à votre propre convenance. Le remplissage de l'index de recherche en texte intégral supprime les métadonnées spécifiques à la propriété de chaque propriété de recherche supprimée, en rendant l'index de recherche en texte intégral plus petit et plus efficace.  
  
 *property_list_name*  
 Spécifie le nom de la liste de propriétés de recherche à associer à l'index de recherche en texte intégral.  
  
 L'ajout d'une liste de propriétés de recherche à un index de recherche en texte intégral nécessite le remplissage de l'index pour indexer les propriétés de recherche inscrites pour la liste de propriétés de recherche associée. Si vous spécifiez WITH NO POPULATION lorsque vous ajoutez la liste des propriétés de recherche, vous devrez effectuer un remplissage sur l'index, à un moment approprié.  
  
> [!IMPORTANT]  
>  Si l'index de recherche en texte intégral était associé précédemment à une recherche différente, il doit être reconstruit suite au changement de la liste de propriétés pour remettre l'index dans un état cohérent. L'index est immédiatement tronqué et reste vide jusqu'à ce qu'à l'exécution du remplissage complet. Pour plus d’informations, consultez [La modification de la liste des propriétés de recherche provoque la reconstruction de l’index](#change-search-property-rebuild-index). 
  
> [!NOTE]  
>  Vous pouvez associer une liste de propriétés de recherche donnée à plusieurs index de recherche en texte intégral dans la même base de données.  
  
 **Pour trouver les listes de propriétés de recherche sur la base de données actuelle**  
  
-   [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
  
 Pour plus d’informations sur les listes de propriétés de recherche, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="interactions-of-change-tracking-and-no-population-parameter"></a><a name="change-tracking-no-population"></a> Interactions entre le suivi des modifications et le paramètre NO POPULATION  
 Le remplissage de l'index de recherche en texte intégral dépend de l'activation du suivi des modifications et varie selon que WITH NO POPULATION est spécifié ou non dans l'instruction ALTER FULLTEXT INDEX. Le tableau suivant résume le résultat de leur interaction.  
  
|Suivi des modifications|WITH NO POPULATION|Résultats|  
|---------------------|------------------------|------------|  
|Non activé|Non spécifié|Un remplissage complet est effectué sur l'index.|  
|Non activé|Spécifié|Le remplissage de l'index n'a pas lieu tant qu'une instruction ALTER FULLTEXT INDEX...START POPULATION n'a pas été émise.|  
|activé|Spécifié|Une erreur est levée et l'index n'est pas modifié.|  
|activé|Non spécifié|Un remplissage complet est effectué sur l'index.|  
  
 Pour plus d’informations sur l’alimentation d’index les index de recherche en texte intégral, consultez [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).  
  
## <a name="changing-the-search-property-list-causes-rebuilding-the-index"></a><a name="change-search-property-rebuild-index"></a> La modification de la liste des propriétés de recherche provoque la reconstruction de l’index  
 La première fois qu'un index de recherche en texte intégral est associé à une liste de propriétés de recherche, l'index doit faire l'objet d'un nouveau remplissage afin d'indexer des termes de recherche spécifiques aux propriétés. Les données d'index existantes ne sont pas tronquées.  
  
 Toutefois, si vous associez l'index de recherche en texte intégral à une liste de propriétés différente, l'index est reconstruit. La reconstruction tronque immédiatement l'index de recherche en texte intégral, en supprimant toutes les données existantes, et l'index doit être de nouveau rempli. Pendant le remplissage, les requêtes de texte intégral sur la table de base recherchent uniquement les lignes de table qui ont déjà été indexées par le remplissage. Les données d'index remplies incluront des métadonnées des propriétés inscrites de la liste de propriétés de recherche récemment ajoutée.  
  
 Scénarios à l'origine d'une reconstruction :  
  
-   Basculement direct vers une liste de propriétés de recherche différente (consultez le « Scénario A », dans la suite de cette section).  
  
-   Désactivation de la liste de propriétés de recherche et association ultérieure de l'index à toute liste de propriétés de recherche (consulter le « Scénario B » dans la suite de cette section)  
  
> [!NOTE]  
>  Pour plus d’informations sur le fonctionnement de la recherche en texte intégral avec les listes de propriétés de recherche, consultez [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md). Pour plus d’informations sur les remplissages complets, consultez [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md).  
  
### <a name="scenario-a-switching-directly-to-a-different-search-property-list"></a>Scénario A : basculement direct vers une liste de propriétés de recherche différente  
  
1.  Un index de recherche en texte intégral est créé sur `table_1` avec une liste de propriétés de recherche `spl_1` :  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1,   
       CHANGE_TRACKING OFF, NO POPULATION;   
    ```  
  
2.  Un remplissage complet est exécuté sur l'index de recherche en texte intégral :  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 START FULL POPULATION;  
    ```  
  
3.  L'index de recherche en texte intégral est associé ultérieurement à une liste de propriétés de recherche différente, `spl_2`, à l'aide de l'instruction suivante :  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_2;  
    ```  
  
     Cette instruction provoque un remplissage complet, ce qui correspond au comportement par défaut.  Toutefois, avant de commencer ce remplissage, le moteur d'indexation et de recherche en texte intégral tronque automatiquement l'index.  
  
### <a name="scenario-b-turning-off-the-search-property-list-and-later-associating-the-index-with-any-search-property-list"></a>Scénario B : désactivation de la liste de propriétés de recherche et association ultérieure de l'index à toute liste de propriétés de recherche  
  
1.  Un index de recherche en texte intégral est créé sur `table_1` avec une liste de propriétés de recherche `spl_1`, suivi d’un remplissage complet automatique (comportement par défaut) :  
  
    ```sql  
    CREATE FULLTEXT INDEX ON table_1 (column_name) KEY INDEX unique_key_index   
       WITH SEARCH PROPERTY LIST=spl_1;   
    ```  
  
2.  La liste des propriétés de recherche est désactivée, comme suit :  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1   
       SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
    ```  
  
3.  L'index de recherche en texte intégral est encore une fois associé à la même liste de propriétés de recherche ou à une liste différente.  
  
     Par exemple, l’instruction suivante ré-associe l’index de recherche en texte intégral à la liste des propriétés de recherche d’origine, `spl_1` :  
  
    ```sql  
    ALTER FULLTEXT INDEX ON table_1 SET SEARCH PROPERTY LIST spl_1;  
    ```  
  
     Cette instruction entraîne un remplissage complet, ce qui correspond au comportement par défaut.  
  
    > [!NOTE]  
    >  La reconstruction serait également nécessaire pour une liste de propriétés de recherche différente, telle que `spl_2`.  
  
## <a name="permissions"></a>Autorisations  
 L’utilisateur doit soit disposer de l’autorisation ALTER sur la table ou la vue indexée, soit être membre du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_ddladmin** ou **db_owner**.  
  
 Si SET STOPLIST est spécifié, l'utilisateur doit disposer de l'autorisation REFERENCES sur la liste de mots vides. Si SET SEARCH PROPERTY LIST est spécifié, l'utilisateur doit avoir l'autorisation REFERENCES sur la liste des propriétés de recherche. Le propriétaire de la liste de mots vides spécifiée ou de la liste des propriétés de recherche peut accorder l'autorisation REFERENCES, s'il dispose d'autorisations ALTER FULLTEXT CATALOG.  
  
> [!NOTE]  
>  L'autorisation REFERENCES est accordée au public sur la liste de mots vides par défaut qui accompagne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-setting-manual-change-tracking"></a>R. Définition du suivi des modifications manuel  
 L'exemple ci-après définit le suivi des modifications manuel sur l'index de recherche en texte intégral sur la table `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate  
   SET CHANGE_TRACKING MANUAL;  
GO  
```  
  
### <a name="b-associating-a-property-list-with-a-full-text-index"></a>B. Association d'une liste de propriétés à un index de recherche en texte intégral  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 L’exemple suivant associe la liste de propriétés `DocumentPropertyList` à l’index de recherche en texte intégral sur la table `Production.Document`. Cette instruction ALTER FULLTEXT INDEX entraîne un remplissage complet, ce qui correspond au comportement par défaut de la clause SET SEARCH PROPERTY LIST.  
  
> [!NOTE]  
>  Pour obtenir un exemple qui crée la liste de propriétés `DocumentPropertyList`, consultez [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList;   
GO  
```  
  
### <a name="c-removing-a-search-property-list"></a>C. Suppression d'une liste de propriétés de recherche  
  
**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.  
  
 L'exemple suivant supprime la liste de propriétés `DocumentPropertyList` de l'index de recherche en texte intégral sur le `Production.Document`. Dans cet exemple, il n'est pas urgent de supprimer les propriétés de l'index. Par conséquent, l'option WITH NO POPULATION est spécifiée. Toutefois, la recherche au niveau de propriété est autorisée plus longtemps par rapport à cet index de recherche en texte intégral.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST OFF WITH NO POPULATION;   
GO  
```  
  
### <a name="d-starting-a-full-population"></a>D. Démarrage d'un remplissage complet  
 L’exemple suivant démarre un remplissage complet sur l’index de recherche en texte intégral sur la table `JobCandidate`.  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   START FULL POPULATION;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [Recherche en texte intégral](../../relational-databases/search/full-text-search.md)   
 [Alimenter des index de recherche en texte intégral](../../relational-databases/search/populate-full-text-indexes.md)  
  
  
