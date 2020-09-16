---
description: Méthode setString (long, java.lang.String)
title: Méthode setString (long, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerClob.setString (long, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1b2190e9-5ace-497a-8554-0e913ea9b0cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 147cca169c1106557c36e644f7166da6cad18b3f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88450727"
---
# <a name="setstring-method-long-javalangstring"></a>Méthode setString (long, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Écrit la chaîne **String** donnée dans le CLOB, en commençant à la position donnée.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public int setString(long pos,  
                     java.lang.String s)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *pos*  
  
 Position à laquelle démarrer l'écriture dans l'objet CLOB.  
  
 *s*  
  
 String à écrire sur le **CLOB**.  
  
## <a name="return-value"></a>Valeur de retour  
 Nombre de caractères écrits.  
  
## <a name="exceptions"></a>Exceptions  
 java.sql.SQLException  
  
## <a name="remarks"></a>Notes  
 Cette méthode setString est spécifiée par la méthode setString de l’interface java.sql.NClob.  
  
 Les données de caractères sont remplacées en démarrant à la position spécifiée et peuvent dépasser la longueur initiale de l'objet CLOB. La spécification d'une valeur position+1 permet d'ajouter la chaîne. La spécification d'une valeur position+2 ou supérieure (ou de zéro ou d'une valeur inférieure) entraîne la levée d'une erreur de position.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode setString &#40;SQLServerClob&#41;](../../../connect/jdbc/reference/setstring-method-sqlserverclob.md)   
 [SQLServerClob, méthodes](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [SQLServerClob, membres](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Classe SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
