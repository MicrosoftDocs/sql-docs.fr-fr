---
description: Méthode getCharacterStream (int)
title: Méthode getCharacterStream (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.getCharacterStream (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4f9f230d-be4c-469a-b3dc-f24531429aae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fae765e165eaefd67caf5f07e32c4d61f479c74a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88436861"
---
# <a name="getcharacterstream-method-int"></a>Méthode getCharacterStream (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de l’index de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet java.io.Reader.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.io.Reader getCharacterStream(int columnIndex)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet Reader.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getCharacterStream est spécifiée par la méthode getCharacterStream de l’interface java.sql.ResultSet.  
  
 Cette méthode lit uniquement les types de données de caractères Unicode [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], comme nchar, nvarchar, nvarchar(max) et ntext. Tous les autres types de données, dont les types de caractères ASCII, entraînent la levée d'une exception. Pour lire les types de données ASCII, utilisez la méthode [getAsciiStream](../../../connect/jdbc/reference/getasciistream-method-sqlserverresultset.md).  
  
## <a name="see-also"></a>Voir aussi  
 [getCharacterStream, méthode (SQLServerResultSet)](../../../connect/jdbc/reference/getcharacterstream-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
