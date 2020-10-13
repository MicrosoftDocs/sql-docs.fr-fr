---
description: 'Tutorial: Ownership Chains and Context Switching'
title: 'Tutorial: Ownership Chains and Context Switching'
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: quickstart
helpviewer_keywords:
- context switching [SQL Server], tutorials
- ownership chains [SQL Server]
ms.assetid: db5d4cc3-5fc5-4cf5-afc1-8d4edc1d512b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d4c1494efa032ba58315e8a5f7fe5fd855b3e51f
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2020
ms.locfileid: "91809365"
---
# <a name="tutorial-ownership-chains-and-context-switching"></a>Tutoriel : Chaînes de propriétés et changement de contexte
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Ce didacticiel explore en se fondant sur un scénario les concepts de sécurité de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] impliquant les chaînes de propriétés et le changement de contexte utilisateur.  
  
> [!NOTE]  
> Pour exécuter le code de ce tutoriel, vous devez à la fois configurer la sécurité en mode mixte et installer la base de données AdventureWorks2017. Pour plus d’informations sur la sécurité en mode mixte, consultez [Choisir un mode d’authentification](../relational-databases/security/choose-an-authentication-mode.md).  
  
## <a name="scenario"></a>Scénario  
Dans ce scénario, deux utilisateurs ont besoin de comptes leur permettant d'accéder aux données des bons de commande stockées dans la base de données AdventureWorks2017. Les exigences requises sont les suivantes :  
  
-   Le premier compte (TestManagerUser) doit être en mesure de dévoiler toutes les données détaillées dans chaque bon de commande.  
-   Le deuxième compte (TestEmployeeUser) doit afficher le numéro de bon de commande, la date de commande, la date d'expédition, les ID des produits et les articles commandés et reçus par bon de commande (par numéro de bon de commande) pour les articles pour lesquels des livraisons partielles ont été reçues.  
-   Tous les autres comptes doivent conserver leurs autorisations actuelles.   
Pour répondre aux exigences de ce scénario, l'exemple est divisé en quatre parties décrivant les concepts relatifs aux chaînes de propriétés et au changement de contexte :  
  
1.  Configuration de l'environnement   
2.  Création d'une procédure stockée pour l'accès aux données par bon de commande   
3.  Accès aux données par le biais de la procédure stockée  
4.  Réinitialisation de l'environnement  

Chaque bloc de code dans cet exemple est présenté sous forme de lignes. Pour copier l'exemple tout entier, consultez la section [Exemple complet](#CompleteExample) à la fin de ce didacticiel.

## <a name="prerequisites"></a>Conditions préalables requises
Pour suivre ce tutoriel, vous avez besoin de SQL Server Management Studio, de l’accès à un serveur qui exécute SQL Server et d’une base de données AdventureWorks.

