---
description: sys.fulltext_languages (Transact-SQL)
title: sys.fulltext_languages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_languages
- sys.fulltext_languages_TSQL
- fulltext_languages
- fulltext_languages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- languages [full-text search]
- sys.fulltext_languages catalog view
ms.assetid: 2ed6b53d-1cf2-4763-9d58-36ea24a610ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 47032ad75fb96842f278533e184307b99017c798
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191497"
---
# <a name="sysfulltext_languages-transact-sql"></a>sys.fulltext_languages (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Cet affichage catalogue contient une ligne par langue dont les analyseurs lexicaux sont enregistrés avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Chaque ligne affiche l'identificateur de paramètres régionaux (LCID) et le nom de la langue. Lorsque des analyseurs lexicaux sont inscrits pour une langue, ses autres ressources linguistiques (générateurs de formes dérivées, mots parasites (mots vides) et fichiers de dictionnaire des synonymes) sont disponibles pour les opérations d’indexation et d’interrogation de texte intégral. La valeur du **nom** ou du **LCID** peut être spécifiée dans les requêtes de texte intégral et les instructions d’index de recherche en texte intégral [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
   
|Colonne|Type de données|Description|  
|------------|---------------|-----------------|  
|**lcid**|**int**|Identificateur des paramètres régionaux (LCID) [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows de la langue.|  
|**name**|**sysname**|Valeur de l’alias dans [sys.syslangues](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) correspondant à la valeur de **LCID** ou représentation sous forme de chaîne du LCID numérique.|  
  
## <a name="values-returned-for-default-languages"></a>Valeurs retournées pour les langues par défaut  
 Le tableau suivant présente uniquement les valeurs des langues dont les analyseurs lexicaux sont inscrits par défaut.  
  
|Language|LCID|  
|--------------|----------|  
|Arabe|1025|  
|Bengali (India)|1093|  
|British English|2057|  
|Bulgare|1026|  
|Catalan|1027|  
|Chinois (Hong Kong R.A.S., RPC)|3076|  
|Chinese (Macao (R.A.S.))|5124|  
|Chinese (Singapore)|4100|  
|Croate|1050|  
|Tchèque|1029|  
|Danois|1030|  
|Néerlandais|1043|  
|Anglais|1033|  
|Français|1036|  
|Allemand|1031|  
|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Grec|1032|  
|Goudjrati|1095|  
|Hébreu|1037|  
|Hindi|1081|  
|Islandais|1039|  
|Indonésien|1057|  
|Italien|1040|  
|Japonais|1041|  
|Kannada|1099|  
|Coréen|1042|  
|Letton|1062|  
|Lituanien|1063|  
|Malay - Malaysia|1086|  
|Malayalam|1100|  
|Marathi|1102|  
|Neutre|0|  
|Norvégien (bokmål)|1044|  
|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Polonais|1045|  
|Portugais (Brésil)|1046|  
|Portugais (Portugal)|2070|  
|Pendjabi|1094|  
|Roumain|1048|  
|Russe|1049|  
|Serbe (cyrillique)|3098|  
|Serbe (latin)|2074|  
|Chinois simplifié|2052|  
|Slovaque|1051|  
|Slovène|1060|  
|Espagnol|3082|  
|Suédois|1053|  
|Tamoul|1097|  
|Télougou|1098|  
|Thaï|1054|  
|Chinois traditionnel|1028|  
|**S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et versions ultérieures.<br /><br /> Turc|1055|  
|Ukrainien|1058|  
|Ourdou|1056|  
|Vietnamien|1066|  
  
## <a name="remarks"></a>Notes  
 Pour mettre à jour la liste des langues inscrites avec la recherche en texte intégral, utilisez [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)'**update_languages**'.  
  
## <a name="permissions"></a>Autorisations  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Voir aussi  
 [sp_fulltext_load_thesaurus_file &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)   
 [sp_fulltext_service &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)   
 [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurer et gérer les fichiers de dictionnaire des synonymes pour la recherche de Full-Text](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [Configurer et gérer les mots vides et listes de mots vides pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [Mise à niveau de la fonction de recherche en texte intégral](../../relational-databases/search/upgrade-full-text-search.md)  
  
  
