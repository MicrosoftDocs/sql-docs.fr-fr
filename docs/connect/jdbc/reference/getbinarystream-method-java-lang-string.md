---
description: Méthode getBinaryStream (java.lang.String)
title: Méthode getBinaryStream (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBinaryStream (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 149609b5-a6de-4e23-a440-7061775d0899
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d9457c8042d41b3044e3ceaeb25ffe2bc5d36a97
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165725"
---
# <a name="getbinarystream-method-javalangstring"></a>Méthode getBinaryStream (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur du nom de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant que flux binaire d’octets non interprétés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.io.InputStream getBinaryStream(java.lang.String columnName)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnName*  
  
 Valeur **String** qui contient le nom de la colonne.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet InputStream.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getBinaryStream est spécifiée par la méthode getBinaryStream de l’interface java.sql.ResultSet.  
  
 Cette méthode peut être utilisée seulement avec des types de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] binary, varbinary, varbinary(max) et image. Si vous essayez de l'utiliser avec d'autres types de données, une exception est levée.  
  
 Une fois que cette méthode a obtenu la valeur sous forme de flux, cette valeur peut être lue par segments à partir du flux. Cette méthode convient particulièrement à la récupération de grandes valeurs LONGVARBINARY.  
  
> [!NOTE]  
>  Toutes les données figurant dans le flux retourné doivent être lues avant d'obtenir la valeur de toute autre colonne. L'appel suivant à une méthode getter fermera implicitement le flux. De même, un flux peut retourner 0 quand la méthode InputStream.available est appelée, que des données soient ou non disponibles.  
  
## <a name="see-also"></a>Voir aussi  
 [getBinaryStream, méthode (SQLServerResultSet)](../../../connect/jdbc/reference/getbinarystream-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
