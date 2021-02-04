---
description: Méthode getSchemas (String, String)
title: Méthode getSchemas (chaîne, chaîne) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 672171ac-976f-4605-9bee-2a5e141d92cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02451ce51b9d8df378f5560d06d8e716694fc0aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175076"
---
# <a name="getschemas-method-string-string"></a>Méthode getSchemas (String, String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère les noms de schémas disponibles dans la base de données actuelle à l'aide du nom de catalogue spécifié et du nom de schéma.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public ResultSet getSchemas(java.lang.String catalog,  
                       java.lang.String schemaPattern)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *catalog*  
  
 Nom d'un catalogue dans la base de données. Si la chaîne est vide « », le résultat inclut les schémas sans catalogue. Si elle est **null**, le nom du catalogue n’est pas utilisé pour la recherche.  
  
 *schemaPattern*  
  
 Nom d'un schéma. Si elle est **null**, le nom du schéma n’est pas utilisé pour la recherche.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getSchemas est spécifiée par la méthode getSchemas de l’interface java.sql.DatabaseMetaData.  
  
 Le jeu de résultats retourné par la méthode getSchemas contient les informations suivantes :  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|TABLE_SCHEM|**Chaîne**|Nom du schéma.|  
|TABLE_CATALOG|**Chaîne**|Nom de catalogue pour le schéma.|  
  
 Les résultats sont classés par TABLE_CATALOG, puis par TABLE_SCHEM. Chaque ligne inclut TABLE_SCHEM comme première colonne et TABLE_CATALOG comme deuxième.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDatabaseMetaData, méthodes](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData, membres](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData, classe](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
