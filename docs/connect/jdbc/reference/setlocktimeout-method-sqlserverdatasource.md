---
description: Méthode setLockTimeout (SQLServerDataSource)
title: Méthode setLockTimeout (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setLockTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 10dca5aa-1851-4326-9ae9-7a8430d12d11
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 98f9ee813d8f1b4999d44b5ec56a65efe35888e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178901"
---
# <a name="setlocktimeout-method-sqlserverdatasource"></a>Méthode setLockTimeout (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Définit une valeur **int** qui indique le nombre de millisecondes pendant lesquelles la base de données patiente avant de signaler un dépassement de délai d’attente de verrou.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void setLockTimeout(int lockTimeout)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *lockTimeout*  
  
 Valeur **int** contenant le nombre de millisecondes à attendre.  
  
## <a name="remarks"></a>Notes  
 Le délai d'un verrou correspond au nombre de millisecondes à attendre avant que la base de données ne rapporte l'expiration d'un délai de verrou. La valeur par défaut de -1 indique une durée d'attente indéfinie. Si elle est spécifiée, cette valeur sera la valeur par défaut pour toutes les instructions de la connexion.  
  
> [!NOTE]  
>  La valeur 0 indique qu'il n'y aura pas d'attente. Si la propriété lockTimeout n’est pas définie, la méthode [getLockTimeout](../../../connect/jdbc/reference/getlocktimeout-method-sqlserverdatasource.md) retourne la valeur par défaut (-1).  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerDataSource, membres](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource, classe](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
