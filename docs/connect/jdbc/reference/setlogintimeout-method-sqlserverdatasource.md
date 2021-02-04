---
description: Méthode setLoginTimeout (SQLServerDataSource)
title: Méthode setLoginTimeout (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setLoginTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b63d1cf4-dc1b-4e35-9a56-50436642eaaf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 725dae8c42fa83b92db0a56f8cd6b8c758fd4f6d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173384"
---
# <a name="setlogintimeout-method-sqlserverdatasource"></a>Méthode setLoginTimeout (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit le nombre de secondes pendant lesquelles cet objet [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) patiente lors de la tentative de connexion.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setLoginTimeout(int loginTimeout)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *loginTimeout*  
  
 Valeur **int** représentant le nombre de secondes à attendre. Zéro indique que le délai d'attente correspond au délai d'attente système par défaut, lequel est de 15 secondes.  
  
## <a name="remarks"></a>Notes  
 Cette méthode setLoginTimeout est spécifiée par la méthode setLoginTimeout de l’interface javax.sql.DataSource.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
