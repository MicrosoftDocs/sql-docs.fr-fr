---
title: Informations de référence sur azdata bdc hdfs key
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc hdfs key.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c208d1c726bae41b349abdf475627afbc82ed2b8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342505"
---
# <a name="azdata-bdc-hdfs-key"></a>azdata bdc hdfs key

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes
|Commande|Description|
| --- | --- |
[azdata bdc hdfs key create](#azdata-bdc-hdfs-key-create) | Crée une clé HDFS.
[azdata bdc hdfs key list](#azdata-bdc-hdfs-key-list) | Liste toutes les clés de zone de chiffrement Hadoop.
[azdata bdc hdfs key roll](#azdata-bdc-hdfs-key-roll) | Substitue une clé HDFS.
## <a name="azdata-bdc-hdfs-key-create"></a>azdata bdc hdfs key create
Créez une clé HDFS avec le nom donné et de la taille donnée.
```bash
azdata bdc hdfs key create --name -n 
                           [--size -size]
```
### <a name="examples"></a>Exemples
Pour créer une clé 256 bits dont le nom est Key1 : azdata hdfs key create --name key1 --size 256
```bash
azdata hdfs key create --name key1 --size 256
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de la clé de zone de chiffrement Hadoop. 
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--size -size`
Longueur en bits de la clé de chiffrement Hadoop. La longueur par défaut est 256 bits.
`256`
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
## <a name="azdata-bdc-hdfs-key-list"></a>azdata bdc hdfs key list
Liste toutes les clés de zone de chiffrement Hadoop.
```bash
azdata bdc hdfs key list 
```
### <a name="examples"></a>Exemples
Pour lister toutes les clés de zone de chiffrement Hadoop : azdata bdc hdfs key list
```bash
azdata bdc hdfs key list
```
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
## <a name="azdata-bdc-hdfs-key-roll"></a>azdata bdc hdfs key roll
Substitue une clé HDFS avec le nom donné.
```bash
azdata bdc hdfs key roll --name -n 
                         
```
### <a name="examples"></a>Exemples
Pour substituer une clé portant le nom Key1 : azdata hdfs key roll --name key1
```bash
azdata hdfs key roll --name key1
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de la clé de zone de chiffrement à substituer à une nouvelle version. 
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
