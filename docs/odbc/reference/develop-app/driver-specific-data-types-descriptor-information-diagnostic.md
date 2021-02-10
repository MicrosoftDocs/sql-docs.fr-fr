---
description: Types de données spécifique du pilote, types de descripteurs, types d’informations, types de diagnostics et attributs
title: Types de Driver-Specific-données, descripteur, informations, diagnostic | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver-specific diagnostic values [ODBC]
- diagnostic information [ODBC], driver-specific values
- ODBC drivers [ODBC], driver-specific diagnostic values
ms.assetid: ad4c76d3-5191-4262-b47c-5dd1d19d1154
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03cd6b0ed9a424a161f88f4bd525941895d3d201
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062564"
---
# <a name="driver-specific-data-types-descriptor-types-information-types-diagnostic-types-and-attributes"></a>Types de données spécifique du pilote, types de descripteurs, types d’informations, types de diagnostics et attributs
Les pilotes peuvent allouer des valeurs spécifiques au pilote pour les éléments suivants :  
  
-   **Indicateurs de type de données SQL** Celles-ci sont utilisées dans *ParameterType* dans **SQLBindParameter** et dans *DataType* dans **SQLGetTypeInfo** et retournées par **SQLColAttribute**, **SQLColumns**, **SQLDescribeCol**, **SQLGetTypeInfo**, **SQLDescribeParam**, **SQLProcedureColumns** et **SQLSpecialColumns**.  
  
-   **Champs de descripteur** Celles-ci sont utilisées dans *FieldIdentifier* dans **SQLColAttribute**, **SQLGetDescField** et **SQLSetDescField**.  
  
-   **Champs de diagnostic** Celles-ci sont utilisées dans *DiagIdentifier* dans **SQLGetDiagField** et **SQLGetDiagRec**.  
  
-   **Types d’informations** Celles-ci sont utilisées dans l' *infotype* dans **SQLGetInfo**.  
  
-   **Attributs de connexion et d’instruction** Celles-ci sont utilisées dans l' *attribut* dans **SQLGetConnectAttr**, **SQLGetStmtAttr**, **SQLSetConnectAttr** et **SQLSetStmtAttr**.  
  
 Pour chacun de ces éléments, il existe deux ensembles de valeurs : les valeurs réservées pour une utilisation par ODBC et les valeurs réservées pour une utilisation par les pilotes. Avant d’implémenter des valeurs spécifiques au pilote, un enregistreur de pilotes doit demander une valeur pour chaque type, champ ou attribut spécifique au pilote à partir d’un groupe ouvert. Pour le développement de nouveaux pilotes, utilisez la plage décrite dans le tableau ci-dessous. Le gestionnaire de pilotes ODBC 3,8 ne génère pas d’erreur si l’utilisation d’une valeur inconnue n’est pas comprise dans la plage décrite ci-dessous. Toutefois, les versions ultérieures du gestionnaire de pilotes peuvent générer une erreur si les valeurs inconnues qui ne sont pas comprises dans la plage sont reçues.  
  
 Quand l’une de ces valeurs est transmise à une fonction ODBC, le pilote doit vérifier si la valeur est valide. Les pilotes retournent SQLSTATE HYC00 (fonctionnalité facultative non implémentée) pour les valeurs spécifiques au pilote qui s’appliquent à d’autres pilotes.  
  
 À compter de ODBC 3,8, les rédacteurs de pilotes peuvent allouer des attributs spécifiques au pilote au sein d’une plage réservée.  
  
> [!NOTE]  
>  Le gestionnaire de pilotes ODBC 3,8 ne valide pas et n’applique pas ces plages pour la compatibilité descendante. Toutefois, une version future du gestionnaire de pilotes peut les appliquer.  
  
|Type d'attribut|Type de données ODBC|Base de plage spécifique au pilote|Limite de plage spécifique au pilote|Constante ODBC pour la base de plage de valeurs spécifique au pilote|  
|--------------------|--------------------|---------------------------------|----------------------------------|---------------------------------------------------------|  
|Indicateurs de type de données SQL|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_SQL_TYPE_BASE|  
|Champs de descripteur|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DESCRIPTOR_BASE|  
|Champs de diagnostic|SQLSMALLINT|0x4000|0x7FFF|SQL_DRIVER_DIAGNOSTIC_BASE|  
|Types d’informations|SQLUSMALLINT|0x4000|0x7FFF|SQL_DRIVER_INFO_TYPE_BASE|  
|Attributs de connexion|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_CONNECT_ATTR_BASE|  
|Attributs d’instruction|SQLINTEGER|0x00004000|0x00007FFF|SQL_DRIVER_STATEMENT_ATTR_BASE|  
  
> [!NOTE]  
>  Les types de données spécifiques au pilote, les champs de descripteur, les champs de diagnostic, les types d’informations, les attributs d’instruction et les attributs de connexion doivent être décrits dans la documentation du pilote. Quand l’une de ces valeurs est transmise à une fonction ODBC, le pilote doit vérifier si la valeur est valide. Les pilotes retournent SQLSTATE HYC00 (fonctionnalité facultative non implémentée) pour les valeurs spécifiques au pilote qui s’appliquent à d’autres pilotes.  
  
 Les valeurs de base sont définies pour faciliter le développement des pilotes. Par exemple, les attributs de diagnostic spécifiques au pilote peuvent être définis dans le format suivant :  
  
```  
SQL_DRIVER_DIAGNOSTIC_BASE+0, SQL_DRIVER_DIAGNOSTIC_BASE +1  
```
