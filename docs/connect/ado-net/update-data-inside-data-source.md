---
title: Mise à jour des données dans une source de données
description: Décrit l'exécution de commandes ou de procédures stockées qui modifient les données dans une base de données.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 55c545e5-dcd5-4323-a5b9-3825c2157462
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: aca7b1adb8ce91a12832bccfd6cbd27b07229d22
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771481"
---
# <a name="updating-data-in-a-data-source"></a>Mise à jour des données dans une source de données

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Les instructions SQL qui modifient les données (comme INSERT, UPDATE ou DELETE) ne retournent pas de ligne. De même, de nombreuses procédures stockées effectuent une action mais ne retournent pas de ligne. Pour exécuter des commandes qui ne retournent pas de ligne, créez un objet **Commande** avec la commande SQL appropriée et un objet **Connexion** incluant tous les **Paramètres** requis. Exécutez la commande avec la méthode <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> de l’objet <xref:Microsoft.Data.SqlClient.SqlCommand>.

> [!NOTE]
> La méthode **ExecuteNonQuery** retourne un entier qui représente le nombre de lignes affectées par l'instruction ou la procédure stockée exécutée. Si plusieurs instructions sont exécutées, la valeur retournée est la somme des enregistrements affectés par toutes ces instructions.

## <a name="example"></a>Exemple

L'exemple de code suivant exécute une instruction INSERT pour insérer un enregistrement dans une base de données à l'aide de **ExecuteNonQuery**.
  
[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#1)]

L'exemple de code suivant exécute la procédure stockée créée par l'exemple de code dans [Exécution des opérations de catalogue](perform-catalog-operations.md). Aucune ligne n'est retournée par la procédure stockée, la méthode **ExecuteNonQuery** est donc utilisée mais la procédure stockée reçoit un paramètre d'entrée et retourne un paramètre de sortie et une valeur de retour.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#2](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#2)]

## <a name="see-also"></a>Voir aussi

- [Utilisation des commandes pour modifier les données](use-commands-to-modify-data.md)
- [Mise à jour de sources de données avec des DataAdapter](update-data-sources-with-dataadapters.md)
- [Commandes et paramètres](commands-parameters.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
