---
title: Informations de référence sur azdata bdc debug
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc debug.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 68efee6c692b375c98a5dabf9dc69fa58aba21a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052480"
---
# <a name="azdata-bdc-debug"></a>azdata bdc debug

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata bdc debug copy-logs](#azdata-bdc-debug-copy-logs) | Copie les journaux.
[azdata bdc debug dump](#azdata-bdc-debug-dump) | Déclenchez le vidage de la mémoire.
## <a name="azdata-bdc-debug-copy-logs"></a>azdata bdc debug copy-logs
Copiez les journaux de débogage à partir du cluster Big Data. La configuration Kubernetes est requise sur votre système.
```bash
azdata bdc debug copy-logs --namespace -ns 
                           [--container -c]  
                           
[--target-folder -d]  
                           
[--pod -p]  
                           
[--timeout -t]  
                           
[--skip-compress -sc]  
                           
[--exclude-dumps -ed]
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--namespace -ns`
Nom du cluster Big Data, utilisé pour l’espace de noms Kubernetes.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--container -c`
Copier les journaux des conteneurs avec un nom similaire. Facultatif : par défaut, copie les journaux de tous les conteneurs. Ne peut pas être spécifié plusieurs fois. S’il est spécifié plusieurs fois, c’est le dernier qui sera utilisé.
#### `--target-folder -d`
Chemin du dossier cible dans lequel copier les journaux. Facultatif : par défaut, crée le résultat dans le dossier local.  Ne peut pas être spécifié plusieurs fois. S’il est spécifié plusieurs fois, c’est le dernier qui sera utilisé.
#### `--pod -p`
Copier les journaux des pods avec un nom similaire. Facultatif : par défaut, copie les journaux de tous les pods. Ne peut pas être spécifié plusieurs fois. S’il est spécifié plusieurs fois, c’est le dernier qui sera utilisé.
#### `--timeout -t`
Nombre de secondes à attendre pour l’exécution de la commande. La valeur par défaut est 0, ce qui signifie qu’elle est illimitée.
#### `--skip-compress -sc`
Indique s’il convient ou non d’ignorer la compression du dossier de résultats. La valeur par défaut est False, ce qui compresse le dossier de résultats.
#### `--exclude-dumps -ed`
Indique s’il convient ou non d’exclure les vidages du dossier de résultats. La valeur par défaut est False, ce qui comprend les vidages.
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
## <a name="azdata-bdc-debug-dump"></a>azdata bdc debug dump
Déclenchez le vidage de la mémoire et copiez le contenu hors du conteneur. La configuration Kubernetes est requise sur votre système.
```bash
azdata bdc debug dump --namespace -ns 
                      [--container -c]  
                      
[--target-folder -d]
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--namespace -ns`
Nom du cluster Big Data, utilisé pour l’espace de noms Kubernetes.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--container -c`
Conteneur cible à déclencher pour vider les processus en cours d’exécution `controller`
#### `--target-folder -d`
Dossier cible où copier le contenu du vidage. `./output/dump`
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

