---
description: Méthode setString (long, java.lang.String, int, int)
title: setString, méthode (long, java.lang.String, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.setString (long, java.lang.String, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9fb59b09-e825-46a6-ba5d-85d4a8dc143a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1084ba6293ec6fa8056306558ec4d463848f312e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173050"
---
# <a name="setstring-method-long-javalangstring-int-int"></a>Méthode setString (long, java.lang.String, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Écrit la chaîne donnée dans le CLOB, en démarrant à la position spécifiée et en fonction du décalage et de la longueur donnés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int setString(long pos,  
                     java.lang.String str,  
                     int offset,  
                     int len)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *pos*  
  
 Position à laquelle démarrer l'écriture dans l'objet CLOB.  
  
 *str*  
  
 Chaîne à écrire sur l'objet CLOB.  
  
 *offset*  
  
 Décalage dans la chaîne à partir duquel démarrer la lecture des caractères.  
  
 *len*  
  
 Nombre de caractères à écrire.  
  
## <a name="return-value"></a>Valeur de retour  
 Nombre de caractères écrits.  
  
## <a name="exceptions"></a>Exceptions  
 java.sql.SQLException  
  
## <a name="remarks"></a>Notes  
 Cette méthode setString est spécifiée par la méthode setString de l’interface java.sql.NClob.  
  
 Les données de caractères sont remplacées, en démarrant à la position spécifiée et peuvent remplacer la longueur initiale de l'objet CLOB. La spécification d'une valeur position+1 permet d'ajouter la chaîne. La spécification d'une valeur position+2 ou supérieure (ou de zéro ou d'une valeur inférieure) génère une erreur de position.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode setString &#40;SQLServerClob&#41;](../../../connect/jdbc/reference/setstring-method-sqlserverclob.md)   
 [SQLServerClob, méthodes](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [SQLServerClob, membres](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Classe SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
