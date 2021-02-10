---
description: Création de fichiers de valeurs de variables (SybaseToSQL)
title: Création de fichiers de valeurs de variables (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Creating Variable Value Files
- Sybase Console,Variable Value File Validation
ms.assetid: 395be464-4b19-44f7-91e5-b8876d6743dc
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 3ce72fdb5c93ac6a78dcd9a8ed14f42682d72182
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056644"
---
# <a name="creating-variable-value-files-sybasetosql"></a>Création de fichiers de valeurs de variables (SybaseToSQL)
Le fichier de valeurs de variable est un fichier XML comprenant les valeurs de paramètres des commandes, telles que le nom du serveur source ou de destination qui changent fréquemment d’une migration de serveur à une autre. En cas de migration d’un grand nombre de bases de données, plusieurs fichiers de variables pour stocker la valeur de chaque serveur source sont créés et référencés dans un fichier de script principal avec le commutateur **-v** à la ligne de commande. Cela permet de conserver des valeurs statiques dans quelques fichiers de script avec les valeurs des variables dans plusieurs fichiers de variables.  
  
> [!NOTE]  
> 1.  Les noms de variables sont préfixés et suffixés par un symbole $ (dollar). Si aucune valeur n’est assignée aux variables dans le fichier de valeurs de variable, vous rencontrerez une erreur lors de l’analyse du fichier de script, ce qui entraînera le blocage du processus d’exécution de la console.  
> 2.  Le caractère d’échappement pour **$** est **$$** . Si la valeur d’une variable ou d’une valeur statique d’un paramètre contient un **$** symbole (dollar), **$$** doit être spécifié pour le traiter en tant que caractère au lieu d’une variable.  
> 3.  À des fins de maintenabilité, les variables peuvent être déclarées à l’intérieur d' `'variable-group'` éléments pour la séparation logique des variables définies par l’utilisateur.  L’utilisation de cet élément n’est pas obligatoire.  
  
**Exemples :**  
  
**Exemple 1 :**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$project_folder$" value="<project-folder>"/>  
  
    <variable name="$project_name$" value="<project-name>"/>  
  
    <variable name="$project_overwrite$" value="<true/false>"/>  
  
    <variable name="$project_type$" value="<project-type>"/>  
  
  </variable-group>  
  
</variables>  
```  
**Exemple 2 :**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetUserName$" value="<user-name>"/>  
  
      <variable name="$TargetServerName$" value="<server-name>"/>  
  
      <variable name="$TargetDB$" value="<database-name>"/>  
  
      <variable name="$TargetPassword$" value="<password>"/>  
  
      <variable name="$TrustedConnection$" value="<true/false>"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="<object-name>"/>  
  
      <variable name="$ObjectName2$" value="<object-name>"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>Validation de fichier de valeur de variable  
L’utilisateur peut facilement valider son fichier de valeurs de variable par rapport au fichier de définition de schéma **ConsoleScriptVariablesSchema. xsd** disponible dans le dossier « schemas ».  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante de l’utilisation de la console consiste à [créer les fichiers de connexion au serveur &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Création des fichiers du serveur (Sybase)](./creating-the-server-connection-files-sybasetosql.md)  
