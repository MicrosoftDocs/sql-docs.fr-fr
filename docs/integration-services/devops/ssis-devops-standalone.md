---
title: SQL Server Integration Services (SSIS) DevOps Tools standalone | Microsoft Docs
description: Découvrez comment créer des tâches de CI/CD SSIS avec SSIS DevOps Tools standalone.
ms.date: 10/16/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 65712139819c65c2fe8804cc8e0f6768025b322a
ms.sourcegitcommit: e3ab17fe9822629eda4df5fb4550df5d495f3134
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98207858"
---
# <a name="standalone-sql-server-integration-service-ssis-devops-tools-preview"></a>SQL Server Integration Services (SSIS) DevOps Tools standalone (préversion)

L’ensemble d’exécutables **SSIS DevOps Tools** standalone permet d’effectuer des tâches de CI/CD SSIS. Sans dépendance à l’installation de Visual Studio ou du runtime SSIS, ces exécutables peuvent être intégrés facilement à n’importe quelle plateforme de CI/CD. Voici les exécutables fournis :

- SSISBuild.exe : générer des projets SSIS dans le modèle de déploiement de projet ou de package.
- SSISDeploy.exe : déployer les fichiers ISPAC dans le catalogue SSIS, ou les fichiers DTSX et leurs dépendances dans le système de fichiers.

## <a name="installation"></a>Installation

La version 4.6.2 ou une version ultérieure de .NET Framework est nécessaire.

