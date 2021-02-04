---
description: Méthode getApplicationName (SQLServerDataSource)
title: Méthode getApplicationName (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getApplicationName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f71e501c-ccd7-4a1e-b6ea-4d47a81c18c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 45e4ce6a37e24ee4d3e8692fe4b145e8e8f019b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168417"
---
# <a name="getapplicationname-method-sqlserverdatasource"></a>Méthode getApplicationName (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Renvoie le nom de l'application.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public java.lang.String getApplicationName()  
```  
  
## <a name="return-value"></a>Valeur de retour  
 **Chaîne** qui contient le nom de l’application ou « [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] » si aucune valeur n’est définie.  
  
## <a name="remarks"></a>Remarques  
 Le nom d’application est utilisé pour identifier l’application spécifique dans différents de profilage et de journalisation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si le nom d’application n’est pas défini, la méthode getApplicationName retourne la chaîne non localisée « [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] ».  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
