---
description: Utiliser la sortie de FOR JSON dans SQL Server et les applications clientes (SQL Server)
title: Utilisez la sortie de FOR JSON dans SQL Server et les applications clientes
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, using in client apps
- FOR JSON, using in SQL Server
ms.assetid: 302e5397-b499-4ea3-9a7f-c24ccad698eb
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4bd5911552144084550abdadb3d432ab08f3bf9a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172501"
---
# <a name="use-for-json-output-in-sql-server-and-in-client-apps-sql-server"></a>Utiliser la sortie de FOR JSON dans SQL Server et les applications clientes (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

Les exemples suivants montrent comment utiliser la clause **FOR JSON** et sa sortie JSON dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou les applications clientes.  
  
## <a name="use-for-json-output-in-sql-server-variables"></a>Utiliser la sortie de FOR JSON dans des variables SQL Server  
La sortie de la clause FOR JSON est de type NVARCHAR(MAX). Vous pouvez donc l’assigner à une variable, comme indiqué dans l’exemple suivant.  
  
```sql  
DECLARE @x NVARCHAR(MAX) =
  (SELECT TOP 10 *
     FROM Sales.SalesOrderHeader
     FOR JSON AUTO)  
```  
  
## <a name="use-for-json-output-in-sql-server-user-defined-functions"></a>Utiliser la sortie de FOR JSON dans des fonctions SQL Server définies par l’utilisateur  
 Vous pouvez créer des fonctions définies par l’utilisateur qui convertissent les jeux de résultats au format JSON et renvoient cette sortie JSON. l’exemple suivant crée une fonction définie par l’utilisateur, qui extrait certaines lignes de détails de commande et les organise en un tableau JSON.  
  
```sql  
CREATE FUNCTION GetSalesOrderDetails(@salesOrderId int)  
 RETURNS NVARCHAR(MAX)  
AS  
BEGIN  
   RETURN (SELECT UnitPrice, OrderQty  
           FROM Sales.SalesOrderDetail  
           WHERE SalesOrderID = @salesOrderId  
           FOR JSON AUTO)  
END
```  
  
 Vous pouvez utiliser cette fonction dans un lot ou une requête, comme indiqué dans l’exemple suivant.  
  
```sql  
DECLARE @x NVARCHAR(MAX) = dbo.GetSalesOrderDetails(43659)

PRINT dbo.GetSalesOrderDetails(43659)

SELECT TOP 10
  H.*, dbo.GetSalesOrderDetails(H.SalesOrderId) AS Details
FROM Sales.SalesOrderHeader H
```  
  
## <a name="merge-parent-and-child-data-into-a-single-table"></a>Fusionner les données parentes et enfants dans une seule table  
Dans l’exemple suivant, chaque ensemble de lignes enfants est mis en forme en tant que tableau JSON. Le tableau JSON devient la valeur de la colonne Détails dans la table parente.  
  
```sql  
SELECT TOP 10 SalesOrderId, OrderDate,  
      (SELECT TOP 3 UnitPrice, OrderQty  
         FROM Sales.SalesOrderDetail D  
         WHERE H.SalesOrderId = D.SalesOrderID  
         FOR JSON AUTO) AS Details  
INTO SalesOrder  
FROM Sales.SalesOrderHeader H  
```  
  
## <a name="update-the-data-in-json-columns"></a>Mettre à jour les données dans les colonnes JSON  
 L’exemple suivant montre comment mettre à jour la valeur d’une colonne qui contient du texte JSON.  
  
```sql  
UPDATE SalesOrder  
SET Details =  
     (SELECT TOP 1 UnitPrice, OrderQty  
       FROM Sales.SalesOrderDetail D  
       WHERE D.SalesOrderId = SalesOrder.SalesOrderId  
      FOR JSON AUTO) 
```  
  
## <a name="use-for-json-output-in-a-c-client-app"></a>Utiliser la sortie de FOR JSON dans une application cliente C#  
 l’exemple suivant montre comment récupérer la sortie JSON d’une requête dans un objet StringBuilder d’une application cliente C#. Supposons que la variable `queryWithForJson` contienne le texte d’une instruction SELECT avec une clause FOR JSON.  
  
```csharp  
var queryWithForJson = "SELECT ... FOR JSON";
using(var conn = new SqlConnection("<connection string>"))
{
    using(var cmd = new SqlCommand(queryWithForJson, conn))
    {
        conn.Open();
        var jsonResult = new StringBuilder();
        var reader = cmd.ExecuteReader();
        if (!reader.HasRows)
        {
            jsonResult.Append("[]");
        }
        else
        {
            while (reader.Read())
            {
                jsonResult.Append(reader.GetValue(0).ToString());
            }
        }
    }
}
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>En savoir plus sur JSON dans SQL Server et Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vidéos Microsoft

Pour obtenir une présentation visuelle de la prise en charge intégrée de JSON dans SQL Server et Azure SQL Database, consultez les vidéos suivantes :

-   [SQL Server 2016 et prise en charge de JSON](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [Utilisation de JSON dans SQL Server 2016 et Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON : un pont entre les mondes relationnel et NoSQL](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
 
## <a name="see-also"></a>Voir aussi  
 [Mettre les résultats de requête au format JSON avec FOR JSON &#40;SQL Server&#41;](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
  
  