Téléchargez le dernier programme d’installation sur le [centre de téléchargement](https://aka.ms/AA9xp65), puis effectuez l’installation avec l’Assistant ou en ligne de commande :

- Installation avec l’Assistant

Double-cliquez sur le fichier .exe à installer, puis spécifiez un dossier pour extraire les fichiers exécutables et les fichiers de dépendance.

![Emplacement d'installation](media/installation-location.png)

- Installation en ligne de commande

```
SSISDevOpsTools.exe /Q /C /T:<full path>
```

![Ligne de commande d'installation](media/installation-command-line.png)


## <a name="ssisbuildexe"></a>SSISBuild.exe

**Syntaxe**

```
SSISBuild.exe -project|-p:<dtproj file path> [-configuration|-c:<configuration name>] [-projectPassword|-pp:<project password>] [-stripSensitive|-ss] [-output|-o:<output path>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Paramètres**

|Paramètre|Description|
|---------|---------|
|-project \|-p:\<dtproj file path>|Chemin du fichier dtproj à générer.|
|-configuration\|-c:\<configuration name>|Nom de la configuration de projet à utiliser pour la génération. S’il n’est pas spécifié, il s’agit par défaut de la première configuration de projet définie dans le fichier dtproj.|
|-projectPassword\|-pp:\<project password>|Mot de passe du projet SSIS et de ses packages. Cet argument n’est valide que si le niveau de protection du projet SSIS et des packages est EncryptSensitiveWithPassword ou EncryptAllWithPassword. Dans le modèle de déploiement de package, tous les packages doivent partager le même mot de passe que celui spécifié par cet argument.|
|-stripSensitive\|-ss|Permet de convertir le niveau de protection du projet SSIS en DontSaveSensitve. Si le niveau de protection est EncryptSensitiveWithPassword ou EncryptAllWithPassword, l’argument -projectPassword doit être défini correctement. Cette option n’est valide que pour le modèle de déploiement de projet.|
|-output\|-o:\<output path>|Chemin de sortie de l’artefact de build. La valeur de cet argument remplacera le chemin de sortie par défaut dans la configuration du projet.|
|-log\|-l:\<log level>[;\<log path>]|Paramètres relatifs aux journaux. <li>Niveau de journalisation : seuls les journaux présentant un niveau de journalisation égal ou supérieur seront écrits dans le fichier journal. Il existe quatre niveaux de journalisation (du plus faible au plus élevé) : DIAG, INFO, WRN et ERR. Le niveau de journalisation par défaut est INFO s’il n’est pas spécifié. <li> Chemin des journaux : Chemin du fichier servant à conserver les journaux. Le fichier journal n’est pas généré si le chemin n’est pas spécifié.|
|-quiet\|-q|Permet de n’afficher aucun journal dans la sortie standard.|
|-help\|-h\|-?|Permet d’afficher des informations détaillées sur l’utilisation de cet utilitaire de ligne de commande.|

**Exemples**

- Créer un dtproj avec la première configuration de projet définie, non chiffré par mot de passe :    
    ```
    SSISBuild.exe -p:"C:\projects\demo\demo.dtproj"
    ```

- Créer un dtproj avec la configuration « DevConfiguration », chiffré par mot de passe, et générez les artefacts de build dans un dossier spécifique :
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -o:D:\folder
    ```

- Créer un dtproj avec la configuration « DevConfiguration », chiffré par mot de passe, en entrelaçant ses données sensibles et le niveau de journalisation DIAG :
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -ss -l:diag
    ```

## <a name="ssisdeployexe"></a>SSISDeploy.exe

**Syntaxe**

```
SSISDeploy.exe -source|-s:<source path> -destination|-d:<type>;<path>[;server] [-authType|-at:<auth type name>] [-connectionStringSuffix|-css:<connection string suffix>] [-projectPassword|-pp:<project password>] [-username|-u:<username>] [-password|-p:<password>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Paramètres**

|Paramètre|Description|
|---------|---------|
|-source\|-s:\<source path>|Chemin de fichier local des artefacts à déployer. ISPAC, DTSX, le chemin de dossier de DTSX et SSISDeploymentManfiest sont autorisés.|
|-destination\|-d:\<type>;\<path>[;server]|Le type de destination, le chemin du dossier de destination et le nom du serveur du catalogue SSIS sur lequel le fichier source sera déployé. Sont actuellement pris en charge les deux types de destination suivants : <li> *CATALOGUE* : déployer un ou plusieurs fichiers ISPAC dans le catalogue SSIS spécifié. Le chemin de la destination CATALOGUE doit être au format suivant : <br> /SSISDB/\<folder name>[/\<project name>] <br> Le <project name> facultatif n’est valide que lorsque la source spécifie un seul chemin de fichier ISPAC. Le nom du serveur doit être spécifié pour la destination CATALOGUE. <li> *FICHIER* : déployer des packages ou des fichiers SSIS spécifiés dans un seul ou plusieurs fichiers SSISDeploymentManifest dans le chemin du système de fichiers spécifié. Le chemin de la destination FICHIER peut être un chemin de dossier local ou réseau au format suivant : <br>\\\\\<machine name>\\\<folder name>[\\\<sub folder name>\...]|
|-authType\|-at:\<auth type name>|Type d’authentification pour accéder à SQL Server. Obligatoire pour la destination CATALOGUE. Les types suivants sont pris en charge : <li> WIN :  Authentification Windows <li> SQL :  l’authentification SQL Server <li> ADPWD :  Active Directory - Authentification par mot de passe <li> ADINT :  Active Directory - Authentification intégrée|
|-connectionStringSuffix\|-css:\<connection string suffix> |Suffixe de la chaîne de connexion utilisée pour se connecter au catalogue SSIS.|
|-projectPassword\|-pp:\<project password> |Mot de passe servant à déchiffrer les fichiers ISPAC ou DTSX.|
|-username\|-u:\<username>  |Nom d’utilisateur utilisé pour accéder au catalogue SSIS ou au système de fichiers spécifié. Le préfixe avec nom de domaine est autorisé pour l’accès au système de fichiers.|
|-password\|-p:\<password>  |Mot de passe utilisé pour accéder au catalogue SSIS ou au système de fichiers spécifié.|
|-log\|-l:\<log level>[;\<log path>] |Paramètres relatifs aux journaux pour l’exécution de cet utilitaire. <li> Niveau de journalisation : seuls les journaux présentant un niveau de journalisation égal ou supérieur seront écrits dans le fichier journal. Il existe quatre niveaux de journalisation (du plus faible au plus élevé) : DIAG, INFO, WRN et ERR. Le niveau de journalisation par défaut est INFO s’il n’est pas spécifié. <li> Chemin des journaux : Chemin du fichier servant à conserver les journaux. Le fichier journal n’est pas généré si le chemin n’est pas spécifié.|
|-quiet\|-q |Permet de ne pas afficher les journaux dans la sortie standard.|
|-help\|-h\|-?  |Permet d’afficher des informations détaillées sur l’utilisation de cet utilitaire de ligne de commande.|

**Exemples**

- Déployer un fichier ISPAC unique non chiffré par mot de passe dans le catalogue SSIS avec l’Authentification Windows.
    ```
    SSISDeploy.exe -s:D:\myfolder\demo.ispac -d:catalog;/SSISDB/destfolder;myssisserver -at:win
    ```

- Déployer un fichier ISPAC unique chiffré par mot de passe dans le catalogue SSIS avec l’authentification SQL, puis renommer le projet.
    ```
    SSISDeploy.exe -s:D:\myfolder\test.ispac -d:catalog;/SSISDB/folder/testproj;myssisserver -at:sql -u:sqlusername -p:sqlpassword -pp:encryptionpassword
    ```

- Déployer un fichier SSISDeploymentManifest unique et les fichiers associés dans le partage de fichiers Azure.
    ```
    SSISDeploy.exe -s:D:\myfolder\mypackage.SSISDeploymentManifest -d:file;\\myssisshare.file.core.windows.net\destfolder -u:Azure\myssisshare -p:storagekey
    ```

- Déployer un dossier de fichiers DTSX dans le système de fichiers local.
    ```
    SSISDeploy.exe -s:D:\myfolder -d:file;\\myssisshare\destfolder
    ```

## <a name="release-notes"></a>Notes de publication

### <a name="version-012-preview"></a>Version 0.1.2 - préversion

Date de publication : 14 janvier 2021

- Correction d’un problème qui provoquait l’échec de la génération du projet par SSISBuild.exe, avec l’exception NullReference, lorsque les métadonnées de paramètre du package dans le fichier projet SSIS et le package SSIS ne correspondaient pas.
- Correction d’un problème qui faisait échouer l’exécution du package avec l’erreur « Impossible de déchiffrer le nœud XML protégé », même si le package était correctement déployé sur SSISDB avec SSISDeploy.exe, lorsque le projet SSIS contenant le package était chiffré avec EncryptSensitiveWithUserKey et que le package contenait un CM avec des données sensibles.

### <a name="version-011-preview"></a>Version 0.1.1 - préversion

Date de publication : 11 novembre 2020

- Correction du problème selon lequel SSISDeploy.exe ne parvenait pas à charger un assembly lors du déploiement d’ispac dans le catalogue SSIS.

### <a name="version-010-preview"></a>Version 0.1.0 - préversion

Date de publication : 16 octobre 2020

Préversion initiale de SSIS DevOps Tools standalone.

## <a name="next-steps"></a>Étapes suivantes

- Téléchargez [SSIS DevOps Tools standalone](https://aka.ms/AA9xp65).
- Si vous avez des questions, consultez [Q&A](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools&ssr=false#qna).
