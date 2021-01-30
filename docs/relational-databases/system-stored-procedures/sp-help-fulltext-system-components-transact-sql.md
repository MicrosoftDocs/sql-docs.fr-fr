---
description: sp_help_fulltext_system_components (Transact-SQL)
title: sp_help_fulltext_system_components (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_components_TSQL
- sp_help_fulltext_components
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_system_components
ms.assetid: ac1fc7a0-7f46-4a12-8c5c-8d378226a8ce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2adf7a7ec8b598accb876529eb274158c01391c9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198246"
---
# <a name="sp_help_fulltext_system_components-transact-sql"></a>sp_help_fulltext_system_components (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Retourne des informations sur les analyseurs lexicaux, le filtre et les gestionnaires de protocoles. **sp_help_fulltext_system_components** retourne également une liste d’identificateurs de bases de données et de catalogues de texte intégral qui ont utilisé le composant spécifié.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_help_fulltext_system_components   
         { 'all'| [ @component_type = ] 'component_type' }  
    , [ @param = ] 'param'  
```  
  
## <a name="arguments"></a>Arguments  
 'all'  
 Retourne des informations pour tous les composants de recherche en texte intégral.  
  
`[ @component_type = ] component_type` Spécifie le type de composant. *component_type* peut prendre l’une des valeurs suivantes :  
  
-   **analyseur lexical**  
  
-   **filter**  
  
-   **Gestionnaire de protocole**  
  
-   **FullPath**  
  
 Si vous spécifiez un chemin d'accès complet, *param* doit également être spécifié avec le chemin d'accès complet à la bibliothèque de liens dynamiques (DLL) du composant, sans quoi un message d'erreur est retourné.  
  
`[ @param = ] param` Selon le type de composant, il s’agit de l’un des éléments suivants : un identificateur de paramètres régionaux (LCID), l’extension de fichier avec le préfixe « . », le nom complet du composant du gestionnaire de protocole ou le chemin d’accès complet à la DLL du composant.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (succès) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Le jeu de résultats suivant est retourné pour les composants système.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**componenttype**|**sysname**|Type de composant. Celui-ci peut avoir l'une des valeurs suivantes :<br /><br /> Filter<br /><br /> protocol handler<br /><br /> wordbreaker|  
|**ComponentName**|**sysname**|Nom du composant.|  
|**identificateur**|**uniqueidentifier**|Identificateur de classe du composant.|  
|**FullPath**|**nvarchar (256)**|Chemin d'accès de l'emplacement du composant.<br /><br /> NULL = l’appelant n’est pas membre du rôle serveur fixe **ServerAdmin** .|  
|**version**|**nvarchar(30)**|Numéro de version du composant.|  
|**fécule**|**sysname**|Nom du fabricant du composant.|  
  
 Le jeu de résultats suivant est retourné uniquement si un ou plusieurs catalogues de texte intégral existent qui utilisent *component_type*.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**dbid**|**int**|ID de la base de données.|  
|**ftcatid**|**int**|Identificateur du catalogue de texte intégral.|  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’appartenance au rôle **public** ; Toutefois, les utilisateurs peuvent uniquement afficher des informations sur les catalogues de texte intégral pour lesquels ils disposent de l’autorisation VIEW DEFINITION. Seuls les membres du rôle de serveur fixe **serveradmin** peuvent voir les valeurs de la colonne **fullpath** .  
  
## <a name="remarks"></a>Notes  
 Cette méthode est particulièrement importante lors de la préparation d'une mise à niveau. Exécutez la procédure stockée dans une base de données spécifique et utilisez le résultat afin de déterminer si un catalogue sera affecté par la mise à niveau.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-all-full-text-system-components"></a>R. Liste de tous les composants système de texte intégral  
 L'exemple suivant répertorie tous les composants systèmes de texte intégral enregistrés sur l'instance de serveur.  
  
```  
EXEC sp_help_fulltext_system_components 'all';  
GO  
```  
  
### <a name="b-listing-word-breakers"></a>B. Liste des analyseurs lexicaux  
 L'exemple suivant répertorie tous les analyseurs lexicaux enregistrés sur l'instance du service.  
  
```  
EXEC sp_help_fulltext_system_components 'wordbreaker';  
GO  
```  
  
### <a name="c-determining-whether-a-specific-word-breaker-is-registered"></a>C. Détermination de si un analyseur lexical spécifique est inscrit  
 L'exemple suivant répertorie l'analyseur lexical pour la langue turque (LCID = 1055) si celle-ci a été installée sur le système et enregistrée sur l'instance du service. Cet exemple spécifie les noms des paramètres, **\@ component_type** et **\@ param**.  
  
```  
EXEC sp_help_fulltext_system_components @component_type = 'wordbreaker', @param = 1055;  
GO  
```  
  
 Par défaut, cet analyseur lexical n'est pas installé, de sorte que le jeu de résultats est vide.  
  
### <a name="d-determining-whether-a-specific-filter-has-been-registered"></a>D. Détermination de si un filtre spécifique est inscrit  
 L'exemple suivant répertorie le filtre pour le composant .xdoc si celui-ci a été installé manuellement sur le système et enregistré sur l'instance de serveur.  
  
```  
EXEC sp_help_fulltext_system_components 'filter', '.xdoc';  
GO  
```  
  
 Par défaut, ce filtre n'est pas installé, de sorte que le jeu de résultats est vide.  
  
### <a name="e-listing-a-specific-dll-file"></a>E. Affichage d'un fichier .dll spécifique  
 L'exemple suivant affiche un fichier .ddl spécifique, `nlhtml.dll`, installé par défaut.  
  
```  
EXEC sp_help_fulltext_system_components 'fullpath',   
   'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\nlhtml.dll';  
GO  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher ou modifier des filtres et des analyseurs lexicaux inscrits](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md)   
 [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurer et gérer des filtres pour la recherche](../../relational-databases/search/configure-and-manage-filters-for-search.md)   
 [Procédures stockées de recherche en texte intégral et de recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
