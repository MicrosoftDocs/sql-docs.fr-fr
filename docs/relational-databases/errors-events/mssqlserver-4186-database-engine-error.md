---
description: MSSQLSERVER_4186
title: MSSQLSERVER_4186 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4186 (Database Engine error)
ms.assetid: 1ae88554-f291-45bc-a186-6f41d9cd0fca
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 32df0694f8398bc39cb9e306b3e3da7905b1e212
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185603"
---
# <a name="mssqlserver_4186"></a>MSSQLSERVER_4186
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|4186|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique||  
|Texte du message|La colonne '%ls.%.*ls' ne peut pas être référencée dans la clause OUTPUT parce que la définition de colonne contient une sous-requête ou référence une fonction qui offre un accès aux données utilisateur ou système. Une fonction est censée par défaut offrir un accès aux données si elle n'est pas liée au schéma. Pensez à supprimer la sous-requête ou la fonction de la définition de colonne ou à supprimer la colonne de la clause OUTPUT.|  
  
## <a name="explanation"></a>Explication  
Pour empêcher tout comportement non déterministe, la clause OUTPUT ne peut pas référencer une colonne d'une vue ou fonction table lorsque cette colonne est définie par l'une des méthodes suivantes :  
  
-   Une sous-requête.  
  
-   Une fonction définie par l'utilisateur qui offre un accès à des données utilisateur ou système, ou qui est supposée permettre d'y accéder.  
  
-   Une colonne calculée qui contient une fonction définie par l’utilisateur qui effectue un accès aux données utilisateur ou système dans sa définition.  
  
### <a name="examples"></a>Exemples  
**Colonne de vue définie par une sous-requête**  
  
L'exemple suivant crée une vue qui utilise une sous-requête dans la liste de sélection pour définir la colonne `State`. Une instruction UPDATE référence ensuite la colonne `State` dans la clause OUTPUT et échoue en raison de la sous-requête dans la liste de sélection.  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW dbo.V1  
AS  
    SELECT City,  
-- subquery to return the State name  
           (SELECT Name FROM Person.StateProvince AS sp   
            WHERE sp.StateProvinceID = a.StateProvinceID) AS State  
    FROM Person.Address AS a;  
GO  
--Reference the State column in the OUTPUT clause of an UPDATE statement  
UPDATE dbo.V1   
SET City = City + 'Test'   
OUTPUT deleted.City, deleted.State, inserted.City, inserted.State  
WHERE State = 'Texas';  
GO  
```  
  
**Colonne de vue définie par une fonction**  
  
L’exemple suivant crée une vue qui utilise la fonction scalaire d’accès aux données `dbo.ufnGetStock` dans la liste de sélection pour définir la colonne `CurrentInventory`. Une instruction UPDATE référence ensuite la colonne `CurrentInventory` dans la clause OUTPUT.  
  
```  
USE AdventureWorks2012;  
GO  
CREATE VIEW Production.ReorderLevels  
AS  
    SELECT ProductID, ProductModelID, ReorderPoint,  
           dbo.ufnGetStock(ProductID) AS CurrentInventory  
    FROM Production.Product;  
GO  
  
UPDATE Production.ReorderLevels  
SET ReorderPoint += CurrentInventory  
OUTPUT deleted.ReorderPoint, deleted.CurrentInventory,  
       inserted.ReorderPoint, inserted.CurrentInventory  
WHERE ProductModelID BETWEEN 75 and 80;  
```  
  
## <a name="user-action"></a>Action de l'utilisateur  
L'erreur 4186 peut être corrigée de l'une des façons suivantes :  
  
-   Utilisez des jointures au lieu de sous-requêtes pour définir la colonne dans la vue ou fonction. Par exemple, vous pouvez réécrire la vue `dbo.V1` de la façon suivante.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE VIEW dbo.V1  
    AS  
        SELECT City, sp.Name AS State  
        FROM Person.Address AS a   
        JOIN Person.StateProvince AS sp   
        ON sp.StateProvinceID = a.StateProvinceID;  
    ```  
  
-   Examinez la définition de la fonction définie par l'utilisateur. Si la fonction n'effectue pas accès aux données utilisateur ou système, modifiez la fonction de façon à y inclure la clause WITH SCHEMABINDING.  
  
-   Supprimez la colonne de la clause OUTPUT.  
  
## <a name="see-also"></a>Voir aussi  
[OUTPUT, clause &#40;Transact-SQL&#41;](~/t-sql/queries/output-clause-transact-sql.md)  
  
