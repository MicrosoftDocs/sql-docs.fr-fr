---
description: Méthode updateAsciiStream (int, java.io.InputStream, long)
title: Méthode updateAsciiStream (int, java.io.InputStream, long)
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 143bff3e-2b5c-485d-9529-1c2387560094
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a208ff14f0830820971b1944751042790fdb4d9c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182106"
---
# <a name="updateasciistream-method-int-javaioinputstream-long"></a>Méthode updateAsciiStream (int, java.io.InputStream, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur de flux ASCII, qui disposera du nombre spécifique d'octets.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateAsciiStream(int columnIndex,  
                              java.io.InputStream x,  
                              long length)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
 *x*  
  
 Objet InputStream.  
  
 *length*  
  
 Longueur du flux.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode updateAsciiStream est spécifiée par la méthode updateAsciiStream de l’interface java.sql.ResultSet.  
  
 Cette méthode passe les caractères ASCII (octets) à partir d’un objet InputStream vers des colonnes de caractères convertibles, qui correspondent à la plage ASCII [0x00 - 0x7F] Unicode et aux pages de codes 874, 932, 936, 949, 950 et 1250 jusqu’à 1258. Cette méthode effectue une conversion de la page de classement de destination. La tentative de mise à jour d'une colonne de destination non convertible entraîne la levée d'une exception. Pour les colonnes binaires, des octets bruts sont passés.  
  
 Si la longueur du flux diffère de celle spécifiée dans le paramètre *length*, le pilote JDBC lève une exception lors de la mise à jour ou de l’insertion de la ligne.  
  
 Si la longueur du flux est inconnue, le paramètre *length* peut être défini sur -1 pour indiquer que le pilote doit accepter le flux, quelle que soit sa longueur. Avec sqljdbc4.jar, nous vous recommandons d’utiliser la méthode JDBC 4.0 [updateAsciiStream, méthode &#40;int, java.io.InputStream&#41;](../../../connect/jdbc/reference/updateasciistream-method-int-java-io-inputstream.md) quand l’application veut mettre à jour la colonne à partir d’un flux dont la longueur est inconnue.  
  
## <a name="see-also"></a>Voir aussi  
 [updateAsciiStream, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
