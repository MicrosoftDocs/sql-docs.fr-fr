---
description: Création de fichiers de valeurs de variables (AccessToSQL)
title: Création de fichiers de valeurs de variables (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 808595c3-8ef1-40bd-a93e-5cf237950e08
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7653d131ca9f345353de46b2579735a527bca5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044929"
---
# <a name="creating-variable-value-files-accesstosql"></a>Création de fichiers de valeurs de variables (AccessToSQL)
Un fichier de valeurs de variable est un fichier XML comprenant les valeurs de paramètres des commandes (telles que le nom du serveur source ou de destination) qui changent fréquemment au sein des migrations du serveur. Quand un grand nombre de migrations de base de données se produisent, plusieurs fichiers de variables pour stocker la valeur de chaque serveur source sont créés et référencés dans un fichier de script principal avec le commutateur **-v** à la ligne de commande. Ce comportement permet de conserver des valeurs statiques dans quelques fichiers de script avec les valeurs des variables dans plusieurs fichiers de variables.  
  
> [!NOTE]  
> -  Les noms de variables sont préfixés et suffixés par un symbole $ (dollar). Si aucune valeur n’est affectée à une variable dans le fichier de valeurs de variable, une erreur se produit lors de l’analyse du fichier de script, provoquant la blocage du processus d’exécution de la console.  
> -  Le caractère d’échappement pour **$** est **$$** . Si la valeur d’une variable ou d’une valeur statique d’un paramètre contient un **$** symbole (dollar), **$$** doit être spécifié pour le traiter en tant que caractère au lieu d’une variable.  
> -  À des fins de maintenabilité, les variables peuvent être déclarées à l’intérieur d' `'variable-group'` éléments pour la séparation logique des variables définies par l’utilisateur.  L’utilisation de cet élément n’est pas obligatoire.  
  
**Exemples :**  
  
**Exemple 1 :**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$type$" value="MyProject"/>  
  
    <variable name="$project_folder$" value=".\$project_name$"/>  
  
    <variable name="$project_name$" value="$type$ConsoleProject"/>  
  
    <variable name="$project_overwrite$" value="true"/>  
  
    <variable name="$project_type$" value="sql-server-2008"/>  
  
  </variable-group>  
  
</variables>  
```  
**Exemple 2 :**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetServerName$" value="xxx"/>  
  
      <variable name="$TargetDB$" value="xxx"/>  
  
      <variable name="$TargetUserName$" value="xxx"/>  
  
      <variable name="$TargetPassword$" value="xxx"/>  
  
      <variable name="$TargetIsTrusted$" value="xxx"/>  
  
      <variable name="$TrustedConnection$" value="xxx"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="TestTable1"/>  
  
      <variable name="$ObjectName2$" value="TestProc1"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>Validation de fichier de valeur de variable  
L’utilisateur peut facilement valider son fichier de valeurs de variable par rapport au fichier de définition de schéma **ConsoleScriptVariablesSchema. xsd** disponible dans le dossier « schemas ».  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante de l’utilisation de la console consiste à [créer les fichiers de connexion au serveur &#40;AccessToSQL&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
  
## <a name="see-also"></a>Voir également  
[Création des fichiers de connexion au serveur (accès)](./creating-the-server-connection-files-accesstosql.md)  
