---
title: Informations de référence sur azdata bdc spark statement
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc spark statement.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3e43dcb6cc5bb28876179bcd2d2da25ae9ac4a2f
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87243485"
---
# <a name="azdata-bdc-spark-statement"></a>azdata bdc spark statement

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

L’article suivant fournit des références sur les commandes `sql` disponibles dans l’outil `azdata`. Pour plus d’informations sur les autres commandes `azdata`, consultez [Informations de référence sur azdata](reference-azdata.md).

## <a name="commands"></a>Commandes
| Commande | Description |
| --- | --- |
[azdata bdc spark statement list](#azdata-bdc-spark-statement-list) | Liste toutes les instructions d’une session Spark donnée.
[azdata bdc spark statement create](#azdata-bdc-spark-statement-create) | Crée une nouvelle instruction Spark dans la session donnée.
[azdata bdc spark statement info](#azdata-bdc-spark-statement-info) | Obtient des informations sur l’instruction demandée dans une session Spark donnée.
[azdata bdc spark statement cancel](#azdata-bdc-spark-statement-cancel) | Annule une instruction dans une session Spark donnée.
## <a name="azdata-bdc-spark-statement-list"></a>azdata bdc spark statement list
Liste toutes les instructions d’une session Spark donnée.
```bash
azdata bdc spark statement list --session-id -i 
                                
```
### <a name="examples"></a>Exemples
Liste toutes les instructions d’une session.
```bash
azdata spark statement list --session-id 0
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--session-id -i`
Numéro d’ID de session Spark.
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
## <a name="azdata-bdc-spark-statement-create"></a>azdata bdc spark statement create
Crée et exécute une nouvelle instruction dans une session donnée.  Si l’instruction EXECUTE est rapide, le résultat contient la sortie de l’exécution.  Sinon, le résultat peut être récupéré à l’aide de « spark session info » une fois l’instruction terminée.
```bash
azdata bdc spark statement create --session-id -i 
                                  --code -c
```
### <a name="examples"></a>Exemples
Exécute une instruction.
```bash
azdata spark statement create --session-id 0 --code "2+2"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--session-id -i`
Numéro d’ID de session Spark.
#### `--code -c`
Chaîne contenant le code à exécuter dans le cadre de l’instruction.
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
## <a name="azdata-bdc-spark-statement-info"></a>azdata bdc spark statement info
Permet d’obtenir l’état de l’exécution et le résultat de l’exécution si l’instruction est terminée. L’ID d’instruction est retourné à partir de « spark statement create ».
```bash
azdata bdc spark statement info --session-id -i 
                                --statement-id -s
```
### <a name="examples"></a>Exemples
Obtient des informations sur les instructions d’une session avec un ID de 0 et un ID d’instruction de 0.
```bash
azdata spark statement info --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--session-id -i`
Numéro d’ID de session Spark.
#### `--statement-id -s`
ID de l’instruction Spark dans l’ID de session donné.
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
## <a name="azdata-bdc-spark-statement-cancel"></a>azdata bdc spark statement cancel
Annule une instruction dans une session Spark donnée. L’ID d’instruction est retourné à partir de « spark statement create ».
```bash
azdata bdc spark statement cancel --session-id -i 
                                  --statement-id -s
```
### <a name="examples"></a>Exemples
Annule une instruction.
```bash
azdata spark statement cancel --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--session-id -i`
Numéro d’ID de session Spark.
#### `--statement-id -s`
ID de l’instruction Spark dans l’ID de session donné.
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

Pour plus d’informations sur les autres commandes `azdata`, consultez [Informations de référence sur azdata](reference-azdata.md). Pour plus d’informations sur l’installation de l’outil `azdata`, consultez [Installer azdata pour gérer les clusters Big Data SQL Server 2019](deploy-install-azdata.md).
