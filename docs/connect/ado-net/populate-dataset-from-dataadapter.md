---
title: Remplissage d’un DataSet à partir d'un DataAdapter
description: Découvrez comment remplir un DataSet à partir d’un DataAdapter dans ADO.NET, ce qui fournit un modèle de programmation relationnel cohérent, indépendant de la source de données.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 3fa0ac7d-e266-4954-bfac-3fbe2f913153
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e6c50bf7255dc77edfd0b93e03dedeec83ed4c4d
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771501"
---
# <a name="populate-a-dataset-from-a-dataadapter"></a>Remplissage d’un DataSet à partir d'un DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le <xref:System.Data.DataSet> d’ADO.NET est une représentation des données résidente en mémoire, qui propose un modèle de programmation relationnel cohérent indépendant de la source de données. Le `DataSet` représente un jeu de données complet qui comprend des tables, des contraintes et des relations entre les tables. Étant donné que le `DataSet` est indépendant de la source de données, le `DataSet` peut inclure des données locales par rapport à l'application ainsi que des données provenant de plusieurs sources. L'interaction avec les sources de données existantes est contrôlée par le `DataAdapter`.

La propriété `SelectCommand` du `DataAdapter` est un objet `Command` qui extrait les données de la source de données. Les propriétés `InsertCommand`, `UpdateCommand`et `DeleteCommand` du `DataAdapter` sont des objets `Command` qui gèrent les mises à jour des données dans la source de données conformément aux modifications apportées dans le `DataSet`. Ces propriétés sont décrites plus en détail dans [Mettre à jour les sources de données avec des DataAdapters](update-data-sources-with-dataadapters.md).

La méthode `Fill` du `DataAdapter` sert à remplir un `DataSet` avec les résultats de la `SelectCommand` du `DataAdapter`. La méthode`Fill` prend comme arguments un `DataSet` à remplir, ainsi qu'un objet `DataTable` , ou le nom du `DataTable` à remplir avec les lignes retournées par `SelectCommand`.

> [!NOTE]
> L'utilisation du `DataAdapter` pour extraire une table dans son intégralité prend du temps, particulièrement si la table contient de nombreuses lignes. Cela vient du fait qu'accéder à la base de données, localiser et traiter les données, puis transférer les données au client prend du temps. L'extraction de l'intégralité de la table vers le client verrouille également toutes les lignes sur le serveur. Pour améliorer les performances, vous pouvez utiliser la clause `WHERE` pour réduire drastiquement le nombre de lignes retournées au client. Vous pouvez également réduire la quantité de données retournées au client en répertoriant de façon explicite uniquement les colonnes requises dans l'instruction `SELECT` . Une autre solution de contournement efficace consiste à extraire les lignes par lots, par exemple plusieurs centaines de lignes à la fois, et à extraire le lot suivant uniquement lorsque le client a terminé le traitement du lot actuel.

La méthode `Fill` utilise l'objet `DataReader` de manière implicite pour retourner les noms et les types de colonnes utilisés pour créer les tables du `DataSet`ainsi que les données pour remplir les lignes des tables du `DataSet`. Les tables et les colonnes ne sont créées que si elles n'existent pas encore ; sinon `Fill` utilise le schéma `DataSet` existant. Les types de colonnes sont créés en tant que types .NET Framework conformément aux tableaux de [Mappages des types de données dans ADO.NET](data-type-mappings-ado-net.md). Les clés primaires ne sont pas créées, sauf si elles existent dans la source de données et que `DataAdapter` **.** `MissingSchemaAction`. est défini sur `MissingSchemaAction` **.** `AddWithKey`. Si `Fill` détecte qu'il existe une clé primaire pour une table, elle remplace les données du `DataSet` par celles de la source de données pour les lignes dont les valeurs de colonne de clé primaire correspondent à celles de la ligne retournée par la source de données. Si aucune clé primaire n'est trouvée, les données sont ajoutées à la table dans le `DataSet`. `Fill` utilise tous les mappages qui peuvent exister quand vous remplissez le `DataSet` (consultez [Mappages de DataAdapter, DataTable et DataColumn](dataadapter-datatable-datacolumn-mappings.md)).

> [!NOTE]
> Si `SelectCommand` retourne les résultats d'une jointure externe, le `DataAdapter` ne définit pas de valeur `PrimaryKey` pour le `DataTable`obtenu. Vous devez définir vous-même la `PrimaryKey` pour garantir une résolution correcte des lignes dupliquées.

L'exemple de code suivant crée une instance d'un objet <xref:Microsoft.Data.SqlClient.SqlDataAdapter> qui utilise un objet <xref:Microsoft.Data.SqlClient.SqlConnection> sur la base de données `Northwind` Microsoft SQL Server et remplit un objet <xref:System.Data.DataTable> dans un `DataSet` avec la liste des clients. L'instruction SQL et les arguments <xref:Microsoft.Data.SqlClient.SqlConnection> passés au constructeur <xref:Microsoft.Data.SqlClient.SqlDataAdapter> sont utilisés pour créer la propriété <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> de l'objet <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

