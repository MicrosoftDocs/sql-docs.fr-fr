---
description: Méthode setDateTimeOffset (SQLServerPreparedStatement)
title: setDateTimeOffset, méthode (SQLServerPreparedStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5014dba9-1755-4769-b070-6cbeecee864e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1734cb149b45175f6a6df9a4b19711d05281a225
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179058"
---
# <a name="setdatetimeoffset-method-sqlserverpreparedstatement"></a>Méthode setDateTimeOffset (SQLServerPreparedStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cette méthode a été ajoutée dans [!INCLUDE[msCoName](../../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] JDBC Driver 3.0.  
  
 Définit la valeur de la colonne spécifiée sur la valeur [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public final void setDateTimeOffset(int n, microsoft.sql.DateTimeOffset x)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *n*  
  
 Valeur ordinale de base zéro d'une colonne.  
  
 *x*  
  
 Objet [DateTimeOffset Class](../../../connect/jdbc/reference/datetimeoffset-class.md).  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerPreparedStatement, membres](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Classe SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
