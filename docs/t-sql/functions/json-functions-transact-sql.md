---
description: Fonctions JSON (Transact-SQL)
title: Fonctions JSON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
helpviewer_keywords:
- JSON functions
ms.assetid: ec97d451-06af-44a3-8304-305d410cfc8e
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: c55ffdeec513c71c89cd5ed4bb8ef9f6e88e7330
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478610"
---
# <a name="json-functions-transact-sql"></a>Fonctions JSON (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Utilisez les fonctions décrites dans les pages de cette section pour valider ou modifier le texte JSON, ou pour extraire des valeurs simples ou complexes.  
  
|Fonction|Description|  
|--------------|-----------------|  
|[ISJSON](../../t-sql/functions/isjson-transact-sql.md)|Teste si une chaîne contient des données JSON valides.|  
|[JSON_VALUE](../../t-sql/functions/json-value-transact-sql.md)|Extrait une valeur scalaire d’une chaîne JSON.|  
|[JSON_QUERY](../../t-sql/functions/json-query-transact-sql.md)|Extrait un objet ou un tableau à partir d’une chaîne JSON.|  
|[JSON_MODIFY](../../t-sql/functions/json-modify-transact-sql.md)|Met à jour la valeur d’une propriété dans une chaîne JSON et renvoie la chaîne JSON mise à jour.|

 Pour plus d’informations sur la prise en charge intégrée de JSON dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Données JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md).  

## <a name="see-also"></a>Voir aussi

 - [Validate, Query, and Change JSON Data with Built-in Functions &#40;SQL Server&#41; [Valider, interroger et modifier les données JSON avec les fonctions intégrées &#40;SQL Server&#41;]](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md)
 - [Expressions de chemin JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)
 - [Données JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
