---
title: Informations de référence sur azdata bdc sql status
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc sql status.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 96344a054aad6d0e962c3ff63bda05df6bb26e70
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052330"
---
# <a name="azdata-bdc-sql-status"></a>azdata bdc sql status

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata bdc sql status show](#azdata-bdc-sql-status-show) | État du service SQL Server.
## <a name="azdata-bdc-sql-status-show"></a>azdata bdc sql status show
État du service SQL Server.
```bash
azdata bdc sql status show [--resource -r] 
                           [--all -a]
```
### <a name="examples"></a>Exemples
Obtenez l’état du service SQL.
```bash
azdata bdc sql status show
```
Obtenez l’état du service SQL avec toutes les instances.
```bash
azdata bdc sql status show --all
```
Obtenez l’état de la ressource maître au sein du service SQL.
```bash
azdata bdc sql status show --resource master
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--resource -r`
Obtenez cette ressource dans ce service.
#### `--all -a`
Affichez toutes les instances de chaque ressource dans le service.
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

