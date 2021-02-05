---
description: SQLServerXADataSource, classe
title: Classe SQLServerXADataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 95fc7b07-2498-4a7e-8f7f-ee0d86b598b4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 61d86781a9f730c78a6e2d78da0d5152d3f84e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180360"
---
# <a name="sqlserverxadatasource-class"></a>SQLServerXADataSource, classe
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente une fabrique d’objets [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md) destinée à un usage interne.  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Étend :** [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
 **Implémente :** javax.sql.XADataSource  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public class SQLServerXADataSource  
```  
  
## <a name="remarks"></a>Notes  
 Un objet qui implémente l’interface SQLServerXADataSource est généralement inscrit avec un service de nommage qui utilise l’interface JNDI (Java Naming and Directory Interface).  
  
 La classe SQLServerXADataSource fournit des connexions de bases de données en vue d’une utilisation dans des transactions distribuées (XA). La classe SQLServerXADataSource prend également en charge le regroupement de connexions physiques. Les interfaces SQLServerXADataSource et SQLServerXAConnection, définies dans le package javax.sql, sont implémentées par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Un objet SQLServerXAConnection est une connexion en pool qui peut participer à une transaction distribuée. Plus précisément, SQLServerXAConnection étend l’interface SQLServerPooledConnection en ajoutant la méthode [getXAResource](../../../connect/jdbc/reference/getxaresource-method-sqlserverxaconnection.md). Cette méthode produit un objet [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) qui peut être utilisé par un gestionnaire de transactions afin de coordonner le travail effectué sur cette connexion avec les autres participants de la transaction distribuée. Comme ils étendent l’interface SQLServerPooledConnection, les objets SQLServerXAConnection prennent en charge toutes les méthodes des objets SQLServerPooledConnection. Il s'agit de connexions physiques réutilisables à une source de données sous-jacente qui produisent des handles de connexions logiques pouvant être renvoyés à une application JDBC.  
  
 Les objets SQLServerXAConnection sont générés par un objet SQLServerXADataSource. Les objets SQLServerConnectionPoolDataSource et les objets SQLServerXADataSource sont similaires en ce qu’ils sont implémentés sous une couche source de données visible par l’application JDBC. Cette architecture permet à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de prendre en charge les transactions distribuées de façon transparente pour l’application. SQLServerXADataSource peut être configuré pour s’intégrer à [!INCLUDE[msCoName](../../../includes/msconame_md.md)] DTC (Distributed Transaction Coordinator) pour offrir un véritable traitement des transactions distribuées.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerXADataSource, membres](../../../connect/jdbc/reference/sqlserverxadatasource-members.md)   
 [Informations de référence sur l'API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
