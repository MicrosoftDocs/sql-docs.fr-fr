---
description: Classe SQLServerPreparedStatement
title: Classe SQLServerPreparedStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a8481c06-fbba-432b-8c69-4f4619c20ad4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 75a04a0873308cc09c16b54f5d482ed54a20c0b1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88472135"
---
# <a name="sqlserverpreparedstatement-class"></a>Classe SQLServerPreparedStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente l'implémentation de base de la fonctionnalité d'instruction préparée JDBC.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Étend :** SQLServerStatement  
  
 **Implémente :** [ISQLServerPreparedStatement](../../../connect/jdbc/reference/isqlserverpreparedstatement-interface.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public class SQLServerPreparedStatement  
```  
  
## <a name="remarks"></a>Notes  
 SQLServerPreparedStatement fournit des méthodes qui vous permettent de passer des paramètres de tout type Java natif et de nombreux types d’objet Java. SQLServerPreparedStatement prépare une instruction à l’aide de la procédure stockée [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **sp_prepare**, puis réutilise le handle d’instruction retourné pour chaque exécution suivante de l’instruction, généralement en utilisant différents paramètres fournis par l’utilisateur.  
  
 SQLServerPreparedStatement prend en charge le traitement par lot au cours duquel un ensemble d’instructions préparées est exécuté en un seul aller-retour à la base de données afin d’améliorer les performances du runtime.  
  
 Cette classe prend en charge la désencapsulation (unwrapping) dans la classe SQLServerPreparedStatement, l’interface SQLServerPreparedStatement, l’interface java.sql.PreparedStatement et les classes et interfaces prises en charge par SQLServerPreparedStatement pour la désencapsulation. Pour plus d’informations, consultez [Wrappers et interfaces](../../../connect/jdbc/wrappers-and-interfaces.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerPreparedStatement, membres](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Informations de référence sur l'API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
