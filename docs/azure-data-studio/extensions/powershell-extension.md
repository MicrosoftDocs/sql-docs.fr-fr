---
title: Extension PowerShell
description: Découvrez comment installer et utiliser l’extension Azure Data Studio PowerShell, qui offre une prise en charge enrichie de l’éditeur PowerShell pour l’écriture et le débogage de scripts.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: SQLvariant
ms.author: aanelson
ms.reviewer: alayu, maghan, sstein
ms.custom: ''
ms.date: 04/19/2019
ms.openlocfilehash: 4de6c653503458ba839147b9a68b457330345755
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048499"
---
# <a name="powershell-editor-support-for-azure-data-studio"></a>Prise en charge de l'éditeur PowerShell pour Azure Data Studio

Cette extension fournit une prise en charge complète de l'éditeur PowerShell dans [Azure Data Studio](https://github.com/Microsoft/azuredatastudio).
Vous pouvez maintenant écrire et déboguer des scripts PowerShell en utilisant l'excellente interface de type IDE fournie par Azure Data Studio.

![Extension PowerShell](media/powershell-extension/powershell-extension.png)

## <a name="features"></a>Fonctionnalités

- Mise en surbrillance de la syntaxe
- Extraits de code
- IntelliSense pour cmdlets et plus encore
- Analyse basée sur des règles fournie par [PowerShell Script Analyzer](http://github.com/PowerShell/PSScriptAnalyzer)
- Accéder aux définitions des cmdlets et des variables
- Trouver des références de cmdlets et de variables
- Découverte des documents et des symboles de l'espace de travail
- Exécuter la sélection du code PowerShell en utilisant <kbd>F8</kbd>
- Lancer l'aide en ligne pour le symbole sous le curseur à l’aide de <kbd>Ctrl</kbd>+<kbd>F1</kbd>
- Prise en charge de base de la console interactive !

## <a name="installing-the-extension"></a>Installation de l’extension

Vous pouvez installer la version officielle de l'extension PowerShell en suivant les étapes de la documentation [Azure Data Studio](./add-extensions.md).
Dans le volet Extensions, recherchez l'extension « PowerShell » et installez-la.  Vous recevrez automatiquement une notification à chaque mise à jour de l'extension !

Vous pouvez également installer un package VSIX depuis notre [page des versions](https://github.com/PowerShell/vscode-powershell/releases) et l'installer via la ligne de commande :

```powershell
azuredatastudio --install-extension PowerShell-<version>.vsix
```

## <a name="platform-support"></a>Plateforme prise en charge

- **Windows 7 à 10** avec Windows PowerShell 3 et versions ultérieures, et PowerShell Core
- **Linux** avec PowerShell Core (toutes les distributions PowerShell prises en charge)
- **macOS** avec PowerShell Core

Consultez le [Forum Aux Questions](https://github.com/PowerShell/vscode-powershell/wiki/FAQ) pour lire les réponses aux questions courantes.

## <a name="installing-powershell-core"></a>Installation de PowerShell Core

Si vous utilisez Azure Data Studio sur macOS ou Linux, vous devrez peut-être aussi installer PowerShell Core.

PowerShell Core est un projet Open Source sur [GitHub](https://github.com/powershell/powershell).
Pour plus d'informations sur l'installation de PowerShell Core sur les plateformes macOS ou Linux, consultez les articles suivants :

- [Installation de PowerShell Core sur Linux](/powershell/scripting/install/installing-powershell-core-on-linux)
- [Installation de PowerShell Core sur macOS](/powershell/scripting/install/installing-powershell-core-on-macos)

## <a name="example-scripts"></a>Exemples de scripts

Le dossier `examples` de l'extension contient quelques exemples de scripts que vous pouvez utiliser pour découvrir les fonctionnalités d'édition et de débogage PowerShell.  Consultez le fichier [README.md](https://github.com/PowerShell/vscode-powershell/blob/master/examples/README.md) inclus pour en savoir plus sur leur utilisation.

Ce dossier se trouve à l’emplacement suivant :

```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.PowerShell-<version>/examples
```

ou si vous utilisez la préversion de l'extension

 ```powershell
$HOME/.azuredatastudio/extensions/ms-vscode.powershell-preview-<version>/examples
```

Pour ouvrir/afficher les exemples de l'extension dans Azure Data Studio, exécutez le code suivant depuis votre invite de commande PowerShell :

```powershell
azuredatastudio (Get-ChildItem $Home\.azuredatastudio\extensions\ms-vscode.PowerShell-*\examples)[-1]
```

### <a name="creating-and-opening-files"></a>Création et ouverture de fichiers

Pour créer et ouvrir un nouveau fichier dans l'éditeur, utilisez la commande New-EditorFile depuis le terminal intégré PowerShell.

```powershell
PS C:\temp> New-EditorFile ExportData.ps1
```

Cette commande fonctionne pour n'importe quel type de fichier, pas seulement pour les fichiers PowerShell.

```powershell
PS C:\temp> New-EditorFile ImportData.py
```

Pour ouvrir un ou plusieurs fichiers dans Azure Data Studio, utilisez la commande `Open-EditorFile`.

```powershell
Open-EditorFile ExportData.ps1, ImportData.py
```

### <a name="no-focus-on-console-when-executing"></a>Aucun focus sur la console lors de l'exécution

Les utilisateurs habitués à travailler avec SSMS savent qu’ils peuvent exécuter une requête, puis la réexécuter sans avoir à revenir dans le volet de la requête.  Dans ce cas, le comportement par défaut de l'éditeur de code peut vous sembler étrange.  Pour garder le focus dans l'éditeur lorsque vous exécutez une commande avec <kbd>F8</kbd>, modifiez le paramètre suivant :

```json
"powershell.integratedConsole.focusConsoleOnExecute": false
```

La valeur par défaut est `true` pour des raisons d'accessibilité.

Sachez que ce paramètre empêchera le focus vers la console, même lorsque vous utilisez une commande qui appelle explicitement l'entrée, par exemple `Get-Credential`.

## <a name="sql-powershell-examples"></a>Exemples SQL PowerShell

Pour utiliser ces exemples (ci-dessous), vous devez installer le module SqlServer à partir de la [galerie PowerShell](https://www.powershellgallery.com/packages/SqlServer).

```powershell
Install-Module -Name SqlServer
```

> [!NOTE]
> Avec `21.1.18102` et versions ultérieures, le module `SqlServer` prend en charge [PowerShell Core](https://github.com/PowerShell/PowerShell) 6.2 et versions ultérieures, en plus de Windows PowerShell.

Dans cet exemple, nous utilisons la cmdlet `Get-SqlInstance` pour obtenir les objets SMO du serveur pour ServerA & ServerB.  La sortie par défaut de cette commande inclura le nom de l'instance, la version, le Service Pack et le niveau de mise à jour CU des instances.

```powershell
Get-SqlInstance -ServerInstance ServerA, ServerB
```

Voici un exemple de ce à quoi ressemblera ce résultat :

```console
Instance Name             Version    ProductLevel UpdateLevel  HostPlatform HostDistribution
-------------             -------    ------------ -----------  ------------ ----------------
ServerA                   13.0.5233  SP2          CU4          Windows      Windows Server 2016 Datacenter
ServerB                   14.0.3045  RTM          CU12         Linux        Ubuntu
```

Le module `SqlServer` contient un fournisseur appelé `SQLRegistration` qui vous permet d'accéder par programmation aux types suivants de connexions SQL Server enregistrées :

+ Serveur de moteur de base de données (serveurs enregistrés)
+ Serveur de gestion centralisée (CMS)
+ Analysis Services
+ Integration Services
+ Reporting Services

 Dans l'exemple suivant, nous allons effectuer une opération `dir` (alias pour `Get-ChildItem`) pour obtenir la liste de toutes les instances SQL Server répertoriées dans votre fichier Serveurs enregistrés.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse
```

Voici un exemple de ce à quoi ce résultat pourrait ressembler :

```powershell
Mode Name
---- ----
-    ServerA
-    ServerB
-    localhost\SQL2017
-    localhost\SQL2016Happy
-    localhost\SQL2017
```

Vous pouvez utiliser la cmdlet `Get-SqlDatabase` dans de nombreuses opérations impliquant une base de données ou des objets d’une base de données.  Si vous fournissez des valeurs pour les paramètres `-ServerInstance` et `-Database`, un seul objet de base de données sera récupéré.  Cependant, si vous spécifiez uniquement le paramètre `-ServerInstance`, une liste complète de toutes les bases de données de cette instance sera retournée.

Voici un exemple de ce à quoi ressemblera ce résultat :

```console
Name                 Status           Size     Space  Recovery Compat. Owner
                                            Available  Model     Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

L’exemple suivant utilise la cmdlet `Get-SqlDatabase` pour récupérer une liste de toutes les bases de données sur l'instance ServerB, puis présente une grille/table (à l'aide de la cmdlet `Out-GridView`) pour sélectionner les bases de données à sauvegarder.  Lorsque l'utilisateur clique sur le bouton « OK », seules les bases de données sélectionnées seront sauvegardées.

```powershell
Get-SqlDatabase -ServerInstance ServerB |
Out-GridView -PassThru |
Backup-SqlDatabase -CompressionOption On
```

Cet exemple, encore une fois, obtient la liste de toutes les instances SQL Server répertoriées dans votre fichier Serveurs enregistrés, puis appelle `Get-SqlAgentJobHistory`, qui rapporte chaque tâche SQL Agent qui a échoué depuis minuit, pour chaque instance SQL Server mentionnée.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE {$_.Mode -ne 'd' } |
FOREACH {
    Get-SqlAgentJobHistory -ServerInstance  $_.Name -Since Midnight -OutcomesType Failed
}
```

Dans cet exemple, nous allons effectuer une opération `dir` (alias pour `Get-ChildItem`) pour obtenir la liste de toutes les instances SQL Server répertoriées dans votre fichier Serveurs enregistrés, puis utilisez la cmdlet `Get-SqlDatabase` afin d’obtenir une liste des bases de données pour chacune de ces instances.

```powershell
dir 'SQLSERVER:\SQLRegistration\Database Engine Server Group' -Recurse |
WHERE { $_.Mode -ne 'd' } |
FOREACH {
    Get-SqlDatabase -ServerInstance $_.Name
}
```

Voici un exemple de ce à quoi ressemblera ce résultat :

```console
Name                 Status           Size  Space     Recovery Compat. Owner
                                            Available Model    Level
----                 ------           ---- ---------- -------- ------- -----
AdventureWorks2017   Normal      336.00 MB   57.01 MB Simple       140 sa
master               Normal        6.00 MB  368.00 KB Simple       140 sa
model                Normal       16.00 MB    5.53 MB Full         140 sa
msdb                 Normal       48.44 MB    1.70 MB Simple       140 sa
PBIRS                Normal      144.00 MB   55.95 MB Full         140 sa
PBIRSTempDB          Normal       16.00 MB    4.20 MB Simple       140 sa
SSISDB               Normal      325.06 MB   26.21 MB Full         140 sa
tempdb               Normal       72.00 MB   61.25 MB Simple       140 sa
WideWorldImporters   Normal         3.2 GB     2.6 GB Simple       130 sa
```

## <a name="reporting-problems"></a>Signalement des problèmes

Si vous rencontrez des problèmes avec l'extension PowerShell, consultez [la documentation de dépannage](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md) pour plus d'informations sur le diagnostic et le signalement des problèmes.

### <a name="security-note"></a>Remarque relative à la sécurité

Pour tout problème relatif à la sécurité, cliquez [ici](https://github.com/PowerShell/vscode-powershell/blob/master/docs/troubleshooting.md#note-on-security).

## <a name="contributing-to-the-code"></a>Contribution au code

Consultez la [documentation de développement](https://github.com/PowerShell/vscode-powershell/blob/master/docs/development.md) pour plus de détails sur la façon de contribuer à cette extension.

## <a name="maintainers"></a>Chargés de maintenance

- [Keith Hill](https://github.com/rkeithhill) - [@r_keith_hill](http://twitter.com/r_keith_hill)
- Tyler Leonhardt - [@TylerLeonhardt](http://twitter.com/tylerleonhardt)
- [Rob Holt](https://github.com/rjmholt)

## <a name="license"></a>Licence

Cette extension est [accordée sous licence MIT](https://github.com/PowerShell/vscode-powershell/blob/master/LICENSE.txt). Pour plus de détails sur les binaires tiers que nous incluons avec les versions de ce projet, consultez le fichier [Mentions tierces](https://github.com/PowerShell/vscode-powershell/blob/master/Third%20Party%20Notices.txt).

## <a name="code-of-conduct"></a>Code de conduite

Ce projet a adopté le [Code de conduite Open Source de Microsoft][conduct-code].
Pour plus d'informations, voir la [FAQ sur le Code de conduite ][conduct-FAQ] ou contacter [opencode@microsoft.com][conduct-email] pour toute question ou commentaire supplémentaire.

[conduct-code]: https://opensource.microsoft.com/codeofconduct/
[conduct-FAQ]: https://opensource.microsoft.com/codeofconduct/faq/
[conduct-email]: mailto:opencode@microsoft.com