---
title: MSSQLSERVER_4104 | Microsoft Docs
description: L’identificateur en plusieurs parties spécifié n'a pas pu être mappé à une entité existante. Consultez une explication de l’erreur et les solutions possibles.
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4104 (Database Engine error)
ms.assetid: 52dc32d8-97ad-4ef0-834d-2e68f215d007
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 9f28d2e4cf04427887584cab6ee27de80024eae7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185110"
---
# <a name="mssqlserver_4104"></a>MSSQLSERVER_4104
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|4104|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|ALG_MULTI_ID_BAD|  
|Texte du message|L'identificateur en plusieurs parties "%.*ls" ne peut pas être lié.|  
  
## <a name="explanation"></a>Explication  
Le nom d’une entité dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est appelé son *identificateur*. Vous utilisez des identificateurs chaque fois que vous référencez des entités, par exemple en spécifiant les noms des colonnes et des tables dans une requête. Un identificateur en plusieurs parties contient un ou plusieurs qualificateurs en tant que préfixe pour l'identificateur. Par exemple, un identificateur de table peut avoir pour préfixe des qualificateurs tels que le nom de la base de données et le nom du schéma dans lesquels la table est contenue, ou un identificateur de colonne peut avoir pour préfixe des qualificateurs tels qu'un nom de table ou un alias de table.  
  
L'erreur 4104 indique que l'identificateur en plusieurs parties spécifié n'a pas pu être mappé à une entité existante. Cette erreur peut être retournée dans les conditions suivantes :  
  
-   Le qualificateur fourni en tant que préfixe pour un nom de colonne ne correspond à aucun nom de table ou d'alias utilisé dans la requête.  
  
    Par exemple, l'instruction suivante utilise un alias de table (`Dept`) en tant que préfixe de colonne, mais l'alias de table n'est pas référencé dans la clause FROM.  
  
    ```  
    SELECT Dept.Name FROM HumanResources.Department;  
    ```  
  
    Dans les instructions suivantes, un identificateur de colonne en plusieurs parties `TableB.KeyCol` est spécifié dans la clause WHERE dans le cadre d'une condition JOIN entre deux tables ; toutefois, `TableB` n'est pas référencé explicitement dans la requête.  
  
    ```  
    DELETE FROM TableA WHERE TableA.KeyCol = TableB.KeyCol;  
    ```  
  
    ```  
    SELECT 'X' FROM TableA WHERE TableB.KeyCol = TableA.KeyCol;  
    ```  
  
-   Un nom d'alias est spécifié pour la table dans la clause FROM, mais le qualificateur fourni pour une colonne correspond au nom de la table. Par exemple, l'instruction suivante utilise le nom de table `Department` en tant que préfixe de colonne ; toutefois, la table a un alias (`Dept`) référencé dans la clause FROM.  
  
    ```  
    SELECT Department.Name FROM HumanResources.Department AS Dept;  
    ```  
  
    Lorsqu'un alias est utilisé, le nom de table ne peut pas être utilisé ailleurs dans l'instruction.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas déterminer si l'identificateur en plusieurs parties fait référence à une colonne préfixée par une table ou à un type de données défini par l'utilisateur (UDT) CLR préfixé par une colonne. Cette situation se présente, car les propriétés des colonnes UDT sont référencées à l'aide du point de séparation (.), placé entre le nom de la colonne et le nom de la propriété, de la même manière qu'un nom de colonne est préfixé à l'aide d'un nom de table. L’exemple suivant crée deux tables, `a` et `b`. La table `b` contient la colonne `a`, qui utilise un UDT CLR `dbo.myudt2` en tant que type de données. L'instruction SELECT contient un identificateur en plusieurs parties `a.c2`.  
  
    ```  
    CREATE TABLE a (c2 int);   
    GO  
    ```  
  
    ```  
    CREATE TABLE b (a dbo.myudt2);   
    GO  
    ```  
  
    ```  
    SELECT a.c2 FROM a, b;   
    ```  
  
    En supposant que l’UDT `myudt2`ne dispose pas d’une propriété nommée `c2`, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas déterminer si l’identificateur `a.c2` fait référence à la colonne `c2` dans la table `a` ou à la colonne `b`, propriété `a` dans la table `c2`.  
  
## <a name="user-action"></a>Action de l'utilisateur  
  
-   Faites correspondre les préfixes de colonnes aux noms de tables ou d'alias spécifiés dans la clause FROM de la requête. Si un alias est défini pour un nom de table dans la clause FROM, seul cet alias peut être utilisé en tant que qualificateur pour les colonnes associées à cette table.  
  
    Les instructions ci-dessus qui référencent la table `HumanResources.Department` peuvent être corrigées comme suit :  
  
    ```  
    SELECT Dept.Name FROM HumanResources.Department AS Dept;  
    GO  
    ```  
  
    ```  
    SELECT Department.Name FROM HumanResources.Department;  
    GO  
    ```  
  
-   Vérifiez que toutes les tables sont spécifiées dans la requête et que les conditions JOIN entre les tables sont spécifiées correctement. L'instruction DELETE ci-dessus peut être corrigée comme suit :  
  
    ```  
    DELETE FROM dbo.TableA  
    WHERE TableA.KeyCol = (SELECT TableB.KeyCol   
                            FROM TableB   
                            WHERE TableA.KeyCol = TableB.KeyCol);  
    GO  
    ```  
  
    L'instruction SELECT ci-dessus pour `TableA` peut être corrigée comme suit :  
  
    ```  
    SELECT 'X' FROM TableA, TableB WHERE TableB.KeyCol = TableA.KeyCol;  
    ```  
  
    or  
  
    ```  
    SELECT 'X' FROM TableA INNER JOIN TableB ON TableB.KeyCol = TableA.KeyCol;  
    ```  
  
-   Utilisez des noms uniques et clairement définis pour les identificateurs. Vous facilitez ainsi la lecture et la gestion de votre code et vous réduisez également le risque de références ambiguës à plusieurs entités.  
  
## <a name="see-also"></a>Voir aussi  
[MSSQLSERVER_107](~/relational-databases/errors-events/mssqlserver-107-database-engine-error.md)  
[Identificateurs de base de données](~/relational-databases/databases/database-identifiers.md)  
  
