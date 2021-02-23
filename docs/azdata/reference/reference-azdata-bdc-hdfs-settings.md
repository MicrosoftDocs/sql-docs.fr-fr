---
title: Informations de référence sur azdata bdc hdfs settings
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc hdfs settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f88bd1dc16f7d91ce4611820c40ff0f033c193ab
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567294"
---
# <a name="azdata-bdc-hdfs-settings"></a>azdata bdc hdfs settings

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **hdfs settings** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes
|Commande|Description|
| --- | --- |
[azdata bdc hdfs settings set](#azdata-bdc-hdfs-settings-set) | Définie les paramètres d’étendue du service hdfs.
[azdata bdc hdfs settings show](#azdata-bdc-hdfs-settings-show) | Affiche les paramètres d’étendue du service hdfs et éventuellement les paramètres hdfs des ressources spécifiées.

## <a name="azdata-bdc-hdfs-settings-set"></a>azdata bdc hdfs settings set
Offre la possibilité de définir un paramètre qui s’étend au service ou à la ressource. Spécifiez le nom complet du paramètre et la valeur. N’applique pas le paramètre au cluster Big Data en cours d’exécution. Pour ce faire, exécutez la mise à niveau.
```bash
azdata bdc hdfs settings set --settings -s 
                        
```
### <a name="examples"></a>Exemples
Désactiver le volume lu pour le service HDFS 
```bash 
azdata bdc hdfs settings set --settings hdfs-site dfs.datanode.provided.volume.readthrough=false 
``` 
Définir le facteur de réplication pour la ressource du pool de stockage
```bash 
azdata bdc hdfs settings set --settings hdfs-site.dfs.replication=3 –resources storage-0 
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

## <a name="azdata-bdc-hdfs-settings-show"></a>azdata bdc hdfs settings show
Affiche les paramètres d’étendue du service `hdfs` (éventuellement d’étendue des ressources) du cluster Big Data. Par défaut, cette commande affiche les paramètres d’étendue du service configurés par l’utilisateur. Des filtres sont disponibles pour afficher tous les paramètres (gérés par le système et configurables), les paramètres configurables ou les paramètres en attente. Pour voir un paramètre d’étendue du service ou d’étendue de la ressource spécifique, vous pouvez fournir le nom du paramètre. Utilisez « recursive » pour afficher les paramètres de toutes les ressources dans le cadre du service. 
```bash

azdata bdc hdfs settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Exemples
Affichez les paramètres d’étendue du service HDFS configurés par l’utilisateur. 
```bash
azdata bdc hdfs settings show
```
Affichez le facteur de réplication pour HDFS dans le pool de stockage.
```bash
azdata bdc hdfs settings show --settings hdfs-site.dfs.replication --resources storage-0 
```
Affichez les modifications de paramètres en attente pour l’étendue du service HDFS et l’étendue des ressources. 
```bash
azdata bdc hdfs settings show --filter-options=pending --recursive --include-details
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
