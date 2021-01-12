---
description: ALTER SEARCH PROPERTY LIST (Transact-SQL)
title: ALTER SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SEARCH_PROPERTY_TSQL
- ALTER_SEARCH_PROPERTY_LIST_TSQL
- ALTER SEARCH PROPERTY LIST
- ALTER SEARCH PROPERTY
- ALTER_SEARCH_TSQL
- ALTER SEARCH
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], altering
- ALTER SEARCH PROPERTY LIST statement
ms.assetid: 0436e4a8-ca26-4d23-93f1-e31e2a1c8bfb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 510bd7abb561fca8a425b843a1647290054b425a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094712"
---
# <a name="alter-search-property-list-transact-sql"></a>ALTER SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Ajoute une propriété de recherche spécifiée à la liste de propriétés de recherche spécifiée ou la supprime de cette liste.  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER SEARCH PROPERTY LIST list_name  
{  
   ADD 'property_name'  
     WITH   
      (   
          PROPERTY_SET_GUID = 'property_set_guid'  
        , PROPERTY_INT_ID = property_int_id  
      [ , PROPERTY_DESCRIPTION = 'property_description' ]  
      )  
 | DROP 'property_name'   
}  
;  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *list_name*  
 Nom de la liste de propriétés modifiée. *list_name* est un identificateur.  
  
 Pour consulter les noms des listes de propriétés existantes, utilisez la vue de catalogue [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md), comme suit :  
  
```sql  
SELECT name FROM sys.registered_search_property_lists;  
```  
  
 ADD  
 Ajoute une propriété de recherche spécifiée à la liste de propriétés spécifiée par *list_name*. La propriété est inscrite pour la liste des propriétés de recherche. Avant que les propriétés récemment ajoutées puissent être utilisées pour la recherche de propriétés, l'index ou les index de recherche en texte intégral associé doivent être remplis à nouveau. Pour plus d’informations, consultez [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md).  
  
> [!NOTE]  
>  Pour ajouter une propriété de recherche donnée à une liste de propriétés de recherche, vous devez fournir son GUID du jeu de propriétés (*property_set_guid*) et son ID entier de propriété (*property_int_id*). Pour plus d'informations, consultez la section« Obtention de GUID de jeux de propriétés et d'identificateurs », dans la suite de cette rubrique.  
  
 *property_name*  
 Spécifie le nom à utiliser pour identifier la propriété dans les requêtes de texte intégral. *property_name* doit identifier de manière unique la propriété dans le jeu de propriétés. Un nom de propriété peut contenir des espaces internes. La longueur maximale de *property_name* est de 256 caractères. Ce nom peut être un nom convivial, comme Auteur ou Domicile, ou ce peut être le nom canonique Windows de la propriété, comme **System.Author** ou **System.Contact.HomeAddress**.  
  
 Les développeurs devront utiliser la valeur que vous spécifiez pour *property_name* pour identifier la propriété dans le prédicat [CONTAINS](../../t-sql/queries/contains-transact-sql.md). Par conséquent, lors de l’ajout d’une propriété, il est important de spécifier une valeur qui représente de manière significative la propriété définie par le GUID du jeu de propriétés spécifié (*property_set_guid*) et l’identificateur de propriété (*property_int_id*). Pour plus d'informations sur les noms de propriété, consultez la section « Notes », plus loin dans cette rubrique.  
  
 Pour consulter les noms des propriétés qui existent actuellement dans une liste de propriétés de recherche de la base de données active, utilisez la vue de catalogue [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md), comme suit :  
  
```sql  
SELECT property_name FROM sys.registered_search_properties;  
```  
  
 PROPERTY_SET_GUID ='*property_set_guid*'  
 Spécifie l'identificateur du jeu de propriétés auquel appartient la propriété. Il s'agit d'un identificateur global unique (GUID). Pour plus d'informations sur l'obtention de cette valeur, consultez les « Notes », dans la suite de cette rubrique.  
  
 Pour consulter le GUID du jeu de propriétés d’une propriété qui figure actuellement dans une liste de propriétés de recherche de la base de données active, utilisez la vue de catalogue [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md), comme suit :  
  
```sql  
SELECT property_set_guid FROM sys.registered_search_properties;  
```  
  
 PROPERTY_INT_ID =*property_int_id*  
 Spécifie l'entier qui identifie la propriété dans son jeu de propriétés. Pour plus d'informations sur l'obtention de cette valeur, consultez la section « Notes ».  
  
 Pour consulter l’identificateur entier d’une propriété figurant dans une liste de propriétés de recherche de la base de données active, utilisez la vue de catalogue [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md), comme suit :  
  
```sql  
SELECT property_int_id FROM sys.registered_search_properties;  
```  
  
> [!NOTE]  
>  Une combinaison donnée de *property_set_guid* et de *property_int_id* doit être unique dans une liste de propriétés de recherche. Si vous essayez d'ajouter une combinaison existante, l'opération ALTER SEARCH PROPERTY LIST échoue et génère une erreur. Cela signifie que vous pouvez définir uniquement un nom pour une propriété donnée.  
  
 PROPERTY_DESCRIPTION ='*property_description*'  
 Spécifie une description définie par l'utilisateur de la propriété. *property_description* est une chaîne comprenant jusqu’à 512 caractères. Cette option est facultative.  
  
 DROP  
 Supprime la propriété spécifiée de la liste de propriétés spécifiée par *list_name*. La suppression d'une propriété annule son inscription ; elle ne peut donc plus faire l'objet d'une recherche.  
  
