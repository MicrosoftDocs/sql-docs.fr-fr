---
description: SQLServerBlob, constructeur (SQLServerConnection, byte)
title: SQLServerBlob, constructeur (SQLServerConnection, byte) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection, byte[].SQLServerBlob
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9fe573e3-30db-4828-abab-e9346493e931
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d2ff6f2a7fadaa23df917e07b10b20f2cf3df427
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178412"
---
# <a name="sqlserverblob-constructor-sqlserverconnection-byte"></a>SQLServerBlob, constructeur (SQLServerConnection, byte)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Initialise une nouvelle instance de la classe [SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md) lors de la spécification d’un objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) et d’un tableau d’**octets**.  
  
> [!NOTE]  
>  Cette méthode a été déconseillée dans la version 2.0 du pilote JDBC. Utilisez plutôt la méthode [createBlob](../../../connect/jdbc/reference/createblob-method-sqlserverconnection.md) de la classe [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public SQLServerBlob(SQLServerConnection connection,  
                     byte[] data)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *connection*  
  
 Objet SQLServerConnection.  
  
 *data*  
  
 Tableau d’**octets**.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerBlob, constructeurs](../../../connect/jdbc/reference/sqlserverblob-constructors.md)   
 [SQLServerBlob, membres](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [SQLServerBlob, classe](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
