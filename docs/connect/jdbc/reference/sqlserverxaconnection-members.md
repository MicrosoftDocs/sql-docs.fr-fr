---
description: Membres de SQLServerXAConnection
title: Membres de SQLServerXAConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4b61dabd-369b-460c-8450-9fe424f76541
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4676ec4ff2495468f043384d2b3e4761dbd1f725
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179965"
---
# <a name="sqlserverxaconnection-members"></a>Membres de SQLServerXAConnection
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants présentent les membres exposés par la classe [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md).  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
 Aucun.  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[addConnectionEventListener](../../../connect/jdbc/reference/addconnectioneventlistener-method-sqlserverpooledconnection.md)|(Hérité de [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Inscrit le détecteur d’événements donné afin qu’il soit informé lorsqu’un événement se produit sur cet objet Connection.|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverpooledconnection.md)|(Hérité de [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Ferme la connexion physique représentée par cet objet Connection.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverpooledconnection.md)|(Hérité de [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Crée un descripteur d’objet pour la connexion physique représentée par cet objet Connection.|  
|[getXAResource](../../../connect/jdbc/reference/getxaresource-method-sqlserverxaconnection.md)|Récupère un objet [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) qui sera utilisé par le gestionnaire de transactions pour gérer la participation de cet objet [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md) dans une transaction distribuée.|  
|[removeConnectionEventListener](../../../connect/jdbc/reference/removeconnectioneventlistener-method-sqlserverpooledconnection.md)|(Hérité de [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)) Supprime le détecteur d’événements donné.|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|com.microsoft.sqlserver.jdbc.SQLServerPooledConnection|addConnectionEventListener, close, getConnection, removeConnectionEventListener|  
|java.lang.Object|clone, equals, finalize, getClass, hashCode, notify, notifyAll, toString, wait|  
|javax.sql.PooledConnection|addConnectionEventListener, close, getConnection, removeConnectionEventListener|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerXAConnection, classe](../../../connect/jdbc/reference/sqlserverxaconnection-class.md)  
  
  
