---
description: Méthode updateObject (int, java.lang.Object, int)
title: updateObject, méthode (int, java.lang.Object, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.updateObject (int, java.lang.Object, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9d33571b-4887-49d3-96df-8abda7b5a904
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1f55f80bc65ca1efd5f2d300e46f7f62db317f2c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88478311"
---
# <a name="updateobject-method-int-javalangobject-int"></a>Méthode updateObject (int, java.lang.Object, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec une valeur **Object** en fonction de l’index de colonne et de l’échelle.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateObject(int index,  
                         java.lang.Object x,  
                         int scale)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *index*  
  
 **int** indiquant l’index de la colonne.  
  
 *obj*  
  
 Valeur **Object**.  
  
 *scale*  
  
 Pour les types java.sql.Types.DECIMAL ou java.sql.Types.NUMERIC, il s'agit du nombre de chiffres après la virgule décimale. Pour les autres types, cette valeur est ignorée.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="see-also"></a>Voir aussi  
 [updateObject, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateobject-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