## <a name="example"></a>Exemple

[!code-csharp[SqlDataAdapter_FillDataSet#1](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#1)]

> [!NOTE]
> Ce code n'ouvre pas et ne ferme pas la `Connection`de manière explicite. La méthode `Fill` ouvre implicitement la `Connection` que le `DataAdapter` utilise si la connexion n'est pas déjà ouverte. Si `Fill` a ouvert la connexion, `Fill` la ferme aussi lorsque son exécution est terminée. Cela peut simplifier votre code lorsque vous ne traitez qu'une seule opération comme `Fill` ou `Update`. Cependant, si vous effectuez plusieurs opérations qui nécessitent une connexion ouverte, vous pouvez améliorer les performances de votre application en appelant de manière explicite la méthode `Open` de `Connection`, en effectuant les opérations sur la source de données, puis en appelant la méthode `Close` de `Connection`. Vous devez tenter de réduire autant que possible le temps d'ouverture des connexions à la source de données pour libérer des ressources afin que d'autres applications clientes puissent les utiliser.

## <a name="multiple-result-sets"></a>Jeux de résultats multiples

Si le `DataAdapter` détecte plusieurs jeux de résultats, il crée plusieurs tables dans le `DataSet`. Les tables reçoivent un nom incrémentiel par défaut de Table *N*, commençant par « Table » pour Table0. Si un nom de table est passé comme argument à la méthode `Fill` , les tables reçoivent le nom incrémentiel par défaut TableName *N*, commençant par « TableName » pour TableName0.  
  
## <a name="populate-a-dataset-from-multiple-dataadapters"></a>Remplissage d’un DataSet à partir de plusieurs DataAdapters  

Vous pouvez utiliser un nombre quelconque d’objets `DataAdapter` avec un `DataSet`. Chaque `DataAdapter` peut être utilisé pour remplir un ou plusieurs objets `DataTable` et répercuter les mises à jour dans la source de données concernée. Les objets`DataRelation` et `Constraint` peuvent être ajoutés localement au `DataSet` , ce qui vous permet de relier des données provenant de sources de données hétérogènes. Par exemple, un `DataSet` peut contenir des données provenant d'une base de données Microsoft SQL Server, d'une base de données IBM DB2 exposée via OLE DB et d'une source de données qui diffuse le XML en continu. Un ou plusieurs objets `DataAdapter` peuvent gérer la communication vers chaque source de données.  
  
### <a name="example"></a>Exemple  

L’exemple de code suivant remplit une liste de clients provenant de la base de données `Northwind` sur Microsoft SQL Server, ainsi qu’une liste de commandes provenant de la base de données `Northwind` stockée dans Microsoft Access. Les tables remplies sont reliées par un `DataRelation`et la liste des clients est ensuite affichée avec les commandes du client concerné.

[!code-csharp[SqlDataAdapter_FillDataSet#2](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#2)]

## <a name="sql-server-decimal-type"></a>Type décimal de SQL Server

Par défaut, le `DataSet` stocke les données en utilisant des types de données .NET. Pour la plupart des applications, ils fournissent une représentation pratique des informations de la source de données. Cette représentation peut néanmoins poser un problème lorsque le type de données dans la source de données est un decimal SQL Server ou un type de données numérique. Le type de données `decimal` de .NET autorise un nombre maximal de 28 chiffres significatifs, tandis que le type de données `decimal` de SQL Server en autorise 38. Si `SqlDataAdapter` détermine pendant une opération `Fill` que la précision d'un champ `decimal` SQL Server est supérieure à 28 caractères, la ligne actuelle n'est pas ajoutée au `DataTable`. Au lieu de cela, l'événement `FillError` se produit, ce qui vous permet de déterminer une éventuelle perte de précision et de répondre de manière appropriée. Pour plus d’informations sur l’événement `FillError`, consultez [Gérer les événements de DataAdapter](handle-dataadapter-events.md). Pour obtenir la valeur `decimal` SQL Server, vous pouvez également utiliser un objet <xref:Microsoft.Data.SqlClient.SqlDataReader> et appeler la méthode <xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A> .

ADO.NET inclut une prise en charge avancée de <xref:System.Data.SqlTypes> dans le `DataSet`. Pour plus d'informations, consultez [SqlTypes and the DataSet](./sql/sqltypes-dataset.md).

## <a name="see-also"></a>Voir aussi

- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Mappages des types de données dans ADO.NET](data-type-mappings-ado-net.md)
- [MARS (Multiple Active Result Sets)](./sql/multiple-active-result-sets-mars.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
