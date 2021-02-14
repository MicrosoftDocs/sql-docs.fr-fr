---
title: azdata bdc spark session reference
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes de la session azdata bdc spark.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6d48576bf62b71c66a8bbdc33766084e2aaed37f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052410"
---
# <a name="azdata-bdc-spark-session"></a>azdata bdc spark session

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata bdc spark session create](#azdata-bdc-spark-session-create) | Créez une nouvelle session Spark.
[azdata bdc spark session list](#azdata-bdc-spark-session-list) | Répertoriez toutes les sessions actives dans Spark.
[azdata bdc spark session info](#azdata-bdc-spark-session-info) | Obtenir des informations sur une session Spark active.
[azdata bdc spark session log](#azdata-bdc-spark-session-log) | Obtenir des journaux d’exécution pour une session Spark active.
[azdata bdc spark session state](#azdata-bdc-spark-session-state) | Obtenir un état d’exécution pour une session Spark active.
[azdata bdc spark session delete](#azdata-bdc-spark-session-delete) | Supprimer une session Spark.
## <a name="azdata-bdc-spark-session-create"></a>azdata bdc spark session create
Cela crée une nouvelle session Spark interactive. L’appelant doit spécifier le type de session Spark. Cette session se situe au-delà de la durée de vie d’une exécution azdata et doit être supprimée avec « supprimer session spark »
```bash
azdata bdc spark session create [--session-kind -k] 
                                [--jar-files -j]  
                                
[--py-files -p]  
                                
[--files -f]  
                                
[--driver-memory]  
                                
[--driver-cores]  
                                
[--executor-memory]  
                                
[--executor-cores]  
                                
[--executor-count]  
                                
[--archives -a]  
                                
[--queue -q]  
                                
[--name -n]  
                                
[--config -c]  
                                
[--timeout-seconds -t]
```
### <a name="examples"></a>Exemples
Créer une session.
```bash
azdata bdc spark session create --session-kind pyspark
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--session-kind -k`
Nom du type de session à créer.  L’une des spark, pyspark, sparkr ou sql.
#### `--jar-files -j`
Liste des chemins d’accès aux fichiers jar.  Pour transmettre dans une liste JSON, encodez les valeurs.  Exemple : « [« entry1 », « entry2 »] ».
#### `--py-files -p`
Liste des chemins d’accès de fichier Python.  Pour transmettre dans une liste JSON, encodez les valeurs.  Exemple : « [« entry1 », « entry2 »] ».
#### `--files -f`
Liste des chemins d’accès de fichier.  Pour transmettre dans une liste JSON, encodez les valeurs.  Exemple : « [« entry1 », « entry2 »] ».
#### `--driver-memory`
Quantité de mémoire à allouer au pilote.  Spécifiez les unités dans le cadre de la valeur.  Exemple 512 M ou 2 G.
#### `--driver-cores`
Quantité de cœurs UC à allouer au pilote.
#### `--executor-memory`
Quantité de mémoire à allouer à l’exécuteur.  Spécifiez les unités dans le cadre de la valeur.  Exemple 512 M ou 2 G.
#### `--executor-cores`
Quantité de cœurs UC à allouer à l’exécuteur.
#### `--executor-count`
Nombre d'instances de l’exécuteur à exécuter.
#### `--archives -a`
Liste des chemins d’accès à l’archive.  Pour transmettre dans une liste JSON, encodez les valeurs.  Exemple : « [« entry1 », « entry2 »] ».
#### `--queue -q`
Nom de la file d’attente Spark dans laquelle exécuter la session.
#### `--name -n`
Nom de la session Spark.
#### `--config -c`
Liste de paires nom/valeur contenant des valeurs de configuration Spark.  Encodé en tant que dictionnaire JSON.  Exemple : « {"name":"value", "name2":"value2"} ».
#### `--timeout-seconds -t`
Délai d’inactivité de la session en secondes.
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
## <a name="azdata-bdc-spark-session-list"></a>azdata bdc spark session list
Répertoriez toutes les sessions actives dans Spark.
```bash
azdata bdc spark session list 
```
### <a name="examples"></a>Exemples
Répertoriez toutes les sessions actives.
```bash
azdata bdc spark session list
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
## <a name="azdata-bdc-spark-session-info"></a>azdata bdc spark session info
Cela permet d’obtenir les informations de session pour une session Spark active avec l’ID donné.  L’ID de session est retourné à partir de « création de session spark ».
```bash
azdata bdc spark session info --session-id -i 
                              
```
### <a name="examples"></a>Exemples
Obtenir les informations de session pour la session avec l’ID 0.
```bash
azdata bdc spark session info --session-id 0
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
## <a name="azdata-bdc-spark-session-log"></a>azdata bdc spark session log
Cela permet d’obtenir les entrées de journal de sessions pour une session Spark active avec l’ID donné.  L’ID de session est retourné à partir de « création de session spark ».
```bash
azdata bdc spark session log --session-id -i 
                             
```
### <a name="examples"></a>Exemples
Obtenir le journal de sessions pour la session avec l’ID 0.
```bash
azdata bdc spark session log --session-id 0
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
## <a name="azdata-bdc-spark-session-state"></a>azdata bdc spark session state
Cela permet d’obtenir l’état de session pour une session Spark active avec l’ID donné.  L’ID de session est retourné à partir de « création de session spark ».
```bash
azdata bdc spark session state --session-id -i 
                               
```
### <a name="examples"></a>Exemples
Obtenir l’état de session pour la session avec l’ID 0.
```bash
azdata bdc spark session state --session-id 0
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
## <a name="azdata-bdc-spark-session-delete"></a>azdata bdc spark session delete
Cela supprime une session Spark interactive. L’ID de session est retourné à partir de « création de session spark ».
```bash
azdata bdc spark session delete --session-id -i 
                                
```
### <a name="examples"></a>Exemples
Supprimer une session.
```bash
azdata bdc spark session delete --session-id 0
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

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les autres commandes **azdata**, consultez [azdata reference](reference-azdata.md). 

Pour plus d’informations sur l’installation de l’outil **azdata**, consultez [Installer azdata](..\install\deploy-install-azdata.md).

