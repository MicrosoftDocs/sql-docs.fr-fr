---
description: getByte, méthode (int) (SQLServerResultSet)
title: Méthode getByte (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.getByte (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b22ba097-6cb8-4c5d-916b-6360dd01d2c5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a94b4ca24a0d00ca45837e230cc3465e8728ffc
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88437081"
---
# <a name="getbyte-method-int-sqlserverresultset"></a>getByte, méthode (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de l’index de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que **octet** dans le langage de programmation Java.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public byte getByte(int columnIndex)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
## <a name="return-value"></a>Valeur de retour  
 Valeur **d’octet**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getByte est spécifiée par la méthode getByte de l’interface java.sql.ResultSet.  
  
 Cette méthode est prise en charge seulement sur les types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui peuvent retourner de façon sécurisée une valeur d’octet, comme tinyint et bit. Tous les autres types de données lèveront une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [getByte, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getbyte-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
