---
description: Méthode setSendTimeAsDatetime (SQLServerDataSource)
title: Méthode setSendTimeAsDatetime (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 705a0494-b5e2-43db-940a-1b8cec550cdb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 52136134199ac4811d22b48bfbad768dbcdf8e7d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88458401"
---
# <a name="setsendtimeasdatetime-method-sqlserverdatasource"></a>Méthode setSendTimeAsDatetime (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cette méthode a été ajoutée dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 Modifie le paramètre de la propriété de connexion **sendTimeAsDatetime**.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setSendTimeAsDatetime(boolean sendTimeAsDateTime)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *sendTimeAsDateTime*  
  
 Valeur booléenne. Si true, les valeurs java.sql.Time sont envoyées au serveur en tant que types [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **datetime**. Si false, les valeurs java.sql.Time sont envoyées au serveur en tant que types [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **time**.  
  
## <a name="remarks"></a>Notes  
 [SQLServerDataSource.getSendTimeAsDatetime](../../../connect/jdbc/reference/getsendtimeasdatetime-method-sqlserverdatasource.md) retourne le paramètre de la propriété de connexion **sendTimeAsDatetime**.  
  
 Pour plus d’informations sur la propriété de connexion **sendTimeAsDatetime**, consultez [Définir les propriétés de connexion](../../../connect/jdbc/setting-the-connection-properties.md).  
  
 Pour plus d’informations, consultez [Configurer le mode d’envoi des valeurs java.sql.Time au serveur](../../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
