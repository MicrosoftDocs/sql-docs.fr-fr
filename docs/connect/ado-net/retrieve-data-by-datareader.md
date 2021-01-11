---
title: Récupération de données par un DataReader
description: Découvrez comment récupérer des données en utilisant un DataReader dans ADO.NET avec cet exemple de code. Un DataReader fournit un flux de données sans mise en mémoire tampon.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: dfbe41fe29a3dadc69a2a428f1bf8b606a2a7050
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771333"
---
# <a name="retrieve-data-by-a-datareader"></a>Récupération de données par un DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Pour récupérer des données en utilisant un **DataReader**, créez une instance de l’objet **Command**, puis créez un **DataReader** en appelant **Command.ExecuteReader** pour extraire des lignes d’une source de données. Le **DataReader** fournit un flux de données sans mise en mémoire tampon qui permet à la logique procédurale de traiter efficacement les résultats provenant d’une source de données de façon séquentielle.

> [!NOTE]
> Le **DataReader** se révèle être un bon choix quand vous récupérez de grandes quantités de données, car celles-ci ne sont pas mises en cache dans la mémoire.

Voici un exemple d’utilisation d’un **DataReader** où `reader` représente un DataReader valide et où `command` représente un objet Command valide.  

```csharp
reader = command.ExecuteReader();  
```

Utilisez la méthode **DataReader.Read** pour obtenir une ligne des résultats de la requête. Vous pouvez accéder à chaque colonne de la ligne retournée en passant le nom ou la référence ordinale de la colonne au **DataReader**. Cependant, pour permettre de meilleures performances, le **DataReader** fournit une série de méthodes qui vous permettent d’accéder aux valeurs des colonnes dans leurs types de données natifs (**GetDateTime**, **GetDouble**, **GetGuid**, **GetInt32**, etc.). Pour obtenir une liste de méthodes d’accesseur typées pour les **DataReaders** spécifiques au fournisseur de données, consultez <xref:Microsoft.Data.SqlClient.SqlDataReader>. L’utilisation des méthodes d’accesseur typées quand vous connaissez le type de données sous-jacent réduit la quantité des conversions de type nécessaires lors de l’extraction de la valeur de colonne.  

L’exemple de code suivant boucle dans un objet **DataReader** et retourne deux colonnes à partir de chaque ligne.  

[!code-csharp[DataWorks SqlClient.HasRows#1](~/../sqlclient/doc/samples/SqlDataReader_HasRows.cs#1)]

## <a name="close-the-datareader"></a>Fermer le DataReader  

Appelez toujours la méthode `Close()` quand vous avez fini d’utiliser l’objet `DataReader`.

> [!NOTE]
> Si votre **Command** contient des paramètres de sortie ou des valeurs de retour, ces valeurs ne sont pas disponibles tant que le **DataReader** n’est pas fermé.  

> [!IMPORTANT]
> Pendant qu’un **DataReader** est ouvert, la **Connexion** est utilisée exclusivement par ce **DataReader**. Vous ne pouvez pas exécuter de commandes pour la **Connexion**, y compris créer un autre **DataReader**, tant que le **DataReader** d’origine n’est pas fermé.  

> [!NOTE]
> N’appelez pas **Close** ou **Dispose** sur une **Connexion**, sur un **DataReader** ou sur tout autre objet géré dans la méthode **Finalize** de votre classe. Dans un finaliseur, libérez seulement les ressources non managées que votre classe possède directement. Si votre classe ne possède pas de ressources non gérées, n’incluez pas de méthode **Finalize** dans la définition de votre classe. Pour plus d’informations, consultez [Nettoyage de la mémoire](/dotnet/standard/garbage-collection/index).
 
## <a name="retrieve-multiple-result-sets-using-nextresult"></a>Récupération plusieurs jeux de résultats en utilisant NextResult

Si le **DataReader** retourne plusieurs jeux de résultats, appelez la méthode **NextResult** pour boucler séquentiellement dans les jeux de résultats. L'exemple suivant montre l'objet <xref:Microsoft.Data.SqlClient.SqlDataReader> traitant les résultats de deux instructions SELECT utilisant la méthode <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A>.  

[!code-csharp[DataWorks SqlClient.NextResult#1](~/../sqlclient/doc/samples/SqlDataReader_NextResult.cs#1)]

## <a name="get-schema-information-from-the-datareader"></a>Obtenir des informations de schéma auprès du DataReader  

Pendant qu’un **DataReader** est ouvert, vous pouvez extraire des informations de schéma sur le jeu de résultats actif en utilisant la méthode **GetSchemaTable**. **GetSchemaTable** retourne un objet <xref:System.Data.DataTable> rempli de lignes et de colonnes qui contiennent les informations de schéma pour le jeu de résultats actif. La **DataTable** contient une ligne pour chaque colonne du jeu de résultats. Chaque colonne de la table du schéma est mappée à une propriété des colonnes retournées dans les lignes du jeu de résultats, où **ColumnName** est le nom de la propriété et où la valeur de la colonne est la valeur de la propriété. L’exemple de code suivant écrit les informations de schéma pour **DataReader**.  

[!code-csharp[DataWorks SqlClient.GetSchemaTable#1](~/../sqlclient/doc/samples/SqlDataReader_GetSchemaTable.cs#1)]

## <a name="see-also"></a>Voir aussi

- [DataAdapters et DataReaders](dataadapters-datareaders.md)
- [Commandes et paramètres](commands-parameters.md)
- [Extraction des informations de schéma de base de données](retrieving-database-schema-information.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
