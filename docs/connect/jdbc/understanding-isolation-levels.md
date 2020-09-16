---
description: Présentation des niveaux d’isolation
title: Comprendre les niveaux d’isolation | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 2c41e23a-da6c-4650-b5fc-b5fe53ba65c3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 758012112df2bce1b0a7624834bfde3c3c4ece10
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88488042"
---
# <a name="understanding-isolation-levels"></a>Présentation des niveaux d’isolation

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Les transactions spécifient un niveau d'isolement. Ce niveau définit le degré d'isolement d'une transaction par rapport aux modifications de ressource ou de données apportées par d'autres transactions. Les niveaux d'isolement déterminent les effets secondaires de la concurrence (lectures erronées, lectures fantômes) qui sont autorisés.  
  
Les niveaux d'isolement des transactions contrôlent les éléments suivants :  
  
- l'acquisition de verrous lors de la lecture de données, et le type de verrous nécessaires ;  
  
- la durée de vie des verrous de lecture ;  
  
- la réaction d'une opération de lecture qui fait référence à des lignes modifiées par une autre transaction :  
  
  - Blocage jusqu'à ce que le verrou exclusif sur la ligne soit levé  
  
  - Récupération de la version validée de la ligne telle qu'elle était au début de l'instruction ou de la transaction  
  
  - Lecture de la modification des données non validées  

Le choix du niveau d’isolation de la transaction n’affecte pas les verrous acquis pour protéger les modifications de données. Une transaction acquiert toujours un verrou exclusif sur les données qu'elle modifie et garde celui-ci jusqu'à ce qu'elle ait terminé son travail, indépendamment du niveau d'isolement défini pour elle. Dans le cas des opérations de lecture, le niveau d'isolation d'une transaction définit principalement son niveau de protection contre les effets des modifications apportées par les autres transactions.  
  
Plus le niveau d'isolement est faible, plus le nombre de personnes susceptibles d'accéder aux données en même temps est élevé, et plus les effets secondaires de la concurrence (lectures erronées, mises à jour perdues) sont nombreux. Inversement, plus le niveau d'isolement est élevé, plus le nombre de types d'effets secondaires de la concurrence qu'un utilisateur est susceptible de rencontrer est réduit. Cependant, la quantité de ressources système nécessaires et la probabilité d'un blocage mutuel de transactions sont plus élevées. Le choix du niveau d'isolation adéquat dépend d'une mise en équilibre de l'espace réservé et des exigences en matière d'intégrité des données de l'application. Le niveau le plus élevé, sérialisable, garantit qu'une transaction récupère exactement les mêmes données à chaque fois qu'elle répète une opération de lecture, mais en utilisant un niveau de verrouillage susceptible de gêner les autres utilisateurs dans les systèmes multi-utilisateurs. Le niveau le plus bas, lecture non validée, permet la récupération de données qui ont été modifiées mais non validées par d'autres transactions. Tous les effets secondaires de concurrence peuvent se produire en lecture de données non validées, mais il n’a pas de verrouillage ou de contrôle de version de lecture, ce qui réduit le traitement.  

## <a name="remarks"></a>Notes

 Le tableau suivant répertorie les effets secondaires de la concurrence provoqués par les différents niveaux d'isolement.  
  
| Niveau d’isolation  | Lecture erronée | Lecture non reproductible | Fantôme |
| ---------------- | ---------- | ------------------- | ------- |
| Lecture non validée | Oui        | Oui                 | Oui     |
| Lecture validée   | Non          | Oui                 | Oui     |
| Lecture renouvelable  | Non         | Non                   | Oui     |
| Instantané         | Non         | Non                  | Non       |
| Sérialisable     | Non         | Non                  | Non       |
  
Les transactions doivent être exécutées à un niveau d'isolement au moins égal à la lecture reproductible afin d'empêcher les pertes de mises à jour qui peuvent se produire lorsque deux transactions récupèrent la même ligne, puis mettent ultérieurement à jour la ligne en fonction des valeurs récupérées initialement. Si les deux transactions mettent à jour des lignes avec une seule instruction UPDATE sans se baser sur les valeurs récupérées auparavant, les mises à jour perdues ne peuvent pas se produire au niveau d’isolation par défaut de la lecture de données validées.  

Pour définir le niveau d’isolement pour une transaction, vous pouvez utiliser la méthode [setTransactionIsolation](../../connect/jdbc/reference/settransactionisolation-method-sqlserverconnection.md) de la classe [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md). Cette méthode accepte une valeur **int** comme argument, qui est basée sur l’une des constantes de connexion comme dans l’exemple suivant :  

```java
con.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);  
```

Pour utiliser le nouveau niveau d'isolement d'instantané de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez utiliser l'une des constantes `SQLServerConnection` :  

```java
con.setTransactionIsolation(SQLServerConnection.TRANSACTION_SNAPSHOT);  
```

ou vous pouvez utiliser :  

```java
con.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED + 4094);  
```

Pour plus d’informations sur les niveaux d’isolement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez « Niveaux d’isolement du [!INCLUDE[ssDE](../../includes/ssde_md.md)] » dans la documentation en ligne [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="see-also"></a>Voir aussi

[Réalisation de transactions avec le pilote JDBC](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  
