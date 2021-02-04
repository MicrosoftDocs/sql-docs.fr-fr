---
description: Méthode getUser (SQLServerDataSource)
title: Méthode getUser (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getUser
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3513dd7f-6ae5-4010-bde0-454ac4365bce
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4bd3ac0a7c17d0bd10e8709194c8a13eef7eb25d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154719"
---
# <a name="getuser-method-sqlserverdatasource"></a>Méthode getUser (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Retourne le nom d'utilisateur qui est utilisé pour la connexion à la source de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getUser()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **Chaîne** qui contient le nom de l’utilisateur.  
  
## <a name="remarks"></a>Notes  
 La méthode [setUser](../../../connect/jdbc/reference/setuser-method-sqlserverdatasource.md) définit le nom d’utilisateur à utiliser lors de la connexion à l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si la valeur de nom d'utilisateur n'est pas définie, la méthode getUser retourne la valeur par défaut Null.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
