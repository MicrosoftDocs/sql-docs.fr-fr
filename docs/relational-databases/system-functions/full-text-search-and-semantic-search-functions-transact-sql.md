---
description: Fonctions de recherche en texte intégral et de recherche sémantique (Transact-SQL)
title: Fonctions de recherche de Full-Text et de recherche sémantique (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- semantic search [SQL Server], system functions
ms.assetid: a61a3694-7604-4583-962e-fc30f771c6fa
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 29b1abedf27c163cd21ab0113896c4a3ebaae459
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207468"
---
# <a name="full-text-search-and-semantic-search-functions-transact-sql"></a>Fonctions de recherche en texte intégral et de recherche sémantique (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette section décrit les fonctions système relatives à la recherche en texte intégral et à la recherche sémantique.  
  
## <a name="full-text-search-functions"></a>Fonctions de recherche en texte intégral  
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)  
 Retourne une table composée de zéros, d'une ou de plusieurs ligne(s) pour les colonnes contenant des correspondances exactes ou floues (moins précises) pour des mots simples ou des expressions, la proximité de mots à une certaine distance les uns des autres ou des correspondances pondérées.  
  
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)  
 Retourne une table de zéro, une ou plusieurs lignes pour les colonnes qui contiennent des valeurs qui correspondent à la signification, et pas seulement à la formulation exacte, du texte dans le *freetext_string* spécifié.  
  
## <a name="semantic-search-functions"></a>Fonctions de recherche sémantique  
 [semantickeyphrasetable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semantickeyphrasetable-transact-sql.md)  
 Retourne une table de zéro, une ou plusieurs lignes pour les expressions clés associées aux colonnes de la table spécifiée.  
  
 [semanticsimilaritydetailstable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritydetailstable-transact-sql.md)  
 Retourne une table de zéro, une ou plusieurs lignes d'expressions clés communes à deux documents (document source et document mis en correspondance) dont le contenu est similaire d'un point de vue sémantique.  
  
 [semanticsimilaritytable &#40;Transact-SQL&#41;](../../relational-databases/system-functions/semanticsimilaritytable-transact-sql.md)  
 Retourne une table de zéro, une ou plusieurs lignes pour les colonnes dont le contenu est sémantiquement similaire à un document spécifié.  
  
  
