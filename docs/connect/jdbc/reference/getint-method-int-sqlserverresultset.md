---
description: getInt, méthode (int) (SQLServerResultSet)
title: Méthode getInt (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.getInt (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c465ff91-ab96-41de-8917-96c4974c2624
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ff2b65d6254e0ea65ad2fd6830032c217b9fe50a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88435921"
---
# <a name="getint-method-int-sqlserverresultset"></a>getInt, méthode (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de l’index de colonne désigné dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **int** dans le langage de programmation Java.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int getInt(int columnIndex)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur **int**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getInt est spécifiée par la méthode getInt de l’interface java.sql.ResultSet.  
  
 Cette méthode est prise en charge seulement sur les types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui peuvent retourner de façon sécurisée une valeur entière, comme int, smallint, tinyint et bit. L'utilisation de cette méthode sur n'importe quel autre type de données entraîne la levée d'une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode getInt &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getint-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
