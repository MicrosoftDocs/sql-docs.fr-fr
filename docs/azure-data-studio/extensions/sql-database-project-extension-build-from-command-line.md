---
title: Créer un projet depuis la ligne de commande
description: Générer des projets de base de données SQL Server à partir de la ligne de commande
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan, sstein
ms.custom: ''
ms.date: 08/07/2020
ms.openlocfilehash: 1233b8319c4fbd893159ba88fbcd1c3401f05ce0
ms.sourcegitcommit: fa63019cbde76dd981b0c5a97c8e4d57e8d5ca4e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99495717"
---
# <a name="build-a-database-project-from-command-line"></a>Générer un projet de base de données à partir de la ligne de commande

Même si l’extension SQL Database Project (préversion) pour Azure Data Studio fournit une interface utilisateur graphique pour [générer un projet de base de données](sql-database-project-extension-build.md), une expérience de génération via la ligne de commande est également disponible pour les environnements Windows, macOS et Linux. Cet article détaille les conditions préalables et la syntaxe nécessaires à la création d’un projet SQL dans dacpac à partir de la ligne de commande.

## <a name="prerequisites"></a>Prérequis

1. Installez et configurez l’[extension SQL Database Projects pour Azure Data Studio](sql-database-project-extension.md).

2. Les dll .NET Core suivantes et le fichier `Microsoft.Data.Tools.Schema.SqlTasks.targets` cible sont nécessaires pour générer un projet de base de données SQL à partir de la ligne de commande dans toutes les plateformes prises en charge par l’extension Azure Data Studio pour les projets de base de données SQL. Ces fichiers sont créés par l’extension lors de la première génération effectuée dans l’interface Azure Data Studio, puis placés dans le dossier de l’extension sous `BuildDirectory`.  Par exemple, sur Linux, ces fichiers sont placés dans `~\.azuredatastudio\extensions\microsoft.sql-database-projects-x.x.x\BuildDirectory\`.  Copiez ces 10 fichiers dans un nouveau dossier accessible, ou notez leur emplacement.  Cet emplacement est appelé `DotNet Core build folder` dans ce document.

    - Microsoft.Data.Tools.Schema.Sql.dll
    - Microsoft.Data.Tools.Schema.Tasks.Sql.dll
    - Microsoft.Data.Tools.Utilities.dll
    - System.Io.Packaging.dll
    - Microsoft.SqlServer.Dac.dll
    - Microsoft.SqlServer.Dac.Extensions.dll
    - Microsoft.SqlServer.TransactSql.ScriptDom.dll
    - Microsoft.SqlServer.Types.dll
    - Microsoft.Data.Tools.Schema.SqlTasks.targets
    - System.ComponentModel.Composition.dll
    - Microsoft.Data.SqlClient.dll

3. Si le projet a été créé dans Azure Data Studio, passez directement à [Générer le projet à partir de la ligne de commande](#build-the-project-from-the-command-line). Si le projet a été créé dans SQL Server Data Tools (SSDT), ouvrez le projet dans l’extension de projet Azure Data Studio SQL Database.  L’ouverture du projet dans Azure Data Studio met automatiquement à jour le fichier `sqlproj` avec trois modifications, indiquées ci-dessous à titre d’information :

    1. Conditions d’importation

    ```console
    <Import Condition="'$(NetCoreBuild)' == 'true'" Project="$(NETCoreTargetsPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/> 
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' != ''" Project="$(SQLDBExtensionsRefPath)\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    <Import Condition="'$(NetCoreBuild)' != 'true' AND '$(SQLDBExtensionsRefPath)' == ''" Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets"/>
    ```

    2. Informations de référence sur les packages

    ```console
    <ItemGroup>
        <PackageReference Condition="'$(NetCoreBuild)' == 'true'" Include="Microsoft.NETFramework.ReferenceAssemblies" Version="1.0.0" PrivateAssets="All"/>
    </ItemGroup>
    ```

    3. Cible propre, nécessaire pour la prise en charge de la double modification dans SQL Server Data Tools (SSDT) et Azure Data Studio

        ```console
        <Target Name="AfterClean">
            <Delete Files="$(BaseIntermediateOutputPath)\project.assets.json"/>
        </Target>
        ```

## <a name="build-the-project-from-the-command-line"></a>Générer le projet à partir de la ligne de commande

À partir du dossier .NET complet, utilisez la commande suivante :

```console
dotnet build "<sqlproj file path>" /p:NetCoreBuild=true /p:NETCoreTargetsPath="<DotNet Core build folder>"
```

Par exemple, à partir de `/usr/share/dotnet` sur Linux :

```console
dotnet build "/home/myuser/Documents/DatabaseProject1/DatabaseProject1.sqlproj" /p:NetCoreBuild=true /p:NETCoreTargetsPath="/home/myuser/.azuredatastudio-insiders/extensions/microsoft.sql-database-projects-0.1.2/BuildDirectory"  
```

## <a name="next-steps"></a>Étapes suivantes

- [Extension SQL Database Projects pour Azure Data Studio](sql-database-project-extension.md)
- [Publier des projets de base de données SQL](sql-database-project-extension-build.md#publish-a-database-project)
