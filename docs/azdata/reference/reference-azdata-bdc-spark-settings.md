---
title: Informations de référence sur azdata bdc spark settings
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc spark settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 66c80bc7d77ebc1f1f5c18e5fe972e4c6d61419d
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567298"
---
# <a name="azdata-bdc-spark-settings"></a>azdata bdc spark settings

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **spark settings** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes
|Commande|Description|
| --- | --- |
[azdata bdc spark settings set](#azdata-bdc-spark-settings-set) | Définissez les paramètres d’étendue du service Spark.
[azdata bdc spark settings show](#azdata-bdc-spark-settings-show) | Affichez les paramètres d’étendue du service Spark et éventuellement les paramètres Spark des ressources spécifiées.

## <a name="azdata-bdc-spark-settings-set"></a>azdata bdc spark settings set
Offre la possibilité de définir un paramètre qui s’étend au service ou à la ressource. Spécifiez le nom complet du paramètre et la valeur. N’applique pas le paramètre au cluster Big Data en cours d’exécution. Pour ce faire, exécutez la mise à niveau.
```bash
azdata bdc spark settings set --settings -s 
                        
```
### <a name="examples"></a>Exemples
Définissez le nombre de cœurs de pilotes par défaut sur 1 et la mémoire du pilote sur 1664m pour le service Spark. 
```bash 
azdata bdc spark settings set --settings spark-defaults-conf.spark.driver.cores=1,spark-defaults-conf.spark.driver.memory=1664m 
``` 
Définissez le nombre de cœurs Exécuteur par défaut sur 1 pour le pool de stockage. 
```bash 
azdata bdc spark  settings set --settings spark-defaults-conf.spark.executor.cores=1 –resources storage-0 
``` 

### <a name="required-parameters"></a>Paramètres obligatoires
#### `--settings -s`
Définit la valeur configurée pour les paramètres fournis. Plusieurs paramètres peuvent être définis à l’aide d’une liste séparée par des virgules.
### <a name="optional-parameters"></a>Paramètres facultatifs 
#### `--resources` 
Définit le ou les paramètres donnés pour la ou les ressources fournies. Les ressources peuvent être listées sous la forme d’une liste séparée par des virgules. 

### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.

## <a name="azdata-bdc-spark-settings-show"></a>azdata bdc spark settings show
Affiche les paramètres d’étendue du service `spark` (éventuellement d’étendue des ressources) du cluster Big Data. Par défaut, cette commande affiche les paramètres d’étendue du service configurés par l’utilisateur. Des filtres sont disponibles pour afficher tous les paramètres (gérés par le système et configurables), les paramètres configurables ou les paramètres en attente. Pour voir un paramètre d’étendue du service ou d’étendue de la ressource spécifique, vous pouvez fournir le nom du paramètre. Utilisez « recursive » pour afficher les paramètres de toutes les ressources dans le cadre du service. 
```bash

azdata bdc spark settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Exemples
Affichez les paramètres d’étendue du service Spark configurés par l’utilisateur.
```bash
azdata bdc spark settings show
```
Affichez la valeur en cours d’exécution et configurée des cœurs de pilotes Spark dans le pool de stockage. 
```bash
azdata bdc spark settings show --settings spark-defaults-conf.spark.driver.cores --resources storage-0 --include-details
```
Affichez tous les paramètres liés à la mémoire configurable pour le service Spark.
```bash
azdata bdc spark settings show --settings *memory* --resources storage-0 
```
Affichez les modifications de paramètres en attente pour l’étendue du service Spark et l’étendue des ressources.
```bash
azdata bdc spark settings show --filter-options=pending --recursive --include-details
```
### <a name="optional-parameters"></a>Paramètres facultatifs 
#### `--filter-options | -f` 
Les options permettant de filtrer les paramètres de niveau de service ou d’étendue des ressources sont affichés, au lieu des seuls paramètres configurés par l’utilisateur. Des filtres sont disponibles pour afficher tous les paramètres (gérés par le système et configurables par l’utilisateur), tous les paramètres configurables ou les paramètres en attente. Options : `userConfigured`, `all`, `pending`, `configurable`.
#### `--settings | -s` 
Affiche des informations sur les noms des paramètres spécifiés. 
#### `--include-details | -i` 
Inclut des détails supplémentaires pour les paramètres choisis pour l’affichage. 
#### `--description | -d` 
Inclut la description du paramètre. Doit être utilisé avec --include-details. 
#### `--resources | -r` 
Affiche les informations sur les paramètres des ressources spécifiées. Les ressources peuvent être listées sous la forme d’une liste séparée par des virgules. 
#### `--recursive | -rec` 
Affiche les informations de paramètre pour l’étendue donnée (service ou ressource de service) ainsi que tous les composants d’étendue inférieure (ressources). 

### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmentez le niveau de détail de la journalisation pour afficher tous les journaux de débogage.
#### `--help -h`
Affichez ce message d’aide et quittez.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Valeur par défaut : json.
#### `--query -q`
Chaîne de requêtes JMESPath. Pour obtenir plus d’informations et des exemples, consultez [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Augmentez le niveau de détail de la journalisation. Utilisez --debug pour des journaux de débogage complets.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les autres commandes **azdata**, consultez [azdata reference](reference-azdata.md). 

Pour plus d’informations sur l’installation de l’outil **azdata**, consultez [Installer azdata pour gérer les clusters Big Data SQL Server 2019](../install/deploy-install-azdata.md).
