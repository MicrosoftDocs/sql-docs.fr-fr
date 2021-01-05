---
title: Obtention d'une valeur unique à partir d'une base de données
description: Découvrez comment retourner une seule valeur dans ADO.NET. Cet exemple de code retourne la valeur de la colonne d’identité pour un enregistrement inséré.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: b38526cd-a62a-48cb-822a-e91dfa68e02d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3937c1d4617f3cecb9012d82d590f75634ab1043
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771491"
---
# <a name="obtaining-a-single-value-from-a-database"></a>Obtention d'une valeur unique à partir d'une base de données

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Vous aurez peut-être besoin de retourner des informations de base de données qui sont simplement une valeur unique plutôt qu'une table ou un flux de données. Par exemple, vous souhaitez éventuellement retourner le résultat d'une fonction d'agrégation telle que COUNT(\*), SUM(Prix) ou AVG(Quantité). L'objet **Commande** fournit la capacité permettant de retourner des valeurs uniques à l'aide de la méthode **ExecuteScalar**. La méthode **ExecuteScalar** retourne sous forme de valeur scalaire la valeur de la première colonne de la première ligne du jeu de résultats.

## <a name="example"></a>Exemple

L'exemple de code suivant insère une nouvelle valeur dans la base de données en utilisant un objet <xref:Microsoft.Data.SqlClient.SqlCommand>. La méthode <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalar%2A> est utilisée pour retourner la valeur de la colonne d'identité de l'enregistrement inséré.

[!code-csharp[DataWorks SqlCommand.ExecuteScalar#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteScalar_Return_Id.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Commandes et paramètres](commands-parameters.md)
- [Exécution d'une commande](execute-command.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
