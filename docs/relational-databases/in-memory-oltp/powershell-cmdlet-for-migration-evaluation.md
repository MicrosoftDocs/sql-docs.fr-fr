---
title: Applet de commande PowerShell pour l’évaluation de la migration | Microsoft Docs
description: Découvrez Save-SqlMigrationReport, un outil qui évalue l’aptitude à la migration de plusieurs objets dans une base de données SQL Server pour l’OLTP en mémoire.
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 117250d3-9982-47fe-94fd-6f29f6159940
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 78b4d84a98d7ea2c753a673a222a1a19e0b7e5e2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641271"
---
# <a name="powershell-cmdlet-for-migration-evaluation"></a>Applet de commande PowerShell pour l’évaluation de la migration

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

L’applet de commande `Save-SqlMigrationReport` est un outil qui évalue l’aptitude à la migration de plusieurs objets dans une base de données SQL Server.

Actuellement, cette applet de commande se limite à l’évaluation de cette aptitude pour l’OLTP en mémoire. Cette applet peut être exécutée dans des environnements Windows PowerShell avec élévation des privilèges et sqlps.

Au lieu d’exécuter directement cette applet de commande PowerShell, vous pouvez l’exécuter de manière implicite à l’aide de SSMS (SQL Server Management Studio). Dans l’**Explorateur d’objets** de SSMS, vous pouvez cliquer avec le bouton droit sur une table, puis cliquer sur **Conseiller d’optimisation de la mémoire**.

## <a name="syntax"></a>Syntaxe

```
Save-SqlMigrationReport
    -FolderPath <output_path>
    [ -MigrationType <migration_scenario_type> ]
    [
        [ -Server <server_name> -Database <database_name>
            [ -Schema <schema_name> ] [ -Object <object_name> ]
        ]
       |
        [ -InputObject <smo_object> ]
    ]
;
```

## <a name="parameters"></a>Paramètres

Le tableau suivant décrit les paramètres.

Certains aspects de la syntaxe doivent être soulignés. Si vous spécifiez le paramètre `-InputObject`, vous ne pouvez pas spécifier l’un des paramètres suivants :

- `-Server`
- `-Database`
- `-Schema`
- `-Object`

À l’inverse, si vous _ne spécifiez pas_`-InputObject`, vous devez spécifier `-Server` et `-Database`. Si vous spécifiez `-Server`, vous pouvez limiter l’étendue en spécifiant `-Schema` ou `-Object` ou les deux.

| Nom du paramètre | Description |
| :------------- | :---------- |
| Base de données | Nom de la base de données SQL Server cible. Obligatoire quand `-Server` est obligatoire.<br/><br/> Facultatif dans SQLPS. |
| FolderPath | Dossier sous lequel l’applet de commande doit stocker les rapports générés.<br/><br/> Obligatoire. |
| InputObject | L’objet SMO que l’applet de commande doit cibler.<br/><br/> Obligatoire dans l’environnement Windows Powershell si `-Server` n’est pas fourni.<br/><br/> Facultatif dans SQLPS. |
| MigrationType | Type de scénario de migration ciblé par l’applet de commande. Actuellement, la seule valeur correspond au système **'OLTP'** par défaut.<br/><br/> facultatif. |
| Object | Nom de l’objet à propos duquel créer un rapport. Il peut s’agir d’une table ou d’une procédure stockée. |
| Mot de passe | Obligatoire quand `-Username` est requis. |
| schéma | Nom du schéma qui possède l’objet à propos duquel créer un rapport.<br/><br/> facultatif. |
| Serveur | Nom de l’instance SQL Server cible. Obligatoire dans l’environnement Windows Powershell si `-InputObject` n’est pas fourni.<br/><br/> Facultatif dans SQLPS. |
| Nom d’utilisateur | Obligatoire lors de la connexion par le biais de l’authentification SQL Server (et non l’authentification Windows). Sinon, omettez cette valeur. |
| &nbsp; | &nbsp; |

## <a name="prerequisites"></a>Prérequis

