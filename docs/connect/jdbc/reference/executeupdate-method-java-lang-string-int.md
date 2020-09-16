---
description: Méthode executeUpdate (java.lang.String, int)
title: Méthode executeUpdate (java.lang.String, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerStatement.executeUpdate (java.lang.String, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 4c52a20e-527e-4d14-9a5a-4cd195aac8ed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a0d549d4da78cb58688842e545d94efd8e0f4087
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88437691"
---
# <a name="executeupdate-method-javalangstring-int"></a>Méthode executeUpdate (java.lang.String, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Exécute l’instruction SQL donnée et signale à [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] avec l’indicateur donné si les clés générées automatiquement par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) doivent être récupérables.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final int executeUpdate(java.lang.String sql,  
                               int flag)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sql*  
  
 **Chaîne** contenant une instruction SQL.  
  
 *flag*  
  
 Valeur **int** indiquant si les clés générées automatiquement doivent être disponibles. Il doit s'agir de l'une des constantes suivantes :  
  
 RETURN_GENERATED_KEYS  
  
 NO_GENERATED_KEYS  
  
## <a name="return-value"></a>Valeur de retour  
 **int** indiquant le nombre de lignes affectées, ou 0 en cas d’instruction DDL.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode executeUpdate est spécifiée par la méthode executeUpdate de l’interface java.sql.Statement.  
  
 Si l’exécution d’une procédure stockée aboutit à plusieurs mises à jour, ou si cela génère plusieurs jeux de résultats, utilisez la méthode [execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md) pour exécuter la procédure stockée.  
  
## <a name="see-also"></a>Voir aussi  
 [executeUpdate, méthode &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md)   
 [SQLServerStatement, membres](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
