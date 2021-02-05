---
description: SQLServerXAConnection, classe
title: Classe SQLServerXAConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5ecb4bf1-b8d1-47cf-9cb1-7a18acc11ce2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 13d2ad39b4259d514c099e221961132336d152aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179979"
---
# <a name="sqlserverxaconnection-class"></a>SQLServerXAConnection, classe
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Représente les connexions JDBC qui peuvent participer à des transactions distribuées (XA).  
  
 **Package :** com.microsoft.sqlserver.jdbc  
  
 **Étend :** [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)  
  
 **Implémente :** javax.sql.XAConnection  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public class SQLServerXAConnection  
```  
  
## <a name="remarks"></a>Notes  
 Un objet SQLServerXAConnection peut être inscrit dans une transaction distribuée au moyen d’un objet [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md). Un gestionnaire de transactions, faisant généralement partie d’un serveur de niveau intermédiaire, gère un objet SQLServerXAConnection à travers l’objet SQLServerXAResource.  
  
> [!NOTE]  
>  En général, les programmateurs d'applications n'utilisent pas cette interface directement. Elle est principalement utilisée par un gestionnaire de transactions actif dans le serveur de couche intermédiaire.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerXAConnection, membres](../../../connect/jdbc/reference/sqlserverxaconnection-members.md)   
 [Informations de référence sur l'API du pilote JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
