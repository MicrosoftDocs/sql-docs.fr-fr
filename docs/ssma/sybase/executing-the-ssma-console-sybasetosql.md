---
description: Exécution de la console SSMA (SybaseToSQL)
title: Exécution de la console SSMA (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/27/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Database Connection Commands
- Sybase Console,Manageability Commands
- Sybase Console,Migration Commands
- Sybase Console,Migration Preparation Command
- Sybase Console,Project Commands
- Sybase Console,Report Commands
- Sybase Console,Script File Commands
- Sybase Console,Script Generation Commands
ms.assetid: ea8950b7-fabc-4aa4-89f8-9573a2617d70
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a7eed4ea61ada5979dbecdb106e5954620ae9c81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079370"
---
# <a name="executing-the-ssma-console-sybasetosql"></a>Exécution de la console SSMA (SybaseToSQL)
Microsoft vous fournit un ensemble robuste de commandes de fichier de script pour exécuter et contrôler les activités SSMA. Les sections suivantes détaillent les mêmes sections.  
  
## <a name="script-file-commands"></a>Commandes de fichier de script  
L’application console utilise certaines commandes de fichier de script standard comme énuméré dans cette section.  
  
## <a name="project-commands"></a>Commandes de projet  
Les commandes de projet gèrent la création de projets, l’ouverture, l’enregistrement et la sortie de projets.  
  
### <a name="create-new-project"></a>create-new-project  
Cette commande crée un nouveau projet SSMA.  
  
-   `project-folder` indique le dossier du projet qui est créé.  
  
-   `project-name` indique le nom du projet. {string}  
  
-   `overwrite-if-exists`Attribut facultatif indique si un projet existant doit être remplacé. expression  
  
-   `project-type:`Attribut facultatif. Indique le type de projet, c’est-à-dire « SQL-Server-2005 » ou « projet SQL-Server-2008 » ou « projet SQL-Server-2012 » ou « projet SQL-Server-2014 » ou « SQL-Azure ». La valeur par défaut est « SQL-Server-2008 ».  
  
**Exemple de syntaxe :**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>" (optional)  
  
   project-type="<sql-server-2008/sql-server-2005/sql-server-2012/sql-server-2014/sql-azure>"  
/>  
```  
L’attribut’overwrite-if-exists’a la **valeur false** par défaut.  
  
L’attribut « Project-type » est **SQL-Server-2008** par défaut.  
  
### <a name="open-project"></a>ouvrir un projet  
Cette commande ouvre le projet.

-   `project-folder` indique le dossier du projet qui est créé. La commande échoue si le dossier spécifié n’existe pas.  {string}  
  
-   `project-name` indique le nom du projet. La commande échoue si le projet spécifié n’existe pas.  {string}  
  
**Exemple de syntaxe :**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
> [!NOTE]  
> L’application de console SSMA pour SAP ASE prend en charge la compatibilité descendante. Vous pouvez l’utiliser pour ouvrir des projets créés par une version antérieure de SSMA.  
  
### <a name="save-project"></a>save-project  
Cette commande enregistre le projet de migration.  
  
**Exemple de syntaxe :**  
  
```xml  
<save-project/>  
```  
  
### <a name="close-project"></a>fermer-projet  
Cette commande ferme le projet de migration.  
  
**Exemple de syntaxe :**  
  
```xml  
<close-project   
  if-modified="<save/error/ignore>"   (optional)  
/>  
```  
L’attribut « If-Modified » est facultatif et **ignoré** par défaut.  
  
## <a name="database-connection-commands"></a>Commandes de connexion à la base de données  
Les commandes de connexion à la base de données permettent de se connecter à la base de données.  
  
> [!NOTE]  
> - La fonctionnalité **Parcourir** de l’interface utilisateur n’est pas prise en charge dans la console.  
> - Pour plus d’informations sur la création de fichiers de script, consultez [création de fichiers de script &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-script-files-sybasetosql.md).  
  
### <a name="connect-source-database"></a>Connect-source-base de données  
Cette commande effectue la connexion à la base de données source et charge les métadonnées de haut niveau de la base de données source, mais pas toutes les métadonnées.
  
Si la connexion à la source ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.
  
La définition du serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur du fichier de connexion au serveur ou du fichier de script.  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="force-load-sourcetarget-database"></a>Force-Load-source/cible-base de données  
Cette commande charge les métadonnées sources et est utile pour travailler sur le projet de migration hors connexion.  
  
Si la connexion à la source ou à la cible ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
Cette commande requiert un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
**Exemple de syntaxe :**  
  
```xml  
<force-load metabase="<source/target>" >  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
  
### <a name="reconnect-source-database"></a>reconnecter-Source-base de données  
Cette commande se reconnecte à la base de données source, mais ne charge pas de métadonnées contrairement à la commande Connect-source-Database.  
  
Si la connexion (re) avec la source ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
  
### <a name="connect-target-database"></a>connect-target-Database  
Cette commande se connecte à la base de données SQL Server cible et charge les métadonnées de haut niveau de la base de données cible, mais pas les métadonnées entièrement.  
  
Si la connexion à la cible ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
La définition du serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur du fichier de connexion au serveur ou du fichier de script.  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
  
### <a name="reconnect-target-database"></a>reconnexion-cible-base de données  
  
Cette commande se reconnecte à la base de données cible, mais ne charge pas de métadonnées, contrairement à la commande connect-target-Database.  
  
Si la connexion (re) à la cible ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-commands"></a>Commandes de rapport  
Les commandes de rapport génèrent des rapports sur les performances de diverses activités de la console SSMA.  
  
### <a name="generate-assessment-report"></a>générer un rapport d’évaluation  
  
Cette commande génère des rapports d’évaluation sur la base de données source.  
  
Si la connexion à la base de données source n’est pas exécutée avant l’exécution de cette commande, une erreur est générée et l’application console se ferme.  
  
L’échec de la connexion au serveur de base de données source lors de l’exécution de la commande entraîne l’arrêt de l’application console.  
  
-   `conversion-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
-   `object-name:` Spécifie le ou les objets pris en compte pour la génération de rapports d’évaluation (prend en charge les noms d’objets individuels ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
-   `conversion-report-overwrite:` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
-   `write-summary-report-to:` Spécifie le chemin d’accès auquel le rapport sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **AssessmentReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    -   `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<generate-assessment-report  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  assessment-report-folder="<folder-name>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<generate-assessment-report  
  
  assessment-report-folder="<folder-name>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
<metabase-object object-name="<object-name>"  
  
        object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-commands"></a>Commandes de migration  
Les commandes de migration convertissent le schéma de base de données cible en schéma source et migrent les données vers le serveur cible.  
  
### <a name="convert-schema"></a>convertir-schéma  
Cette commande effectue une conversion de schéma de la source vers le schéma cible.  
  
Si la connexion à la base de données source ou cible n’est pas exécutée avant l’exécution de cette commande ou si la connexion au serveur de base de données source ou cible échoue lors de l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
-   `conversion-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
-   `object-name:` Spécifie le ou les objets sources pris en compte pour la conversion du schéma (prend en charge les noms d’objets individuels ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
-   `conversion-report-overwrite:` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
-   `write-summary-report-to:` Spécifie le chemin d’accès auquel le rapport de synthèse sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **SchemaConversionReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    -   `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<convert-schema  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  write-summary-report-to="<file-name/folder-name>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder-name>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```  
or  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder-name>"         (optional)  
  
  conversion-report-overwrite="<true/false>"> (optional)  
  
  <metabase-object object-name="<object-name>"  
  
    object-type="<object-category>"/>  
  
</convert-schema>  
```  
  
### <a name="migrate-data"></a>migrer-données  
Cette commande migre les données sources vers la cible.  
  
-   `object-name:` Spécifie le ou les objets source pris en compte pour la migration des données (prend en charge les noms d’objets individuels ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut nom-objet (si la catégorie d’objet est spécifiée, le type d’objet est « catégorie »).  
  
-   `write-summary-report-to:` Spécifie le chemin d’accès auquel le rapport sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **DataMigrationReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    -   `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>">  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <metabase-object object-name="<object-name>"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```  
or  
  
```xml  
<migrate-data  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  write-summary-report-to="<file-name/folder-name>"  
  
  report-errors="<true/false>" verbose="<true/false>"/>  
```  
  
## <a name="migration-preparation-command"></a>Commande de préparation de la migration  
La commande de préparation de la migration lance le mappage de schéma entre les bases de données source et cible.  
  
> [!NOTE]  
> Le paramètre de sortie par défaut de la console pour les commandes de migration est un rapport de sortie « complet » sans rapport d’erreurs détaillé : uniquement Résumé au niveau du nœud racine de l’arborescence de l’objet source.  
  
### <a name="map-schema"></a>mappage-schéma  
Cette commande fournit le mappage de schéma de la base de données source au schéma cible.  
  
-   `source-schema` Spécifie le schéma source à migrer.  
  
-   `sql-server-schema` Spécifie le schéma cible vers lequel le schéma source sera migré.  
  
**Exemple de syntaxe :**  
  
```xml  
<map-schema source-schema="<source-schema>"  
  
sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-commands"></a>Commandes de gestion  
Les commandes de gestion aident à synchroniser les objets de base de données cible avec la base de données source.  
  
> [!NOTE]  
> Le paramètre de sortie par défaut de la console pour les commandes de migration est un rapport de sortie « complet » sans rapport d’erreurs détaillé : uniquement Résumé au niveau du nœud racine de l’arborescence de l’objet source.  
  
### <a name="synchronize-target"></a>synchroniser-cible  
Cette commande synchronise les objets cibles avec la base de données cible.  
 
Si cette commande est exécutée sur la base de données source, une erreur est détectée.  
  
Si la connexion à la base de données cible n’est pas exécutée avant l’exécution de cette commande ou si la connexion au serveur de base de données cible échoue au cours de l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
-   `object-name:` Spécifie le ou les objets cibles pris en compte pour la synchronisation avec la base de données cible (prend en charge les noms d’objets individuels ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut nom-objet (si la catégorie d’objet est spécifiée, le type d’objet est « catégorie »).  
  
-   `on-error:` Spécifie si les erreurs de synchronisation doivent être spécifiées en tant qu’avertissements ou erreurs. Options disponibles pour on-Error :  
  
    -   Rapport-total-AVERTISSEMENT  
  
    -   rapport-chaque AVERTISSEMENT  
  
    -   échec du script  
  
-   `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération de synchronisation (attribut facultatif). Si seul le chemin d’accès au dossier est spécifié, le fichier par nom **TargetSynchronizationReport.XML** est créé.  
  
**Exemple de syntaxe :**  
  
```xml  
<synchronize-target  
  
object-name="<object-name>"  
  
on-error="<report-total-as-warning/  
  
report-each-as-warning/  
  
fail-script>" (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
or  
  
```xml  
<synchronize-target  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"/>  
```  
or  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
  <metabase-object object-name="<object-name>"/>  
  
</synchronize-target>  
```  
  
### <a name="refresh-from-database"></a>actualisation à partir de la base de données  
Cette commande actualise les objets sources de la base de données.  
  
Si cette commande est exécutée sur la base de données cible, une erreur est générée.  
  
Cette commande requiert un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
-   `object-name:` Spécifie le ou les objets source pris en compte pour l’actualisation à partir de la base de données source (prend en charge les noms d’objets individuels ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
-   `on-error:` Spécifie s’il faut appeler des erreurs d’actualisation comme des avertissements ou des erreurs. Options disponibles pour on-Error :  
  
    -   Rapport-total-AVERTISSEMENT  
  
    -   rapport-chaque AVERTISSEMENT  
  
    -   échec du script  
  
-   `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération d’actualisation (attribut facultatif). Si seul le chemin d’accès au dossier est spécifié, le fichier par nom **SourceDBRefreshReport.XML** est créé.  
  
**Exemple de syntaxe :**  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  on-error="<report-total-as-warning/  
  
             report-each-as-warning/  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name/folder-name>"        (optional)  
  
/>  
```  
or  
  
```xml  
<refresh-from-database  
  
  object-name="<object-name>"  
  
  object-type="<object-category>" />  
```  
or  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-commands"></a>Commandes de génération de script  
Les commandes de génération de script effectuent deux tâches : elles aident à enregistrer la sortie de la console dans un fichier de script, et elles enregistrent la sortie T-SQL dans la console ou un fichier en fonction du paramètre que vous spécifiez.  
  
### <a name="save-as-script"></a>enregistrer en tant que script  
Cette commande permet d’enregistrer les scripts des objets dans un fichier mentionné quand Metabase = cible. Il s’agit d’une alternative à la commande de synchronisation dans le fait que nous obtenons les scripts et exécutons le même sur la base de données cible.  
  
Cette commande requiert un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
-   `object-name:` Spécifie le ou les objets dont les scripts doivent être enregistrés (prend en charge les noms d’objets individuels ou un nom d’objet de groupe).  
  
-   `object-type:` Spécifie le type de l’objet appelé dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
-   `metabase:` Spécifie s’il s’agit de la métabase source ou cible.  
  
-   `destination:` Spécifie le chemin d’accès ou le dossier dans lequel le script doit être enregistré. Si le nom de fichier n’est pas donné, un nom de fichier au format (object_name valeur d’attribut). out sera fourni.
  
-   `overwrite:` Si la valeur est true, elle remplace le même nom de fichier, le cas échéant. Il peut avoir les valeurs (true/false).  
  
**Exemple de syntaxe :**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="<object-name>"  
  
  object-type="<object-category>"  
  
  destination="<file-name/folder-name>"  
  
  overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file-name/folder-name>"  
  
    <metabase-object object-name="<object-name>"  
  
                     object-type="<object-category>"/>  
  
</save-as-script>  
```  
  
### <a name="convert-sql-statement"></a>Convert-SQL-Statement
Cette commande convertit l’instruction SQL.  
  
-   `context` Spécifie le nom du schéma.  
  
-   `destination` Spécifie si la sortie doit être stockée dans un fichier.  
  
    Si cet attribut n’est pas spécifié, l’instruction T-SQL convertie est affichée sur la console. (attribut facultatif)  
  
-   `conversion-report-folder` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
-   `conversion-report-overwrite` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
-   `write-converted-sql-to` Spécifie le chemin d’accès au dossier (ou) du fichier dans lequel le T-SQL converti doit être stocké. Quand un chemin d’accès au dossier est spécifié avec l' `sql-files` attribut, chaque fichier source a un fichier T-SQL cible correspondant créé dans le dossier spécifié. Quand un chemin d’accès au dossier est spécifié avec l' `sql` attribut, le T-SQL converti est écrit dans un fichier nommé result. out dans le dossier spécifié.  
  
-   `sql` spécifie les instructions SQL Sybase à convertir, une ou plusieurs instructions peuvent être séparées à l’aide d’un « ; »  
  
-   `sql-files` Spécifie le chemin d’accès des fichiers SQL qui doivent être convertis en code T-SQL.  
  
-   `write-summary-report-to` Spécifie le chemin d’accès où le rapport de synthèse sera généré. Si seul le chemin d’accès au dossier est mentionné, le fichier par nom **ConvertSQLReport.XML** est créé. (attribut facultatif)  
  
    La création d’un rapport de synthèse a deux sous-catégories supplémentaires, à savoir :  
  
    -   Report-Errors (= "true/false", avec la valeur par défaut "false" (attributs facultatifs)).  
  
    -   verbose (= "true/false", avec la valeur par défaut "false" (attributs facultatifs)).  
  
Cette commande requiert un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
**Exemple de syntaxe :**  
  
```  
<convert-sql-statement  
  
       context="<database-name>.<schema-name>"  
  
        conversion-report-folder="<folder-name>"  
  
        conversion-report-overwrite="<true/false>"  
  
        write-summary-report-to="<file-name/folder-name>"   (optional)  
  
        verbose="<true/false>"   (optional)  
  
        report-errors="<true/false>"   (optional)  
  
        destination="<stdout/file>"   (optional)  
  
        write-converted-sql-to ="<file-name/folder-name>"  
  
        sql="SELECT 1 FROM DUAL;">  
  
    <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
or  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         write-summary-report-to="<file-name/folder-name>"   (optional)  
  
         verbose="<true/false>"   (optional)  
  
         report-errors="<true/false>"   (optional)  
  
         destination="<stdout/file>"   (optional)  
  
         write-converted-sql-to ="<file-name/folder-name>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
or  
  
```  
<convert-sql-statement  
  
         context="<database-name>.<schema-name>"  
  
         conversion-report-folder="<folder-name>"  
  
         conversion-report-overwrite="<true/false>"  
  
         sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-steps"></a>Étapes suivantes  
Pour plus d’informations sur les options de ligne de commande, consultez [options de ligne de commande dans la console SSMA (AccessToSQL)](../access/command-line-options-in-ssma-console-accesstosql.md).  
  
Pour plus d’informations sur un exemple de fichier de script de console, consultez [utilisation des exemples de fichiers de script de console &#40;SybaseToSQL&#41;](../../ssma/sybase/working-with-the-sample-console-script-files-sybasetosql.md)  
  
L’étape suivante dépend des exigences de votre projet :  
  
-   Pour spécifier un mot de passe ou exporter/importer des mots de passe, consultez [gestion des mots de passe &#40;&#41;SybaseToSQL ](../../ssma/sybase/managing-passwords-sybasetosql.md).  
  
-   Pour générer des rapports, consultez [génération de rapports &#40;&#41;SybaseToSQL ](../../ssma/sybase/generating-reports-sybasetosql.md).  
  
-   Pour résoudre les problèmes dans la console, consultez [troubleshooting &#40;SybaseToSQL&#41;](../../ssma/sybase/troubleshooting-sybasetosql.md).  
  
