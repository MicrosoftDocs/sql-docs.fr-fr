---
description: Méthode getBytes (SQLServerBlob)
title: Méthode getBytes (SQLServerBlob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.getBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bea1b810-b5c1-466d-bdc4-561468214632
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 32fd57b6be09a09036d3bd29c5e22ee9934e7ab8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168109"
---
# <a name="getbytes-method-sqlserverblob"></a>Méthode getBytes (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Obtient les données de BLOB sous forme de tableau d'octets.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public byte[] getBytes(long pos,  
                       int length)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *pos*  
  
 Position de départ à 1 (et non 0).  
  
 *length*  
  
 Longueur des données à obtenir.  
  
## <a name="return-value"></a>Valeur de retour  
 Tableau **d’octets** contenant les données demandées.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getBytes est spécifiée par la méthode getBytes de l’interface java.sql.Blob.  
  
 Si vous avez un BLOB de valeur Null ou de longueur zéro et que vous essayez d’obtenir exactement zéro octet à la position 1, un tableau **d’octets** vide (de longueur zéro) est retourné.  
  
 Si vous avez un BLOB de valeur Null ou de longueur zéro et que vous essayez d'obtenir une longueur d'octets quelconque à une position autre que la position 1, une exception de position est levée.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerBlob, méthodes](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [SQLServerBlob, membres](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob, classe](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