## <a name="remarks"></a>Remarques  
 Chaque index de recherche en texte intégral ne peut avoir qu'une seule liste de propriétés de recherche.  
  
 Pour permettre l'interrogation sur une propriété de recherche donnée, vous devez l'ajouter à la liste des propriétés de recherche de l'index de recherche en texte intégral, puis remplir à nouveau l'index.  
  
 Lors de la spécification d'une propriété, vous pouvez réorganiser les clauses PROPERTY_SET_GUID, PROPERTY_INT_ID et PROPERTY_DESCRIPTION dans n'importe quel ordre, comme une liste séparée par des virgules mise entre parenthèses. Exemple :  
  
```sql  
ALTER SEARCH PROPERTY LIST CVitaProperties  
ADD 'System.Author'   
WITH (   
      PROPERTY_DESCRIPTION = 'Author or authors of a given document.',  
      PROPERTY_SET_GUID   = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9',   
      PROPERTY_INT_ID = 4   
      );  
```  
  
> [!NOTE]  
>  Cet exemple utilise le nom de la propriété, `System.Author`, qui est semblable au concept de noms de propriété canoniques présenté dans Windows Vista (nom canonique Windows).  
  
## <a name="obtaining-property-values"></a>Obtention de valeurs de propriété  
 La recherche en texte intégral mappe une propriété de recherche à un index de recherche en texte intégral à l'aide de son GUID du jeu de propriétés et d'un identificateur de propriété entier. Pour plus d’informations sur la façon d’obtenir ces valeurs pour les propriétés définies par Microsoft, consultez [Recherche des GUID du jeu de propriétés et des ID d’entier de propriétés pour les propriétés de recherche](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md). Pour plus d'informations sur les propriétés définies par un éditeur de logiciels indépendant (ISV), consultez la documentation de ce dernier.  
  
## <a name="making-added-properties-searchable"></a>Recherche possible avec des propriétés ajoutées  
 L'ajout d'une propriété de recherche à une liste de propriétés de recherche permet d'inscrire la propriété. Une propriété ajoutée récemment peut être spécifiée immédiatement dans les requêtes [CONTAINS](../../t-sql/queries/contains-transact-sql.md). Toutefois, les requêtes de texte intégral avec étendue aux propriétés sur une propriété ajoutée récemment ne retourneront pas de documents tant que l'index de recherche en texte intégral associé n'aura pas été à nouveau rempli. Par exemple, la requête délimitée aux propriétés suivante sur une propriété ajoutée récemment, *new_search_property*, ne retourne pas de documents tant que l’index de recherche en texte intégral associé à la table cible (*table_name*) n’a pas été à nouveau rempli :  
  
```sql  
SELECT column_name  
FROM table_name  
WHERE CONTAINS( PROPERTY( column_name, 'new_search_property' ), 
               'contains_search_condition');  
GO   
```  
  
 Pour démarrer un remplissage complet, utilisez l’instruction[ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md) suivante :  
  
```sql  
USE database_name;  
GO  
ALTER FULLTEXT INDEX ON table_name START FULL POPULATION;  
GO  
```  
  
> [!NOTE]  
>  Le remplissage n'est pas nécessaire après la suppression d'une propriété d'une liste de propriétés, car seules les propriétés qui restent dans la liste des propriétés de recherche sont disponibles pour l'interrogation en texte intégral.  
  
## <a name="related-references"></a>Références connexes  
 **Pour créer une liste de propriétés**  
  
-   [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)  
  
 **Pour supprimer une liste de propriétés**  
  
-   [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
 **Pour ajouter ou supprimer une liste de propriétés sur un index de recherche en texte intégral**  
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
 **Pour exécuter un remplissage sur un index de recherche en texte intégral**  
  
-   [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l'autorisation CONTROL sur la liste de propriétés.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-adding-a-property"></a>R. Ajout d'une propriété  
 L’exemple suivant ajoute plusieurs propriétés (`Title`, `Author` et `Tags`) à une liste de propriétés nommée `DocumentPropertyList`.  
  
> [!NOTE]  
>  Pour obtenir un exemple qui crée la liste de propriétés `DocumentPropertyList`, consultez [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md).  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
   ADD 'Title'   
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 2,   
      PROPERTY_DESCRIPTION = 'System.Title - Title of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Author'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 4,   
      PROPERTY_DESCRIPTION = 'System.Author - Author or authors of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Tags'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 5,   
      PROPERTY_DESCRIPTION = 
          'System.Keywords - Set of keywords (also known as tags) assigned to the item.' );  
```  
  
> [!NOTE]  
>  Vous devez associer une liste de propriétés de recherche donnée à un index de recherche en texte intégral avant de pouvoir l'utiliser dans le cadre de requêtes avec étendue aux propriétés. Pour ce faire, utilisez une instruction [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) et spécifiez la clause SET SEARCH PROPERTY LIST.  
  
### <a name="b-dropping-a-property"></a>B. Suppression d'une propriété  
 L'exemple suivant supprime la propriété `Comments` dans la liste de propriétés `DocumentPropertyList`.  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
DROP 'Comments' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [Rechercher les propriétés du document à l’aide des listes de propriétés de recherche](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [Recherche des GUID du jeu de propriétés et des ID d'entier de propriétés pour les propriétés de recherche](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