Avant de pouvoir exécuter cette applet de commande, vous devez d’abord installer le module nommé **SqlServer** :

- `Install-Module -Name SqlServer`

> [!NOTE]
> L’ancien module `SQLPS` n’est plus tenu à jour. Utilisez le module `SqlServer` plus récent.

Pour plus d’informations, consultez [Installer le module SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md).

## <a name="example-cmdlet-line"></a>Exemple de ligne d’applet de commande

Voici la ligne d’applet de commande qui a été exécutée pour générer le rapport affiché plus loin dans cet article.

```powershell
Save-SqlMigrationReport `
  -FolderPath 'C:\Test\PowerShell-ps1\Save-SqlMigrationReport\' `
  -Server 'MyUserName123456.database.windows.net' `
  -Database 'MyDatabaseName_31' `
  -Schema 'dbo' `
  -Object 'Table2' `
  -Username 'MyUserName' `
  -Password 'MyPassword' `
  -MigrationType 'OLTP' `
;
```

## <a name="example-output-report"></a>Exemple de rapport de sortie

Dans le dossier spécifié pour le paramètre `-FolderPath`, les deux chemins de dossier suivants sont créés en exécutant cette applet de commande. Les deux chemins commencent par la valeur _nom\_serveur_ :

- MyDatabaseName_31\Tables\
- MyDatabaseName_31\Stored Procedures\

Chaque fichier de rapport objet est stocké dans le dossier approprié.

Les noms des fichiers de rapport portent l’extension **.html**. Par exemple, un nom de fichier généré est **MigrationAdvisorChecklistReport_Table2_20190728.html**.

Le code HTML est principalement une table à deux colonnes avec les en-têtes suivants :

- Description
- Résultat de la validation

Voici un exemple réel du rapport HTML pour une table.

```html
<?xml version="1.0" encoding="utf-8"?>
<html>
  <head>
    <title>Memory optimization checklist for [MyDatabaseName_31].[Table2]</title>
  </head>
  <body>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 14pt;">
      <b>Memory optimization checklist for [MyDatabaseName_31].[Table2]</b>
    </p>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 10pt;">
      <b>Report Date/Time:</b>7/28/2019 2:25 PM<br /></p>
    <table border="1" cellpadding="5" cellspacing="0" STYLE="font-family: Verdana, Arial, sans-serif; font-size: 9pt;">
      <tr style="background-color:Silver">
        <th colspan="2" align="center">Description</th>
        <th align="center">Validation Result</th>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported data types are defined on this table. </td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No sparse columns are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No identity columns with unsupported seed and increment are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No foreign key relationships are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported constraints are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No unsupported indexes are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported triggers are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">Post migration row size does not exceed the row size limit of memory-optimized tables.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">Table is not partitioned or replicated.</td>
        <td>Succeeded</td>
      </tr>
    </table>
  </body>
</html>
```

Et voici une approximation de ce à quoi ressemble la table.

| Description | Résultat de la validation |
| :---------- | :---------------- |
| Aucun type de données non pris en charge n’est défini sur cette table. | Opération réussie |
| Aucune colonne éparse n’est définie pour cette table. | Opération réussie |
| Aucune colonne d’identité avec des seeds et des incréments non pris en charge n’est définie pour cette table. | Opération réussie |
| Aucune relation de clé étrangère n’est définie sur cette table. | Opération réussie |
| Aucune contrainte non prise en charge n’est définie sur cette table. | Opération réussie |
| Aucun index non pris en charge n’est défini sur cette table. | Opération réussie |
| Aucun déclencheur non pris en charge n’est défini sur cette table. | Opération réussie |
| La taille de ligne post-migration n’est pas supérieure à la taille limite de ligne des tables à mémoire optimisée. | Opération réussie |
| La table n’est pas partitionnée ou répliquée. | Opération réussie |
| &nbsp; | &nbsp; |

## <a name="related-links"></a>Liens connexes

- Documentation de référence : [Save-SqlMigrationReport](/powershell/module/sqlserver/save-sqlmigrationreport)
