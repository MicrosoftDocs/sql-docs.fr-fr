---
title: DbProviderFactories
description: Décrit le modèle fabrique de fournisseurs et illustre l'utilisation des classes de base dans l'espace de noms `System.Data.Common`.
ms.date: 12/22/2020
ms.assetid: 2a8e2640-3a49-42a1-a3a9-b43026907ae1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1a373e7bde1e93d8dc92294f983a96de64e28ae1
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771773"
---
# <a name="dbproviderfactories"></a>DbProviderFactories

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

L'espace de noms <xref:System.Data.Common> fournit des classes permettant de créer des instances <xref:System.Data.Common.DbProviderFactory> afin d'utiliser des sources de données spécifiques. Lorsque vous créez une instance <xref:System.Data.Common.DbProviderFactory> et que vous lui passez des informations sur le fournisseur de données, `DbProviderFactory` peut déterminer l'objet de connexion fortement typé correct à retourner en fonction des informations qui lui ont été fournies.  

Le fournisseur de données <xref:Microsoft.Data.SqlClient> n’est plus répertorié dans le fichier machine.config, mais les fournisseurs personnalisés continueront d’y figurer.  

## <a name="in-this-section"></a>Contenu de cette section  

[Obtenir un SqlClientFactory](obtain-sqlclientfactory.md)  
Montre comment obtenir un `SqlClientFactory` de la classe `DbProviderFactories` pour travailler avec des sources de données spécifiques dans .NET.  

## <a name="see-also"></a>Voir aussi

- [Récupération et modification de données dans ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
