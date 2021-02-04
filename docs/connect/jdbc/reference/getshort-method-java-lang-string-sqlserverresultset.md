---
description: getShort, méthode (java.lang.String) (SQLServerResultSet)
title: getShort, méthode (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getShort (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 183af414-b0a3-4ca7-b160-d199bcf469b0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c25233a2ab9bb029949e4ed3a0b1ca5279654c23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175037"
---
# <a name="getshort-method-javalangstring-sqlserverresultset"></a>getShort, méthode (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur du nom de colonne désigné dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **short** dans le langage de programmation Java.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public short getShort(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnName*  
  
 Valeur **String** qui contient le nom de la colonne.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur **short**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getShort est spécifiée par la méthode getShort de l’interface java.sql.ResultSet.  
  
 Cette méthode est prise en charge seulement sur les types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui peuvent retourner une valeur entière de façon sécurisée, comme smallint, tinyint et bit. L'utilisation de cette méthode sur n'importe quel autre type de données entraîne la levée d'une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [getShort, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
