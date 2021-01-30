---
description: Affichages catalogue de recherche en texte intégral et de recherche sémantique (Transact-SQL)
title: Affichages catalogue de recherche Full-Text et sémantique (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], full-text search
- full-text search [SQL Server], catalog views
- full-text indexes [SQL Server], catalog views
ms.assetid: b08ad2fd-e3d8-458f-96f1-678217e0f419
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 90a7bcde4caa40158bf55314c84054bb94a8426f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194617"
---
# <a name="full-text-search-and-semantic-search-catalog-views-transact-sql"></a>Affichages catalogue de recherche en texte intégral et de recherche sémantique (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette section décrit les affichages catalogue qui fournissent des informations sur les index de recherche en texte intégral et les index sémantiques.  
  
## <a name="full-text-search-catalog-views"></a>Affichages catalogue de recherche en texte intégral  
 [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)  
 Contient une ligne pour chaque catalogue de texte intégral.  
  
 [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md)  
 Retourne une ligne pour chaque type de document qui est disponible pour des opérations d'indexation de texte intégral. Chaque ligne représente l’interface **IFilter** qui est inscrite dans l’instance de SQL Server.  
  
 [sys.fulltext_index_catalog_usages](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)  
 Retourne une ligne pour chaque catalogue de texte intégral vers une référence d'index de recherche en texte intégral.  
  
 [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)  
 Contient une ligne pour chaque colonne qui fait partie d'un index de recherche en texte intégral.  
  
 [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)  
 Contient une ligne pour chaque fragment d'index de recherche en texte intégral dans chaque table qui contient un index.  
  
 [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)  
 Contient une ligne par index de recherche en texte intégral d'un objet tabulaire.  
  
 [sys.fulltext_languages](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)  
 Contient une ligne par langue dont les analyseurs lexicaux sont inscrits avec SQL Server. Chaque ligne affiche l'identificateur de paramètres régionaux (LCID) et le nom de la langue.  
  
 [sys.fulltext_stoplists](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)  
 Contient une ligne par liste de mots vides de texte intégral dans la base de données.  
  
 [sys.fulltext_stopwords](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)  
 Contient une ligne par mot vide pour toutes les listes de mots vides de la base de données.  
  
 [sys.fulltext_system_stopwords](../../relational-databases/system-catalog-views/sys-fulltext-system-stopwords-transact-sql.md)  
 Fournit l'accès à la liste de mots vides système.  
  
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)  
 Contient une ligne pour chaque propriété de recherche contenue par une liste de propriétés de recherche sur la base de données actuelle.  
  
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
 Contient une ligne pour chaque liste de propriétés de recherche sur la base de données actuelle.  
  
## <a name="semantic-search-catalog-views"></a>Affichages catalogue de recherche sémantique  
 [sys.fulltext_semantic_language_statistics_database &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md)  
 Retourne une ligne relative à la base de données de statistiques linguistiques de sémantique installée sur l'instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [sys.fulltext_semantic_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md)  
 Retourne une ligne pour chaque langue dont le modèle statistique est inscrit avec l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsqu'un modèle linguistique est inscrit, cette langue est activée pour l'indexation sémantique.  
  
## <a name="see-also"></a>Voir aussi  
 [Vues système &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Affichages catalogue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Fonctions et vues de gestion dynamique de la recherche en texte intégral et de la recherche sémantique &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
