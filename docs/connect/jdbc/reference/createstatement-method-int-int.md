---
description: Méthode createStatement (int, int)
title: Méthode createStatement (int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.createStatement (int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 90dbf639-c3d8-4519-9300-5447c79aec17
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9adab8077a33fe1856c70c543521dcac3566d200
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168557"
---
# <a name="createstatement-method-int-int"></a>Méthode createStatement (int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crée un objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) qui génère des objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) avec la concurrence et le type donnés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.sql.Statement createStatement(int resultSetType,  
                                          int resultSetConcurrency)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *resultSetType*  
  
 Valeur **int** représentant le type du jeu de résultats.  
  
 *resultSetConcurrency*  
  
 Valeur **int** représentant le type de concurrence du jeu de résultats.  
  
## <a name="return-value"></a>Valeur de retour  
 Objet Statement.  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode createStatement est spécifiée par la méthode createStatement de l’interface java.sql.Connection.  
  
## <a name="see-also"></a>Voir aussi  
 [createStatement, méthode &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md)   
 [SQLServerConnection, membres](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Classe SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
