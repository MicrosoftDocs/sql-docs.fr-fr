---
description: Méthode close (SQLServerResultSet)
title: Méthode close (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.close
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 8f3adf5b-874e-4cf2-b4ef-672dda42d77a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0a6f5e311333da6c16015acd2fe6ab007204b8d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168597"
---
# <a name="close-method-sqlserverresultset"></a>Méthode close (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Libère immédiatement les ressources de base de données et JDBC de cet objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md), au lieu d’attendre que cette opération se produise lors de sa fermeture automatique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
public void close()  
```  
  
## <a name="exceptions"></a>Exceptions  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Notes  
 Cette méthode close est spécifiée par la méthode close de l’interface java.sql.ResultSet.  
  
 Un objet SQLServerResultSet est automatiquement fermé par l’objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) l’ayant généré lorsque cet objet SQLServerStatement est fermé, réexécuté ou utilisé pour récupérer le résultat suivant d’une séquence comportant plusieurs résultats. Un objet SQLServerResultSet est également fermé automatiquement lorsqu’il est récupéré par le récupérateur de mémoire.  
  
 Lors de l'exécution d'une instruction qui produit un ensemble unique et important de résultats avant uniquement, en lecture seule, un ensemble initial de lignes uniquement doit vous intéresser dans le jeu de résultats retourné. Dans ce cas, l’application peut appeler la méthode [cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md) de l’objet d’instruction associé avant de fermer le jeu de résultats, afin de réduire le temps de traitement nécessaire pour ignorer les lignes restantes inutiles. Nous vous recommandons de faire la part entre le temps de traitement gagné et le temps, ainsi que l'aller-retour jusqu'au serveur requis pour annuler l'exécution, lorsque la décision doit être prise d'utiliser cette technique ou pas.  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerResultSet, membres](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet, classe](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
