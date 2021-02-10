---
description: Exécution de la console SSMA (AccessToSQL)
title: Exécution de la console SSMA (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: aa1bf665-8dc0-4259-b36f-46ae67197a43
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: cf369ee1104f4b686dc0e7b6ab4bbde7bab458de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044699"
---
# <a name="executing-the-ssma-console-accesstosql"></a>Exécution de la console SSMA (AccessToSQL)
Microsoft vous fournit un ensemble robuste de commandes de fichier de script et d’options de ligne de commande pour exécuter et contrôler les activités SSMA. Les sections suivantes détaillent les mêmes sections.  
  
## <a name="project--script-file-commands"></a>Commandes de fichier de script de projet  
Les commandes de projet gèrent la création de projets, l’ouverture, l’enregistrement et la sortie de projets.  
  
**Commande**  
  
Create-New-Project : crée un nouveau projet SSMA.  
  
**Script**  
  
-   `project-folder` indique le dossier du projet qui est créé.  
  
-   `project-name` indique le nom du projet. {string}  
  
-   `overwrite-if-exists`Attribut facultatif indique si un projet existant doit être remplacé. expression  
  
-   `project-type` est un attribut facultatif.  Les options suivantes sont disponibles pour le type de projet :  
  
    -   SQL-Server-2005  
  
    -   SQL-Server-2008  
  
    -   SQL-Server-2012  
  
    -   SQL-Server-2014  
  
    -   SQL-Server-2016  
  
    -   SQL-Azure  
  
    La valeur par défaut est « SQL-Server-2008 ».  
  
**Exemple :**  
  
```xml  
<create-new-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
  overwrite-if-exists="<true/false>"  
  
  project-type="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014 | sql-azure>"  
  
/>  
```  
L’attribut’overwrite-if-exists’a la **valeur false** par défaut.  
  
L’attribut « Project-type » est **SQL-Server-2008** par défaut.  
  
**Commande**  
  
Open-Project : ouvre un projet existant.  
  
**Script**  
  
-   `project-folder` indique le dossier du projet qui est créé. La commande échoue si le dossier spécifié n’existe pas.  {string}  
  
-   `project-name` indique le nom du projet. La commande échoue si le projet spécifié n’existe pas.  {string}  
  
**Exemple de syntaxe :**  
  
```xml  
<open-project  
  
  project-folder="<project-folder>"  
  
  project-name="<project-name>"  
  
/>  
```  
**Remarque :** SSMA pour l’application console Access prend en charge la compatibilité descendante. Vous pouvez ouvrir des projets créés à l’aide d’une version antérieure de SSMA.  
  
**Commande**  
  
Save-Project : enregistre le projet de migration.  
  
**Script**  
  
**Exemple de syntaxe :**  
  
```xml  
<save-project/>  
```  
**Commande**  
  
Close-Project : ferme le projet de migration.  
  
**Script**  
  
**Exemple de syntaxe :**  
  
```xml  
<close-project  
  
  if-modified="<save/error/ignore>"   (optional)  
  
/>  
```  
L’attribut « If-Modified » est facultatif et **ignoré** par défaut.  
  
## <a name="database-connection-script-file-commands"></a>Commandes du fichier de script de connexion à la base de données  
Les commandes de connexion à la base de données permettent de se connecter à la base de données.  
  
La fonctionnalité **Parcourir** de l’interface utilisateur n’est pas prise en charge dans la console.  
  
Les paramètres **d’authentification** et de **port** Windows ne s’appliquent pas lors de la connexion à SQL Azure.  
  
Pour plus d’informations sur la création de fichiers de script, consultez [création de fichiers de script &#40;AccessToSQL&#41;](../../ssma/access/creating-script-files-accesstosql.md).  
  
**Commande**
  
Connect-source-base de données  
  
- Effectue la connexion à la base de données source et charge les métadonnées de haut niveau de la base de données source, mais pas toutes les métadonnées.  
  
- Si la connexion à la source ne peut pas être établie, une erreur est générée et l’application console cesse d’être exécutée  
  
**Script**
  
La définition de serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur du fichier de connexion au serveur ou du fichier de script.  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
```  
**Commande**  
  
Load-Access-Database : utilisé pour charger les fichiers de base de données Access  
  
**Script**  
  
**Exemple de syntaxe :**  
  
```xml  
<load-access-database  database-file="<Access-database>"/>  
```  
or  
  
```xml  
<load-access-database>  
  
  <access-database database-file="<Access-database1>"/>  
  
  <access-database database-file="<Access-database2>"/>  
  
</load-access-database>  
```  
**Commande**  
  
Force-Load-source/cible-base de données  
  
-   Charge les métadonnées sources.  
  
-   Utile pour travailler sur un projet de migration hors connexion.  
  
-   Si la connexion à la source/cible ne peut pas être établie, une erreur est générée et l’application console s’arrête de s’exécuter.  
  
**Script**  
  
Nécessite un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
**Exemple de syntaxe :**  
  
```xml  
<force-load  
  
  object-name="<object-name>"  
  
  metabase="<source/target>"/>  
```  
or  
  
```xml  
<force-load>  
  
  <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
**Commande**  
  
reconnecter-Source-base de données  
  
- Se reconnecte à la base de données source, mais ne charge pas de métadonnées contrairement à la commande Connect-source-Database.  
  
- Si la connexion (re) avec la source ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Script**
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```

**Commande**
  
connect-target-Database  
  
- Établit une connexion à la base de données SQL Server ou SQL Azure cible et charge les métadonnées de haut niveau de la base de données cible, mais pas les métadonnées entièrement.  
  
- Si la connexion à la cible ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Script**
  
La définition de serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur du fichier de connexion au serveur ou du fichier de script  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  

**Commande**
  
reconnexion-cible-base de données  
  
- Se reconnecte à la base de données cible, mais ne charge pas de métadonnées, contrairement à la commande connect-target-Database.  
  
- Si la connexion (re) à la cible ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Script**
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  

## <a name="report-script-file-commands"></a>Commandes de fichier de script de rapport

Les commandes de rapport génèrent des rapports sur les performances de diverses activités de la console SSMA.

**Commande**
  
générer un rapport d’évaluation  
  
- Génère des rapports d’évaluation sur la base de données source.  
  
- Si la connexion à la base de données source n’est pas exécutée avant l’exécution de cette commande, une erreur est générée et l’application console se ferme.  
  
- L’échec de la connexion au serveur de base de données source lors de l’exécution de la commande entraîne également la fin de l’application console.  
  
**Script**

- `assessment-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
- `object-name:` Spécifie le ou les objets pris en compte pour la génération de rapports d’évaluation (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
- `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
- `assessment-report-overwrite:` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
- `write-summary-report-to:` Spécifie le chemin d’accès où le rapport sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **AssessmentReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    - `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    - `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<generate-assessment-report  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  
  write-summary-report-to="<file>"             (optional)  
  
  verbose="<true/false>"                       (optional)  
  
  report-errors="<true/false>"                 (optional)  
  
  conversion-report-folder="<folder>"          (optional)  
  
  conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```

or
  
```xml  
<generate-assessment-report  
  
  conversion-report-folder="<folder>"            (optional)  
  
  conversion-report-overwrite="<true/false>"     (optional)  
  
>  
    <metabase-object object-name="ssma.Procedures"  
  
        object-type="category"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration-script-file-commands"></a>Commandes du fichier de script de migration

Les commandes de migration convertissent le schéma de base de données cible en schéma source et migrent les données vers le serveur cible.  
  
Le paramètre de sortie par défaut de la console pour les commandes de migration est un rapport de sortie « complet » sans rapport d’erreurs détaillé : uniquement Résumé au niveau du nœud racine de l’arborescence de l’objet source.  
  
**Commande**

convertir-schéma  
  
- Effectue une conversion de schéma de la source vers le schéma cible.  
  
- Si la connexion à la base de données source ou cible n’est pas exécutée avant l’exécution de cette commande ou si la connexion au serveur de base de données source ou cible échoue lors de l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
**Script**

- `conversion-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation peut être stocké. (attribut facultatif)  
  
- `object-name:` Spécifie le ou les objets sources pris en compte pour la conversion du schéma (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
- `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
- `conversion-report-overwrite:` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
- `write-summary-report-to:` Spécifie le chemin d’accès où le rapport sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **SchemaConversionReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    - `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    - `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<convert-schema  
  
  object-name="ssma.Procedures"  
  
  object-type="category"  
  write-summary-report-to="<filepath/folder>"     (optional)  
  
  verbose="<true/false>"                          (optional)  
  
  report-errors="<true/false>"                    (optional)  
  
  conversion-report-folder="<folder>"             (optional)  
  
  conversion-report-overwrite="<true/false>"      (optional)  
  
/>  
```

or  
  
```xml  
<convert-schema  
  
  conversion-report-folder="<folder>"         (optional)  
  
  conversion-report-overwrite="<true/false>"  (optional)  
  
  <metabase-object object-name="ssma.Procedures"  
  
    object-type="category"/>  
  
</convert-schema>  
```

**Commande**
  
migrer-données  
  
- Migre les données sources vers la cible.  
  
**Script**
  
- `object-name:` Spécifie le ou les objets source pris en compte pour la migration des données (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
- `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
- `write-summary-report-to:` Spécifie le chemin d’accès où le rapport sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **DataMigrationReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    - `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    - `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<migrate-data  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true">  
  
    <metabase-object object-name="ssma.TT1"/>  
  
    <metabase-object object-name="ssma.TT2"/>  
  
    <metabase-object object-name="ssma.TT3"/>  
  
    <data-migration-connection  
  
      source-use-last-used="true"/source-server="<server-unique-name>"  
  
      target-use-last-used="true"/target-server="<server-unique-name>"/>  
  
</migrate-data>  
```

or  
  
```xml  
<migrate-data  
  
  object-name="ssma.Tables"  
  
  object-type="category"  
  
  write-summary-report-to="<filepath/folder>"  
  
  report-errors="true" verbose="true"/>  
```

**Commande**
  
Link-tables : cette commande lie la table source (accès) à la table cible.  
  
**Script**

**Exemple de syntaxe :**  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</link-tables>  
```

or  
  
```xml  
<link-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</link-tables>  
```

**Commande**
  
unlink-tables : cette commande dissocie la table source (Access) de la table cible.  
  
**Script**
  
**Exemples de syntaxe :**  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.table1" object-type="Tables"/>  
  
  <metabase-object object-name="AccessDatabase.table2" object-type="Tables"/>  
  
</unlink-tables>  
```

or  
  
```xml  
<unlink-tables>  
  
  <metabase-object object-name="AccessDatabase.Tables" object-type="category"/>  
  
</unlink-tables>  
```  
  
## <a name="migration-preparation-script-file-commands"></a>Commandes du fichier de script de préparation de la migration

La commande de préparation de la migration démarre le mappage de schéma entre les bases de données source et cible.  
  
**Commande**
  
Map-Schema : mappage de schéma de la base de données source vers le schéma cible.  
  
**Script**
  
- `source-schema` Spécifie le schéma source que nous avons l’intention de migrer.  
  
- `sql-server-schema` Spécifie le schéma cible dans lequel vous souhaitez qu’il soit migré.  
  
**Exemple de syntaxe :**  
  
```xml  
<map-schema source-schema="source-schema"  
  
            sql-server-schema="target-schema"/>  
```  
  
## <a name="manageability-commands"></a>Commandes de gestion

Les commandes de gestion aident à synchroniser les objets de base de données cible avec la base de données source.  
  
Le paramètre de sortie par défaut de la console pour les commandes de migration est un rapport de sortie « complet » sans rapport d’erreurs détaillé : uniquement Résumé au niveau du nœud racine de l’arborescence de l’objet source.  
  
**Commande**

synchroniser-cible  
  
- Synchronise les objets cibles avec la base de données cible.  
  
- Si cette commande est exécutée sur la base de données source, vous recevez une erreur.  
  
- Si la connexion à la base de données cible n’est pas exécutée avant l’exécution de cette commande ou si la connexion au serveur de base de données cible échoue au cours de l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
**Script**
  
- `object-name:` Spécifie le ou les objets cibles pris en compte pour la synchronisation avec la base de données cible (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
- `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
- `on-error:` Spécifie si les erreurs de synchronisation doivent être spécifiées en tant qu’avertissements ou erreurs. Options disponibles pour on-Error :  
  
    - Rapport-total-AVERTISSEMENT  
  
    - rapport-chaque AVERTISSEMENT  
  
    - échec du script  
  
- `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération de synchronisation (attribut facultatif) si seul le chemin d’accès au dossier est donné, puis le fichier par nom **TargetSynchronizationReport.XML** est créé.  
  
**Exemple de syntaxe :**  
  
```xml  
<synchronize-target  
  
  object-name="ots_triggers.dbo"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

or  
  
```xml  
<synchronize-target  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"/>  
```

or  
  
```xml  
<synchronize-target>  
  
  <metabase-object object-name="ssma.dbo.TT1"/>  
  
  <metabase-object object-name="ssma.dbo.TT2"/>  
  
  <metabase-object object-name="ssma.dbo.TT3"/>  
  
</synchronize-target>  
```

**Commande**
  
actualisation à partir de la base de données  
  
- Actualise les objets sources de la base de données.  
  
- Si cette commande est exécutée sur la base de données cible, une erreur est générée.  
  
**Script**
  
Nécessite un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
- `object-name:` Spécifie le ou les objets source pris en compte pour l’actualisation à partir de la base de données source (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
- `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
- `on-error:` Spécifie s’il faut spécifier des erreurs d’actualisation comme avertissements ou erreurs. Options disponibles pour on-Error :  
  
    - Rapport-total-AVERTISSEMENT  
  
    - rapport-chaque AVERTISSEMENT  
  
    - échec du script  
  
- `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération d’actualisation (attribut facultatif) si seul le chemin d’accès au dossier est donné, puis le fichier par nom **SourceDBRefreshReport.XML** est créé.  
  
**Exemple de syntaxe :**  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.TT1"  
  
  on-error="<report-total-as-warning|  
  
             report-each-as-warning|  
  
             fail-script>"              (optional)  
  
  report-errors-to="<file-name>"        (optional)  
  
/>  
```

or  
  
```xml  
<refresh-from-database  
  
  object-name="ssma.Procedures"  
  
  object-type="category"/>  
```

or  
  
```xml  
<refresh-from-database>  
  
  <metabase-object object-name="ssma.TT_1"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>Commandes du fichier de script de génération de script

Les commandes de génération de script permettent d’enregistrer la sortie de la console dans un fichier de script.  
  
**Commande**
  
enregistrer en tant que script  
  
Utilisé pour enregistrer les scripts des objets dans un fichier mentionné dans la métabase, il s’agit d’une alternative à la commande de synchronisation dans laquelle, dans, nous obtenons les scripts et exécutons le même sur la base de données cible.  
  
**Script**
  
Nécessite un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
- `object-name:` Spécifie le ou les objets dont les scripts doivent être enregistrés. (Il peut avoir des noms d’objet individuels ou un nom d’objet de groupe)  
  
- `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
- `metabase:` Spécifie s’il s’agit de la métabase source ou cible.  
  
- `destination:` Spécifie le chemin d’accès ou le dossier dans lequel le script doit être enregistré. Si le nom de fichier n’est pas donné, un nom de fichier au format (valeur d’attribut object_name). out  
  
- `overwrite:` Si la valeur est true, elle remplace si des noms de fichiers identiques existent. Il peut avoir les valeurs (true/false).  
  
**Exemple de syntaxe :**  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  object-name="ssma.dbo.Procedures"  
  
  object-type="category"  
  
  destination="<file/folder>"  
  
  overwrite="<true/false>"             (optional)  
  
/>  
```

or  
  
```xml  
<save-as-script  
  
  metabase="<source/target>"  
  
  destination="<file/folder>"  
  
    <metabase-object object-name="ssma.dbo.Procedures"  
  
                     object-type="category"/>  
  
</save-as-script>  
```  
  
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les options de ligne de commande, consultez [options de ligne de commande dans la console SSMA &#40;AccessToSQL&#41;](../../ssma/access/command-line-options-in-ssma-console-accesstosql.md) .  
  
Pour plus d’informations sur les exemples de fichiers de script de console, consultez [utilisation de l’exemple de script de console FilesExecuting la console SSMA &#40;AccessToSQL&#41;](../../ssma/access/working-sample-console-script-filesexecuting-ssma-console-accesstosql.md)  
  
L’étape suivante dépend des exigences de votre projet :  
  
- Pour spécifier un mot de passe ou exporter/importer des mots de passe, consultez [gestion des mots de passe &#40;&#41;AccessToSQL ](../../ssma/access/managing-passwords-accesstosql.md).  
  
- Pour générer des rapports, consultez [génération de rapports &#40;&#41;AccessToSQL ](../../ssma/access/generating-reports-accesstosql.md).  
  
- Pour résoudre les problèmes dans la console, consultez [troubleshooting &#40;AccessToSQL&#41;](../../ssma/access/troubleshooting-accesstosql.md).  
