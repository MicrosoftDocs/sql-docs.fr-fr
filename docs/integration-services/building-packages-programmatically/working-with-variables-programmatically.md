---
description: Utilisation de variables par programmation
title: Utilisation de variables par programmation | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- System namespace
- scope [Integration Services]
- variables [Integration Services], programmatically
- configuration files [Integration Services]
- Variables collection
- User namespace
- custom variables [Integration Services]
- variables [Integration Services], customizing
ms.assetid: c4b76a3d-94ca-4a8e-bb45-cb8bd0ea3ec1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8258d74d7f81b17d1e3e7973011a036031bd78da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355008"
---
# <a name="working-with-variables-programmatically"></a>Utilisation de variables par programmation

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Les variables constituent un moyen de définir des valeurs et de contrôler des processus dans les packages, conteneurs, tâches et gestionnaires d'événements de manière dynamique. Les variables peuvent également être utilisées par les contraintes de précédence pour contrôler la direction du flux de données vers différentes tâches. Les variables ont diverses utilisations :  
  
-   Mise à jour des propriétés d'un package au moment de l'exécution.  
  
-   Remplissage de valeurs de paramètres pour des instructions Transact-SQL au moment de l’exécution.  
  
-   Contrôle du flux d'une boucle Foreach. Pour plus d’informations, consultez [Ajouter une énumération à un flux de contrôle](../control-flow/foreach-loop-container.md).  
  
-   Contrôle d'une contrainte de précédence par son utilisation dans une expression. Une contrainte de précédence peut inclure des variables dans la définition de contrainte. Pour plus d’informations, consultez [Ajouter des expressions aux contraintes de précédence](../control-flow/precedence-constraints.md).  
  
-   Contrôle de la répétition conditionnelle d'un conteneur de boucles For. Pour plus d’informations, consultez [Ajouter une itération à un flux de contrôle](../control-flow/for-loop-container.md).  
  
-   Création d’expressions qui incluent des valeurs de variables.  
  
-   Vous pouvez créer des variables personnalisées pour tous les types de conteneurs : packages, conteneurs de **boucles Foreach**, conteneurs de **boucles For**, conteneurs de **séquences**, TaskHosts et gestionnaires d’événements. Pour plus d’informations, consultez [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) et [Utiliser des variables dans des packages](../integration-services-ssis-variables.md).  
  
