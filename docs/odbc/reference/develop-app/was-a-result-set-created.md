---
description: Un jeu de résultats a-t-il été créé ?
title: Un jeu de résultats a-t-il été créé ? | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- result sets [ODBC], determining if created
ms.assetid: 4a83b8cb-2d57-4e64-b497-80bd587ee1f9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 57b65c254f48d9c3f5078c3b2c1f576ae54d4740
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88421383"
---
# <a name="was-a-result-set-created"></a>Un jeu de résultats a-t-il été créé ?
Dans la plupart des cas, les programmeurs d’applications savent si les instructions exécutées par leur application créent un jeu de résultats. C’est le cas si l’application utilise des instructions SQL codées en dur écrites par le programmeur. C’est généralement le cas lorsque l’application construit des instructions SQL au moment de l’exécution : le programmeur peut facilement inclure du code qui signale si une instruction **Select** ou une instruction **Insert** est en cours de construction. Dans certains cas, le programmeur ne peut pas savoir si une instruction crée un jeu de résultats. Cela est vrai si l’application permet à l’utilisateur d’entrer et d’exécuter une instruction SQL. C’est également le cas lorsque l’application construit une instruction au moment de l’exécution pour exécuter une procédure.  
  
 Dans ce cas, l’application appelle **SQLNumResultCols** pour déterminer le nombre de colonnes dans le jeu de résultats. Si la valeur est 0, l’instruction n’a pas créé de jeu de résultats ; s’il s’agit d’un autre nombre, l’instruction a créé un jeu de résultats.  
  
 L’application peut appeler **SQLNumResultCols** à tout moment une fois l’instruction préparée ou exécutée. Toutefois, étant donné que certaines sources de données ne peuvent pas facilement décrire les jeux de résultats qui seront créés par des instructions préparées, les performances seront affectées si **SQLNumResultCols** est appelé après la préparation d’une instruction mais avant son exécution.  
  
 Certaines sources de données prennent également en charge la détermination du nombre de lignes retournées par une instruction SQL dans un jeu de résultats. Pour ce faire, l’application appelle **SQLRowCount**. Le nombre exact de lignes représenté est indiqué par le paramètre de l’option SQL_DYNAMIC_CURSOR_ATTRIBUTES2, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2, SQL_KEYSET_CURSOR_ATTRIBUTES2 ou SQL_STATIC_CURSOR_ATTRIBUTES2 (selon le type du curseur) retourné par un appel à **SQLGetInfo**. Ce masque de masque indique pour chaque type de curseur si le nombre de lignes retourné est exact, approximatif ou n’est pas disponible du tout. Le nombre de lignes pour les curseurs statiques ou de jeu de clés est affecté par les modifications effectuées via **SQLBulkOperations** ou **SQLSetPos**, ou par les instructions Update ou DELETE positionnées, dépend d’autres bits retournés par les mêmes arguments d’option répertoriés précédemment. Pour plus d’informations, consultez la description de la fonction [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) .
