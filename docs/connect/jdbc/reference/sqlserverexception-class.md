---
description: Classe SQLServerException
title: Classe SQLServerException | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: af5ef257-7cf6-4db3-b1ee-07d22d82bef1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 706312c690052a89e2243c2714a151a56c4f200b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178224"
---
# <a name="sqlserverexception-class"></a>Classe SQLServerException
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente une exécution incorrecte ou incomplète d'une instruction SQL.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Étend :** java.sql.SQLException  
  
 **Implémente :** java.io.Serializable  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final class SQLServerException  
```  
  
## <a name="remarks"></a>Notes  
 La classe SQLServerException gère à la fois les codes d'état SQL 92 et XOPEN. Il est possible de passer de l'un à l'autre à l'aide d'une propriété de connexion spécifiée par l'utilisateur. Les exceptions sont écrites dans les fichiers journaux ouverts qui ont été spécifiés.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerException, membres](../../../connect/jdbc/reference/sqlserverexception-members.md)   
 [Informations de référence sur l'API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
