---
title: Classe SQLServerConnection
description: Découvrez les détails de l’API publique pour la classe SQLServerConnection dans JDBC Driver pour SQL Server.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 937292a6-1525-423e-a2b2-a18fd34c2893
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: df6b46047ed99b93d0baf45ea8a8a4f28721b875
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178381"
---
# <a name="sqlserverconnection-class"></a>Classe SQLServerConnection
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente une connexion JDBC à une base de données [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Implémente :** [ISQLServerConnection](../../../connect/jdbc/reference/isqlserverconnection-interface.md), java.io.Serializable  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public class SQLServerConnection  
```  
  
## <a name="remarks"></a>Notes  
 SQLServerConnection prend en charge la mise en pool de connexions JDBC sous la forme d’une connexion JDBC physique ou logique. SQLServerConnection gère le contrôle des transactions pour toutes les instructions qu’il crée et peut participer aux transactions XA distribuées gérées via un adaptateur XAResource.  
  
 SQLServerConnection gère un regroupement de handles d’instructions préparées. Les instructions préparées le sont une seule fois et sont généralement exécutées plusieurs fois avec des valeurs de données différentes pour leurs paramètres. Les instructions préparées sont également conservées entre deux fermetures d'une connexion logique (regroupée).  
  
> [!NOTE]  
>  SQLServerConnection n’est pas thread-safe. Toutefois, il est possible de traiter simultanément dans des threads simultanés plusieurs instructions créées à partir d'une seule connexion.  
  
 Cette classe prend en charge la désencapsulation (unwrapping) dans la classe SQLServerConnection, l’interface java.sql.connection et l’interface ISQLServerConnection. Pour plus d’informations, consultez [Wrappers et interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Informations de référence sur l'API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
