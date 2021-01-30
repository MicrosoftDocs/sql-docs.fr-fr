---
description: sys.external_language_files (Transact-SQL)-SQL Server
title: sys.external_language_files (Transact-SQL)-SQL Server | Microsoft Docs
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
ms.openlocfilehash: f283066ac99b4ef41b0fc6d46fc7b1a2a64b1270
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203781"
---
# <a name="sysexternal_language_files-transact-sql"></a>sys.external_language_files (Transact-SQL)
[!INCLUDE[SQL Server 2019](../../includes/applies-to-version/sqlserver2019.md)]

Cet affichage catalogue fournit une liste des fichiers d’extension de langage externe dans la base de données. **R** et **Python** étant des noms réservés, aucun langage externe ne peut être créé avec ces derniers.

Quand une langue externe est créée à partir d’un file_spec, l’extension elle-même et ses propriétés sont répertoriées dans cette vue. Cette vue contient une entrée par langue, par système d’exploitation.

## <a name="sysexternal_languages"></a>sys.external_languages

L’affichage catalogue sys.external_language_files répertorie une ligne pour chaque extension de langage externe dans la base de données. Paramètres

|Nom de la colonne |Type de données | Description|
|------|------|------|
|external_language_id |int | ID de la langue externe|
|contenu|varbinary(max) |Contenu du fichier d’extension de langage externe|
|file_name|nvarchar (266)|Nom du fichier d’extension de langage|
|plateforme|TINYINT|ID de la plateforme hôte sur laquelle SQL Server est installé|
|platform_desc |nvarchar(60)|Nom de la plateforme hôte. Les valeurs valides sont « WINDOWS », « LINUX ».|
|parameters|nvarchar(4000)|Prameters de langage externe|
|environment_variables |nvarchar(4000)|Variables d’environnement de langage externe|

## <a name="see-also"></a>Voir aussi  

+ [sys.external_languages](sys-external-languages-transact-sql.md)  
+ [CRÉER UNE LANGUE EXTERNE](../../t-sql/statements/create-external-language-transact-sql.md)  
