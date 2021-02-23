---
title: Informations de référence sur azdata arc sql endpoint
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata arc sql endpoint.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 190bc4360d8f8cd35f0ceda04887a1fc912243e3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342534"
---
# <a name="azdata-arc-sql-endpoint"></a>azdata arc sql endpoint

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes
|Commande|Description|
| --- | --- |
[azdata arc sql endpoint list](#azdata-arc-sql-endpoint-list) | Liste les points de terminaison SQL.
## <a name="azdata-arc-sql-endpoint-list"></a>azdata arc sql endpoint list
Liste les points de terminaison SQL.
```bash
azdata arc sql endpoint list [--name -n] 
                             
```
### <a name="examples"></a>Exemples
Listez les points de terminaison d’une instance gérée SQL.
```bash
azdata arc sql endpoint list -n sqlmi1
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--name -n`
Nom de l’instance SQL à afficher. En cas d’omission, tous les points de terminaison de toutes les instances s’affichent.
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