## <a name="scope"></a>Étendue  
 Chaque conteneur possède sa propre collection <xref:Microsoft.SqlServer.Dts.Runtime.Variables>. Lorsqu'une nouvelle variable est créée, elle se trouve dans la portée de son conteneur parent. Le conteneur de packages se trouvant au sommet de la hiérarchie de conteneurs, les variables avec une portée de package fonctionnent comme les variables globales et sont visibles pour tous les conteneurs contenus dans le package. La collection de variables du conteneur est également accessible par les enfants du conteneur via la collection <xref:Microsoft.SqlServer.Dts.Runtime.Variables>, en utilisant le nom de variable ou l'index de la variable dans la collection.  
  
 Parce que la visibilité d'une variable s'étend du haut vers le bas, les variables déclarées au niveau du package sont visibles pour tous les conteneurs situés dans le package. Par conséquent, la collection <xref:Microsoft.SqlServer.Dts.Runtime.Variables> sur un conteneur inclut toutes les variables qui appartiennent à son parent en plus de ses propres variables.  
  
 Inversement, les variables contenues dans une tâche sont limitées en termes de portée et de visibilité et sont uniquement visibles pour la tâche.  
  
 Si un package exécute d'autres packages, les variables définies dans la portée du package appelant sont disponibles pour le package appelé. La seule exception se produit lorsqu'une variable de même nom existe dans le package appelé. Lorsque cette collision se produit, la valeur de la variable dans le package appelé remplace celle du package appelant. Les variables définies dans la portée du package appelé ne sont jamais de nouveau disponibles pour le package appelant.  
  
 L'exemple de code suivant crée une variable, `myCustomVar`, par programme, à la portée du package, puis parcourt toutes les variables visibles pour le package, en imprimant leur nom, leur type de données et leur valeur.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Application app = new Application();  
      // Load a sample package that contains a variable that sets the file name.  
      Package pkg = app.LoadPackage(  
        @"C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" +  
        @"\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx",  
        null);  
      Variables pkgVars = pkg.Variables;  
      Variable myVar = pkg.Variables.Add("myCustomVar", false, "User", "3");  
      foreach (Variable pkgVar in pkgVars)  
      {  
        Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name,  
          pkgVar.DataType, pkgVar.Value.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim app As Application = New Application()  
    ' Load a sample package that contains a variable that sets the file name.  
    Dim pkg As Package = app.LoadPackage( _  
      "C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services" & _  
      "\Package Samples\CaptureDataLineage Sample\CaptureDataLineage\CaptureDataLineage.dtsx", _  
      Nothing)  
    Dim pkgVars As Variables = pkg.Variables  
    Dim myVar As Variable = pkg.Variables.Add("myCustomVar", False, "User", "3")  
    Dim pkgVar As Variable  
    For Each pkgVar In pkgVars  
      Console.WriteLine("Variable: {0}, {1}, {2}", pkgVar.Name, _  
        pkgVar.DataType, pkgVar.Value.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Exemple de sortie :**  
  
 `Variable: CancelEvent, Int32, 0`  
  
 `Variable: CreationDate, DateTime, 4/18/2003 11:57:00 AM`  
  
 `Variable: CreatorComputerName, String,`  
  
 `Variable: CreatorName, String,`  
  
 `Variable: ExecutionInstanceGUID, String, {237AB5A4-7E59-4FC9-8D61-E8F20363DF25}`  
  
 `Variable: FileName, String, Junk`  
  
 `Variable: InteractiveMode, Boolean, False`  
  
 `Variable: LocaleID, Int32, 1033`  
  
 `Variable: MachineName, String, MYCOMPUTERNAME`  
  
 `Variable: myCustomVar, String, 3`  
  
 `Variable: OfflineMode, Boolean, False`  
  
 `Variable: PackageID, String, {F0D2E396-A6A5-42AE-9467-04CE946A810C}`  
  
 `Variable: PackageName, String, DTSPackage1`  
  
 `Variable: StartTime, DateTime, 1/28/2005 7:55:39 AM`  
  
 `Variable: UserName, String, <domain>\<userid>`  
  
 `Variable: VersionBuild, Int32, 198`  
  
 `Variable: VersionComments, String,`  
  
 `Variable: VersionGUID, String, {90E105B4-B4AF-4263-9CBD-C2050C2D6148}`  
  
 `Variable: VersionMajor, Int32, 1`  
  
 `Variable: VersionMinor, Int32, 0`  
  
 Notez que toutes les variables délimitées à l’espace de noms **System** sont disponibles dans le package. Pour plus d’informations, consultez [Variables système](../../integration-services/system-variables.md).  
  
## <a name="namespaces"></a>Espaces de noms  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ( [!INCLUDE[ssIS](../../includes/ssis-md.md)]) fournit deux espaces de noms par défaut dans lesquels résident les variables : **User** et **System**. Par défaut, toute variable personnalisée créée par le développeur est ajoutée à l’espace de noms **User**. Les variables système résident dans l’espace de noms **System**. Vous pouvez créer d’autres espaces de noms dans lesquels placer des variables personnalisées en plus de l’espace de noms **User** et modifier le nom de l’espace de noms **User**. En revanche, vous ne pouvez pas ajouter ou modifier des variables dans l’espace de noms **System**, ni assigner des variables système à un autre espace de noms.  
  
 Les variables système qui sont disponibles diffèrent selon le type de conteneur. Pour obtenir la liste des variables système disponibles pour les packages, conteneurs, tâches et gestionnaires d’événements, consultez [Variables système](../../integration-services/system-variables.md).  
  
## <a name="value"></a>Valeur  
 La valeur d'une variable personnalisée peut être un littéral ou une expression :  
  
-   Si vous souhaitez que la variable contienne une valeur littérale, définissez la valeur de sa propriété <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Value%2A>.  
  
-   Si vous souhaitez que la variable contienne une expression, afin de pouvoir utiliser les résultats de l’expression comme valeur, définissez la propriété <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A> de la variable sur **true** et fournissez une expression dans la propriété <xref:Microsoft.SqlServer.Dts.Runtime.Variable.Expression%2A>. Au moment de l'exécution, l'expression est évaluée et son résultat est utilisé en tant que valeur de la variable. Par exemple, si la propriété de l'expression d'une variable est `"100 * 2""100 * 2"` , la variable est évaluée à une valeur de 200.  
  
 Pour une variable, vous ne pouvez pas définir explicitement la valeur de sa propriété <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A>. La valeur <xref:Microsoft.SqlServer.Dts.Runtime.Variable.DataType%2A> est déduite de la valeur initiale assignée à la variable et ne peut pas être changée par la suite. Pour plus d’informations sur les types de données de variable, consultez [Types de données Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 L’exemple de code suivant crée une variable, définit la propriété <xref:Microsoft.SqlServer.Dts.Runtime.Variable.EvaluateAsExpression%2A> sur **true**, assigne l’expression `"100 * 2"` à la propriété de l’expression de la variable, puis calcule la valeur de la variable.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package pkg = new Package();  
      Variable v100 = pkg.Variables.Add("myVar", false, "", 1);  
      v100.EvaluateAsExpression = true;  
      v100.Expression = "100 * 2";  
      Console.WriteLine("Expression for myVar: {0}",   
        v100.Properties["Expression"].GetValue(v100));  
      Console.WriteLine("Value of myVar: {0}", v100.Value.ToString());  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim pkg As Package = New Package  
    Dim v100 As Variable = pkg.Variables.Add("myVar", False, "", 1)  
    v100.EvaluateAsExpression = True  
    v100.Expression = "100 * 2"  
    Console.WriteLine("Expression for myVar: {0}", _  
      v100.Properties("Expression").GetValue(v100))  
    Console.WriteLine("Value of myVar: {0}", v100.Value.ToString)  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Exemple de sortie :**  
  
 `Expression for myVar: 100 * 2`  
  
 `Value of myVar: 200`  
  
 L'expression doit être une expression valide qui utilise la syntaxe d'expression [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Les littéraux sont autorisés dans les expressions variables, en plus des opérateurs et fonctions que la syntaxe d'expression fournit, mais les expressions ne peuvent pas référencer d'autres variables ou colonnes. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
## <a name="configuration-files"></a>Fichiers de configuration  
 Si un fichier de configuration inclut une variable personnalisée, la variable peut être mise à jour au moment de l'exécution. Cela signifie que lorsque le package s'exécute, la valeur de la variable qui se trouvait à l'origine dans le package est remplacée par une nouvelle valeur provenant du fichier de configuration. Cette technique de remplacement s'avère utile lorsqu'un package est déployé sur plusieurs serveurs qui requièrent des valeurs de variables différentes. Par exemple, une variable peut spécifier combien de fois un conteneur de **boucles Foreach** répète son flux de travail, répertorier les destinataires auxquels un gestionnaire d’événements envoie des e-mails lorsqu’une erreur est déclenchée ou changer le nombre d’erreurs pouvant se produire avant que le package n’échoue. Ces variables sont fournies de manière dynamique dans des fichiers de configuration pour chaque environnement. Par conséquent, seules les variables accessibles en lecture/écriture sont autorisées dans les fichiers de configuration. Pour plus d’informations, consultez [Créer des configurations de package](../packages/legacy-package-deployment-ssis.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md)   
 [Utiliser des variables dans des packages](../integration-services-ssis-variables.md)  
  
