---
title: Mise à jour de sources de données avec des DataAdapter
description: Découvrez comment la méthode de mise à jour de DataAdapter résout les modifications apportées à un DataSet pour les appliquer à la source de données dans les applications ADO.NET.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6f2feb876d0f232f4d7951de8ee1cc84587e6486
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771349"
---
# <a name="update-data-sources-with-dataadapters"></a>Mise à jour de sources de données avec des DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La méthode `Update` de l'objet <xref:System.Data.Common.DataAdapter> est appelée pour répercuter les modifications d'un objet <xref:System.Data.DataSet> dans la source de données. La méthode `Update`, comme la méthode `Fill`, prend comme arguments une instance d’un `DataSet` et un objet <xref:System.Data.DataTable> optionnel ou un nom de `DataTable`. L'instance `DataSet` est le `DataSet` qui contient les modifications apportées et le `DataTable` identifie la table de laquelle les modifications doivent être récupérées. Si aucun `DataTable` n'est spécifié, le premier `DataTable` du `DataSet` est utilisé.

Lorsque vous appelez la méthode `Update`, le `DataAdapter` analyse les modifications apportées et exécute la commande appropriée (INSERT, UPDATE ou DELETE). Lorsque le `DataAdapter` rencontre une modification apportée à un objet <xref:System.Data.DataRow>, il utilise la propriété <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A>, ou <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> pour traiter la modification.

Ces propriétés vous permettent d’optimiser les performances de votre application ADO.NET en spécifiant la syntaxe des commandes au moment de la conception et, là où c’est possible, via l’utilisation de procédures stockées. Vous devez explicitement définir les commandes avant d'appeler la méthode `Update`. Si la méthode `Update` est appelée et si la commande appropriée n'existe pas pour une mise à jour particulière (par exemple, pas de `DeleteCommand` pour les lignes supprimées), une exception est levée.

> [!IMPORTANT]
> Si vous utilisez des procédures stockées SQL Server pour modifier ou supprimer des données en utilisant un `DataAdapter`, veillez à ne pas utiliser `SET NOCOUNT ON` dans la définition de la procédure stockée. En effet, le nombre de lignes affectées retourné serait alors la valeur zéro, ce que `DataAdapter` interprète comme un conflit d'accès concurrentiel. Dans ce cas, l'exception <xref:System.Data.DBConcurrencyException> est levée.

Des paramètres de commande peuvent être utilisés pour spécifier les valeurs d'entrée et de sortie d'une instruction SQL ou d'une procédure stockée pour chaque ligne modifiée dans un `DataSet`. Pour plus d’informations, consultez [Paramètres du DataAdapter](dataadapter-parameters.md).

> [!NOTE]
> Il est important de comprendre la différence entre la suppression d'une ligne dans un objet <xref:System.Data.DataTable> et la suppression de la ligne. Lorsque vous appelez la méthode `Remove` ou `RemoveAt`, la ligne est immédiatement supprimée. Les lignes correspondantes dans la source de données du back-end **ne seront pas affectées** si vous passez ensuite la `DataTable` ou le `DataSet` à un `DataAdapter` et que vous appelez `Update`. Lorsque vous utilisez la méthode `Delete`, la ligne reste dans le `DataTable` et est marquée pour suppression. Si vous passez ensuite la `DataTable` ou le `DataSet` à un `DataAdapter` et que vous appelez `Update`, la ligne correspondante dans la source de données du back-end **est supprimée**.

Si votre `DataTable` est mappé à une table de base de données unique ou est généré par celle-ci, vous pouvez tirer parti de l'objet <xref:System.Data.Common.DbCommandBuilder> pour générer automatiquement les objets `DeleteCommand`, `InsertCommand` et `UpdateCommand` du `DataAdapter`. Pour plus d’informations, consultez [Génération de commandes avec CommandBuilders](generate-commands-with-commandbuilders.md).

## <a name="use-updatedrowsource-to-map-values-to-a-dataset"></a>Utiliser UpdatedRowSource pour mapper des valeurs à un DataSet

