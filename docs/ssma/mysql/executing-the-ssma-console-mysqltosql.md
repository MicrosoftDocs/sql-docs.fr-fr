---
description: Exécution de la console SSMA (MySQLToSQL)
title: Exécution de la console SSMA (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Script file commands, Database connection commands
- Script file commands, Manageability commands
- Script file commands, Migration commands
- Script file commands, Migration preparation commands
- Script file commands, project commands
- Script file commands, Report commands
- Script file commands, Script generation commands
ms.assetid: e3e9f7e4-0619-4861-a202-3d5d39953b26
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9f9fa74533d7887f968d45dde715b191ac00be88
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015172"
---
# <a name="executing-the-ssma-console-mysqltosql"></a>Exécution de la console SSMA (MySQLToSQL)
Microsoft vous fournit un ensemble robuste de commandes de fichier de script pour exécuter et contrôler les activités SSMA.  
  
L’application console utilise certaines commandes de fichier de script standard comme énuméré dans cette section.  
  
## <a name="project--script-file-commands"></a>Commandes de fichier de script de projet  
**Commande**  
  
créer-nouveau-projet :   
                   Crée un nouveau projet SSMA.  
  
Les commandes de projet gèrent la création de projets, l’ouverture, l’enregistrement et la sortie de projets.  
  
**Script**  
  
1.  `project-folder` indique le dossier du projet qui est créé.  
  
2.  `project-name` indique le nom du projet. {string}  
  
3.  `overwrite-if-exists`Attribut facultatif indique si un projet existant doit être remplacé. expression  
  
4.  `project-type:`Attribut facultatif. Indique le type de projet, c’est-à-dire le projet « SQL-Server-2005 » ou « SQL-Server-2008 » ou le projet « SQL-Server-2012 » ou « SQL-Server-2014 » ou le projet « SQL-Azure ». La valeur par défaut est « SQL-Server-2008 ».  
  
**Exemple de syntaxe :**  
  
```xml  
<create-new-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
   overwrite-if-exists="<true/false>"   (optional)  
  
   project-type=="<sql-server-2008 | sql-server-2005 | sql-server-2012 | sql-server-2014 | sql-azure>"   (optional)  
  
/>  
```  
L’attribut’overwrite-if-exists’a la **valeur false** par défaut.  
  
L’attribut « Project-type » est **SQL-Server-2008** par défaut.  
  
**Commande**  
  
Open-Project :   
                  Ouvre un projet existant.  
  
**Script**  
  
1.  `project-folder` indique le dossier du projet qui est créé. La commande échoue si le dossier spécifié n’existe pas.  {string}  
  
2.  `project-name` indique le nom du projet. La commande échoue si le projet spécifié n’existe pas.  {string}  
  
**Exemple de syntaxe :**  
  
```xml  
<open-project  
  
   project-folder="<project-folder>"  
  
   project-name="<project-name>"  
  
/>  
```  
> [!IMPORTANT]  
> SSMA pour l’application console MySQL prend en charge la compatibilité descendante. Vous pourrez ouvrir des projets créés par la version précédente de SSMA.  
  
**Commande**  
  
Save-Project : enregistre le projet de migration.  
  
**Script**  
  
**Exemple de syntaxe :**  
  
```xml  
<save-project/>  
```  
**Commande**  
  
fermer-projet  
                  : Ferme le projet de migration.  
  
**Script**  
  
**Exemple de syntaxe :**  
  
```xml  
<save-project/>  
```  
**Commande**  
  
fermer-projet  
                  : Ferme le projet de migration.  
  
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
  
1.  La fonctionnalité **Parcourir** de l’interface utilisateur n’est pas prise en charge dans la console.  
  
2.  Les paramètres **d’authentification** et de **port** Windows ne s’appliquent pas lors de la connexion à SQL Azure.  
  
3.  Pour plus d’informations sur la création de fichiers de script, consultez [création de fichiers de script &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-script-files-mysqltosql.md).  
  
**Commande**  
  
Connect-source-base de données  
  
-   Effectue la connexion à la base de données source et charge les métadonnées de haut niveau de la base de données source, mais pas toutes les métadonnées.  
  
-   Si la connexion à la source ne peut pas être établie, une erreur est générée et l’application console cesse de s’exécuter.  
  
**Script**  
  
La définition de serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur du fichier de connexion au serveur ou du fichier de script.  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-source-database  server="<server-unique-name>"/>  
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
<force-load metabase="<source/target>"  
  
      <metabase-object object-name="<object-name>"/>  
  
</force-load>  
```  
**Commande**  
  
reconnecter-Source-base de données  
  
1.  Se reconnecte à la base de données source, mais ne charge pas de métadonnées contrairement à la commande Connect-source-Database.  
  
2.  Si la connexion (re) avec la source ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Script**  
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-source-database  server="<server-unique-name>"/>  
```  
**Commande**  
  
connect-target-Database  
  
1.  Se connecte au SQL Server ou Azure SQL Database cible et charge les métadonnées de haut niveau de la base de données cible, mais pas les métadonnées entièrement.  
  
2.  Si la connexion à la cible ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Script**  
  
La définition de serveur est récupérée à partir de l’attribut de nom défini pour chaque connexion dans la section serveur du fichier de connexion au serveur ou du fichier de script  
  
**Exemple de syntaxe :**  
  
```xml  
<connect-target-database  server="<server-unique-name>"/>  
```  
**Commande**  
  
reconnexion-cible-base de données  
  
1.  Se reconnecte à la base de données cible, mais ne charge pas de métadonnées, contrairement à la commande connect-target-Database.  
  
2.  Si la connexion (re) à la cible ne peut pas être établie, une erreur est générée et l’application console arrête d’être exécutée.  
  
**Script**  
  
**Exemple de syntaxe :**  
  
```xml  
<reconnect-target-database  server="<server-unique-name>"/>  
```  
  
## <a name="report-script-file-commands"></a>Commandes de fichier de script de rapport  
Les commandes de rapport génèrent des rapports sur les performances de diverses activités de la console SSMA.  
  
**Commande**  
  
générer un rapport d’évaluation  
  
1.  Génère des rapports d’évaluation sur la base de données source.  
  
2.  Si la connexion à la base de données source n’est pas exécutée avant l’exécution de cette commande, une erreur est générée et l’application console se ferme.  
  
3.  L’échec de la connexion au serveur de base de données source lors de l’exécution de la commande entraîne l’arrêt de l’application console.  
  
**Script**  
  
1.  `assessment-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation est stocké. (attribut facultatif)  
  
2.  `object-name:` Spécifie le ou les objets pris en compte pour la génération de rapports d’évaluation (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
3.  `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
4.  `assessment-report-overwrite:` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
5.  `write-summary-report-to:` Spécifie le chemin d’accès où le rapport de synthèse sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **AssessmentReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    -   `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<generate-assessment-report  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<generate-assessment-report  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
>  
  
      <metabase-object object-name="<object-name>"  
  
         object-type="<object-category>"/>  
  
</generate-assessment-report>  
```  
  
## <a name="migration--script-file-commands"></a>Commandes du fichier de script de migration  
Les commandes de migration convertissent le schéma de base de données cible en schéma source et migrent les données vers le serveur cible.  
  
Le paramètre de sortie par défaut de la console pour les commandes de migration est un rapport de sortie « complet » sans rapport d’erreurs détaillé : uniquement Résumé au niveau du nœud racine de l’arborescence de l’objet source.  
  
**Commande**  
  
convertir-schéma  
  
1.  Effectue une conversion de schéma de la source vers le schéma cible.  
  
2.  Si la connexion à la base de données source ou cible n’est pas exécutée avant l’exécution de cette commande ou si la connexion au serveur de base de données source ou cible échoue lors de l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
**Script**  
  
1.  `conversion-report-folder:` Spécifie le dossier dans lequel le rapport d’évaluation est stocké. (attribut facultatif)  
  
2.  `object-name:` Spécifie le ou les objets pris en compte pour la conversion du schéma (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
3.  `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
4.  `conversion-report-overwrite:` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
5.  `write-summary-report-to:` Spécifie le chemin d’accès où le rapport de synthèse sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **SchemaConversionReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création d’un rapport de synthèse a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    -   `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<convert-schema  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
/>  
```  
or  
  
```xml  
<convert-schema  
  
   conversion-report-folder="<folder-name>"   (optional)  
  
   conversion-report-overwrite="<true/false>"   (optional)  
  
      <metabase-object object-name="<object-names>"  
  
         object-type="<object-category>"/>  
  
</convert-schema>  
```  
**Commande**  
  
migrer-données  
  
1.  Migre les données sources vers la cible.  
  
**Script**  
  
1.  `object-name:` Spécifie le ou les objets source pris en compte pour la migration des données (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
2.  `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
3.  `write-summary-report-to:` Spécifie le chemin d’accès où le rapport de synthèse sera généré.  
  
    Si seul le chemin d’accès au dossier est mentionné, nommez-le **DataMigrationReport &lt; n &gt; . XML** créé. (attribut facultatif)  
  
    La création de rapports a deux sous-catégories supplémentaires :  
  
    -   `report-errors` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
    -   `verbose` (= "true/false", avec la valeur par défaut "false" (attributs facultatifs))  
  
**Exemple de syntaxe :**  
  
```xml  
<migrate-data  
  
   write-summary-report-to="<file-name/folder-name>"  
  
   report-errors="true" verbose="true">  
  
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
  
   report-errors="true" verbose="true"/>  
```  
  
## <a name="migration-preparation-script-file-command"></a>Commande de fichier de script de préparation de la migration  
La commande de préparation de la migration lance le mappage de schéma entre les bases de données source et cible.  
  
**Commande**  
  
mappage-schéma  
  
Mappage de schéma de la base de données source vers le schéma cible.  
  
**Script**  
  
1.  `source-schema` Spécifie le schéma source que nous avons l’intention de migrer.  
  
2.  `sql-server-schema` Spécifie le schéma cible dans lequel vous souhaitez qu’il soit migré.  
  
**Exemple de syntaxe :**  
  
```xml  
<map-schema  
  
   source-schema="<source-schema>"  
  
   sql-server-schema="<target-schema>"/>  
```  
  
## <a name="manageability-script-file-commands"></a>Commandes de fichier de script de gestion  
Les commandes de gestion aident à synchroniser les objets de base de données cible avec la base de données source.  
  
> [!NOTE]  
> Le paramètre de sortie par défaut de la console pour les commandes de migration est un rapport de sortie « complet » sans rapport d’erreurs détaillé : uniquement Résumé au niveau du nœud racine de l’arborescence de l’objet source.  
  
**Commande**  
  
synchroniser-cible  
  
1.  Synchronise les objets cibles avec la base de données cible.  
  
2.  Si cette commande est exécutée sur la base de données source, une erreur est détectée.  
  
3.  Si la connexion à la base de données cible n’est pas exécutée avant l’exécution de cette commande ou si la connexion au serveur de base de données cible échoue au cours de l’exécution de la commande, une erreur est générée et l’application console se ferme.  
  
**Script**  
  
1.  `object-name:` Spécifie le ou les objets pris en compte pour la synchronisation avec la base de données cible (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
2.  `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
3.  `on-error:` Spécifie si les erreurs de synchronisation doivent être spécifiées en tant qu’avertissements ou erreurs. Options disponibles pour on-Error :  
  
    -   Rapport-total-AVERTISSEMENT  
  
    -   rapport-chaque AVERTISSEMENT  
  
    -   échec du script  
  
4.  `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération de synchronisation (attribut facultatif) si seul le chemin d’accès au dossier est donné, puis le fichier par nom **TargetSynchronizationReport.XML** est créé.  
  
**Exemple de syntaxe :**  
  
```xml  
<synchronize-target  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name>"   (optional)  
  
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
**Commande**  
  
actualisation à partir de la base de données  
  
1.  Actualise les objets sources de la base de données.  
  
2.  Si cette commande est exécutée sur la base de données cible, une erreur est générée.  
  
**Script**  
  
1.  `object-name:` Spécifie le ou les objets source pris en compte pour l’actualisation à partir de la base de données source (il peut avoir des noms d’objet individuels ou un nom d’objet de groupe).  
  
2.  `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
3.  `on-error:` Spécifie si les erreurs de synchronisation doivent être spécifiées en tant qu’avertissements ou erreurs. Options disponibles pour on-Error :  
  
    -   Rapport-total-AVERTISSEMENT  
  
    -   rapport-chaque AVERTISSEMENT  
  
    -   échec du script  
  
4.  `report-errors-to:` Spécifie l’emplacement du rapport d’erreurs pour l’opération de synchronisation (attribut facultatif) si seul le chemin d’accès au dossier est donné, puis le fichier par nom **SourceDBRefreshReport.XML** est créé.  
  
Nécessite un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
**Exemple de syntaxe :**  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   on-error="<report-total-as-warning/  
  
               report-each-as-warning/  
  
               fail-script>"   (optional)  
  
   report-errors-to="<file-name>"   (optional)  
  
/>  
```  
or  
  
```xml  
<refresh-from-database  
  
   object-name="<object-name>"  
  
   object-type="<object-category>"/>  
```  
or  
  
```xml  
<refresh-from-database>  
  
   <metabase-object object-name="<object-name>"/>  
  
</refresh-from-database>  
```  
  
## <a name="script-generation-script-file-commands"></a>Commandes du fichier de script de génération de script  
Les commandes de génération de script effectuent des tâches doubles : elles permettent d’enregistrer la sortie de la console dans un fichier de script. et enregistrent la sortie T-SQL dans la console ou un fichier en fonction du paramètre que vous spécifiez.  
  
**Commande**  
  
enregistrer en tant que script  
  
Utilisé pour enregistrer les scripts des objets dans un fichier mentionné dans la métabase, il s’agit d’une alternative à la commande de synchronisation dans laquelle, dans, nous obtenons les scripts et exécutons le même sur la base de données cible.  
  
**Script**  
  
Nécessite un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
1.  `object-name:` Spécifie le ou les objets dont les scripts doivent être enregistrés. (Il peut avoir des noms d’objet individuels ou un nom d’objet de groupe)  
  
2.  `object-type:` Spécifie le type de l’objet spécifié dans l’attribut Object-Name (si la catégorie Object est spécifiée, le type d’objet est « Category »).  
  
3.  `metabase:` Spécifie s’il s’agit de la métabase source ou cible.  
  
4.  `destination:` Spécifie le chemin d’accès ou le dossier dans lequel le script doit être enregistré, si le nom de fichier n’est pas donné, un nom de fichier au format (valeur d’attribut object_name). out  
  
5.  `overwrite:` Si la valeur est true, elle remplace si le même nom de fichier existe. Il peut avoir les valeurs (true/false).  
  
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
**Commande**  
  
Convert-SQL-Statement  
  
1.  `context` Spécifie le nom du schéma.  
  
2.  `destination` Spécifie si la sortie doit être stockée dans un fichier.  
  
    Si cet attribut n’est pas spécifié, l’instruction T-SQL convertie est affichée sur la console. (attribut facultatif)  
  
3.  `conversion-report-folder` Spécifie le dossier dans lequel le rapport d’évaluation est stocké. (attribut facultatif)  
  
4.  `conversion-report-overwrite` Spécifie si le dossier de rapport d’évaluation doit être remplacé s’il existe déjà.  
  
    **Valeur par défaut :** false. (attribut facultatif)  
  
5.  `write-converted-sql-to` Spécifie le chemin d’accès au dossier (ou) du fichier dans lequel le T-SQL converti doit être stocké. Quand un chemin d’accès au dossier est spécifié avec l' `sql-files` attribut, chaque fichier source a un fichier T-SQL cible correspondant créé dans le dossier spécifié. Quand un chemin d’accès au dossier est spécifié avec l' `sql` attribut, le T-SQL converti est écrit dans un fichier nommé result. out dans le dossier spécifié.  
  
6.  `sql` spécifie les instructions SQL MySQL à convertir, une ou plusieurs instructions peuvent être séparées à l’aide d’un « ; »  
  
7.  `sql-files` Spécifie le chemin d’accès des fichiers SQL qui doivent être convertis en code T-SQL.  
  
8.  `write-summary-report-to` Spécifie le chemin d’accès où le rapport de synthèse sera généré. Si seul le chemin d’accès au dossier est mentionné, le fichier par nom **ConvertSQLReport.XML** est créé. (attribut facultatif)  
  
    La création de rapports a 2 sous-catégories supplémentaires, à savoir :  
  
    -   Report-Errors (= "true/false", avec la valeur par défaut "false" (attributs facultatifs)).  
  
    -   verbose (= "true/false", avec la valeur par défaut "false" (attributs facultatifs)).  
  
**Script**  
  
Nécessite un ou plusieurs nœuds de la métabase comme paramètre de ligne de commande.  
  
**Exemple de syntaxe :**  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   destination="stdout/file"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
   sql="SELECT 1 FROM DUAL;">  
  
      <output-window suppress-messages="<true/false>" />  
  
</convert-sql-statement>  
```  
or  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   write-summary-report-to="<file-name/folder-name>"   (optional)  
  
   verbose="<true/false>"   (optional)  
  
   report-errors="<true/false>"   (optional)  
  
   destination="<stdout/file>"   (optional)  
  
   write-converted-sql-to="<file-name/folder-name>"  
  
   sql-files="<folder-name>\*.sql"  
  
/>  
```  
or  
  
```  
<convert-sql-statement  
  
   context="<schema-name>"  
  
   conversion-report-folder="<folder-name>"  
  
   conversion-report-overwrite="<true/false>"  
  
   sql-files="<folder-name>\*.sql"  
  
/>  
```  
  
## <a name="next-step"></a>étape suivante  
Pour plus d’informations sur les options de ligne de commande, consultez [options de ligne de commande dans la console SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/command-line-options-in-ssma-console-mysqltosql.md) .  
  
Pour plus d’informations sur les exemples de fichiers de script de console, consultez [utilisation des exemples de fichiers de script de console &#40;MySQLToSQL&#41;](../../ssma/mysql/working-with-the-sample-console-script-files-mysqltosql.md)  
  
L’étape suivante dépend des exigences de votre projet :  
  
1.  Pour spécifier un mot de passe ou exporter/importer des mots de passe, consultez [gestion des mots de passe &#40;&#41;MySQLToSQL ](../../ssma/mysql/managing-passwords-mysqltosql.md).  
  
2.  Pour générer des rapports, consultez [génération de rapports &#40;&#41;MySQLToSQL ](../../ssma/mysql/generating-reports-mysqltosql.md).  
  
3.  Pour résoudre les problèmes dans la console, consultez [troubleshooting &#40;MySQLToSQL&#41;](../../ssma/mysql/troubleshooting-mysqltosql.md).  
  
