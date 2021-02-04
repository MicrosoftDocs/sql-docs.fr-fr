---
description: Méthode getPropertyInfo (SQLServerDriver)
title: Méthode getPropertyInfo (SQLServerDriver) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDriver.getPropertyInfo
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b5eaad8a-31ef-44ac-af11-d5caa13ac3e2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c090ff7d42c67924f0e097713edc59557aa385eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175224"
---
# <a name="getpropertyinfo-method-sqlserverdriver"></a>Méthode getPropertyInfo (SQLServerDriver)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Utilisée pour découvrir les propriétés requises pour la connexion à une base de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.DriverPropertyInfo[] getPropertyInfo(java.lang.String Url,  
                                                     java.util.Properties Info)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Url*  
  
 Valeur **String** contenant l’URL utilisée pour la connexion à la base de données.  
  
 *Info*  
  
 Liste de paires de valeurs de propriété, Null à la première utilisation.  
  
## <a name="return-value"></a>Valeur de retour  
 Tableau d’objets DriverPropertyInfo.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode getPropertyInfo est spécifiée par la méthode getPropertyInfo de l’interface java.sql.Driver.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDriver, méthodes](../../../connect/jdbc/reference/sqlserverdriver-methods.md)   
 [SQLServerDriver, membres](../../../connect/jdbc/reference/sqlserverdriver-members.md)   
 [Classe SQLServerDriver](../../../connect/jdbc/reference/sqlserverdriver-class.md)  
  
  
