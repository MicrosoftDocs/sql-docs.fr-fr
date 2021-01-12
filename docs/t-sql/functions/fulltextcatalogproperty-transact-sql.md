---
description: FULLTEXTCATALOGPROPERTY (Transact-SQL)
title: FULLTEXTCATALOGPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FULLTEXTCATALOGPROPERTY_TSQL
- FULLTEXTCATALOGPROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- full-text catalogs [SQL Server], properties
- FULLTEXTCATALOGPROPERTY function
- status information [SQL Server], full-text catalogs
ms.assetid: f841dc79-2044-4863-aff0-56b8bb61f250
author: cawrites
ms.author: chadam
ms.openlocfilehash: 57e53c26b7f1797daf6a1ccc53f7578eabda734a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087303"
---
# <a name="fulltextcatalogproperty-transact-sql"></a>FULLTEXTCATALOGPROPERTY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Renvoie des informations concernant les propriétés de catalogue de texte intégral dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
FULLTEXTCATALOGPROPERTY ('catalog_name' ,'property')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
  
> [!NOTE]  
>  Les propriétés suivantes seront supprimées dans une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **LogSize** et **PopulateStatus**. Évitez par conséquent d'utiliser ces propriétés dans un nouveau travail de développement et prévoyez la modification des applications qui les utilisent actuellement.  
  
_catalog\_name_  
Expression contenant le nom du catalogue de texte intégral.  
  
_property_  
Expression contenant le nom de la propriété de catalogue de texte intégral. Le tableau donne la liste des propriétés et fournit les descriptions des informations renvoyées.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**AccentSensitivity**|Respect des accents.<br /><br /> 0 = Accents ignorés<br /><br /> 1 = Accents respectés|  
|**IndexSize**|Taille logique du catalogue de texte intégral en mégaoctets (Mo). Inclut la taille des index d'expression de clé sémantique et de ressemblance du document.<br /><br /> Pour plus d'informations, consultez la section « Remarques » plus loin dans cette rubrique.|  
|**ItemCount**|Nombre d’éléments indexés, y compris tous les index de recherche en texte intégral, d’expression de clé et de ressemblance du document dans un catalogue|  
|**LogSize**|Pris en charge pour la compatibilité descendante uniquement. Retourne toujours 0.<br /><br /> Taille en octets du jeu combiné de journaux d'erreurs associés à un catalogue de texte intégral du service de Recherche [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**MergeStatus**|Indique si une fusion principale est en cours.<br /><br /> 0 = Aucune fusion principale en cours<br /><br /> 1 = Fusion principale en cours|  
|**PopulateCompletionAge**|Différence en secondes entre la fin du remplissage du dernier index de texte intégral et le 01/01/1990 00:00:00<br /><br /> Propriété uniquement mise à jour pour des analyses complètes ou incrémentielles. Renvoie la valeur 0 si aucun remplissage ne s'est produit.|  
|**PopulateStatus**|0 = Inactif <br /><br /> 1 = Remplissage complet en cours<br /><br /> 2 = En pause <br /><br /> 3 = Accéléré<br /><br /> 4 = Récupération<br /><br /> 5 = Arrêt<br /><br /> 6 = Remplissage incrémentiel en cours<br /><br /> 7 = Indexation en cours<br /><br /> 8 = Disque plein Suspendu.<br /><br /> 9 = Suivi des modifications|  
|**UniqueKeyCount**|Nombre de clés uniques dans le catalogue de texte intégral.|  
|**ImportStatus**|Indique si le catalogue de texte intégral est en cours d’importation.<br /><br /> 0 = Le catalogue de texte intégral n’est pas en cours d’importation.<br /><br /> 1 = Indique que le catalogue de texte intégral est en cours d'importation.|  
  
## <a name="return-types"></a>Types de retour  
**int**  
  
## <a name="exceptions"></a>Exceptions  
Retourne NULL en cas d’erreur ou si un appelant n’est pas autorisé à voir l’objet.  
  
Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], un utilisateur ne peut qu’afficher les métadonnées des sécurisables. Ces sécurisables sont ceux dont il est propriétaire ou pour lesquels il dispose des autorisations nécessaires. Par conséquent, les fonctions intégrées générant des métadonnées, telles que FULLTEXTCATALOGPROPERTY, peuvent retourner la valeur NULL si l’utilisateur ne dispose d’aucune autorisation sur l’objet. Pour plus d’informations, consultez [sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md).  
  
## <a name="remarks"></a>Remarques  
FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') n’examine que les fragments ayant l’état 4 ou 6, comme indiqué dans [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md). Ces fragments font partie de l'index logique. Par conséquent, la propriété **IndexSize** ne retourne que la taille de l’index logique. 

Pendant une fusion d'index, toutefois, la taille d'index réelle peut être le double de sa taille logique. Pour rechercher la taille réelle consommée par un index de recherche en texte intégral pendant une fusion, utilisez la procédure stockée système [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md). Cette procédure examine tous les fragments associés à un index de recherche en texte intégral. 

Le remplissage de texte intégral peut échouer. Il peut échouer si vous limitez la croissance du fichier de catalogue de texte intégral et si vous n’assignez pas un espace suffisant au processus de fusion. Dans ce cas, FULLTEXTCATALOGPROPERTY ('_catalog\_name_','**IndexSize**') retourne 0 et l’erreur suivante est écrite dans le journal de texte intégral :  
  
`Error: 30059, Severity: 16, State: 1. A fatal error occurred during a full-text population and caused the population to be cancelled. Population type is: FULL; database name is FTS_Test (id: 13); catalog name is t1_cat (id: 5); table name t1 (id: 2105058535). Fix the errors that are logged in the full-text crawl log. Then, resume the population. The basic Transact-SQL syntax for this is: ALTER FULLTEXT INDEX ON table_name RESUME POPULATION.`  
  
Il est important que les applications n’attendent pas en boucle serrée, en vérifiant que la propriété **PopulateStatus** devient inactive. Cela indique que le remplissage est terminé. Cette vérification supprime des cycles d’UC de la base de données et du processus de recherche en texte intégral, et provoque des délais d’attente. En général, il vaut mieux vérifier la propriété **PopulateStatus** correspondante au niveau de la table, **TableFullTextPopulateStatus** dans la fonction système OBJECTPROPERTYEX. Cette propriété et d'autres propriétés de texte intégral nouvelles dans OBJECTPROPERTYEX fournissent des informations plus précises sur les tables d'indexation de texte intégral. Pour plus d’informations, consultez [OBJECTPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/objectpropertyex-transact-sql.md).  
  
## <a name="examples"></a>Exemples  
L'exemple suivant renvoie le nombre d'éléments indexés en texte intégral dans un catalogue de texte intégral nommé `Cat_Desc`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT fulltextcatalogproperty('Cat_Desc', 'ItemCount');  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[FULLTEXTSERVICEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
[Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
[sp_help_fulltext_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
  
  
