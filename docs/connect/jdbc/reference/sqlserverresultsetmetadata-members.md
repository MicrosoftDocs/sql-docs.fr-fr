---
description: Membres de SQLServerResultSetMetaData
title: Membres de SQLServerResultSetMetaData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 37587981-2979-49a3-a6ab-df4bfb9b8748
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d10abafa4c3fb2f3bccc7d0ee897cc55a2c92e37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180071"
---
# <a name="sqlserverresultsetmetadata-members"></a>Membres de SQLServerResultSetMetaData
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants présentent les membres exposés par la classe [SQLServerResultSetMetaData](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md).  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Nom|Description|  
|----------|-----------------|  
|java.sql.ResultSetMetaData|columnNoNulls, columnNullable, columnNullableUnknown|  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[getCatalogName](../../../connect/jdbc/reference/getcatalogname-method-sqlserverresultsetmetadata.md)|Obtient le nom de catalogue pour la table qui inclut la colonne désignée.|  
|[getColumnClassName](../../../connect/jdbc/reference/getcolumnclassname-method-sqlserverresultsetmetadata.md)|Retourne le nom complet de la classe Java dont les instances sont créées si la méthode [getObject](../../../connect/jdbc/reference/getobject-method-sqlserverresultset.md) de la classe [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) est appelée pour récupérer une valeur à partir de la colonne.|  
|[getColumnCount](../../../connect/jdbc/reference/getcolumncount-method-sqlserverresultsetmetadata.md)|Retourne le nombre de colonnes dans le jeu de résultats.|  
|[getColumnDisplaySize](../../../connect/jdbc/reference/getcolumndisplaysize-method-sqlserverresultsetmetadata.md)|Retourne la largeur maximale normale, en caractères, de la colonne désignée.|  
|[getColumnLabel](../../../connect/jdbc/reference/getcolumnlabel-method-sqlserverresultsetmetadata.md)|Récupère le titre suggéré pour l’impression et l’affichage de la colonne désignée.|  
|[getColumnName](../../../connect/jdbc/reference/getcolumnname-method-sqlserverresultsetmetadata.md)|Récupère le nom de la colonne désignée.|  
|[getColumnType](../../../connect/jdbc/reference/getcolumntype-method-sqlserverresultsetmetadata.md)|Récupère le type SQL de la colonne désignée.|  
|[getColumnTypeName](../../../connect/jdbc/reference/getcolumntypename-method-sqlserverresultsetmetadata.md)|Récupère le nom de type spécifique à la base de données de la colonne désignée.|  
|[getPrecision](../../../connect/jdbc/reference/getprecision-method-sqlserverresultsetmetadata.md)|Obtient le nombre de décimales pour la colonne désignée.|  
|[getScale](../../../connect/jdbc/reference/getscale-method-sqlserverresultsetmetadata.md)|Obtient le nombre de chiffres à droite de la virgule décimale pour la colonne désignée.|  
|[getSchemaName](../../../connect/jdbc/reference/getschemaname-method-sqlserverresultsetmetadata.md)|Obtient le nom de schéma de la table pour la colonne désignée.|  
|[getTableName](../../../connect/jdbc/reference/gettablename-method-sqlserverresultsetmetadata.md)|Obtient le nom de table de la colonne désignée.|  
|[isAutoIncrement](../../../connect/jdbc/reference/isautoincrement-method-sqlserverresultsetmetadata.md)|Indique si la colonne désignée est numérotée automatiquement, la plaçant ainsi en lecture seule.|  
|[isCaseSensitive](../../../connect/jdbc/reference/iscasesensitive-method-sqlserverresultsetmetadata.md)|Indique si une colonne respecte la casse.|  
|[isCurrency](../../../connect/jdbc/reference/iscurrency-method-sqlserverresultsetmetadata.md)|Indique si la colonne désignée est une valeur de devise.|  
|[isDefinitelyWritable](../../../connect/jdbc/reference/isdefinitelywritable-method-sqlserverresultsetmetadata.md)|Indique si une écriture dans la colonne désignée réussit toujours.|  
|[isNullable](../../../connect/jdbc/reference/isnullable-method-sqlserverresultsetmetadata.md)|Indique l'acceptation des valeurs NULL dans la colonne désignée.|  
|[isReadOnly](../../../connect/jdbc/reference/isreadonly-method-sqlserverresultsetmetadata.md)|Indique si la colonne désignée n'accepte jamais d'écriture.|  
|[isSearchable](../../../connect/jdbc/reference/issearchable-method-sqlserverresultsetmetadata.md)|Indique si la colonne désignée peut être utilisée dans une clause SQL WHERE.|  
|[isSigned](../../../connect/jdbc/reference/issigned-method-sqlserverresultsetmetadata.md)|Indique si les valeurs de la colonne désignée sont des nombres signés.|  
|[isSparseColumnSet](../../../connect/jdbc/reference/issparsecolumnset-method-sqlserverresultsetmetadata.md)|Indique si une colonne dans un jeu de résultats est un jeu de colonnes éparses.|  
|[isWritable](../../../connect/jdbc/reference/iswritable-method-sqlserverresultsetmetadata.md)|Indique si une écriture dans la colonne désignée peut réussir.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSetMetaData, classe](../../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md)  
  
  
