---
description: Méthode position (java.lang.String, long) (SQLServerNClob)
title: position, méthode (java.lang.String, long) (SQLServerNClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 46d4beec-831a-449f-98b6-322a80cc499a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a16db92eb2181cfe6db2ee9e6b1c3039acb6df93
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88433051"
---
# <a name="position-method-javalangstring-long-sqlservernclob"></a>Méthode position (java.lang.String, long) (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Récupère la position de caractère à laquelle la sous-chaîne spécifiée *searchstr* apparaît dans la valeur **NCLOB** représentée par cet objet **NClob**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public long position(java.lang.String searchstr,  
              long start)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *searchstr*  
  
 Sous-chaîne à rechercher.  
  
 *start*  
  
 Position à laquelle démarrer la recherche ; la première position est 1.  
  
## <a name="return-value"></a>Valeur de retour  
 Position à laquelle la sous-chaîne doit s'afficher, ou -1 si elle est absente. La première position est 1.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode position est spécifiée par la méthode position de l’interface java.sql.NClob.  
  
## <a name="see-also"></a>Voir aussi  
 [Méthode position &#40;SQLServerNClob&#41;](../../../connect/jdbc/reference/position-method-sqlservernclob.md)   
 [SQLServerNClob, méthodes](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [SQLServerNClob, membres](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [SQLServerNClob, classe](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
