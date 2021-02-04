---
description: getTime, méthode (int, java.util.Calendar) (SQLServerResultSet)
title: getTime, méthode (int, java.util.Calendar) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getTime (int, java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d21e0c1d-9d6e-468f-8b11-cc7209b2c2e5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 77e2120caa53dbebbdf6a28b69b7ab86027d268d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174902"
---
# <a name="gettime-method-int-javautilcalendar-sqlserverresultset"></a>getTime, méthode (int, java.util.Calendar) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la valeur de l’index de la colonne désignée dans la ligne actuelle de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) en tant qu’objet java.sql.Time dans le langage de programmation Java, en utilisant l’objet Calendar donné.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.Time getTime(int columnIndex,  
                             java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
 *cal*  
  
 Objet de calendrier.  
  
## <a name="return-value"></a>Valeur de retour  
 Un objet Time.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getTime est spécifiée par la méthode getTime de l’interface java.sql.ResultSet.  
  
 Cette méthode retourne une partie heure valide d’un type de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] datetime ou smalldatetime, avec la partie date définie sur la date de référence Java 01/01/1970 dans le fuseau horaire du calendrier fourni.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode getTime &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/gettime-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
