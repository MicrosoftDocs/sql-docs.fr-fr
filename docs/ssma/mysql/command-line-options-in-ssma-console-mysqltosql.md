---
description: Options de ligne de commande dans la console SSMA (MySQLToSQL)
title: Options de ligne de commande dans la console SSMA (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Command line options, help option
- Command line options, log file option
- Command line options, project environment folder option
- Command line options, script file option
- Command line options, secure password option
- Command line options, SecurePassword help option
- Command line options, server connection file option
- Command line options, variable value file option
- Command line options, XML output option
ms.assetid: a2310b10-68ad-4285-a08b-c8694cf84416
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: baf69067c681432d83e2350d13c7240179b84cba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069084"
---
# <a name="command-line-options-in-ssma-console-mysqltosql"></a>Options de ligne de commande dans la console SSMA (MySQLToSQL)
Microsoft vous fournit des options de ligne de commande Set robustes pour exécuter et contrôler les activités SSMA. Les sections suivantes détaillent les mêmes sections.  
  
## <a name="command-line-options-in-ssma-console"></a>Options de ligne de commande dans la console SSMA  
Dans les présentes, vous trouverez les options de commande de la console.  
  
Dans le cadre de cette section, le terme « option » est également appelé « commutateur ».  
  
Les options ne respectent pas la casse et peuvent commencer par un caractère « » ou « » **-** **/** .  
  
Si des options sont spécifiées, il devient obligatoire de spécifier les paramètres d’option correspondants.  
  
Les paramètres d’option doivent être séparés du caractère d’option par un espace blanc.  
  
**Exemples de syntaxe :**  
  
`C:\> SSMAforMySQLConsole.EXE -s scriptfile`  
  
`C:\> SSMAforMySQLConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\VariableValueFileSample.xml" -c "C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ServersConnectionFileSample.xml"`  
  
Les noms de dossiers ou de fichiers contenant des espaces doivent être spécifiés entre guillemets doubles.  
  
La sortie des entrées de ligne de commande et des messages d’erreur est stockée dans STDOUT ou dans un fichier spécifié.  
  
### <a name="script-file-option--sscript"></a>Option de fichier de script :-s/script  
Un commutateur obligatoire, le chemin d’accès/nom du fichier de script spécifie le script des séquences de commandes à exécuter par SSMA.  
  
**Exemples de syntaxe :**  
  
`C:\>SSMAforMySQLConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="variable-value-file-option--vvariable"></a>Option de fichier de valeur de variable :-v/variable  
Ce fichier comprend des variables utilisées dans le fichier de script. Il s’agit d’un commutateur facultatif. Si les variables ne sont pas déclarées dans un fichier de variables et utilisées dans le fichier de script, l’application génère une erreur et met fin à l’exécution de la console.  
  
**Exemples de syntaxe :**  
  
Les variables définies dans plusieurs fichiers de valeurs de variables, éventuellement avec une valeur par défaut et une autre avec une valeur spécifique à l’instance, le cas échéant. Le dernier fichier de variable spécifié dans les arguments de ligne de commande prend la préférence, en cas de duplication de variables :  
  
`C:\>SSMAforMySQLConsole.EXE -s`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml" -v c:\migration`  
  
`projects\global_variablevaluefile.xml -v "c:\migrationprojects\instance_variablevaluefile.xml"`  
  
### <a name="server-connection-file-option--cserverconnection"></a>Option du fichier de connexion au serveur :-c/ServerConnection  
Ce fichier contient des informations de connexion au serveur pour chaque serveur. Chaque définition de serveur est identifiée par un ID de serveur unique. Les ID de serveur sont référencés dans le fichier de script pour les commandes associées à la connexion.  
  
La définition de serveur peut faire partie d’un fichier de connexion au serveur et/ou d’un fichier de script. L’ID du serveur dans le fichier de script est prioritaire sur le fichier de connexion au serveur, en cas de duplication de l’ID du serveur.  
  
**Exemples de syntaxe :**  
  
Les ID de serveur sont utilisés dans le fichier de script et sont définis dans un fichier de connexion au serveur distinct, le fichier de connexion au serveur utilise des variables définies dans le fichier de valeurs de la variable :  
  
`C:\>SSMAforMySQLConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -v`  
  
`c:\SsmaProjects\myvaluefile1.xml -c`  
  
`c:\SsmaProjects\myserverconnectionsfile1.xml`  
  
La définition de serveur est incorporée dans le fichier de script :  
  
`C:\>SSMAforMySQLConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="xml-output-option--xxmloutput-xmloutputfile"></a>Option de sortie XML :-x/XMLOutput [xmloutputfile]  
Cette commande permet d’afficher les messages de sortie de commande dans un format XML, qu’il s’agisse d’une console ou d’un fichier XML.  
  
Deux options sont disponibles pour XMLOutput, à savoir :  
  
-   Si le chemin d’accès est fourni après le commutateur XMLOutput, la sortie est redirigée vers le fichier.  
  
    **Exemple de syntaxe :**  
  
    `C:\>SSMAforMySQLConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -x d:\xmloutput\project1output.xml`  
  
-   Si aucun filePath n’est fourni après le commutateur XMLOutput, le XMLout est affiché sur la console.  
  
    **Exemple de syntaxe :**  
  
    `C:\>SSMAforMySQLConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -xmloutput`  
  
### <a name="log-file-option--llog"></a>Option de fichier journal :-l/log  
Toutes les opérations SSMA dans l’application console sont enregistrées dans un fichier journal. Il s’agit d’un commutateur facultatif. Si un fichier journal et son chemin d’accès sont spécifiés sur la ligne de commande, le journal est généré à l’emplacement spécifié. Dans le cas contraire, elle est générée dans son emplacement par défaut.  
  
**Exemple de syntaxe :**  
  
`C:\>SSMAforMySQLConsole.EXE`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -l c:\SsmaProjects\migration1.log`  
  
### <a name="project-environment-folder-option--eprojectenvironment"></a>Option de dossier d’environnement de projet :-e/projectenvironment  
Cela désigne le dossier des paramètres de l’environnement du projet pour le projet SSMA actuel. Ce commutateur est facultatif.  
  
**Exemple de syntaxe :**  
  
`C:\>SSMAforMySQLConsole.EXE -s`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for MySQL\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -e c:\SsmaProjects\CommonEnvironment`  
  
### <a name="secure-password-option--psecurepassword"></a>Option de mot de passe sécurisé :-p/SecurePassword  
Cette option indique le mot de passe chiffré pour les connexions au serveur. Elle diffère de toutes les autres options : l’option n’exécute aucun script et ne permet pas d’effectuer des activités liées à la migration, mais elle permet de gérer le chiffrement des mots de passe pour les connexions serveur utilisées dans le projet de migration.  
  
Vous ne pouvez pas entrer une autre option ou un autre mot de passe en tant que paramètre de ligne de commande. Dans le cas contraire, une erreur est générée. Pour plus d’informations, reportez-vous à la section [gestion des mots de passe](managing-passwords-mysqltosql.md) .  
  
Les sous-options suivantes sont prises en charge pour `-p/securepassword` :  
  
-   Pour ajouter un mot de passe au stockage protégé pour un ID de serveur spécifié ou pour tous les ID de serveur définis dans le fichier de connexion au serveur. L’option-overwrite, ci-dessous, met à jour le mot de passe s’il existe déjà :  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}` `-c|-serverconnection <server-connection-file> [-v|variable <variable-value-file>]``[-o|overwrite]`  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-s|-script <server-connection-file> [-v|variable <variable-value-file>] [-o|overwrite]`  
  
-   Pour supprimer le mot de passe chiffré du stockage protégé de l’ID de serveur spécifié ou pour tous les ID de serveur :  
  
    `-p/securepassword -r/remove {<server_id> [, ...n] | all}`  
  
-   Pour afficher la liste des ID de serveur pour lesquels le mot de passe est chiffré :  
  
    `-p/securepassword -l/list`  
  
-   Pour exporter les mots de passe stockés dans le stockage protégé dans un fichier chiffré. Ce fichier est chiffré avec la phrase secrète spécifiée par l’utilisateur.  
  
    `-p/securepassword -e/export {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
-   Le fichier chiffré précédemment exporté est importé dans le stockage protégé local à l’aide de la phrase secrète spécifiée par l’utilisateur. Une fois le fichier déchiffré, il est stocké dans un nouveau fichier qui, à son tour, est chiffré sur l’ordinateur local.  
  
    `-p/securepassword -i/import {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
    Plusieurs ID de serveur peuvent être spécifiés à l’aide de séparateurs de virgule.  
  
### <a name="help-option--help"></a>Option d’aide :- ?/help  
Affiche le résumé de la syntaxe des options de la console SSMA :  
  
`C:\>SSMAforMySQLConsole.EXE -?`  
  
Pour obtenir un affichage sous forme de tableau des options de ligne de commande de la console SSMA, reportez-vous à [l’annexe-1 &#40;MySQLToSQL&#41;](../../ssma/mysql/appendix-1-mysqltosql.md).  
  
### <a name="securepassword-help-option--securepassword--help"></a>Option d’aide SecurePassword :-SecurePassword- ?/Help  
Affiche le résumé de la syntaxe des options de la console SSMA :  
  
`C:\>SSMAforMySQLConsole.EXE -securepassword -?`  
  
Pour obtenir un affichage sous forme de tableau des options de ligne de commande de la console SSMA, reportez-vous à [l’annexe-1 &#40;MySQLToSQL&#41;](../../ssma/mysql/appendix-1-mysqltosql.md)  
  
### <a name="next-step"></a>étape suivante  
L’étape suivante dépend des exigences de votre projet :  
  
-   Pour spécifier un mot de passe ou exporter/importer des mots de passe, consultez [gestion des mots de passe &#40;&#41;MySQLToSQL ](../../ssma/mysql/managing-passwords-mysqltosql.md).  
  
-   Pour générer des rapports, consultez [génération de rapports &#40;&#41;MySQLToSQL ](../../ssma/mysql/generating-reports-mysqltosql.md).  
  
-   Pour résoudre les problèmes dans la console, consultez [troubleshooting &#40;MySQLToSQL&#41;](../../ssma/mysql/troubleshooting-mysqltosql.md).  
  
