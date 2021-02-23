---
title: Informations de référence sur azdata bdc hdfs encryption-zone
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc hdfs encryption-zone.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c094d489cb925e280f5a44a8234d70fc817554b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342518"
---
# <a name="azdata-bdc-hdfs-encryption-zone"></a>azdata bdc hdfs encryption-zone

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes
|Commande|Description|
| --- | --- |
[azdata bdc hdfs encryption-zone create](#azdata-bdc-hdfs-encryption-zone-create) | Convertit un dossier HDFS en zone de chiffrement.
[azdata bdc hdfs encryption-zone list](#azdata-bdc-hdfs-encryption-zone-list) | Liste toutes les zones de chiffrement et leurs clés associées.
[azdata bdc hdfs encryption-zone get-file-encryption-info](#azdata-bdc-hdfs-encryption-zone-get-file-encryption-info) | Obtient les informations de chiffrement d’un chemin de fichier HDFS donné.
[azdata bdc hdfs encryption-zone status](#azdata-bdc-hdfs-encryption-zone-status) | Obtient l’état de rechiffrement du cluster HDFS.
[azdata bdc hdfs encryption-zone reencrypt](#azdata-bdc-hdfs-encryption-zone-reencrypt) | Contrôle le rechiffrement de la zone de chiffrement spécifiée par l’argument --path.
## <a name="azdata-bdc-hdfs-encryption-zone-create"></a>azdata bdc hdfs encryption-zone create
Convertit un dossier HDFS en zone de chiffrement à l’aide de la clé fournie pour chiffrer les fichiers dans la zone de chiffrement.
```bash
azdata bdc hdfs encryption-zone create --path -p 
                                       --keyname -k
```
### <a name="examples"></a>Exemples
Pour convertir un dossier existant /user/securefolder en zone de chiffrement, à l’aide d’une clé appelée securelake :
```bash
azdata bdc hdfs encryption-zone create --path /home/securefolder --keyname securelake
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin du dossier HDFS à convertir en zone de chiffrement.
#### `--keyname -k`
Clé Hadoop KMS à utiliser pour protéger la zone de chiffrement.
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
## <a name="azdata-bdc-hdfs-encryption-zone-list"></a>azdata bdc hdfs encryption-zone list
Liste toutes les zones de chiffrement et leurs clés associées.
```bash
azdata bdc hdfs encryption-zone list 
```
### <a name="examples"></a>Exemples
Listez toutes les zones de chiffrement.
```bash
azdata bdc hdfs encryption-zone list
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
## <a name="azdata-bdc-hdfs-encryption-zone-get-file-encryption-info"></a>azdata bdc hdfs encryption-zone get-file-encryption-info
Obtient les informations de chiffrement d’un chemin de fichier HDFS donné.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path -p 
                                                         
```
### <a name="examples"></a>Exemples
Obtenez les informations de chiffrement d’un fichier /user/securefolder/data.csv.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path /user/securefolder/data.csv
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin du fichier HDFS pour lequel les informations de chiffrement doivent être récupérées.
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
## <a name="azdata-bdc-hdfs-encryption-zone-status"></a>azdata bdc hdfs encryption-zone status
Obtient l’état de rechiffrement du cluster HDFS.
```bash
azdata bdc hdfs encryption-zone status 
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
## <a name="azdata-bdc-hdfs-encryption-zone-reencrypt"></a>azdata bdc hdfs encryption-zone reencrypt
Contrôle le rechiffrement de la zone de chiffrement spécifiée par l’argument --path.
```bash
azdata bdc hdfs encryption-zone reencrypt --path -p 
                                          --action -a
```
### <a name="examples"></a>Exemples
Démarrez le rechiffrement de la zone de chiffrement securelake.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
```
Annulez le rechiffrement de la zone de chiffrement securelake.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action cancel
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin du dossier de la zone de chiffrement HDFS à rechiffrer.
#### `--action -a`
Action de rechiffrement à effectuer. Les valeurs valides sont start et cancel.
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
