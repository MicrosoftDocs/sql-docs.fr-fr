---
title: Obtenir un SqlClientFactory
description: Découvrez comment obtenir un SqlClientFactory à partir de la classe DbProviderFactories pour travailler avec des sources de données spécifiques dans .NET.
ms.date: 12/22/2020
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 891cabf04bc8e63537983a91d8526a5cbdf238bf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771755"
---
# <a name="obtain-a-sqlclientfactory"></a>Obtenir un SqlClientFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Le processus d'obtention d'un objet <xref:System.Data.Common.DbProviderFactory> implique de passer des informations à propos d'un fournisseur de données à la classe <xref:System.Data.Common.DbProviderFactories>. En fonction de ces informations, la méthode <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> crée une fabrique de fournisseurs fortement typée. Par exemple, pour créer un <xref:Microsoft.Data.SqlClient.SqlClientFactory>, vous pouvez passer à `GetFactory` une chaîne avec le nom du fournisseur spécifié, comme « **Microsoft.Data.SqlClient** ».

L'autre surcharge de `GetFactory` prend un objet <xref:System.Data.DataRow>. Une fois que vous avez créé la fabrique de fournisseurs, vous pouvez ensuite utiliser ses méthodes pour créer des objets supplémentaires. Parmi les méthodes d'un `SqlClientFactory`, citons <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateConnection%2A>, <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateCommand%2A> et <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>.

## <a name="register-sqlclientfactory"></a>Inscrire SqlClientFactory

Pour récupérer l’objet <xref:Microsoft.Data.SqlClient.SqlClientFactory> par la classe <xref:System.Data.Common.DbProviderFactories> dans .NET Framework, il est nécessaire de l’inscrire dans un fichier **App.config** ou **web.config**. Le fragment de fichier de configuration suivant montre la syntaxe et le format de <xref:Microsoft.Data.SqlClient>.  

```xml  
<system.data>
  <DbProviderFactories>
    <add name="Microsoft SqlClient Data Provider"
      invariant="Microsoft.Data.SqlClient"
      description="Microsoft SqlClient Data Provider for SQL Server"
      type="Microsoft.Data.SqlClient.SqlClientFactory, Microsoft.Data.SqlClient, Version=2.0.20168.4, Culture=neutral, PublicKeyToken=23ec7fc2d6eaa4a5"/>
  </DbProviderFactories>
</system.data>  
```  

L’attribut **invariant** identifie le fournisseur de données sous-jacent. Cette syntaxe d'attribution de nom en trois parties est également utilisée lors de la création d'une fabrique et pour l'identification du fournisseur dans un fichier de configuration d'application de manière à ce que le nom du fournisseur, ainsi que sa chaîne de connexion associée, puissent être récupérés au moment de l'exécution.  

> [!NOTE]  
> Dans .NET Core, étant donné qu’il n’existe pas de prise en charge de GAC ou de la configuration globale, l’objet <xref:Microsoft.Data.SqlClient.SqlClientFactory> doit être inscrit en appelant la méthode <xref:System.Data.Common.DbProviderFactories.RegisterFactory%2A> dans le projet.

L’exemple suivant montre comment utiliser <xref:Microsoft.Data.SqlClient.SqlClientFactory> dans une application .NET Core.

[!code-csharp[SqlClientFactory_Netcoreapp#1](~/../sqlclient/doc/samples/SqlClientFactory_Netcoreapp.cs#1)]

## <a name="see-also"></a>Voir aussi

- [DbProviderFactories](dbproviderfactories.md)
- [Chaînes de connexion](connection-strings.md)
- [Utilisation des classes de configuration](/previous-versions/aspnet/ms228063(v=vs.100))
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
