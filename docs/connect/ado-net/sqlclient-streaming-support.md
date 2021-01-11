---
title: Prise en charge de la diffusion en continu SqlClient
description: Explique comment écrire des applications qui diffusent en continu les données provenant de SQL Server sans avoir à les charger complètement en mémoire.
ms.date: 12/04/2020
ms.assetid: c449365b-470b-4edb-9d61-8353149f5531
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c5ae05856f3f1e01d5831a6e80338f19e3994966
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744362"
---
# <a name="sqlclient-streaming-support"></a>Prise en charge de la diffusion en continu SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La prise en charge de la diffusion en continu entre SQL Server et une application prend en charge des données non structurées sur le serveur (documents, images et fichiers multimédias). Une base de données SQL Server peut stocker des objets BLOB, mais la récupération de BLOB peut utiliser beaucoup de mémoire.

La prise en charge de la diffusion en continu de SQL Serveur simplifie l’écriture d’applications qui diffusent les données, sans devoir entièrement charger les données dans la mémoire, ce qui réduit le nombre d’exceptions de dépassement de capacité de mémoire.

La prise en charge de la diffusion en continu permet également aux applications de couche intermédiaire de mieux se mettre à l’échelle, en particulier dans les scénarios où les objets métier se connectent à Azure SQL pour envoyer, récupérer et manipuler de grands objets Blob.

> [!WARNING]
> Les membres qui prennent en charge la diffusion en continu sont utilisés pour récupérer des données à partir de requêtes et pour passer des paramètres aux requêtes et aux procédures stockées. La fonctionnalité de diffusion en continu résout les scénarios OLTP et de migration des données de base et s’applique aux environnements de migration de données locaux et hors site.

## <a name="streaming-support-from-sql-server"></a>Prise en charge de la diffusion en continu depuis SQL Server

La prise en charge de la diffusion en continu de SQL Server introduit une nouvelle fonctionnalité dans les classes <xref:System.Data.Common.DbDataReader> et <xref:Microsoft.Data.SqlClient.SqlDataReader> pour obtenir les objets <xref:System.IO.Stream>, <xref:System.Xml.XmlReader> et <xref:System.IO.TextReader> et réagir à ces derniers. Ces classes sont utilisées pour récupérer les données des requêtes. Par conséquent, la prise en charge de la diffusion en continu de SQL Server répond aux scénarios OLTP et s’applique aux environnements sur site et hors site.

Les membres suivants ont été ajoutés à <xref:Microsoft.Data.SqlClient.SqlDataReader> pour activer la prise en charge de la diffusion en continu de SQL Server :

- <xref:Microsoft.Data.SqlClient.SqlDataReader.IsDBNullAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValue%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValueAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetStream%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetTextReader%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetXmlReader%2A>

Les membres suivants ont été ajoutés à <xref:System.Data.Common.DbDataReader> pour activer la prise en charge de la diffusion en continu de SQL Server :

- <xref:System.Data.Common.DbDataReader.GetFieldValue%2A>

- <xref:System.Data.Common.DbDataReader.GetStream%2A>

- <xref:System.Data.Common.DbDataReader.GetTextReader%2A>

## <a name="streaming-support-to-sql-server"></a>Prise en charge de la diffusion en continu vers SQL Server

La prise en charge de la diffusion en continu vers SQL Server se trouve dans la classe <xref:Microsoft.Data.SqlClient.SqlParameter> de façon à ce qu’elle reçoive et réagisse aux objets <xref:System.Xml.XmlReader>, <xref:System.IO.Stream> et <xref:System.IO.TextReader>. <xref:Microsoft.Data.SqlClient.SqlParameter> est utilisé pour passer des paramètres aux requêtes et aux procédures stockées.

