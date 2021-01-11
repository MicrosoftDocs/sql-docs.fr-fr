---
title: Chaînes de connexion
description: En savoir plus sur les chaînes de connexion dans le Fournisseur de données Microsoft SqlClient pour SQL Server, qui contiennent les informations d’initialisation transmises comme paramètre d’un fournisseur de données à une source de données.
ms.date: 11/13/2020
ms.assetid: 745c5f95-2f02-4674-b378-6d51a7ec2490
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 257ed7d43f8ab204c7c7e7575c69251be6f2efdf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771624"
---
# <a name="connection-strings-in-adonet"></a>Chaînes de connexion dans ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Une chaîne de connexion contient des informations d'initialisation qui sont passées en tant que paramètre d'un fournisseur de données à une source de données. Le fournisseur de données reçoit la chaîne de connexion en tant que valeur de la propriété <xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType>. Le fournisseur analyse la chaîne de connexion et s’assure que la syntaxe est correcte et que les mots clés sont pris en charge. Ensuite, la méthode <xref:System.Data.Common.DbConnection.Open?displayProperty=nameWithType> transmet les paramètres de connexion analysés à la source de données. La source de données effectue une validation supplémentaire et établit une connexion.

## <a name="connection-string-syntax"></a>Syntaxe de la chaîne de connexion

Une chaîne de connexion est une liste délimitée par des points-virgules de paires de paramètres clé/valeur :

```csharp
keyword1=value; keyword2=value;
```

Les mots clés ne respectent pas la casse. Toutefois, les valeurs peuvent respecter la casse, en fonction de la source de données. Les mots clés et les valeurs peuvent contenir des [caractères d’espaces blancs](https://en.wikipedia.org/wiki/Whitespace_character#Unicode). Les espaces blancs de début et de fin sont ignorés dans les mots clés et les valeurs sans guillemets.

Si une valeur contient le point-virgule, [les caractères de contrôle Unicode](https://en.wikipedia.org/wiki/Unicode_control_characters) ou les espaces blancs de début ou de fin, elle doit être placée entre guillemets simples ou doubles. Par exemple :

```csharp
Keyword=" whitespace  ";
Keyword='special;character';
```

Le caractère englobant peut ne pas se trouver dans la valeur qu’il englobe. Par conséquent, une valeur contenant des guillemets simples peut être placée entre guillemets doubles et vice versa :

```csharp
Keyword='double"quotation;mark';
Keyword="single'quotation;mark";
```

Vous pouvez également échapper le caractère englobant à l’aide de deux d’entre eux :

```csharp
Keyword="double""quotation";
Keyword='single''quotation';
```

Les guillemets eux-mêmes, ainsi que le signe égal, ne nécessitent pas d’échappement, donc les chaînes de connexion suivantes sont valides :

```csharp
Keyword=no "escaping" 'required';
Keyword=a=b=c
```

Étant donné que chaque valeur est lue jusqu’au point-virgule suivant ou jusqu’à la fin de la chaîne, la valeur dans le dernier exemple est `a=b=c`et le point-virgule final est facultatif.

Toutes les chaînes de connexion partagent la même syntaxe de base décrite ci-dessus. L’ensemble des mots clés reconnus dépend du fournisseur. Le fournisseur de données *Microsoft SqlClient* pour *SQL Server* prend en charge de nombreux mots clés d’anciennes API, mais est généralement plus souple et accepte des synonymes pour un grand nombre des mots clés de chaîne de connexion courants.

Les erreurs de frappe peuvent entraîner des erreurs. Par exemple, `Integrated Security=true` est valide, tandis que `IntegratedSecurity=true` provoque une erreur.

Les chaînes de connexion générées manuellement au moment de l’exécution à partir d’une entrée utilisateur non validée peuvent entraîner des attaques par injection de chaîne, ce qui compromet la sécurité au niveau de la source de données. Pour résoudre ces problèmes, la classe <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> a été créée. Cette classe de [générateur de chaînes de connexion](connection-string-builders.md) expose les paramètres en tant que propriétés fortement typées et permet de valider la chaîne de connexion avant qu’elle ne soit envoyée à la source de données.

## <a name="in-this-section"></a>Contenu de cette section

[Générateur de chaînes de connexion](connection-string-builders.md)\
Montre comment utiliser la classe `ConnectionStringBuilder` pour construire des chaînes de connexion valides au moment de l’exécution.

[Chaînes de connexion et fichiers config](connection-strings-and-configuration-files.md)\
Montre comment stocker et extraire des chaînes de connexion dans des fichiers de configuration.

[Syntaxe de la chaîne de connexion](connection-string-syntax.md)\
Décrit comment configurer des chaînes de connexion spécifiques au fournisseur pour `SqlClient`.

[Protection des informations de connexion](protecting-connection-information.md)\
Montre des techniques pour la protection des informations utilisées pour la connexion à une source de données.

## <a name="see-also"></a>Voir aussi

- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
