---
description: sys.external_languages (Transact-SQL)-SQL Server
title: sys.external_languages (Transact-SQL)-SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- external_languages
- external_languages_TSQL
- sys.external_languages
- sys.external_languages_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.external_languages catalog view
author: nelgson
ms.author: negust
ms.reviewer: dphansen
manager: cgronlun
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: 425971304e516670bd15bfcdcc0a3b5777091cc8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061597"
---
# <a name="sysexternal_languages-transact-sql"></a>sys.external_languages (Transact-SQL)
[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Cet affichage catalogue fournit une liste des langues externes dans la base de données. **R** et **Python** étant des noms réservés, aucun langage externe ne peut être créé avec ces derniers.

## <a name="sysexternal_languages"></a>sys.external_languages

L’affichage catalogue sys.external_languages répertorie une ligne pour chaque langue externe de la base de données.

|Nom de la colonne |Type de données | Description|
|------|------|------|
|external_language_id |int | ID de la langue externe|
|langage |sysname |Nom de la langue externe. Unique dans la base de données. R et Python sont des noms réservés par instance|
|create_date |datetime2 |Date et heure de création|
|principal_id |int |ID du principal propriétaire de cette bibliothèque externe|

## <a name="see-also"></a>Voir également  

+ [sys.external_language_files](sys-external-language-files-transact-sql.md)  
+ [CRÉER UNE LANGUE EXTERNE](../../t-sql/statements/create-external-language-transact-sql.md) 
