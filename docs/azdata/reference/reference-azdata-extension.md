---
title: Référence des extensions azdata
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes d’extension azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 45432b3e1c8dda5835b13c2775035382694713ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052280"
---
# <a name="azdata-extension"></a>Extension azdata

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata extension add](#azdata-extension-add) | Ajouter une extension.
[azdata extension remove](#azdata-extension-remove) | Supprimer une extension.
[azdata extension list](#azdata-extension-list) | Lister toutes les extensions installées.
## <a name="azdata-extension-add"></a>azdata extension add
Ajouter une extension.
```bash
azdata extension add --source -s 
                     [--index]  
                     
[--pip-proxy]  
                     
[--pip-extra-index-urls]  
                     
[--yes -y]
```
### <a name="examples"></a>Exemples
Ajouter une extension à partir d’une URL.
```bash
azdata extension add --source https://contoso.com/some_ext-0.0.1-py2.py3-none-any.whl```
Add extension from local disk.
```bash
azdata extension add --source ~/some_ext-0.0.1-py2.py3-none-any.whl```
Add extension from local disk and use pip proxy for dependencies.
```bash
azdata extension add --source ~/some_ext-0.0.1-py2.py3-none-any.whl --pip-proxy https://user:pass@proxy.server:8080
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--source -s`
Chemin d’une roue d’extension sur le disque ou URL d’une extension.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--index`
URL de base de l’index de package Python (https://pypi.org/simple) par défaut), qui doit pointer vers un référentiel conforme à PEP 503 (l’API de référentiel simple) ou un répertoire local du même format.
#### `--pip-proxy`
Proxy pour pip à utiliser pour les dépendances d’extension sous la forme [user:passwd@]proxy.server:port.
#### `--pip-extra-index-urls`
Liste séparée par des espaces d’URL supplémentaires d’index de package à utiliser, qui doit pointer vers un référentiel conforme à PEP 503 (l’API de référentiel simple) ou un répertoire local du même format.
#### `--yes -y`
Ne pas demander de confirmation.
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
## <a name="azdata-extension-remove"></a>azdata extension remove
Supprimer une extension.
```bash
azdata extension remove --name -n 
                        [--yes -y]
```
### <a name="examples"></a>Exemples
Supprimer une extension.
```bash
azdata extension remove --name some-ext
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l’extension.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--yes -y`
Ne pas demander de confirmation.
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
## <a name="azdata-extension-list"></a>azdata extension list
Lister toutes les extensions installées.
```bash
azdata extension list 
```
### <a name="examples"></a>Exemples
Lister les extensions.
```bash
azdata extension list
```
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