Vous pouvez contrôler comment les valeurs retournées depuis la source de données sont mappées à la `DataTable` après un appel à la méthode <xref:System.Data.Common.DbDataAdapter.Update%2A> d’un `DataAdapter`, en utilisant la propriété <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> d’un objet <xref:Microsoft.Data.SqlClient.SqlCommand>. En affectant l'une des valeurs d'énumération `UpdatedRowSource` à la propriété <xref:System.Data.UpdateRowSource>, vous pouvez contrôler si les paramètres de sortie retournés par les commandes `DataAdapter` sont ignorés ou appliqués à la ligne modifiée dans le `DataSet`. Vous pouvez aussi spécifier si la première ligne retournée (si elle existe) doit être appliquée à la ligne modifiée dans le `DataTable`.

Le tableau suivant décrit les différentes valeurs de l'énumération `UpdateRowSource` et la façon dont elles affectent le comportement d'une commande utilisée avec un `DataAdapter`.

|Énumération UpdatedRowSource|Description|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|Les paramètres de sortie et la première ligne d'un jeu de résultats retourné peuvent être mappés à la ligne modifiée dans le `DataSet`.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|Seules les données de la première ligne d'un jeu de résultats retourné peuvent être mappées à la ligne modifiée dans le `DataSet`.|
|<xref:System.Data.UpdateRowSource.None>|Les paramètres de sortie ou les lignes d'un jeu de résultats retourné sont ignorés.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|Seuls les paramètres de sortie peuvent être mappés à la ligne modifiée dans le `DataSet`.|

La méthode `Update` répercute vos modifications dans la source de données, cependant d'autres clients peuvent avoir modifié les données au niveau de la source depuis la dernière fois où vous avez rempli le `DataSet`. Pour actualiser votre `DataSet` avec des données en cours, utilisez le `DataAdapter` et la méthode `Fill`. De nouvelles lignes seront ajoutées à la table et les informations mises à jour seront incorporées dans les lignes existantes.

La méthode `Fill` détermine si une nouvelle ligne sera ajoutée ou si une ligne existante sera mise à jour en se basant sur les valeurs de clé primaire des lignes du `DataSet` et des lignes retournées par `SelectCommand`. Si une valeur de clé primaire d'une ligne du `Fill` correspond à celle d'une ligne des résultats retournés par `DataSet`, la méthode `SelectCommand` met à jour la ligne existante en y insérant les informations de la ligne retournée par `SelectCommand` et affecte la valeur <xref:System.Data.DataRow.RowState%2A> à la propriété `Unchanged`. Si la valeur de clé primaire d'une ligne retournée par `SelectCommand` n'a pas de correspondance dans les lignes du `DataSet`, la méthode `Fill` ajoute une nouvelle ligne avec un `RowState` ayant la valeur `Unchanged`.

> [!NOTE]
> Si la `SelectCommand` retourne les résultats d’une **OUTER JOIN**, le `DataAdapter` ne va pas définir une valeur `PrimaryKey` pour la `DataTable`résultante. Vous devez définir vous-même la `PrimaryKey` pour garantir une résolution correcte des lignes dupliquées.

Pour gérer les exceptions qui peuvent se produire lors de l’appel de la méthode `Update`, vous pouvez utiliser l’événement `RowUpdated` pour répondre aux erreurs de mise à jour des lignes (voir [Gérer les événements de DataAdapter](handle-dataadapter-events.md)), ou vous pouvez définir <xref:System.Data.Common.DataAdapter.ContinueUpdateOnError%2A> sur `true` avant d’appeler `Update` et répondre aux informations d’erreur stockées dans la propriété `RowError` d’une ligne particulière quand la mise à jour est terminée.

> [!NOTE]
> L’appel de `AcceptChanges` sur le `DataSet`, la `DataTable` ou la `DataRow` provoquera le remplacement de toutes les valeurs `Original` d’une `DataRow` par les valeurs `Current` de la `DataRow`. Si les valeurs de champ qui identifient la ligne comme étant unique ont été modifiées, après l'appel de `AcceptChanges`, les valeurs `Original` ne correspondront plus aux valeurs de la source de données. `AcceptChanges` est appelé automatiquement pour chaque ligne lors d’un appel à la méthode `Update` d’un `DataAdapter`. Vous pouvez conserver les valeurs d'origine au cours d'un appel de la méthode Update en commençant par affecter la valeur false à la propriété `AcceptChangesDuringUpdate` du `DataAdapter`, ou en créant un gestionnaire d'événements pour l'événement `RowUpdated` et en affectant la valeur <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> à <xref:System.Data.UpdateStatus.SkipCurrentRow>. Pour plus d’informations, consultez [Gérer les événements de DataAdapter](handle-dataadapter-events.md).

