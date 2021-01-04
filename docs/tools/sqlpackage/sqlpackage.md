---
title: SqlPackage.exe
description: Découvrez comment automatiser des tâches de développement de bases de données avec SqlPackage.exe. Affichez des exemples et des paramètres, des propriétés et des variables SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 16c4200b70647c08ddee6b531acc3227d2942ad2
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577834"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** est un utilitaire en ligne de commande qui automatise les tâches de développement de base de données suivantes :  
  
- [Version](#version) : retourne le numéro de build de l’application SqlPackage.  Ajouté dans la version 18.6.

- [Extraire](sqlpackage-extract.md) : crée un fichier de capture instantanée de base de données (.dacpac) à partir d’une base de données SQL Server ou Azure SQL Database active.  
  
- [Publier](sqlpackage-publish.md) : met à jour de manière incrémentielle un schéma de base de données pour qu’il corresponde au schéma d’un fichier .dacpac source. Si la base de données n'existe pas sur le serveur, elle est créée par l'opération de publication. Dans le cas contraire, une base de données existante est mise à jour.  
  
- [Exporter](sqlpackage-export.md) : exporte une base de données active, y compris son schéma et les données utilisateur, dans un package BACPAC (fichier .bacpac) à partir de SQL Server ou d’Azure SQL Database.  
  
- [Importer](sqlpackage-import.md) : importe le schéma et les données de table à partir d'un package BACPAC dans une nouvelle base de données utilisateur dans une instance de SQL Server ou d’Azure SQL Database.  
  
- [DeployReport](sqlpackage-deploy-drift-report.md) (déployer un rapport) : crée un rapport XML sur les modifications devant être apportées par une action de publication.  
  
- [DriftReport](sqlpackage-deploy-drift-report.md) (dériver un rapport) : crée un rapport XML sur les modifications apportées à une base de données inscrite depuis sa dernière inscription.  
  
- [Script](sqlpackage-script.md) : crée un script de mise à jour incrémentielle Transact-SQL qui met à jour le schéma d'une cible afin qu'il corresponde au schéma d'une source.  
  
La ligne de commande de **SqlPackage.exe** vous permet de spécifier ces actions à l’aide de paramètres et de propriétés spécifiques aux actions.  

**[Téléchargez la version la plus récente](sqlpackage-download.md)** . Pour plus d’informations sur la dernière version, consultez les [notes de publication](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Syntaxe de ligne de commande

**SqlPackage.exe** initie les actions spécifiées à l'aide des paramètres, des propriétés et des variables SQLCMD indiqués sur la ligne de commande.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Exemples d'utilisation

**Générer une comparaison de bases de données à l’aide de fichiers .dacpac avec une sortie de script SQL**

Commencez par créer un fichier .dacpac de vos dernières modifications de base de données :

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Créez un fichier .dacpac de votre cible de base de données (sans modification) :

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Créez un script SQL qui génère les différences entre les deux fichiers .dacpac :

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>Version

Affiche la version sqlpackage comme numéro de build.  Peut être utilisé dans les invites interactives et dans les [pipelines automatisés](sqlpackage-pipelines.md).

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Codes de sortie

Commandes qui retournent les codes de sortie suivants :

- 0 = réussite
- non-zéro = échec


## <a name="next-steps"></a>Étapes suivantes

- Découvrir plus en détail l’action [Extract de SqlPackage](sqlpackage-extract.md)
- Découvrir plus en détail l’action [Publish de SqlPackage](sqlpackage-publish.md)
- Découvrir plus en détail l’action [Export de SqlPackage](sqlpackage-export.md)
- Découvrir plus en détail l’action [Import de SqlPackage](sqlpackage-import.md)