- Installez [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Téléchargez les [exemples de bases de données AdventureWorks2017](../samples/adventureworks-install-configure.md).

Pour obtenir des instructions sur la restauration d’une base de données dans SQL Server Management Studio, consultez [Restaurer une base de données](./backup-restore/restore-a-database-backup-using-ssms.md).   
  
## <a name="1-configure-the-environment"></a>1. Configurez l'environnement  
Utilisez [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] et le code ci-dessous pour ouvrir la base de données `AdventureWorks2017` ; ensuite, à l’aide de l’instruction `CURRENT_USER` [!INCLUDE[tsql](../includes/tsql-md.md)], vérifiez que l’utilisateur dbo est affiché dans le contexte.  
  
```sql
USE AdventureWorks2017;  
GO  
SELECT CURRENT_USER AS 'Current User Name';  
GO  
```  
  
Pour plus d’informations sur l’instruction CURRENT_USER, consultez [CURRENT_USER &#40;Transact-SQL&#41;](../t-sql/functions/current-user-transact-sql.md).  
  
Utilisez ce code en tant qu'utilisateur dbo pour créer deux utilisateurs sur le serveur et dans la base de données AdventureWorks2017.  
  
```sql
CREATE LOGIN TestManagerUser   
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khx';  
GO  
CREATE USER TestManagerUser   
   FOR LOGIN TestManagerUser  
   WITH DEFAULT_SCHEMA = Purchasing;  
GO   
  
CREATE LOGIN TestEmployeeUser  
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
GO  
CREATE USER TestEmployeeUser   
   FOR LOGIN TestEmployeeUser;  
GO   
```  
  
Pour plus d’informations sur l’instruction CREATE USER, consultez [CREATE USER &#40;Transact-SQL&#41;](../t-sql/statements/create-user-transact-sql.md). Pour plus d’informations sur l’instruction CREATE LOGIN, consultez [CREATE LOGIN &#40;Transact-SQL&#41;](../t-sql/statements/create-login-transact-sql.md).  
  
Utilisez le code suivant pour modifier la propriété du schéma `Purchasing` du compte `TestManagerUser` . Le compte peut alors exploiter l'accès à toutes les instructions DML (Data Manipulation Language, langage de manipulation de données), notamment les autorisations `SELECT` et `INSERT` , sur les objets qu'il contient. `TestManagerUser` se voit également octroyer la possibilité de créer des procédures stockées.  
  
```sql
/* Change owner of the Purchasing Schema to TestManagerUser */  
ALTER AUTHORIZATION   
   ON SCHEMA::Purchasing   
   TO TestManagerUser;  
GO  
  
GRANT CREATE PROCEDURE   
   TO TestManagerUser   
   WITH GRANT OPTION;  
GO  
```  
  
Pour plus d’informations sur l’instruction GRANT, consultez [GRANT &#40;Transact-SQL&#41;](../t-sql/statements/grant-transact-sql.md). Pour plus d’informations sur les procédures stockées, consultez [Procédures stockées &#40;moteur de base de données&#41;](../relational-databases/stored-procedures/stored-procedures-database-engine.md). Pour obtenir une affiche de toutes les autorisations du [!INCLUDE[ssDE](../includes/ssde-md.md)], consultez [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster).  
  
## <a name="2-create-a-stored-procedure-to-access-data"></a>2. Créer une procédure stockée pour accéder aux données  
Pour basculer le contexte dans une base de données, utilisez l’instruction EXECUTE AS. EXECUTE AS nécessite des autorisations IMPERSONATE.  
  
Utilisez l'instruction `EXECUTE AS` dans le code ci-après pour modifier le contexte et le redéfinir à `TestManagerUser` et pour créer une procédure stockée affichant uniquement les données requises par `TestEmployeeUser`. Pour répondre aux exigences, la procédure stockée accepte une variable pour le numéro de bon de commande et n'affiche pas de données financières. De même, la clause WHERE limite les résultats aux expéditions partielles.  
  
```sql
EXECUTE AS LOGIN = 'TestManagerUser'  
GO  
SELECT CURRENT_USER AS 'Current User Name';  
GO  
  
/* Note: The user that calls the EXECUTE AS statement must have IMPERSONATE permissions on the target principal */  
CREATE PROCEDURE usp_ShowWaitingItems @ProductID int  
AS  
BEGIN   
   SELECT a.PurchaseOrderID, a.OrderDate, a.ShipDate  
      , b.ProductID, b.OrderQty, b.ReceivedQty  
   FROM Purchasing.PurchaseOrderHeader a  
      INNER JOIN Purchasing.PurchaseOrderDetail b  
         ON a.PurchaseOrderID = b.PurchaseOrderID  
   WHERE b.OrderQty > b.ReceivedQty  
      AND @ProductID = b.ProductID  
   ORDER BY b.ProductID ASC  
END  
GO  
```  
  
Actuellement, `TestEmployeeUser` n'a accès à aucun objet de base de données. Le code suivant (toujours dans le contexte `TestManagerUser` ) accorde au compte de l'utilisateur la possibilité d'interroger les informations des tables de base par l'intermédiaire de la procédure stockée.  
  
```sql
GRANT EXECUTE  
   ON OBJECT::Purchasing.usp_ShowWaitingItems  
   TO TestEmployeeUser;  
GO  
```  
  
La procédure stockée est un élément inhérent au schéma `Purchasing` , même si aucun schéma n'a été spécifié, puisque `TestManagerUser` est attribué par défaut au schéma `Purchasing` . Vous pouvez utiliser les informations du catalogue système pour rechercher des objets, comme le montre le code suivant.  
  
```sql
SELECT a.name AS 'Schema'  
   , b.name AS 'Object Name'  
   , b.type AS 'Object Type'  
FROM sys.schemas a  
   INNER JOIN sys.objects b  
      ON a.schema_id = b.schema_id   
WHERE b.name = 'usp_ShowWaitingItems';  
GO  
```  
  
Une fois cette section de l'exemple terminée, le code change de nouveau de contexte pour repasser à dbo à l'aide de l'instruction `REVERT`.  
  
```sql
REVERT;  
GO  
```  
  
Pour plus d’informations sur l’instruction REVERT, consultez [REVERT &#40;Transact-SQL&#41;](../t-sql/statements/revert-transact-sql.md).  
  
## <a name="3-access-data-through-the-stored-procedure"></a>3. Accéder aux données par le biais de la procédure stockée  
`TestEmployeeUser` ne dispose d’aucune autorisation pour les objets de la base de données [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] en dehors d’une connexion et des droits attribués au rôle de base de données public. Le code suivant retourne une erreur lorsque `TestEmployeeUser` tente d'accéder aux tables de base.  
  
```sql
EXECUTE AS LOGIN = 'TestEmployeeUser'  
GO  
SELECT CURRENT_USER AS 'Current User Name';  
GO  
/* This won't work */  
SELECT *  
FROM Purchasing.PurchaseOrderHeader;  
GO  
SELECT *  
FROM Purchasing.PurchaseOrderDetail;  
GO  
```  

Erreur retournée :
```
Msg 229, Level 14, State 5, Line 6
The SELECT permission was denied on the object 'PurchaseOrderHeader', database 'AdventureWorks2017', schema 'Purchasing'.
```
  
Du fait que les objets référencés par la procédure stockée créée au cours de la dernière section appartiennent à `TestManagerUser` en raison de la propriété du schéma `Purchasing` , `TestEmployeeUser` peut accéder aux tables de base par le biais de la procédure stockée. Le code suivant qui utilise toujours le contexte `TestEmployeeUser` transmet le bon de commande 952 en guise de paramètre.  
  
```sql
EXEC Purchasing.usp_ShowWaitingItems 952  
GO  
```  
  
## <a name="4-reset-the-environment"></a>4. Réinitialisez l'environnement  
Le code ci-dessous exploite la commande `REVERT` pour retourner le contexte du compte actuel à `dbo`, puis réinitialise l'environnement.  
  
```sql
REVERT;  
GO  
ALTER AUTHORIZATION   
ON SCHEMA::Purchasing TO dbo;  
GO  
DROP PROCEDURE Purchasing.usp_ShowWaitingItems;  
GO  
DROP USER TestEmployeeUser;  
GO  
DROP USER TestManagerUser;  
GO  
DROP LOGIN TestEmployeeUser;  
GO  
DROP LOGIN TestManagerUser;  
GO  
```  
  
## <a name="complete-example"></a><a name="CompleteExample"></a>Exemple complet  
Cette section affiche l'exemple de code dans son intégralité.  
  
> [!NOTE]  
> Ce code n'inclut pas les deux erreurs attendues et expliquant l'inaptitude de `TestEmployeeUser` à effectuer une sélection à partir des tables de base.  
  
```sql
/*   
Script:       UserContextTutorial.sql  
Author:       Microsoft  
Last Updated: Books Online  
Conditions:   Execute as DBO or sysadmin in the AdventureWorks database  
Section 1:    Configure the Environment   
*/  
USE AdventureWorks2017;  
GO  
SELECT CURRENT_USER AS 'Current User Name';  
GO  
/* Create server and database users */  
CREATE LOGIN TestManagerUser   
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khx';  
  
GO  
  
CREATE USER TestManagerUser   
   FOR LOGIN TestManagerUser  
   WITH DEFAULT_SCHEMA = Purchasing;  
GO   
  
CREATE LOGIN TestEmployeeUser  
    WITH PASSWORD = '340$Uuxwp7Mcxo7Khy';  
GO  
CREATE USER TestEmployeeUser   
   FOR LOGIN TestEmployeeUser;  
GO   
  
/* Change owner of the Purchasing Schema to TestManagerUser */  
ALTER AUTHORIZATION   
   ON SCHEMA::Purchasing   
   TO TestManagerUser;  
GO  
  
GRANT CREATE PROCEDURE   
   TO TestManagerUser   
   WITH GRANT OPTION;  
GO  
  
/*   
Section 2: Switch Context and Create Objects  
*/  
EXECUTE AS LOGIN = 'TestManagerUser';  
GO  
SELECT CURRENT_USER AS 'Current User Name';  
GO  
  
/* Note: The user that calls the EXECUTE AS statement must have IMPERSONATE permissions on the target principal */  
CREATE PROCEDURE usp_ShowWaitingItems @ProductID int  
AS  
BEGIN   
   SELECT a.PurchaseOrderID, a.OrderDate, a.ShipDate  
      , b.ProductID, b.OrderQty, b.ReceivedQty  
   FROM Purchasing.PurchaseOrderHeader AS a  
      INNER JOIN Purchasing.PurchaseOrderDetail AS b  
         ON a.PurchaseOrderID = b.PurchaseOrderID  
   WHERE b.OrderQty > b.ReceivedQty  
      AND @ProductID = b.ProductID  
   ORDER BY b.ProductID ASC  
END;  
GO  
  
/* Give the employee the ability to run the procedure */  
GRANT EXECUTE   
   ON OBJECT::Purchasing.usp_ShowWaitingItems  
   TO TestEmployeeUser;  
GO   
  
/* Notice that the stored procedure is located in the Purchasing   
schema. This also demonstrates system catalogs */  
SELECT a.name AS 'Schema'  
   , b.name AS 'Object Name'  
   , b.type AS 'Object Type'  
FROM sys.schemas AS a  
   INNER JOIN sys.objects AS b  
      ON a.schema_id = b.schema_id   
WHERE b.name = 'usp_ShowWaitingItems';  
GO  
  
/* Go back to being the dbo user */  
REVERT;  
GO  
  
/*  
Section 3: Switch Context and Observe Security   
*/  
EXECUTE AS LOGIN = 'TestEmployeeUser';  
GO  
SELECT CURRENT_USER AS 'Current User Name';  
GO  
EXEC Purchasing.usp_ShowWaitingItems 952;  
GO  
  
/*   
Section 4: Clean Up Example  
*/  
REVERT;  
GO  
ALTER AUTHORIZATION   
ON SCHEMA::Purchasing TO dbo;  
GO  
DROP PROCEDURE Purchasing.usp_ShowWaitingItems;  
GO  
DROP USER TestEmployeeUser;  
GO  
DROP USER TestManagerUser;  
GO  
DROP LOGIN TestEmployeeUser;  
GO  
DROP LOGIN TestManagerUser;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
[Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
  
