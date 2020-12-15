---
description: SQLRowCount
title: SQLRowCount | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLRowCount function
ms.assetid: 967ed3d4-3d31-4485-ac92-027076ebc829
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 16990f0abf057c7df6aedb8f3c15b7b489bf94e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483421"
---
# <a name="sqlrowcount"></a>SQLRowCount
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Lorsque des tableaux de valeurs de paramètre sont liés pour l'exécution d'une instruction, **SQLRowCount** retourne SQL_ERROR si une ligne de valeurs de paramètre génère une condition d'erreur dans l'exécution d'une instruction. Aucune valeur n'est retournée via l'argument *RowCountPtr* de la fonction.  
  
 L'application peut tirer parti de l'attribut d'instruction SQL_ATTR_PARAMS_PROCESSED_PTR pour capturer le nombre de paramètres traités avant l'erreur.  
  
 En outre, l'application peut utiliser un tableau de valeurs d'état, lié via l'utilisation de l'attribut d'instruction SQL_ATTR_PARAM_STATUS_PTR, pour capturer les décalages des lignes de paramètres incriminées du tableau. L'application peut parcourir le tableau de valeurs d'état pour déterminer le nombre réel de lignes traitées.  
  
 Lorsqu’une [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction INSERT, Update, Delete ou Merge avec une clause OUTPUT est exécutée, SQLRowCount ne retourne pas le nombre de lignes affectées tant que toutes les lignes du jeu de résultats générés par la clause OUTPUT n’ont pas été consommées. Pour consommer ces lignes, vous appelez SQLFetch ou SQLFetchScroll. SQLResultCols retourne-1 jusqu’à ce que toutes les lignes de résultat soient consommées. Une fois SQLFetch ou SQLFetchScroll retournée SQL_NO_DATA, l’application doit appeler SQLRowCount pour déterminer le nombre de lignes affectées avant d’appeler SQLMoreResults pour passer au résultat suivant.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLRowCount fonction)](../../odbc/reference/syntax/sqlrowcount-function.md)   
 [Détails de l’implémentation d’API ODBC](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
