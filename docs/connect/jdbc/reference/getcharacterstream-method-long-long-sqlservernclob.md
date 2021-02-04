---
description: Méthode getCharacterStream (long, long) (SQLServerNClob)
title: getCharacterStream, méthode (long, long) (SQLServerNClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5a8028bc-c877-4668-b662-0746d462040e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dea79fc0a80221462e3c01a9c3b9ca625b77dd86
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168084"
---
# <a name="getcharacterstream-method-long-long-sqlservernclob"></a>Méthode getCharacterStream (long, long) (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne les données **NCLOB** sous forme d’objet **Reader** ou de flux de caractères avec une position et une longueur spécifiées.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.io.Reader getCharacterStream(long pos,  
                                  long length)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *pos*  
  
 **long** indiquant le décalage du premier octet vers le premier caractère de la valeur partielle à récupérer.  
  
 *length*  
  
 **long** indiquant la longueur en caractères de la valeur partielle à récupérer.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet Reader contenant les données **NCLOB**.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getCharacterStream est spécifiée par la méthode getCharacterStream de l’interface java.sql.NClob.  
  
## <a name="see-also"></a>Voir aussi  
 [getCharacterStream, méthode &#40;SQLServerNClob&#41;](../../../connect/jdbc/reference/getcharacterstream-method-sqlservernclob.md)   
 [SQLServerNClob, méthodes](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [SQLServerNClob, membres](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [SQLServerNClob, classe](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
