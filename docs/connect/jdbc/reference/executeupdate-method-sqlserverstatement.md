---
description: Méthode executeUpdate (SQLServerStatement)
title: Méthode executeUpdate (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerStatement.executeUpdate
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 10ae662a-ce3c-4b24-875c-5c2df319d93b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 076b3597b19c8593a2d1921984601c6d066058e4
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88437681"
---
# <a name="executeupdate-method-sqlserverstatement"></a>Méthode executeUpdate (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Exécute l'instruction SQL fournie, qui peut être une instruction INSERT, UPDATE ou DELETE ; ou une instruction SQL qui ne retourne rien, telle qu'une instruction SQL DDL. À compter de [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0, executeUpdate retourne le nombre correct de lignes mises à jour dans une opération MERGE.  
  
## <a name="overload-list"></a>Liste de surcharge  
  
|Nom|Description|  
|----------|-----------------|  
|[executeUpdate (java.lang.String)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-sqlserverstatement.md)|Exécute l'instruction SQL fournie, qui peut être une instruction INSERT, UPDATE, DELETE ou MERGE ; ou une instruction SQL qui ne retourne rien, telle qu'une instruction SQL DDL.|  
|[executeUpdate (java.lang.String, int)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-int.md)|Exécute l’instruction SQL fournie et indique au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] avec l’indicateur fourni si les clés générées automatiquement produites par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) doivent être disponibles pour la récupération.|  
|[executeUpdate (java.lang.String, int&#91;&#93;)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string.md)|Exécute l'instruction SQL fournie et signale au pilote JDBC que les clés générées automatiquement qui sont indiquées dans le tableau spécifié doivent être disponibles pour la récupération.|  
|[executeUpdate (java.lang.String, java.lang.String&#91;&#93;)](../../../connect/jdbc/reference/executeupdate-method-java-lang-string-java-lang-string.md)|Exécute l'instruction SQL fournie et signale au pilote JDBC que les clés générées automatiquement qui sont indiquées dans le tableau spécifié doivent être disponibles pour la récupération.|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
