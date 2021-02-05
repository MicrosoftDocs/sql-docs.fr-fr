---
description: Classe SQLServerCallableStatement
title: Classe SQLServerCallableStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 30710a63-c05d-47d9-9cf9-c087a1c76373
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60faed31bc161bda250a4981d98406f81815a1ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178413"
---
# <a name="sqlservercallablestatement-class"></a>Classe SQLServerCallableStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Permet de spécifier le nom de la procédure stockée à appeler avec les paramètres d'entrée et de sortie. Cette classe offre également la possibilité de récupérer la valeur de l’état retourné avec la syntaxe `? = call( ?, ..)`.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Implémente :** [ ISQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
 **Étend :** [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final class SQLServerCallableStatement  
```  
  
## <a name="remarks"></a>Notes  
 SQLServerCallableStatement permet de spécifier le nom de la procédure stockée à appeler avec des paramètres d’entrée et de sortie. Elle offre également la possibilité de récupérer la valeur de l’état de retour avec la syntaxe `? = call( ?, ..)`.  
  
 Cette classe prend en charge la désencapsulation (unwrapping) dans la classe SQLServerCallableStatement, l’interface ISQLServerCallableStatement, l’interface java.sql.CallableStatement et les classes et interfaces prises en charge par SQLServerPreparedStatement pour la désencapsulation. Pour plus d’informations, consultez [Wrappers et interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
 Lorsque l’une des méthodes set SQLServerCallableStatement est appelée pour un type qui entre en conflit avec celui spécifié avec [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md), le type utilisé sera celui spécifié par la dernière méthode set SQLServerCallableStatement utilisée. Toutefois, cela peut entraîner des erreurs dues à une conversion de types de données qui sont incompatibles. Si la méthode SQLServerCallableStatement n’est pas appelée, le type spécifié avec le premier appel de [registerOutParameter](../../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) est utilisé.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0 est conforme à la recommandation JDBC 4.0, selon laquelle un jeu de résultats et des mises à jour multiples doivent être récupérés avant les paramètres OUT. Si les paramètres OUT sont récupérés avant que le jeu de résultats et les mises à jour n'aient été complètement traités, les résultats et les mises à jour qui n'auront pas été traités seront perdus.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerCallableStatement, membres](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Informations de référence sur l'API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
