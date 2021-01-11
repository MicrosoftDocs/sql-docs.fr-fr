---
title: Modification de données avec des procédures stockées
description: Décrit comment utiliser des paramètres d'entrée et sortie de procédure stockée afin d'insérer une ligne dans une base de données et retourner une nouvelle valeur d'identité.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 7d8e9a46-1af6-4a02-bf61-969d77ae07e0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f04884302bb1f13852097182d6ebc8e06570c66b
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744369"
---
# <a name="modify-data-with-stored-procedures"></a>Modification de données avec des procédures stockées

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Les procédures stockées peuvent accepter des données en tant que paramètres d'entrée et retourner des données en tant que paramètres de sortie, jeux de résultats et valeurs de retour. L’exemple ci-dessous montre comment le fournisseur de données Microsoft SqlClient pour SQL Server envoie et reçoit des paramètres d’entrée, des paramètres de sortie et des valeurs de retour. L’exemple insère un nouvel enregistrement dans une table où la colonne de clé primaire est une colonne d’identité.

> [!NOTE]
> Si vous utilisez des procédures stockées pour modifier ou supprimer des données en utilisant un <xref:Microsoft.Data.SqlClient.SqlDataAdapter>, veillez à ne pas utiliser **SET NOCOUNT ON** dans la définition de la procédure stockée. En effet, le nombre de lignes affectées retourné serait alors la valeur zéro, ce que `DataAdapter` interprète comme un conflit d'accès concurrentiel. Dans ce cas, l'exception <xref:System.Data.DBConcurrencyException> est levée.

## <a name="example"></a>Exemple

L’exemple utilise la procédure stockée suivante pour insérer une nouvelle catégorie dans la table **Northwind** **Categories**. La procédure stockée prend la valeur dans la colonne **CategoryName** en tant que paramètre d’entrée et utilise la fonction **SCOPE_IDENTITY()** pour récupérer la nouvelle valeur du champ d’identité **CategoryID** et la retourner dans un paramètre de sortie. L’instruction RETURN utilise la fonction **\@\@ROWCOUNT** pour retourner le nombre de lignes insérées.

```sql
CREATE PROCEDURE dbo.InsertCategory  
  @CategoryName nvarchar(15),  
  @Identity int OUT  
AS  
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)  
SET @Identity = SCOPE_IDENTITY()  
RETURN @@ROWCOUNT  
```  

L'exemple de code suivant utilise la procédure stockée `InsertCategory` présentée ci-dessus comme source pour <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> de <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. Le paramètre de sortie `@Identity` se reflète dans <xref:System.Data.DataSet> après que l'enregistrement a été inséré dans la base de données lors de l'appel de la méthode `Update` de <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. Le code récupère également la valeur de retour.

[!code-csharp[DataWorks SqlClient.SprocIdentityReturn#1](~/../sqlclient/doc/samples/SqlDataAdapter_SPIdentityReturn.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Récupération et modification de données dans ADO.NET](retrieving-modifying-data.md)
- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Exécution d'une commande](execute-command.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
