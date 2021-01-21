---
title: Configuration des paramètres
description: Les objets de commande utilisent des paramètres pour passer des valeurs à des instructions SQL ou à des procédures stockées, en fournissant le contrôle et la validation de type dans ADO.NET.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 537d8a2c-d40b-4000-83eb-bc1fcc93f707
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12dfcb26484b468bed9f9262403806ea88829385
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595770"
---
# <a name="configuring-parameters"></a>Configuration des paramètres

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Les objets de commande utilisent des paramètres pour passer des valeurs à des instructions SQL ou à des procédures stockées, en fournissant la vérification et la validation des types. Contrairement au texte de la commande, l'entrée de paramètre est traitée comme une valeur littérale et non pas comme du code exécutable. Cela vous permet de vous protéger des attaques « par injection de code SQL », dans lesquelles un attaquant insère une commande qui compromet la sécurité sur le serveur dans une instruction SQL.

Les commandes paramétrées améliorent également les performances d'exécution des requêtes car elles permettent au serveur de base de données de faire correspondre la commande entrante avec un plan de requête mis en cache approprié. Pour plus d’informations, consultez [Mise en cache et réutilisation du plan d'exécution](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse) et [Réutilisation des paramètres et du plan d'exécution](../../relational-databases/query-processing-architecture-guide.md#PlanReuse). Outre les avantages relatifs à la sécurité et aux performances, les commandes paramétrées fournissent une méthode pratique d'organisation des valeurs passées à une source de données.

Un objet <xref:System.Data.Common.DbParameter> peut être créé à l'aide de son constructeur ou en l'ajoutant à la propriété <xref:System.Data.Common.DbCommand.DbParameterCollection%2A> en appelant la méthode `Add` de la collection <xref:System.Data.Common.DbParameterCollection> . La méthode `Add` prendra comme entrée des arguments de constructeur ou un objet Parameter existant, selon le fournisseur de données.

## <a name="supply-the-parameterdirection-property"></a>Fournir la propriété ParameterDirection

Lorsque vous ajoutez des paramètres, vous devez fournir une propriété <xref:System.Data.ParameterDirection> pour les paramètres autres que les paramètres d'entrée. Le tableau ci-dessous indique les valeurs `ParameterDirection` que vous pouvez utiliser avec l'énumération <xref:System.Data.ParameterDirection> .

|Nom du membre|Description|
|-----------------|-----------------|
|<xref:System.Data.ParameterDirection.Input>|Le paramètre est un paramètre d'entrée. Il s’agit de la valeur par défaut.|
|<xref:System.Data.ParameterDirection.InputOutput>|Le paramètre peut être à la fois un paramètre d'entrée et de sortie.|
|<xref:System.Data.ParameterDirection.Output>|Le paramètre est un paramètre de sortie.|
|<xref:System.Data.ParameterDirection.ReturnValue>|Le paramètre représente une valeur de retour d'une opération telle qu'une procédure stockée, une fonction intégrée ou une fonction définie par l'utilisateur.|

## <a name="work-with-parameter-placeholders"></a>Utiliser des espaces réservés de paramètres

La syntaxe des espaces réservés des paramètres dépend de la source de données. Le Fournisseur de données Microsoft SqlClient pour SQL Server gère différemment la dénomination et la spécification des paramètres et des espaces réservés de paramètres. Le Fournisseur de données SqlClient utilise des paramètres nommés au format `@`*parametername*.

## <a name="specify-parameter-data-types"></a>Spécifier les types de données des paramètres

Le type de données d’un paramètre est spécifique au Fournisseur de données Microsoft SqlClient pour SQL Server. La spécification du type convertit la valeur de `Parameter` en Fournisseur de données Microsoft SqlClient pour le type de SQL Server avant de passer la valeur à la source de données. Vous pouvez également spécifier le type d'un `Parameter` de façon générique en affectant à la propriété `DbType` de l'objet `Parameter` un <xref:System.Data.DbType>particulier.

Le type Fournisseur de données Microsoft SqlClient pour SQL Server d’un objet `Parameter` est déduit à partir du type .NET Framework de la valeur `Value` de l’objet `Parameter` ou à partir de `DbType` de l’objet `Parameter`. Le tableau suivant indique le type `Parameter` déduit en fonction de l'objet passé comme valeur `Parameter` ou du `DbType`spécifié.

|Type .NET|DbType|SqlDbType|
|-------------------------|------------|---------------|
|<xref:System.Boolean>|Booléen|bit|
|<xref:System.Byte>|Byte|TinyInt|
|byte[]|Binary|VarBinary. Cette conversion implicite échouera si le tableau d'octets est supérieur à la taille maximale d'un VarBinary, soit 8 000 octets. Pour des tableaux d'octets supérieurs à 8 000 octets, définissez explicitement <xref:System.Data.SqlDbType>.|
|<xref:System.Char>| |La déduction de <xref:System.Data.SqlDbType> à partir de char n'est pas prise en charge.|
|<xref:System.DateTime>|DateTime|DateTime|
|<xref:System.DateTimeOffset>|DateTimeOffset|DateTimeOffset dans SQL Server 2008. La déduction de <xref:System.Data.SqlDbType> à partir de DateTimeOffset n'est pas prise en charge dans les versions de SQL Server antérieures à SQL Server 2008.|
|<xref:System.Decimal>|Decimal|Decimal|
|<xref:System.Double>|Double|Float|
|<xref:System.Single>|Unique|Real|
|<xref:System.Guid>|Guid|UniqueIdentifier|
|<xref:System.Int16>|Int16|SmallInt|
|<xref:System.Int32>|Int32|Int|
|<xref:System.Int64>|Int64|BigInt|
|<xref:System.Object>|Objet|Variante|
|<xref:System.String>|String|NVarChar. Cette conversion implicite échouera si la chaîne est supérieure à la taille maximale de NVarChar, soit 4 000 caractères. Pour les chaînes supérieures à 4 000 caractères, définissez explicitement <xref:System.Data.SqlDbType>.|
|<xref:System.TimeSpan>|Temps|Time dans SQL Server 2008. La déduction de <xref:System.Data.SqlDbType> à partir de TimeSpan n'est pas prise en charge dans les versions de SQL Server antérieures à SQL Server 2008.|
|<xref:System.UInt16>|UInt16|La déduction de <xref:System.Data.SqlDbType> à partir de UInt16 n'est pas prise en charge.|
|<xref:System.UInt32>|UInt32|La déduction de <xref:System.Data.SqlDbType> à partir de UInt32 n'est pas prise en charge.|
|<xref:System.UInt64>|UInt64|La déduction de <xref:System.Data.SqlDbType> à partir de UInt64 n'est pas prise en charge.|
||AnsiString|VarChar|
||AnsiStringFixedLength|Char|
||Devise|Money|
||Date|Date dans SQL Server 2008. La déduction de <xref:System.Data.SqlDbType> à partir de Date n'est pas prise en charge dans les versions de SQL Server antérieures à SQL Server 2008.|
||SByte|La déduction de <xref:System.Data.SqlDbType> à partir de SByte n'est pas prise en charge.|
||StringFixedLength|NChar|
||Temps|Time dans SQL Server 2008. La déduction de <xref:System.Data.SqlDbType> à partir de Time n'est pas prise en charge dans les versions de SQL Server antérieures à SQL Server 2008.|
||VarNumeric|La déduction de <xref:System.Data.SqlDbType> à partir de VarNumeric n'est pas prise en charge.|
|type défini par l'utilisateur (objet avec <xref:Microsoft.SqlServer.Server.SqlUserDefinedAggregateAttribute>)|SqlClient retourne toujours un Objet|`SqlDbType.Udt` si <xref:Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute> est présent ; sinon, `Variant`.|

> [!NOTE]
> Les conversions du type decimal vers d'autres types sont des conversions restrictives qui arrondissent la valeur décimale à la valeur entière la plus proche de zéro. Si le résultat de la conversion ne peut pas être représenté dans le type de destination, un <xref:System.OverflowException> est levé.

> [!NOTE]
> Lorsque vous envoyez une valeur de paramètre Null au serveur, vous devez spécifier <xref:System.DBNull> plutôt que `null` (`Nothing` en Visual Basic). Dans le système, la valeur null désigne un objet vide qui ne possède pas de valeur. <xref:System.DBNull> est utilisé pour représenter des valeurs null.

## <a name="derive-parameter-information"></a>Dériver les informations de paramètre

Les paramètres peuvent aussi être dérivés d'une procédure stockée à l'aide de la classe `DbCommandBuilder` . La classe `SqlCommandBuilder` fournit une méthode statique, `DeriveParameters`, qui remplit automatiquement la collection Paramètres d’un objet de commande qui utilise les informations sur les paramètres d’une procédure stockée. Notez que `DeriveParameters` remplace toutes les informations existantes sur les paramètres pour la commande.

> [!NOTE]
> La dérivation des informations de paramètre entraîne une baisse des performances car elle requiert un aller-retour supplémentaire vers la source de données pour extraire les informations. Si les informations sur les paramètres sont connues au moment du design, vous pouvez améliorer la performance de votre application en définissant les paramètres de manière explicite.

Pour plus d’informations, consultez [Génération de commandes avec CommandBuilders](generate-commands-with-commandbuilders.md).

## <a name="using-parameters-with-a-sqlcommand-and-a-stored-procedure"></a>Utilisation de paramètres avec SqlCommand et une procédure stockée

Les procédures stockées offrent de nombreux avantages dans les applications pilotées par des données. En utilisant les procédures stockées, les opérations de base de données peuvent être encapsulées dans une commande unique, optimisées pour de meilleures performances et améliorées grâce à une sécurité supplémentaire. Bien qu’une procédure stockée puisse être appelée en passant son nom suivi des arguments de paramètre comme instruction SQL, l’utilisation de la collection <xref:System.Data.Common.DbCommand.Parameters%2A> de l’objet ADO.NET <xref:System.Data.Common.DbCommand> vous permet de définir plus explicitement les paramètres de procédure stockée et d’accéder aux paramètres de sortie et aux valeurs de retour.

> [!NOTE]
> Les instructions paramétrées sont exécutées sur le serveur à l'aide de `sp_executesql,` , ce qui permet la réutilisation des plans de requête. Les curseurs ou variables locaux dans le lot `sp_executesql` ne sont pas visibles pour le lot qui appelle `sp_executesql`. Les modifications dans le contexte de la base de données durent uniquement jusqu'à la fin de l'instruction `sp_executesql` . Pour plus d’informations, consultez [sp_executesql (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md).

Lorsque vous utilisez des paramètres avec un objet <xref:Microsoft.Data.SqlClient.SqlCommand> pour exécuter une procédure stockée SQL Server, les noms des paramètres ajoutés à la collection <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A> doivent correspondre à ceux des marqueurs de paramètres dans la procédure stockée. Le Fournisseur de données Microsoft SqlClient pour SQL Server ne prend pas en charge l’espace réservé du point d’interrogation ( ?) pour le passage de paramètres à une instruction SQL ou à une procédure stockée. Il traite les paramètres de la procédure stockée comme des paramètres nommés et recherche les marqueurs de paramètres correspondants. Par exemple, la procédure stockée `CustOrderHist` est définie à l'aide d'un paramètre nommé `@CustomerID`. Lorsque votre code exécute la procédure stockée, il doit également utiliser un paramètre nommé `@CustomerID`.

```sql
CREATE PROCEDURE dbo.CustOrderHist @CustomerID varchar(5)
```

### <a name="example"></a>Exemple

Cet exemple montre comment appeler une procédure stockée SQL Server dans l'exemple de base de données `Northwind` . Le nom de la procédure stockée est `dbo.SalesByCategory` et il possède un paramètre d'entrée nommé `@CategoryName` avec un type de données `nvarchar(15)`. Le code crée un nouveau <xref:Microsoft.Data.SqlClient.SqlConnection> à l'intérieur d'un bloc using pour que la connexion soit libérée une fois la procédure terminée. Les objets <xref:Microsoft.Data.SqlClient.SqlCommand> et <xref:Microsoft.Data.SqlClient.SqlParameter> sont créés et leurs propriétés sont définies. Un <xref:Microsoft.Data.SqlClient.SqlDataReader> exécute `SqlCommand` et retourne le jeu de résultats provenant de la procédure stockée, en affichant la sortie dans la fenêtre de console.

> [!NOTE]
> Au lieu de créer les objets `SqlCommand` et `SqlParameter` puis de définir les propriétés dans des instructions distinctes, vous pouvez choisir d'utiliser l'un des constructeurs surchargés pour définir plusieurs propriétés dans une instruction unique.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Commandes et paramètres](commands-parameters.md)
- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Mappages des types de données dans ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)