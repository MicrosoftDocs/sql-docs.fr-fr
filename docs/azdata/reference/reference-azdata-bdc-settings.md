---
title: Informations de référence sur azdata bdc settings
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 37688a962fc17679a1a642af1b83609d2b8f480a
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342490"
---
# <a name="azdata-bdc-settings"></a>azdata bdc settings

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes
|Commande|Description|
| --- | --- |
[azdata bdc settings set](#azdata-bdc-settings-set) | Définit les paramètres d’étendue du cluster.
[azdata bdc settings apply](#azdata-bdc-settings-apply) | Applique les changements de paramètres en attente au BDC.
[azdata bdc settings cancel-apply](#azdata-bdc-settings-cancel-apply) | Annule l’application des paramètres du BDC.
[azdata bdc settings show](#azdata-bdc-settings-show) | Affiche les paramètres d’étendue du cluster ou tous les paramètres de cluster avec --recursive.
[azdata bdc settings revert](#azdata-bdc-settings-revert) | Restaure les changements de paramètres en attente dans le BDC dans toutes les étendues.
## <a name="azdata-bdc-settings-set"></a>azdata bdc settings set
Définit un paramètre d’étendue du cluster. Spécifiez le nom complet du paramètre et la valeur. Exécutez apply pour appliquer le paramètre et mettre à jour les paramètres BDC.
```bash
azdata bdc settings set --settings -s 
                        
```
### <a name="examples"></a>Exemples
Définissez la valeur par défaut du cluster pour « bdc.telemetry.customerFeedback ».
```bash
azdata bdc settings set --settings bdc.telemetry.customerFeedback=false
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--settings -s`
Définit la valeur configurée pour les paramètres fournis. Plusieurs paramètres peuvent être définis à l’aide d’une liste séparée par des virgules.
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
## <a name="azdata-bdc-settings-apply"></a>azdata bdc settings apply
Applique les changements de paramètres en attente au BDC.
```bash
azdata bdc settings apply [--force -f] 
                          
```
### <a name="examples"></a>Exemples
Appliquez les changements de paramètres en attente au BDC.
```bash
azdata bdc settings apply
```
Application forcée. L’utilisateur ne sera pas invité à confirmer quoi que ce soit, et les éventuels problèmes seront affichés dans le cadre d’une erreur standard.
```bash
azdata bdc settings apply --force
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--force -f`
Application forcée. L’utilisateur ne sera pas invité à confirmer quoi que ce soit, et les éventuels problèmes seront affichés dans le cadre d’une erreur standard.
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
## <a name="azdata-bdc-settings-cancel-apply"></a>azdata bdc settings cancel-apply
En cas de défaillance pendant l’application des paramètres, restaure le cluster Big Data à son dernier état d’exécution. Cette commande est une non-opération si elle est appliquée à un cluster en cours d’exécution. Les changements de paramètres précédemment en attente seront toujours en attente après l’annulation.
```bash
azdata bdc settings cancel-apply [--force -f] 
                                 
```
### <a name="examples"></a>Exemples
Annulez l’application des paramètres du BDC.
```bash
azdata bdc settings cancel-apply
```
Annulation forcée. L’utilisateur ne sera pas invité à confirmer quoi que ce soit, et les éventuels problèmes seront affichés dans le cadre d’une erreur standard.
```bash
azdata bdc settings cancel-apply --force
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--force -f`
Annulation forcée. L’utilisateur ne sera pas invité à confirmer quoi que ce soit, et les éventuels problèmes seront affichés dans le cadre d’une erreur standard.
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
## <a name="azdata-bdc-settings-show"></a>azdata bdc settings show
Affiche les paramètres au niveau du cluster du BDC. Par défaut, cette commande affiche user-configured cluster-scope settings. D’autres filtres sont disponibles pour afficher tous les paramètres (gérés par le système et configurables par l’utilisateur et hérités), tous les paramètres configurables ou les paramètres en attente. Pour voir un paramètre d’étendue du cluster, vous pouvez fournir le nom du paramètre. Si vous souhaitez voir les paramètres de toutes les étendues (cluster, service et ressource), vous pouvez spécifier « récursive ».
```bash
azdata bdc settings show [--settings -s] 
                         [--filter-option -f]  
                         
[--recursive -rec]  
                         
[--include-details -i]  
                         
[--description -d]
```
### <a name="examples"></a>Exemples
Indiquez si la collecte de la télémétrie BDC est activée.
```bash
azdata bdc settings show --settings bdc.telemetry.customerFeedback
```
Affichez les paramètres configurés par l’utilisateur dans le BDC dans toutes les étendues.
```bash
azdata bdc settings show --recursive
```
Affichez tous les paramètres en attente dans le BDC dans toutes les étendues.
```bash
azdata bdc settings show –filter-option=pending --recursive
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--settings -s`
Affiche des informations sur les noms des paramètres spécifiés.
#### `--filter-option -f`
Filtrez sur les paramètres de l’étendue du cluster affichés, plutôt que sur les paramètres « Configurés par l’utilisateur » uniquement. Des filtres sont disponibles pour afficher tous les paramètres (gérés par le système et configurables par l’utilisateur), tous les paramètres configurables ou les paramètres en attente.
`userConfigured`
#### `--recursive -rec`
Affiche les informations de paramètres pour l’étendue du cluster et tous les composants d’étendue inférieure (services, ressources).
#### `--include-details -i`
Inclut des détails supplémentaires pour les paramètres choisis pour être affichés.
#### `--description -d`
Inclut une description du paramètre.
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
## <a name="azdata-bdc-settings-revert"></a>azdata bdc settings revert
Restaure les changements de paramètres en attente dans le BDC dans toutes les étendues.
```bash
azdata bdc settings revert [--force -f] 
                           
```
### <a name="examples"></a>Exemples
Rétablissez les changements de paramètres en attente du BDC dans toutes les étendues.
```bash
azdata bdc settings revert
```
Rétablissement forcé. L’utilisateur ne sera pas invité à confirmer quoi que ce soit, et les éventuels problèmes seront affichés dans le cadre d’une erreur standard.
```bash
azdata bdc settings revert --force
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--force -f`
Rétablissement forcé. L’utilisateur ne sera pas invité à confirmer quoi que ce soit, et les éventuels problèmes seront affichés dans le cadre d’une erreur standard.
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

Pour plus d’informations sur l’installation de l’outil **azdata**, consultez [Installer azdata](..\install\deploy-install-azdata.md).
