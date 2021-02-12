---
description: Options de ligne de commande dans la console SSMA (AccessToSQL)
title: Options de ligne de commande dans la console SSMA (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c1f3b3f0-0f3e-4e07-b745-2fbdde85c67e
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 5e091085616078a93a65922bfaa2176ae4a6b16b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076340"
---
# <a name="command-line-options-in-the-ssma-console-accesstosql"></a>Options de ligne de commande dans la console SSMA (AccessToSQL)
Microsoft vous fournit un ensemble fiable d’options de ligne de commande pour exécuter et contrôler les activités SSMA. Les sections suivantes fournissent des détails supplémentaires.  
  
## <a name="command-line-options-in-the-ssma-console"></a>Options de ligne de commande dans la console SSMA  
Dans les présentes, vous trouverez les options de commande de la console.  
  
Dans le cadre de cette section, le terme « option » est également appelé « commutateur ».  
  
Les options ne respectent pas la casse et peuvent commencer par le **-** caractère « » ou « **/** ».  
  
Si des options sont spécifiées, il est obligatoire de spécifier les paramètres d’option correspondants.  
  
Les paramètres d’option doivent être séparés du caractère d’option par un espace blanc.  
  
**Exemples de syntaxe :**  
  
`C:\> SSMAforAccessConsole.EXE -s scriptfile`  
  
`C:\> SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\VariableValueFileSample.xml" -c "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ServersConnectionFileSample.xml"`  
  
Les noms de dossiers ou de fichiers contenant des espaces doivent être spécifiés entre guillemets doubles.  
  
La sortie des entrées de ligne de commande et des messages d’erreur est stockée dans STDOUT ou dans un fichier spécifié.  
  
### <a name="script-file-option--sscript"></a>Option de fichier de script :-s/script  
Un commutateur obligatoire, le chemin d’accès/nom du fichier de script spécifie le script des séquences de commandes à exécuter par SSMA.  
  
**Exemples de syntaxe :**  
  
`C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="variable-value-file-option--vvariable"></a>Option de fichier de valeur de variable :-v/variable  
Le fichier de valeurs de variable comprend des variables utilisées dans le fichier de script. Le commutateur est facultatif. Si les variables ne sont pas déclarées dans un fichier de variables et utilisées dans le fichier de script, l’application génère une erreur et met fin à l’exécution de la console.  
  
**Exemples de syntaxe :**  
  
-   Les variables définies dans plusieurs fichiers de valeurs de variables, éventuellement avec une valeur par défaut et une autre avec une valeur spécifique à l’instance, le cas échéant. Le dernier fichier de variable spécifié dans les arguments de ligne de commande prend la préférence, en cas de duplication de variables :  
  
    `C:\>SSMAforAccessConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml" -v c:\migration`  
  
    `projects\global_variablevaluefile.xml -v "c:\migrationprojects\instance_variablevaluefile.xml"`  
  
### <a name="server-connection-file-option--cserverconnection"></a>Option du fichier de connexion au serveur :-c/ServerConnection  
Ce fichier contient des informations de connexion au serveur pour chaque serveur. Chaque définition de serveur est identifiée par un ID de serveur unique. Les ID de serveur sont référencés dans le fichier de script pour les commandes associées à la connexion.  
  
La définition de serveur peut faire partie d’un fichier de connexion au serveur et/ou d’un fichier de script. L’ID du serveur dans le fichier de script est prioritaire sur le fichier de connexion au serveur, en cas de duplication de l’ID du serveur.  
  
**Exemples de syntaxe :**  
  
-   Les ID de serveur sont utilisés dans le fichier de script. Ils sont définis dans un fichier de connexion au serveur distinct. Ce fichier utilise des variables définies dans le fichier de valeurs de variable :  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -v`  
  
    `c:\SsmaProjects\myvaluefile1.xml -c`  
  
    `c:\SsmaProjects\myserverconnectionsfile1.xml`  
  
-   La définition de serveur est incorporée dans le fichier de script :  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="xml-output-option--xxmloutput-xmloutputfile"></a>Option de sortie XML :-x/XMLOutput [xmloutputfile]  
Cette commande permet d’afficher les messages de sortie de commande dans un format XML, qu’il s’agisse d’une console ou d’un fichier XML.  
  
Deux options sont disponibles pour XMLOutput, à savoir :  
  
-   Si le FilePath est fourni après le commutateur XMLOutput, la sortie est redirigée vers le fichier.  
  
    **Exemple de syntaxe :**  
  
    `C:\>SSMAforAccessConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -x d:\xmloutput\project1output.xml`  
  
-   Si aucun filePath n’est fourni après le commutateur XMLOutput, le XMLout est affiché sur la console.  
  
    **Exemple de syntaxe :**  
  
    `C:\>SSMAforAccessConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -xmloutput`  
  
### <a name="log-file-option--llog"></a>Option de fichier journal :-l/log  
Toutes les opérations SSMA dans l’application console sont enregistrées dans un fichier journal, et le commutateur est facultatif. Si un fichier journal et son chemin d’accès sont spécifiés sur la ligne de commande, le journal est généré à l’emplacement spécifié. Dans le cas contraire, elle est générée dans son emplacement par défaut.  
  
**Exemple de syntaxe :**  
  
`C:\>SSMAforAccessConsole.EXE`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -l c:\SsmaProjects\migration1.log`  
  
### <a name="project-environment-folder-option--eprojectenvironment"></a>Option de dossier d’environnement de projet :-e/projectenvironment  
Ce commutateur facultatif désigne le dossier des paramètres de l’environnement du projet pour le projet SSMA actuel.  
  
**Exemple de syntaxe :**  
  
`C:\>SSMAforAccessConsole.EXE -s`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Access\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -e c:\SsmaProjects\CommonEnvironment`  
  
### <a name="secure-password-option--psecurepassword"></a>Option de mot de passe sécurisé :-p/SecurePassword  
Cette option indique le mot de passe chiffré pour les connexions au serveur. Il diffère de toutes les autres options dans le cas où il n’exécute pas de script ou d’aide dans les activités liées à la migration, mais il permet de gérer le chiffrement des mots de passe pour les connexions serveur utilisées dans le projet de migration.  
  
Vous ne pouvez pas entrer une autre option ou un autre mot de passe en tant que paramètre de ligne de commande. Dans le cas contraire, une erreur est générée. Pour plus d’informations, consultez la section [gestion des mots de passe](managing-passwords-accesstosql.md) .  
  
Les sous-options suivantes sont prises en charge pour `-p/securepassword` :  
  
-   Pour ajouter un mot de passe, ou mettre à jour un mot de passe existant, vers le stockage protégé pour un ID de serveur spécifié ou pour tous les ID de serveur définis dans le fichier de connexion au serveur :  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-c|-serverconnection <server-connection-file> [-v|variable <variable-value-file>]``[-o|overwrite]`  
  
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
  
`C:\>SSMAforAccessConsole.EXE -?`  
  
Pour obtenir un affichage sous forme de tableau des options de ligne de commande de la console SSMA, reportez-vous à [l’annexe-1 &#40;AccessToSQL&#41;](../../ssma/access/appendix-1-accesstosql.md).  
  
### <a name="securepassword-help-option--securepassword--help"></a>Option d’aide SecurePassword :-SecurePassword- ?/Help  
Affiche le résumé de la syntaxe des options de la console SSMA :  
  
`C:\>SSMAforAccessConsole.EXE -securepassword -?`  
  
Pour obtenir un affichage sous forme de tableau des options de ligne de commande de la console SSMA, reportez-vous à [l’annexe-1 &#40;AccessToSQL&#41;](../../ssma/access/appendix-1-accesstosql.md)  
  
### <a name="next-steps"></a>Étapes suivantes  
L’étape suivante dépend des exigences de votre projet :  
  
1.  Pour spécifier un mot de passe ou exporter/importer des mots de passe, consultez [gestion des mots de passe &#40;&#41;AccessToSQL ](../../ssma/access/managing-passwords-accesstosql.md).  
  
2.  Pour générer des rapports, consultez [génération de rapports &#40;&#41;AccessToSQL ](../../ssma/access/generating-reports-accesstosql.md).  
  
3.  Pour résoudre les problèmes dans la console, consultez [troubleshooting &#40;AccessToSQL&#41;](../../ssma/access/troubleshooting-accesstosql.md).  
  
