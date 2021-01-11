---
title: Extraction de données binaires
description: Décrit comment extraire des données binaires ou de grosses structures de données à l’aide de `CommandBehavior`.`SequentialAccess` pour modifier le comportement par défaut d’un `DataReader`.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 56c5a9e3-31f1-482f-bce0-ff1c41a658d0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1033a6b5394d92a45cd19d70f4dd6c250ec37b62
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744382"
---
# <a name="retrieve-binary-data"></a>Extraction de données binaires

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Par défaut, le **DataReader** charge les données entrantes comme une ligne dès qu’une ligne de données complète est disponible. Les objets binaires volumineux ou BLOB doivent néanmoins être traités différemment car ils peuvent renfermer plusieurs gigaoctets de données qu'une seule ligne ne suffirait pas à contenir. La méthode **Command.ExecuteReader** a une surcharge qui prendra un argument <xref:System.Data.CommandBehavior> pour modifier le comportement par défaut du **DataReader**. Vous pouvez passer <xref:System.Data.CommandBehavior.SequentialAccess> à la méthode **ExecuteReader** pour modifier le comportement par défaut du **DataReader** de sorte qu’au lieu de charger des lignes de données, il chargera les données de façon séquentielle au fur et à mesure de leur réception. Cela est idéal pour charger les BLOB ou d'autres grosses structures de données.

> [!NOTE]
> Lors de la configuration du **DataReader** pour qu’il utilise **SequentialAccess**, il est important de noter l’ordre dans lequel vous accédez aux champs retournés. Le comportement par défaut du **DataReader**, qui charge une ligne entière dès qu’elle est disponible, vous permet d’accéder aux champs retournés dans n’importe quel ordre jusqu’à la lecture de la ligne suivante. Lors de l’utilisation de **SequentialAccess**, cependant, vous devez accéder dans l’ordre aux différents champs retournés par le **DataReader**. Par exemple, si votre requête retourne trois colonnes, la troisième étant un BLOB, vous devez retourner les valeurs des premier et deuxième champs avant d'accéder aux données BLOB du troisième champ. Si vous accédez au troisième champ avant le premier ou le deuxième, les valeurs de ces champs ne seront plus disponibles. Cela s’explique par le fait que **SequentialAccess** a modifié le **DataReader** pour retourner les données dans l’ordre et les données ne sont plus disponibles après que **DataReader** en a dépassé la fin.

Lorsque vous accédez aux données du champ BLOB, utilisez l’accesseur typé **GetBytes** ou **GetChars** du **DataReader** qui remplit un tableau avec les données. Vous pouvez également utiliser **GetString** pour les données de type caractère ; toutefois, pour économiser les ressources système, vous souhaitez peut-être ne pas charger la valeur entière du BLOB dans une seule variable chaîne. Au lieu de cela, vous pouvez spécifier une taille de mémoire tampon spécifique des données à retourner ainsi qu'un emplacement de départ pour le premier octet ou le premier caractère lu des données retournées. **GetBytes** et **GetChars** retourneront une valeur `long`, qui représente le nombre d’octets ou de caractères retournés. Si vous passez un tableau null à **GetBytes** ou **GetChars**, la valeur de type long retournée sera le nombre total d’octets ou de caractères contenus dans le BLOB. Vous pouvez éventuellement spécifier un index dans le tableau comme position de départ pour les données en cours de lecture.

## <a name="example"></a>Exemple

L’exemple suivant retourne l’ID et le logo de l’éditeur de l’[exemple de la base de données **pubs**](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs). L'ID de l'éditeur (`pub_id`) est un champ texte et le logo est une image (un BLOB). Étant donné que le champ **logo** est une bitmap, l’exemple retourne des données binaires au moyen de **GetBytes**. Notez que pour la ligne de données actuelle, il faut accéder à l'ID de l'éditeur avant d'accéder au logo car l'accès aux champs doit être séquentiel.

[!code-csharp[SqlCommand_ExecuteReader_SequentialAccess#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteReader_SequentialAccess.cs#1)]

## <a name="see-also"></a>Voir aussi

- [Données binaires et à valeurs élevées SQL Server](./sql/sql-server-binary-large-value-data.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