Les exemples suivants montrent comment effectuer des mises à jour de lignes modifiées en définissant explicitement la `UpdateCommand` d’un `DataAdapter` et en appelant sa méthode `Update`.

> [!NOTE]
> Le paramètre spécifié dans la `WHERE clause` de la `UPDATE statement` est défini pour utiliser la valeur `Original` de la `SourceColumn`. Cela est important car la valeur `Current` peut avoir été modifiée et ne pas correspondre à la valeur dans la source de données. La valeur `Original` est la valeur qui a été utilisée pour remplir le `DataTable` à partir de la source de données.

[!code-csharp[SqlDataAdapter_Update#1](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#1)]

## <a name="autoincrement-columns"></a>AutoIncrement (colonnes)

Si les tables de votre source de données ont des colonnes à incrémentation automatique, vous pouvez remplir les colonnes de votre `DataSet` soit en retournant la valeur d'auto-incrémentation comme paramètre de sortie d'une procédure stockée et en la mappant à une colonne dans une table, soit en utilisant l'événement `RowUpdated` du `DataAdapter` pour exécuter une instruction SELECT supplémentaire. Pour plus d’informations et pour obtenir un exemple, consultez [Récupérer des valeurs d’identité ou de numérotation automatique](retrieve-identity-or-autonumber-values.md).

## <a name="ordering-of-inserts-updates-and-deletes"></a>Ordre des insertions, des mises à jour et des suppressions

Dans de nombreuses circonstances, l'ordre dans lequel les modifications apportées dans le `DataSet` sont transmises à la source de données est important. Par exemple, si une valeur de clé primaire d'une ligne existante est mise à jour et qu'une nouvelle ligne est ajoutée avec la nouvelle valeur de clé primaire en tant que clé primaire, il est important de traiter la mise à jour avant l'insertion.

Vous pouvez utiliser la méthode `Select` du `DataTable` pour retourner un tableau `DataRow` qui fait uniquement référence à des lignes ayant un `RowState` particulier. Vous pouvez ensuite passer le tableau `DataRow` retourné à la méthode `Update` du `DataAdapter` pour traiter les lignes modifiées. En spécifiant un sous-ensemble des lignes à mettre à jour, vous pouvez contrôler l'ordre dans lequel les insertions, mises à jour et suppressions sont traitées.

## <a name="example"></a>Exemple

Le code suivant garantit, par exemple, que seront d'abord traitées les lignes supprimées de la table puis les lignes mises à jour et enfin les lignes insérées.

[!code-csharp[SqlDataAdapter_Update#2](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#2)]

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>Utiliser un DataAdapter pour récupérer et mettre à jour des données

Vous pouvez utiliser un DataAdapter pour récupérer et mettre à jour les données.

- L’exemple utilise `DataAdapter.AcceptChangesDuringFill` pour cloner les données dans la base de données. Si la propriété est définie sur **false**, **AcceptChanges** n’est pas appelée lors du remplissage de la table, et les lignes nouvellement ajoutées sont traitées comme des lignes insérées. Ainsi, l'exemple utilise ces lignes pour insérer de nouvelles lignes dans la base de données.

- L’exemple utilise `DataAdapter.TableMappings` pour définir le mappage entre la table source et la DataTable.

- L’exemple utilise `DataAdapter.FillLoadOption` pour déterminer comment l’adaptateur remplit la **DataTable** à partir du **DbDataReader**. Quand vous créez une DataTable, vous pouvez seulement écrire les données de la base de données vers la version actuelle ou la version d’origine en définissant la propriété sur **LoadOption.Upsert** ou **LoadOption.PreserveChanges**.

- L’exemple met également à jour la table en utilisant `DbDataAdapter.UpdateBatchSize` pour effectuer les opérations de traitement par lot.

Avant de compiler et d'exécuter l'exemple, vous devez créer l'exemple de base de données :

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

[!code-csharp[SqlDataAdapter_Properties#1](~/../sqlclient/doc/samples/SqlDataAdapter_Properties.cs#1)]

## <a name="see-also"></a>Voir aussi

- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Extraction de l’identité ou de valeurs à numérotation automatique](retrieve-identity-or-autonumber-values.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
