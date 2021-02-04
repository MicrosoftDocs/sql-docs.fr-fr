---
description: Méthode setWorkstationID (SQLServerDataSource)
title: Méthode setWorkstationID (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setWorkstationID
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c1093615-90bf-4918-9f05-8abd765ffb03
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8b0047a866a34f68fe1f4f2be9d51714f04fba73
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172910"
---
# <a name="setworkstationid-method-sqlserverdatasource"></a>Méthode setWorkstationID (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne le nom de l’ordinateur client utilisé pour se connecter à la source de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setWorkstationID(java.lang.String workstationID)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *workstationID*  
  
 **Chaîne** contenant le nom de l’ordinateur client.  
  
## <a name="remarks"></a>Remarques  
 Le workstationID est le nom de l'ordinateur client ou du poste de travail. Si la propriété workstationID n’est pas définie, la valeur par défaut est construite par un appel à la méthode InetAddress.getLocalHost().getHostName(). Si getHostName retourne une valeur vide, la méthode getHostAddress().toString() est appelée.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
