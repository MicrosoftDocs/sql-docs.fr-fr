---
title: azdata app reference
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes de l’application azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d2278059053366fb89e8ecd5a7cd626980bec637
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052710"
---
# <a name="azdata-app"></a>azdata app

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata app template](reference-azdata-app-template.md) | Modèles.
[azdata app init](#azdata-app-init) | Lancez un nouveau squelette d’applications.
[azdata app create](#azdata-app-create) | Créez une application.
[azdata app update](#azdata-app-update) | Mettez à jour une application.
[azdata app list](#azdata-app-list) | Répertoriez une ou des applications.
[azdata app delete](#azdata-app-delete) | Supprimez une application.
[azdata app run](#azdata-app-run) | Exécutez une application.
[azdata app describe](#azdata-app-describe) | Décrivez une application.
## <a name="azdata-app-init"></a>azdata app init
Vous aide à lancer un nouveau squelette et/ou des fichiers de spécification d’applications basés sur des environnements runtime.
```bash
azdata app init [--spec -s] 
                [--name -n]  
                
[--version -v]  
                
[--template -t]  
                
[--destination -d]  
                
[--url -u]
```
### <a name="examples"></a>Exemples
Générer un modèle automatique d’une nouvelle `spec.yaml` d’applications uniquement.
```bash
azdata app init --spec
```
Générer un modèle automatique d’un nouveau squelette d’applications R basé sur le modèle `r`.
```bash
azdata app init --name reduce --template r
```
Générer un modèle automatique d’un nouveau squelette d’applications Python basé sur le modèle `python`.
```bash
azdata app init --name reduce --template python
```
Générer un modèle automatique d’un nouveau squelette d’applications SSIS basé sur le modèle `ssis`.
```bash
azdata app init --name reduce --template ssis            
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--spec -s`
Générez simplement une application spec.yaml.
#### `--name -n`
Nom de l’application.
#### `--version -v`
Version de l'application.
#### `--template -t`
Nom du modèle. Pour obtenir la liste complète des noms de modèles pris en charge, exécuter `azdata app template list`
#### `--destination -d`
Où placer le squelette d’applications. Par défaut : répertoire de travail actuel.
#### `--url -u`
Spécifiez un autre emplacement du référentiel de modèles. Valeur par défaut : https://github.com/Microsoft/SQLBDC-AppDeploy.git
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.
## <a name="azdata-app-create"></a>azdata app create
Créez une application.
```bash
azdata app create --spec -s 
                  
```
### <a name="examples"></a>Exemples
Créez une nouvelle application à partir d’un répertoire contenant une spécification de déploiement spec.yaml valide.
```bash
azdata app create --spec /path/to/dir/with/spec/yaml
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--spec -s`
Chemin d’accès à un répertoire contenant un fichier de spécification YAML décrivant l’application.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.
## <a name="azdata-app-update"></a>azdata app update
Mettez à jour une application.
```bash
azdata app update [--spec -s] 
                  [--yes -y]
```
### <a name="examples"></a>Exemples
Mettez à jour une application existante à partir d’un répertoire contenant une spécification de déploiement spec.yaml valide.
```bash
azdata app update --spec /path/to/dir/with/spec/yaml    
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--spec -s`
Chemin d’accès à un répertoire contenant un fichier de spécification YAML décrivant l’application.
#### `--yes -y`
Ne demandez pas de confirmation lors de la mise à jour d’une application à partir du fichier spec.yalm de CWD.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.
## <a name="azdata-app-list"></a>azdata app list
Répertoriez une ou des applications.
```bash
azdata app list [--name -n] 
                [--version -v]
```
### <a name="examples"></a>Exemples
Répertoriez les applications par nom et par version.
```bash
azdata app list --name reduce  --version v1
```
Répertoriez toutes les versions d’applications par nom.
```bash
azdata app list --name reduce
```
Répertoriez toutes les versions d’applications par nom.
```bash
azdata app list
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--name -n`
Nom de l’application.
#### `--version -v`
Version de l'application.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.
## <a name="azdata-app-delete"></a>azdata app delete
Supprimez une application.
```bash
azdata app delete --name -n 
                  --version -v
```
### <a name="examples"></a>Exemples
Supprimez les applications par nom et par version.
```bash
azdata app delete --name reduce --version v1    
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l’application.
#### `--version -v`
Version de l'application.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.
## <a name="azdata-app-run"></a>azdata app run
Exécutez une application.
```bash
azdata app run --name -n 
               --version -v  
               
[--inputs]
```
### <a name="examples"></a>Exemples
Exécutez l’application sans paramètres d’entrée.
```bash
azdata app run --name reduce --version v1
```
Exécutez l’application avec 1 paramètre d’entrée.
```bash
azdata app run --name reduce --version v1 --inputs x=10
```
Exécutez l’application avec plusieurs paramètres d’entrée.
```bash
azdata app run --name reduce --version v1 --inputs x=10,y5.6    
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l’application.
#### `--version -v`
Version de l'application.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--inputs`
Paramètres d’entrée de l’application au format CSV `name=value`.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.
## <a name="azdata-app-describe"></a>azdata app describe
Décrivez une application.
```bash
azdata app describe [--spec -s] 
                    [--name -n]  
                    
[--version -v]
```
### <a name="examples"></a>Exemples
Décrivez l’application.
```bash
azdata app describe --name reduce --version v1    
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--spec -s`
Chemin d’accès à un répertoire contenant un fichier de spécification YAML décrivant l’application.
#### `--name -n`
Nom de l’application.
#### `--version -v`
Version de l'application.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les autres commandes **azdata**, consultez [azdata reference](reference-azdata.md). 

Pour plus d’informations sur l’installation de l’outil **azdata**, consultez [Installer azdata](..\install\deploy-install-azdata.md).

