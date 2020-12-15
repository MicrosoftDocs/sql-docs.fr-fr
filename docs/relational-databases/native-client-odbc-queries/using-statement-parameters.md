---
description: Utilisation de paramètres d'instruction
title: Utilisation des paramètres d’instruction | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, parameters
- parameters [SQL Server Native Client], ODBC
- ODBC, parameters
- statements [ODBC], parameters
- parameter markers [ODBC]
- SQL Server Native Client ODBC driver, statements
- ODBC applications, statements
ms.assetid: 2427d886-ec6c-49d7-b0b6-0d998b64cdb9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b05c7d20deb1108e5e4c7f74fffce42072ba8515
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478060"
---
# <a name="using-statement-parameters"></a>Utilisation de paramètres d'instruction
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Un paramètre est une variable dans une instruction SQL qui peut permettre à une application ODBC d'effectuer les actions suivantes :  
  
-   fournir de manière efficace des valeurs pour les colonnes d'une table ;  
  
-   améliorer l'interaction de l'utilisateur lors de la construction de critères de requête ;  
  
-   Gérer les données **Text**, **ntext** et **image** et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de données C spécifiques.  
  
 Par exemple, une table de **pièces** contient des colonnes nommées **partid**, **Description** et **Price**. L'ajout d'un article sans paramètres requiert la construction d'une instruction SQL telle que :  
  
```  
INSERT INTO Parts (PartID, Description, Price) VALUES (2100, 'Drive shaft', 50.00)  
```  
  
 Bien que cette instruction soit acceptable pour insérer une ligne avec un jeu connu de valeurs, elle est maladroite lorsqu'une application doit insérer plusieurs lignes. Pour résoudre ce problème, ODBC laisse une application remplacer toute valeur de données dans une instruction SQL par un fabricant de paramètre. Cela est dénoté par un point d'interrogation (?). Dans l'exemple suivant, trois valeurs de données sont remplacées par des marqueurs de paramètres :  
  
```  
INSERT INTO Parts (PartID, Description, Price) VALUES (?, ?, ?)  
```  
  
 Les marqueurs de paramètres sont ensuite liés à des variables d'applications. Pour insérer une nouvelle ligne, l'application doit uniquement définir les valeurs des variables et exécuter l'instruction. Le pilote extrait ensuite les valeurs actuelles des variables et les envoie à la source de données. Si l'instruction est exécutée plusieurs fois, l'application peut rendre le processus encore plus efficace en préparant l'instruction.  
  
 Chaque marqueur de paramètre est référencé par son nombre ordinal assigné aux paramètres de gauche à droite. Le marqueur de paramètre situé le plus à gauche dans une instruction SQL a la valeur ordinale 1 ; le suivant a la valeur ordinale 2, et ainsi de suite.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Liaison de paramètres](../../relational-databases/native-client-odbc-queries/using-statement-parameters-binding-parameters.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution de requêtes &#40;ODBC&#41;](../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
  
  