> [!NOTE]
> Supprimer un objet <xref:Microsoft.Data.SqlClient.SqlCommand> ou appeler <xref:Microsoft.Data.SqlClient.SqlCommand.Cancel%2A> doit annuler toute opération en continu. Si une application envoie <xref:System.Threading.CancellationToken>, l'annulation n'est pas garantie.

Les types suivants <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> acceptent un <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> de <xref:System.IO.Stream> :

- **Binaire**

- **VarBinary**

Les types suivants <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> acceptent un <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> de <xref:System.IO.TextReader> :

- **Char**

- **NChar**

- **NVarChar**

- **Xml**

Le type **XML**<xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> accepte une <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> de <xref:System.Xml.XmlReader>.

<xref:Microsoft.Data.SqlClient.SqlParameter.SqlValue%2A> accepte des valeurs de type <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> et <xref:System.IO.Stream>.

<xref:System.Xml.XmlReader>, <xref:System.IO.TextReader>, et l'objet <xref:System.IO.Stream> seront envoyés jusqu'à la valeur définie par <xref:Microsoft.Data.SqlClient.SqlParameter.Size%2A>.

## <a name="sample----streaming-from-sql-server"></a>Exemple -- Diffusion en continu à partir de SQL Server

Utilisez la commande Transact-SQL suivante pour créer l’exemple de base de données :

```sql
CREATE DATABASE [Demo]
GO
USE [Demo]
GO
CREATE TABLE [Streams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX),
[bindata] VARBINARY(MAX),
[xmldata] XML)
GO
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'This is a test', 0x48656C6C6F, N'<test>value</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Hello, World!', 0x54657374696E67, N'<test>value2</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Another row', 0x666F6F626172, N'<fff>bbb</fff><fff>bbc</fff>')
GO
```

L'exemple montre comment effectuer les actions suivantes :

- Éviter de bloquer un thread d'interface utilisateur en fournissant une façon asynchrone de récupérer des fichiers volumineux.

- Transférer un fichier texte volumineux à partir de SQL Server dans .NET.

- Transférer un fichier XML volumineux à partir de SQL Server dans .NET.

- Récupérez les données de SQL Server.

- Transférer des fichiers volumineux (objets BLOB) d’une base de données SQL Server à une autre sans insuffisance de mémoire.

[!code-csharp[SqlClient_Streaming_FromServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_FromServer.cs#1)]

## <a name="sample----streaming-to-sql-server"></a>Exemple -- Diffusion en continu vers SQL Server

Utilisez la commande Transact-SQL suivante pour créer l’exemple de base de données :

```sql
CREATE DATABASE [Demo2]
GO
USE [Demo2]
GO
CREATE TABLE [BinaryStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
CREATE TABLE [TextStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX))
GO
CREATE TABLE [BinaryStreamsCopy] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
```

L'exemple montre comment effectuer les actions suivantes :

- Transfert d’un objet Blob volumineux vers SQL Server dans .NET.

- Transfert d’un fichier texte volumineux vers SQL Server dans .NET.

- Utilisation de la nouvelle fonctionnalité asynchrone pour transférer un objet BLOB.

- Utilisation de la nouvelle fonctionnalité asynchrone et du mot clé await pour transférer un objet BLOB.

- Annuler le transfert d’un grand BLOB.

- Diffusion en continu de SQL Server à une autre application à l’aide de la fonctionnalité asynchrone.

[!code-csharp[SqlClient_Streaming_ToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ToServer.cs#1)]

## <a name="sample----streaming-from-one-sql-server-to-another-sql-server"></a>Exemple -- Diffusion en continu d’une instance SQL Server vers une autre instance SQL Server

Cet exemple montre comment diffuser en continu un objet BLOB d’un SQL Server à un autre, avec la prise en charge de l’annulation.

> [!NOTE]
> Avant d’exécuter l’exemple suivant, veillez à créer les bases de données Demo et Demo2.

[!code-csharp[SqlClient_Streaming_ServerToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ServerToServer.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Récupération et modification de données dans ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
