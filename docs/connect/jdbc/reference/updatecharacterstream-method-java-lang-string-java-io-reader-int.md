---
description: Méthode updateCharacterStream (java.lang.String, java.io.Reader, int)
title: Méthode updateCharacterStream (java.lang.String, java.io.Reader, int)
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateCharacterStream (java.lang.String, java.io.Reader, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 08cfc4e0-83f0-4f2f-ac55-b381f34fe67f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 73ce13d5b1a40c4aa89dcffe99f55a0e82c67eba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188263"
---
# <a name="updatecharacterstream-method-javalangstring-javaioreader-int"></a>Méthode updateCharacterStream (java.lang.String, java.io.Reader, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur de flux de caractères, qui dispose du nombre spécifié de caractères.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateCharacterStream(java.lang.String columnName,  
                                  java.io.Reader readerValue,  
                                  int length)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnName*  
  
 Valeur **String** qui contient le nom de la colonne.  
  
 *readerValue*  
  
 Objet Reader.  
  
 *length*  
  
 **int** indiquant la longueur du flux.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateCharacterStream est spécifiée par la méthode updateCharacterStream de l’interface java.sql.ResultSet.  
  
 Cette méthode passe les caractères Unicode à partir d’un objet Reader à des colonnes de texte et binaires sélectionnées. Cela inclut toutes les colonnes de texte et les colonnes **binary**, **varbinary**, **varbinary(max)** , **image** et **xml**, mais pas les colonnes **udt**.  
  
 Si la longueur du flux diffère de celle spécifiée dans le paramètre *length*, le pilote JDBC lève une exception lors de la mise à jour ou de l’insertion de la ligne.  
  
 Si la longueur du flux est inconnue, le paramètre *length* peut être défini sur -1 pour indiquer que le pilote doit accepter le flux, quelle que soit sa longueur. Avec sqljdbc4.jar, nous vous recommandons d’utiliser la méthode JDBC 4.0 [updateCharacterStream, méthode &#40;java.lang.String, java.io.Reader&#41;](../../../connect/jdbc/reference/updatecharacterstream-method-java-lang-string-java-io-reader.md) quand l’application veut mettre à jour la colonne à partir d’un flux dont la longueur est inconnue.  
  
## <a name="see-also"></a>Voir aussi  
 [updateCharacterStream, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatecharacterstream-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
