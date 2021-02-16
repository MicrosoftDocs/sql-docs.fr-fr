---
title: Vue d’ensemble de SQL Server Integration Services DevOps | Microsoft Docs
description: Découvrez comment créer le CICD dans SSIS avec SSIS DevOps Tools.
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f44c8c3677f76ebc96b6e0385aa839a9e20aa2f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030607"
---
# <a name="sql-server-integration-services-ssis-devops-tools-azure-devops-extension"></a>Extension Azure DevOps SQL Server Integration Services (SSIS) DevOps Tools

L’extension [SSIS DevOps Tools](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools) est disponible dans la Place de Marché **Azure DevOps**.

Si vous n’avez pas d’organisation **Azure DevOps**, vous devez tout d’abord vous inscrire à [Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops&preserve-view=true), puis ajouter l’extension **SSIS DevOps Tools** en suivant [ces étapes](/azure/devops/marketplace/overview?tabs=browser&view=azure-devops&preserve-view=true#add-an-extension).

**SSIS DevOps Tools** inclut la tâche de **génération SSIS**, la tâche de fin de **déploiement SSIS** et la tâche de **configuration du catalogue SSIS**.

- La tâche de **[génération SSIS](#ssis-build-task)** prend en charge la génération de fichiers dtproj dans un modèle de déploiement de projet ou de package.

- La tâche de **[déploiement SSIS](#ssis-deploy-task)** prend en charge le déploiement d’un ou plusieurs fichiers ispac dans un catalogue SSIS local et un runtime d’intégration Azure-SSIS ou de fichiers SSISDeploymentManifest et des fichiers associés dans un partage de fichiers Azure ou local.

- La tâche de **[configuration du catalogue SSIS](#ssis-catalog-configuration-task)** prend en charge la configuration des éléments dossier/projet/environnement du catalogue SSIS avec un fichier de configuration au format JSON. Cette tâche prend en charge les scénarios suivants :
    - Dossier
        - Créer un dossier.
        - Mettre à jour la description d’un dossier.
    - Projet
        - Configurer la valeur des paramètres (la valeur littérale et la valeur référencée sont prises en charge).
        - Ajouter des références d’environnement.
    - Environnement
        - Créer un environnement.
        - Mettre à jour la description d’un environnement.
        - Créer ou mettre à jour une variable d’environnement.

## <a name="ssis-build-task"></a>Tâche de génération SSIS

![tâche de génération](media/ssis-build-task.png)

### <a name="properties"></a>Propriétés

#### <a name="project-path"></a>Chemin du projet

Chemin du fichier ou dossier de projet à générer. Si un chemin de dossier est spécifié, la tâche de génération SSIS recherchera tous les fichiers dtproj de manière récursive dans ce dossier et les générera tous.

Le chemin du projet ne peut pas être *vide*, défini comme **.** pour effectuer une génération à partir du dossier racine du référentiel.

#### <a name="project-configuration"></a>Configuration du projet

Nom de la configuration de projet à utiliser pour la génération. S’il n’est pas spécifié, il s’agit, par défaut, de la première configuration de projet définie dans chaque fichier dtproj.

#### <a name="output-path"></a>Chemin de sortie

Chemin d’un dossier distinct où seront enregistrés les résultats de la génération. Ceux-ci peuvent être publiés comme artefact de build à l’aide de la [tâche de publication d’artefacts de build](/azure/devops/pipelines/tasks/utility/publish-build-artifacts?view=azure-devops&preserve-view=true).

### <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

- La tâche de génération SSIS s’appuie sur Visual Studio et le concepteur SSIS (obligatoire avec les agents de build). Ainsi, pour exécuter la tâche de génération SSIS dans le pipeline, vous devez choisir **vs2017-win2016** pour les agents hébergés par Microsoft ou installer Visual Studio et le concepteur SSIS (VS2017 + SSDT2017 ou VS2019 + extension de projets SSIS) sur les agents autohébergés.

- Pour générer des projets SSIS à l’aide de composants prêts à l’emploi (quels qu’ils soient, y compris le Feature Pack SSIS Azure ou d’autres composants tiers), veillez à ce que ces composants soient installés sur l’ordinateur sur lequel s’exécute l’agent de pipeline.  Pour l’agent hébergé par Microsoft, l’utilisateur peut ajouter une [tâche de script PowerShell](/azure/devops/pipelines/tasks/utility/powershell?view=azure-devops&preserve-view=true) ou une [tâche de script de ligne de commande](/azure/devops/pipelines/tasks/utility/command-line?view=azure-devops&preserve-view=true) pour télécharger et installer les composants avant l’exécution de la tâche de génération SSIS. Voici l’exemple de script PowerShell permettant d’installer Azure Feature Pack : 

```powershell
wget -Uri https://download.microsoft.com/download/E/E/0/EE0CB6A0-4105-466D-A7CA-5E39FA9AB128/SsisAzureFeaturePack_2017_x86.msi -OutFile AFP.msi

start -Wait -FilePath msiexec -Args "/i AFP.msi /quiet /l* log.txt"

cat log.txt
```

- Les niveaux de protection **EncryptSensitiveWithPassword** et **EncryptAllWithPassword** ne sont pas pris en charge par la tâche de génération SSIS. Veillez à ce qu’aucun des projets SSIS du code base n’utilise ces deux niveaux de protection. Sinon, la tâche de génération SSIS cessera de répondre et dépassera le délai d’attente pendant l’exécution.

## <a name="ssis-deploy-task"></a>Tâche de déploiement SSIS

![tâche de déploiement](media/ssis-deploy-task.png)

### <a name="properties"></a>Propriétés

#### <a name="source-path"></a>Chemin source

Chemin des fichiers ISPAC ou SSISDeploymentManifest sources que vous souhaitez déployer. Il peut s’agir d’un chemin de dossier ou de fichier.

#### <a name="destination-type"></a>Type de destination

Type de la destination. Actuellement, la tâche de déploiement SSIS prend en charge deux types :

- *Système de fichiers* : les fichiers SSISDeploymentManifest et les fichiers associés sont déployés dans un système de fichiers spécifié. Les partages de fichiers locaux et Azure sont pris en charge.
- *SSISDB* : les fichiers ISPAC sont déployés dans un catalogue SSIS spécifié, qui peut être hébergé sur un serveur SQL Server local ou un runtime d’intégration Azure-SSIS.

#### <a name="destination-server"></a>Serveur de destination

Nom du serveur SQL Server de destination. Il peut s’agir du nom d’une instance SQL Server locale, Azure SQL Database ou Azure SQL Managed Instance. Cette propriété est visible uniquement si le type de destination est SSISDB.

#### <a name="destination-path"></a>Chemin de destination

Chemin du dossier de destination dans lequel le fichier source sera déployé. Par exemple :

- /SSISDB/\<folderName\>
- \\\\\<machineName\>\\\<shareFolderName\>\\\<optionalSubfolderName\>

La tâche de déploiement SSIS crée le dossier et le sous-dossier s’ils n’existent pas.

#### <a name="authentication-type"></a>Type d'authentification

Type d’authentification utilisé pour l’accès au serveur de destination spécifié. Cette propriété est visible uniquement si le type de destination est SSISDB. En général, les types d’authentification suivants sont pris en charge :

- Authentification Windows
- l’authentification SQL Server
- Active Directory - Authentification par mot de passe
- Active Directory - Authentification intégrée

Cependant, la prise en charge d’un type d’authentification spécifique dépend du type de serveur de destination et du type d’agent. Reportez-vous au tableau de prise en charge détaillé ci-dessous.

|Type de serveur de destination|Agent hébergé par Microsoft|Agent autohébergé|
|---------|---------|---------|
|Serveur SQL Server local ou machine virtuelle |N/A|Authentification Windows|
|Azure SQL|l’authentification SQL Server <br> Active Directory - Authentification par mot de passe|l’authentification SQL Server <br> Active Directory - Authentification par mot de passe <br> Active Directory - Authentification intégrée|

#### <a name="domain-name"></a>Nom de domaine

Nom de domaine utilisé pour l’accès au système de fichiers spécifié. Cette propriété est visible uniquement si le type de destination est Système de fichiers.
Vous pouvez la laisser vide si le compte d’utilisateur utilisé pour l’exécution de l’agent autohébergé dispose d’un accès en lecture/écriture au chemin de destination spécifié.

#### <a name="username"></a>Nom d’utilisateur

Nom d’utilisateur utilisé pour l’accès à la base de données SSISDB ou au système de fichiers spécifié. Cette propriété est visible si le type de destination est Système de fichiers ou si le type d’authentification est Authentification SQL Server ou Active Directory - Authentification par mot de passe.
Vous pouvez la laisser vide si le type de destination est Système de fichiers et si le compte d’utilisateur utilisé pour l’exécution de l’agent autohébergé dispose d’un accès en lecture/écriture au chemin de destination spécifié.

#### <a name="password"></a>Mot de passe

Mot de passe utilisé pour l’accès à la base de données SSISDB ou au système de fichiers spécifié. Cette propriété est visible si le type de destination est Système de fichiers ou si le type d’authentification est Authentification SQL Server ou Active Directory - Authentification par mot de passe.
Vous pouvez la laisser vide si le type de destination est Système de fichiers et si le compte d’utilisateur utilisé pour l’exécution de l’agent autohébergé dispose d’un accès en lecture/écriture au chemin de destination spécifié.

#### <a name="overwrite-existing-projects-or-ssisdeploymentmanifest-files-of-the-same-names"></a>Remplacer les projets ou fichiers SSISDeploymentManifest existants portant le même nom

Indique si les projets ou fichiers SSISDeploymentManifest existants portant le même nom doivent être remplacés. Si vous sélectionnez « Non », la tâche de déploiement SSIS ignorera le déploiement de ces projets ou fichiers.

#### <a name="continue-deployment-when-error-occurs"></a>Continuer le déploiement si une erreur se produit

Indique si le déploiement des projets ou des fichiers restants doit continuer quand une erreur se produit. Si vous sélectionnez « Non », la tâche de déploiement SSIS s’arrêtera immédiatement si une erreur se produit.

### <a name="limitations-and-known-issues"></a>Limitations et problèmes connus

Actuellement, la tâche de déploiement SSIS ne prend pas en charge les scénarios suivants :

- Configuration de l’environnement dans le catalogue SSIS
- Déploiement de fichiers ISPAC sur Azure SQL Server ou Azure SQL Managed Instance, qui autorise uniquement l’authentification multifacteur (MFA)
- Déploiement de packages dans MSDB ou le magasin de packages SSIS

## <a name="ssis-catalog-configuration-task"></a>Tâche de configuration du catalogue SSIS

![tâche de configuration du catalogue](media/ssis-catalog-configuation-task.png)

### <a name="properties"></a>Propriétés

#### <a name="configuration-file-source"></a>Source du fichier de configuration

Source du fichier JSON utilisé pour la configuration du catalogue SSIS. Les options possibles sont « Chemin fichier » ou « Inline ».

Pour plus de détails, consultez la section [Définir le schéma JSON de configuration](#define-configuration-json) :

- Examinez un [exemple de fichier JSON de configuration inline](#a-sample-inline-configuration-json).
- Prenez connaissance du [schéma JSON](#json-schema).

#### <a name="configuration-json-file-path"></a>Chemin du fichier JSON de configuration

Chemin du fichier JSON utilisé pour la configuration du catalogue SSIS. Cette propriété est visible uniquement si vous sélectionnez « Chemin fichier » comme source du fichier de configuration.

Pour utiliser des [variables de pipeline](/azure/devops/pipelines/process/variables) dans le fichier JSON de configuration, vous devez ajouter une [tâche de transformation de fichier](/azure/devops/pipelines/tasks/utility/file-transform?view=azure-devops&preserve-view=true) avant cette tâche, ce qui a pour effet de remplacer les valeurs de configuration par les variables de pipeline. Pour plus d’informations, consultez la section [Substitution de variables JSON](/azure/devops/pipelines/tasks/transforms-variable-substitution?tabs=Classic&view=azure-devops&preserve-view=true#json-variable-substitution).

#### <a name="inline-configuration-json"></a>Fichier JSON inline de configuration

Fichier JSON inline utilisé pour la configuration du catalogue SSIS. Cette propriété est visible uniquement si vous sélectionnez « Inline » comme source du fichier de configuration. Les variables de pipeline peuvent être utilisées directement.

#### <a name="roll-back-configuration-when-error-occurs"></a>Restaurer la configuration après une erreur

Détermine si la configuration effectuée par cette tâche doit être annulée en cas d’erreur.

#### <a name="target-server"></a>Serveur cible

Nom du serveur SQL Server cible. Il peut s’agir du nom d’une instance SQL Server locale, Azure SQL Database ou Azure SQL Managed Instance.

#### <a name="authentication-type"></a>Type d'authentification

Type d’authentification utilisé pour l’accès au serveur cible spécifié. En général, les types d’authentification suivants sont pris en charge :

- Authentification Windows
- l’authentification SQL Server
- Active Directory - Authentification par mot de passe
- Active Directory - Authentification intégrée

Cependant, la prise en charge d’un type d’authentification spécifique dépend du type de serveur de destination et du type d’agent. Reportez-vous au tableau de prise en charge détaillé ci-dessous.

|Type de serveur de destination|Agent hébergé par Microsoft|Agent autohébergé|
|---------|---------|---------|
|Serveur SQL Server local ou machine virtuelle |N/A|Authentification Windows|
|Azure SQL|l’authentification SQL Server <br> Active Directory - Authentification par mot de passe|l’authentification SQL Server <br> Active Directory - Authentification par mot de passe <br> Active Directory - Authentification intégrée|

#### <a name="username"></a>Nom d’utilisateur

Nom d’utilisateur à utiliser pour l’accès au serveur SQL Server cible. Cette propriété est visible uniquement si le type d’authentification est Authentification SQL Server ou Active Directory - Authentification par mot de passe.

#### <a name="password"></a>Mot de passe

Mot de passe à utiliser pour l’accès au serveur SQL Server cible. Cette propriété est visible uniquement si le type d’authentification est Authentification SQL Server ou Active Directory - Authentification par mot de passe.

### <a name="define-configuration-json"></a>Définir le schéma JSON de configuration

Le schéma JSON de configuration comporte trois couches :

- catalogue
- dossier
- projet et environnement

![schéma de configuration du catalogue](media/catalog-configuration-schema.png)

#### <a name="a-sample-inline-configuration-json"></a>Exemple de fichier JSON de configuration inline

```json
{
  "folders": [
    {
      "name": "devopsdemo",
      "description": "devops demo folder",
      "projects": [
        {
          "name": "catalog devops",
          "parameters": [
            {
              "name": "password",
              "container": "Package.dtsx",
              "value": "passwd",
              "valueType": "referenced"
            },
            {
              "name": "serverName",
              "container": "catalog devops",
              "value": "localhost",
              "valueType": "literal"
            }
          ],
          "references": [
            {
              "environmentName": "test",
              "environmentFolder": "devopsdemo"
            },
            {
              "environmentName": "test",
              "environmentFolder": "."
            }
          ]
        }
      ],
      "environments": [
        {
          "name": "test",
          "description": "test",
          "variables": [
            {
              "name": "passwd",
              "type": "string",
              "description": "",
              "value": "$(SSISDBServerAdminPassword)",
              "sensitive": true
            },
            {
              "name": "serverName",
              "type": "string",
              "description": "",
              "value": "$(TargetServerName)",
              "sensitive": false
            }
          ]
        }
      ]
    }
  ]
}
```

#### <a name="json-schema"></a>Schéma JSON

##### <a name="catalog-attributes"></a>Attributs de catalogue

|Propriété  |Description  |Notes  |
|---------|---------|---------|
|dossiers  |Tableau d’objets dossier. Chaque objet contient des informations de configuration pour un dossier du catalogue.|Consultez *Attributs de dossier* pour voir le schéma d’un objet dossier.|

##### <a name="folder-attributes"></a>Attributs de dossier

|Property  |Description  |Notes  |
|---------|---------|---------|
|name  |Nom du dossier de catalogue.|Le dossier est créé s’il n’existe pas.|
|description|Description du dossier de catalogue.|La valeur *Null* est ignorée.|
|projects|Tableau d’objets projet. Chaque objet contient des informations de configuration pour un projet.|Consultez *Attributs de projet* pour voir le schéma d’un objet projet.|
|environments|Tableau d’objets environnement. Chaque objet contient des informations de configuration pour un environnement.|Consultez *Attributs d’environnement* pour voir le schéma d’un objet environnement.|

##### <a name="project-attributes"></a>Attributs de projet

|Property  |Description  |Notes  |
|---------|---------|---------|
|name|Nom du projet. |L’objet projet est ignoré si le projet n’existe pas dans le dossier parent.|
|parameters|Tableau d'objets de paramètre. Chaque objet contient des informations de configuration pour un paramètre.|Consultez *Attributs de paramètre* pour voir le schéma d’un objet paramètre.|
|references|Tableau d’objets référence. Chaque objet représente une référence d’environnement pour le projet cible.|Consultez *Attributs de référence* pour voir le schéma d’un objet référence.|

##### <a name="parameter-attributes"></a>Attributs de paramètre

|Property  |Description  |Notes  |
|---------|---------|---------|
|name|Nom du paramètre.|<li>Le paramètre peut être un paramètre de projet ou un paramètre de package. <li>Le paramètre est ignoré s’il n’existe pas. <li>Si le paramètre est une propriété du gestionnaire de connexions, le nom doit être au format suivant : **CM.\<Connection Manager Name>.\<Property Name>** . |
|conteneur|Conteneur du paramètre.|<li>Si le paramètre est un paramètre de projet, le *conteneur* doit être le nom du projet. <li>S’il s’agit d’un paramètre de package, le *conteneur* doit être le nom du package avec l’extension **.dtsx**.|
|value|Valeur du paramètre.|<li>Quand *valueType* est *referenced* : la valeur est une référence à une variable d’environnement avec le type *string*. <li> Quand *valueType* est *literal* : cet attribut prend en charge toutes les valeurs JSON de type *boolean*, *number* ou *string* valides. <li> La valeur est convertie au type du paramètre cible. Une erreur se produit si la valeur ne peut pas être convertie.<li> La valeur *Null* n’est pas valide. La tâche ignore cet objet paramètre et génère un avertissement.|
|valueType|Type de la valeur du paramètre.|Les types valides sont : <br> *literal* (littéral) : l’attribut *value* représente une valeur littérale. <br> *referenced* (référencé) : l’attribut *value* représente une référence à une variable d’environnement.|

##### <a name="reference-attributes"></a>Attributs de référence

|Property  |Description  |Notes  |
|---------|---------|---------|
|environmentFolder|Nom du dossier de l’environnement.|Le dossier est créé s’il n’existe pas. <br> La valeur peut être « . », qui représente le dossier parent du projet, qui référence l’environnement.|
|environmentName|Nom de l’environnement référencé.|L’environnement spécifié est créé s’il n’existe pas.|

##### <a name="environment-attributes"></a>Attributs d’environnement

|Property  |Description  |Notes  |
|---------|---------|---------|
|name|Nom de l’environnement.|L’environnement est créé s’il n’existe pas.|
|description|Description de l’environnement.|La valeur *Null* est ignorée.|
|variables|Tableau d’objets variable.|Chaque objet contient des informations de configuration pour une variable d’environnement. Consultez la section *Attributs de variable* pour voir le schéma d’un objet variable.|

##### <a name="variable-attributes"></a>Attributs de variable

|Propriété  |Description  |Notes  |
|---------|---------|---------|
|name|Nom de la variable d’environnement.|La variable d’environnement est créée si elle n’existe pas.|
|type|Type de données de la variable d’environnement.|Les types valides sont : <br> *boolean* <br> *byte* <br> *datetime* <br> Décimal <br> *double* <br> *int16* <br> *int32* <br> *int64* <br> *sbyte* <br> *single* <br> *string* <br> *uint32* <br> *uint64*|
|description|Description de la variable d’environnement.|La valeur *Null* est ignorée.|
|value|Valeur de la variable d’environnement.|Cet attribut prend en charge toutes les valeurs JSON de type boolean, number ou string valides.<br> La valeur est convertie au type spécifié par l’attribut **type**. Une erreur se produit en cas d’échec de la conversion.<br>La valeur *Null* n’est pas valide. La tâche ignore cet objet variable d’environnement et génère un avertissement.|
|sensible|Détermine si la valeur de la variable d’environnement est sensible.|Les entrées valides sont : <br> *true* <br> *false*|

## <a name="release-notes"></a>Notes de publication

### <a name="version-103"></a>Version 1.0.3

Date de publication : 21 octobre 2020

- Autorisation de la spécification du suffixe de chaîne de connexion pour la tâche de déploiement SSIS et la tâche de configuration du catalogue SSIS.

### <a name="version-102"></a>Version 1.0.2

Date de publication : 26 mai 2020

- Correction d’un problème qui peut provoquer l’échec de la tâche de configuration du catalogue SSIS dans certains cas, une fois le travail de configuration effectué.

### <a name="version-101"></a>Version 1.0.1

Date de publication : 9 mai 2020

- Correction d’un problème dans lequel la tâche de génération SSIS génère toujours l’ensemble de la solution, même si un seul fichier dtproj est spécifié en tant que chemin du projet.

### <a name="version-100"></a>Version 1.0.0

Date de publication : 8 mai 2020

- Version en disponibilité générale.
- Ajout d’une restriction de version minimale de .NET Framework sur l’agent. Actuellement, la version minimale de .NET Framework est la version 4.6.2.
- Description améliorée de la tâche de génération SSIS et de la tâche de déploiement SSIS.

### <a name="version-020-preview"></a>Version 0.2.0 - préversion

Date de publication : 31 mars 2020

- Ajout de la tâche de configuration du catalogue SSIS.

### <a name="version-013-preview"></a>Version 0.1.3 - préversion

Date de publication : 19 janvier 2020

- Correction d’un problème qui empêchait le déploiement d’ISPAC si son nom de fichier d’origine était modifié.

### <a name="version-012-preview"></a>Version 0.1.2 - préversion

Date de publication : 13 janvier 2020

- Ajout d’informations plus détaillées sur les exceptions dans le journal des tâches SSIS Deploy lorsque le type de destination est SSISDB.
- Correction de l’exemple de chemin de destination dans le texte d’aide du chemin de destination de la propriété de la tâche SSIS Deploy.

### <a name="version-011-preview"></a>Version 0.1.1 - préversion

Date de publication : 6 janvier 2020

- Ajout d’une restriction pour la version minimale requise de l’agent. Actuellement, la version minimale de l’agent de ce produit est 2.144.0.
- Correction de texte d’affichage incorrect pour la tâche SSIS Deploy.
- Amélioration de certains messages d’erreur.

### <a name="version-010-preview"></a>Version 0.1.0 - préversion

Date de publication : 5 décembre 2019

Version initiale de SSIS DevOps Tools. Il s’agit d’une version préliminaire.

## <a name="next-steps"></a>Étapes suivantes

- Obtenir l’[extension SSIS DevOps](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools)
