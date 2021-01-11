---
title: Générateurs de chaînes de connexion
description: En savoir plus sur les classes de générateur de chaînes de connexion utilisées pour différents fournisseurs dans ADO.NET, qui héritent toutes de DbConnectionStringBuilder.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 38c1ad34fb4819460c08f00da4c1dbd31749b7cb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771431"
---
# <a name="connection-string-builders"></a>Générateurs de chaînes de connexion

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Les versions précédentes d’ADO.NET ne prenaient pas en charge la vérification au moment de la compilation des chaînes de connexion avec des valeurs de chaînes concaténées, ainsi, pendant le temps d'exécution, un mot clé non correct générait un <xref:System.ArgumentException>. Le Fournisseur de données Microsoft SqlClient pour SQL Server comprend la classe du générateur de chaînes de connexion fortement typé <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType> qui hérite de <xref:System.Data.Common.DbConnectionStringBuilder>.

## <a name="connection-string-injection-attacks"></a>Attaques par injection de chaîne de connexion

Une attaque par injection de chaîne de connexion peut se produire lorsqu'une concaténation de chaîne dynamique est utilisée pour générer des chaînes de connexion se basant sur l'entrée d'utilisateur. Si la chaîne n'est pas validée et que du texte ou des caractères malveillants ne font pas l'objet d'un échappement, un attaquant peut éventuellement accéder à des données sensibles ou à d'autres ressources sur le serveur. Par exemple, un attaquant peut organiser une attaque en fournissant un point-virgule et en ajoutant une valeur supplémentaire. La chaîne de connexion est analysée à l’aide de l’algorithme « **dernier gagnant** » et l’entrée hostile est remplacée par une valeur légitime.

Les classes de générateur de chaînes de connexion sont conçues pour éliminer le travail de recherche et pour se protéger contre les erreurs de syntaxe et les failles en matière de sécurité. Elles fournissent des méthodes et des propriétés qui correspondent aux paires clé/valeur connues autorisées par le fournisseur de données. La classe maintient une collection fixe de synonymes et peut traduire un synonyme vers le nom de la clé connue correspondante. Des vérifications sont effectuées pour contrôler la validité des paires clé/valeur et une paire non valide lève une exception. En outre, les valeurs injectées sont gérées de manière sécurisée.

L'exemple suivant montre comment <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> gère une valeur supplémentaire insérée pour le paramètre `Initial Catalog`.

[!code-csharp[SqlConnectionStringBuilder_InjectionAttack#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_InjectionAttack.cs#1)]

La sortie indique que <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> l’a traitée correctement en plaçant la valeur supplémentaire entre des guillemets doubles au lieu de l’ajouter à la chaîne de connexion en tant que nouvelle paire clé/valeur.

```output
data source=(local);Integrated Security=True;
initial catalog="AdventureWorks;NewValue=Bad"
```

## <a name="build-connection-strings-from-configuration-files"></a>Créer des chaînes de connexion à partir de fichiers de configuration

Si certains éléments d'une chaîne de connexion sont connus à l'avance, ils peuvent être stockés dans un fichier de configuration et récupérés au moment de l'exécution pour générer une chaîne de connexion complète. Par exemple, le nom de la base de données peut être connue à l'avance, mais pas celui du serveur. Ou bien, vous pouvez souhaiter qu'un utilisateur fournisse un nom et un mot de passe au moment de l'exécution mais sans pouvoir injecter d'autres valeurs dans la chaîne de connexion.

L’un des constructeurs surchargés d’un générateur de chaînes de connexion prend <xref:System.String> en tant qu’argument, ce qui vous permet de fournir une chaîne de connexion partielle qui pourra ensuite être complétée à partir de l’entrée d’utilisateur. La chaîne de connexion partielle peut être stockée dans un fichier de configuration et récupérée au moment de l'exécution.

> [!NOTE]
> L'espace de noms <xref:System.Configuration> autorise l'accès par programme aux fichiers de configuration qui utilisent <xref:System.Web.Configuration.WebConfigurationManager> pour les applications Web et <xref:System.Configuration.ConfigurationManager> pour les applications Windows. Pour plus d'informations sur l'utilisation des chaînes de connexion et des fichiers de configuration, consultez [Chaînes de connexion et fichiers config](connection-strings-and-configuration-files.md).

### <a name="example"></a> Exemple

Cet exemple montre comment récupérer une chaîne de connexion partielle d'un fichier de configuration et comment la compléter en définissant les propriétés <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A>, <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A> et <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> de <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>. Le fichier de configuration est défini comme suit.

```xml
<connectionStrings>
  <clear/>
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"
    providerName="Microsoft.Data.SqlClient" />
</connectionStrings>
```

> [!NOTE]
> Vous devez définir une référence à `System.Configuration.dll` dans votre projet pour le code à exécuter.

[!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_UserNamePwd.cs#1)]
  
## <a name="see-also"></a>Voir aussi

- [Chaînes de connexion](connection-strings.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
