---
description: Méthode updateNClob (int, java.io.Reader)
title: Méthode updateNClob (int, java.io.Reader) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 17adafd4-3ac3-4ff0-af9d-f087cc5ef936
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ae07680847077fae24e51487e1cbe18819591875
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88353195"
---
# <a name="updatenclob-method-int-javaioreader"></a>Méthode updateNClob (int, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Met à jour la colonne désignée avec l’objet **Reader** spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void updateNClob(int columnIndex,  
                        java.io.Reader reader)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *columnIndex*  
  
 **int** indiquant l’index de la colonne.  
  
 *reader*  
  
 Objet Reader.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getNClob est spécifiée par la méthode getNClob de l’interface java.sql.ResultSet.  
  
 Cette méthode n’est prise en charge que sur les colonnes **nvarchar(max)**, **ntext** et **xml**. L'utilisation de cette méthode sur n'importe quel autre type de données entraîne la levée d'une exception.  
  
## <a name="see-also"></a>Voir aussi  
 [updateNClob, méthode &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)   
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
