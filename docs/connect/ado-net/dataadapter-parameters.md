---
title: Paramètres des DataAdapter
description: Découvrez les propriétés de DbDataAdapter qui retournent des données depuis une source de données et gèrent les modifications apportées à la source de données.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 90985f155392a78da408ffbda3e1d2c0b12d4648
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771601"
---
# <a name="dataadapter-parameters"></a>Paramètres des DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

L'objet <xref:System.Data.Common.DbDataAdapter> possède quatre propriétés qui sont utilisées pour récupérer des données dans la source de données et y mettre des données à jour : la propriété <xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> retourne des données de la source de données et les propriétés <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>, <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> et <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> sont utilisées pour gérer les modifications au niveau de la source de données.

> [!NOTE]
> La  propriété `SelectCommand` doit être définie avant d'appeler la méthode `Fill` du `DataAdapter`. Les propriétés `InsertCommand`, `UpdateCommand` ou `DeleteCommand` doivent être définies avant que la méthode `Update` du `DataAdapter` ne soit appelée, en fonction des modifications qui ont été apportées aux données dans le <xref:System.Data.DataTable>. Par exemple, si des lignes ont été ajoutées, `InsertCommand` doit être défini avant d'appeler `Update`. Lorsque `Update` traite une ligne insérée, mise à jour ou supprimée, le `DataAdapter` utilise la propriété `Command` respective pour traiter l'action. Les informations actuelles concernant la ligne modifiée sont passées à l’objet `Command` par le biais de la collection `Parameters`.

Quand vous mettez à jour une ligne au niveau de la source de données, vous appelez l’instruction UPDATE, qui utilise un identificateur unique pour identifier la ligne de la table à mettre à jour. Cet identificateur unique est généralement la valeur d'un champ de clé primaire. L'instruction UPDATE utilise les paramètres qui contiennent l'identificateur unique ainsi que les colonnes et les valeurs à mettre à jour, comme indiqué dans l'instruction Transact-SQL suivante.

```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  

> [!NOTE]
> La syntaxe des espaces réservés des paramètres dépend de la source de données. Cet exemple montre les espaces réservés pour une source de données SQL Server.

Dans cet exemple , le champ `CompanyName` est mis à jour avec la valeur du paramètre `@CompanyName` pour la ligne où `CustomerID` a la valeur du paramètre `@CustomerID`. Les paramètres extraient les informations de la ligne modifiée en utilisant la propriété <xref:Microsoft.Data.SqlClient.SqlParameter.SourceColumn%2A> de l’objet <xref:Microsoft.Data.SqlClient.SqlParameter>. Les paramètres suivants sont ceux de l'instruction UPDATE de l'exemple précédent. Le code est basé sur l'hypothèse que la variable `adapter` représente un objet <xref:Microsoft.Data.SqlClient.SqlDataAdapter> valide.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#2](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#2)]

La méthode `Add` de la collection `Parameters` prend le nom du paramètre, le type de données, la taille (si elle est applicable au type) et le nom de la propriété <xref:System.Data.Common.DbParameter.SourceColumn%2A> du `DataTable`. Notez que la propriété <xref:System.Data.Common.DbParameter.SourceVersion%2A> du paramètre `@CustomerID` a la valeur `Original`. Cela garantit que la ligne existante dans la source de données est mise à jour si la valeur de la ou des colonnes d'identification a été modifiée dans l'objet <xref:System.Data.DataRow>. Dans ce cas, la valeur de ligne `Original` correspondrait à la valeur actuelle de la source de données et la valeur de ligne `Current` contiendrait la valeur mise à jour. Le `SourceVersion` du paramètre `@CompanyName` n'est pas défini et utilise la valeur de ligne `Current` par défaut.

> [!NOTE]
> Pour les opérations `Fill` de `DataAdapter` comme pour les méthodes `Get` de `DataReader`, le type .NET est inféré du type retourné du Fournisseur de données Microsoft SqlClient pour SQL Server. Les types inférés et méthodes d’accesseur pour les types de données Microsoft SQL Server sont décrits dans [Mappages des types de données dans ADO.NET](data-type-mappings-ado-net.md).

## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn, Parameter.SourceVersion

Le `SourceColumn` et `SourceVersion` peuvent être passés comme arguments au constructeur `Parameter`ou définis comme propriétés d'un `Parameter` existant. `SourceColumn` est le nom de l'objet <xref:System.Data.DataColumn> provenant de l'objet <xref:System.Data.DataRow> dans lequel la valeur de `Parameter` sera récupérée. `SourceVersion` spécifie la version du `DataRow` que le `DataAdapter` utilise pour récupérer la valeur.

Le tableau suivant présente les valeurs d'énumération <xref:System.Data.DataRowVersion> disponibles pour être utilisées avec `SourceVersion`.

|Énumération DataRowVersion|Description|  
|--------------------------------|-----------------|  
|`Current`|Le paramètre utilise la valeur actuelle de la colonne. Il s’agit de la valeur par défaut.|  
|`Default`|Ce paramètre utilise le `DefaultValue` de la colomne.|  
|`Original`|Le paramètre utilise la valeur d'origine de la colonne.|  
|`Proposed`|Le paramètre utilise une valeur proposée.|  

L'exemple de code `SqlClient` de la section suivante définit un paramètre d'une propriété <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> dans lequel la colonne `CustomerID` est utilisée comme `SourceColumn` pour deux paramètres : `@CustomerID` (`SET CustomerID = @CustomerID`) et `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`). Le paramètre `@CustomerID` est utilisé pour mettre à jour la colonne **CustomerID** avec la valeur actuelle dans la `DataRow`. Par conséquent, la `SourceColumn` de `CustomerID` avec une `SourceVersion` ayant la valeur `Current` est utilisé. Le paramètre `@OldCustomerID` est utilisé pour identifier la ligne actuelle dans la source de données. Puisque la valeur de colonne correspondante se trouve dans la version `Original` de la ligne, le même `SourceColumn` (`CustomerID`) avec un `SourceVersion` ayant la valeur `Original` est utilisé.

## <a name="work-with-sqlclient-parameters"></a>Utiliser des paramètres SqlClient

L'exemple suivant montre comment créer un objet <xref:Microsoft.Data.SqlClient.SqlDataAdapter> et affecter la valeur <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> à la propriété <xref:System.Data.MissingSchemaAction.AddWithKey> de manière à récupérer des informations de schéma supplémentaires de la base de données. Les propriétés <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> et <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> définies, ainsi que les objets <xref:Microsoft.Data.SqlClient.SqlParameter> correspondants ajoutés à la collection <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A>. La méthode retourne un objet `SqlDataAdapter`.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#1](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#1)]

## <a name="see-also"></a>Voir aussi

- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Commandes et paramètres](commands-parameters.md)
- [Mise à jour de sources de données avec des DataAdapter](update-data-sources-with-dataadapters.md)
- [Modification de données avec des procédures stockées](modify-data-with-stored-procedures.md)
- [Mappages des types de données dans ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
