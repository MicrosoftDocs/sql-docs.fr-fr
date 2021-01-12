---
description: ALTER FULLTEXT STOPLIST (Transact-SQL)
title: ALTER FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER FULLTEXT STOPLIST
- ALTER_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- ALTER FULLTEXT STOPLIST statement
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: f6ad87d5-6a34-435a-8456-8244947c5c83
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 12e90f8d8b2d0b35ecd406af54f55b6ea96cb4c6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98082383"
---
# <a name="alter-fulltext-stoplist-transact-sql"></a>ALTER FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Insère ou supprime un mot vide dans la liste de mots vides de texte intégral par défaut de la base de données active.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER FULLTEXT STOPLIST stoplist_name  
{   
        ADD [N] 'stopword' LANGUAGE language_term    
  | DROP   
    {  
        'stopword' LANGUAGE language_term   
      | ALL LANGUAGE language_term   
      | ALL  
     }  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *stoplist_name*  
 Nom de la liste de mots vides qui est modifiée. *stoplist_name* peut avoir un maximum de 128 caractères.  
  
 **'** *stopword* **'**  
 Chaîne qui pourrait être un mot avec une signification linguistique dans la langue spécifiée ou un jeton sans signification linguistique. *stopword* est limité à la longueur de jeton maximale (64 caractères). Un mot vide peut être spécifié en tant que chaîne Unicode.  
  
 LANGUAGE *language_term*  
 Spécifie la langue à associer au *stopword* qui est ajouté ou supprimé.  
  
 *language_term* peut être spécifié sous forme de chaîne, d’entier ou de valeur hexadécimale correspondant à l’identificateur de paramètres régionaux (LCID) de la langue, comme suit :  
  
|Format|Description|  
|------------|-----------------|  
|String|*language_term* correspond à la valeur de colonne **alias** dans la vue de compatibilité [sys.syslanguages (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). La chaîne doit être placée entre guillemets simples, comme dans **'** _language_term_*_'_*.|  
|Integer|*language_term* est l’identificateur LCID de la langue.|  
|Valeur hexadécimale|*language_term* est 0x suivi de la valeur hexadécimale de l’identificateur LCID. La valeur hexadécimale ne doit pas dépasser huit caractères, y compris les zéros non significatifs. Si la valeur est au format de jeu de caractères codés sur deux octets (DBCS), SQL Server la convertit au format Unicode.|  
  
 ADD **'** _stopword_*_'_* LANGUAGE *language_term*  
 Ajoute un mot vide à la liste de mots vides pour la langue spécifiée par LANGUAGE *language_term*.  
  
 Si la combinaison spécifiée du mot clé et de la valeur LCID de la langue n'est pas unique dans la liste de mots vides, une erreur est retournée.  Si la valeur LCID ne correspond pas à une langue répertoriée, une erreur est générée.  
  
 DROP { **'** _stopword_*_'_* LANGUAGE *language_term* | ALL LANGUAGE *language_term* | ALL }  
 Supprime un mot vide de la liste de mots vides.  
  
 **'** *stopword* **'** LANGUAGE *language_term*  
 Supprime le mot vide spécifié pour la langue indiquée par *language_term*.  
  
 ALL LANGUAGE *language_term*  
 Supprime tous les mots vides pour la langue spécifiée par *language_term*.  
  
 ALL  
 Supprime tous les mots vides dans la liste de mots vides.  
  
## <a name="remarks"></a>Notes  
 CREATE FULLTEXT STOPLIST est pris en charge uniquement pour un niveau de compatibilité de 100 et supérieur. Pour des niveaux de compatibilité de 80 et 90, la liste de mots vides système est toujours assignée à la base de données.  
  
## <a name="permissions"></a>Autorisations  
 Désigner une liste de mots vides comme la liste de mots vides par défaut de la base de données requiert l'autorisation ALTER DATABASE. En outre, pour pouvoir modifier une liste de mots vides, il est obligatoire d’être le propriétaire de cette liste de mots vides ou d’appartenir au rôle de base de données fixe **db_owner** ou **db_ddladmin**.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous illustre la modification d'une liste de mots vides nommée `CombinedFunctionWordList` par l'ajout du mot « en » en premier pour l'espagnol puis pour le français.  
  
```sql  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'Spanish';  
ALTER FULLTEXT STOPLIST CombinedFunctionWordList ADD 'en' LANGUAGE 'French';  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
