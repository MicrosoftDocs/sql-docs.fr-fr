---
title: Informations de référence sur azdata arc sql mi config
titleSuffix: SQL Server big data clusters
description: Article d’informations de référence sur les commandes azdata arc sql mi config.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1913f0754001f69436b02f465711ac45245c504c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052570"
---
# <a name="azdata-arc-sql-mi-config"></a>azdata arc sql mi config

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata arc sql mi config init](#azdata-arc-sql-mi-config-init) | Initialise les fichiers CRD et de spécification pour une instance gérée SQL.
[azdata arc sql mi config add](#azdata-arc-sql-mi-config-add) | Ajoutez une valeur pour un chemin d’accès json dans un fichier de configuration.
[azdata arc sql mi config remove](#azdata-arc-sql-mi-config-remove) | Supprimez une valeur pour un chemin d’accès json dans un fichier de configuration.
[azdata arc sql mi config replace](#azdata-arc-sql-mi-config-replace) | Remplacez une valeur pour un chemin d’accès json dans un fichier de configuration.
[azdata arc sql mi config patch](#azdata-arc-sql-mi-config-patch) | Corrige un fichier de configuration basé sur un fichier de correctif json.
## <a name="azdata-arc-sql-mi-config-init"></a>azdata arc sql mi config init
Initialise les fichiers CRD et de spécification pour une instance gérée SQL.
```bash
azdata arc sql mi config init --path -p 
                              
```
### <a name="examples"></a>Exemples
Initialise les fichiers CRD et de spécification pour une instance gérée SQL.
```bash
azdata arc sql mi config init --path ./template
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin d’accès où le fichier CRD et de spécification de l’instance gérée SQL doivent être écrits.
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
## <a name="azdata-arc-sql-mi-config-add"></a>azdata arc sql mi config add
Ajoute la valeur au chemin d’accès json dans le fichier config.  Tous les exemples ci-dessous sont fournis dans Bash.  Si vous utilisez une autre ligne de commande, sachez que vous devrez peut-être échapper des devis de manière appropriée.  Vous pouvez également utiliser la fonctionnalité de fichier de correctif.
```bash
azdata arc sql mi config add --path -p 
                             --json-values -j
```
### <a name="examples"></a>Exemples
Ex 1 : ajoutez un stockage.
```bash
azdata arc sql mi config add --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin d’accès à la spécification de ressource personnalisée, ici custom/spec.json
#### `--json-values -j`
Liste de paires clé/valeur de chemins d’accès json aux valeurs : key1.subkey1=value1,key2.subkey2=value2. Vous pouvez fournir des valeurs json incluses, telles que : key='{"kind":"cluster","name":"test-cluster"}' ou fournir un chemin d'accès au fichier, tel que key=./values.json. L’ajout ne prend PAS en charge les conditions.  Si la valeur incluse que vous fournissez est une paire clé-valeur elle-même avec des caractères « = » et « , », mettez ces caractères dans une séquence d’échappement.  Par exemple, key1="key2\=val2\,key3\=val3". Veuillez consulter http://jsonpatch.com/ pour obtenir des exemples d’apparence de votre chemin d’accès.  Si vous souhaitez accéder à un tableau, vous devez le faire en indiquant l’index, tel que key.0=value
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
## <a name="azdata-arc-sql-mi-config-remove"></a>azdata arc sql mi config remove
Supprime la valeur au chemin d’accès json dans le fichier config.  Tous les exemples ci-dessous sont fournis dans Bash.  Si vous utilisez une autre ligne de commande, sachez que vous devrez peut-être échapper des devis de manière appropriée.  Vous pouvez également utiliser la fonctionnalité de fichier de correctif.
```bash
azdata arc sql mi config remove --path -p 
                                --json-path -j
```
### <a name="examples"></a>Exemples
Ex 1 : supprimez un stockage.
```bash
azdata arc sql mi config remove --path custom/spec.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin d’accès à la spécification de ressource personnalisée, ici custom/spec.json
#### `--json-path -j`
Une liste de chemins d’accès json basés sur la bibliothèque jsonpatch qui indique les valeurs que vous souhaitez supprimer, par exemple : key1.subkey1,key2.subkey2. La suppression ne prend PAS en charge les conditions. Veuillez consulter http://jsonpatch.com/ pour obtenir des exemples d’apparence de votre chemin d’accès.  Si vous souhaitez accéder à un tableau, vous devez le faire en indiquant l’index, tel que key.0=value
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
## <a name="azdata-arc-sql-mi-config-replace"></a>azdata arc sql mi config replace
Remplace la valeur au niveau du chemin d’accès json dans le fichier config.  Tous les exemples ci-dessous sont fournis dans Bash.  Si vous utilisez une autre ligne de commande, sachez que vous devrez peut-être échapper des devis de manière appropriée.  Vous pouvez également utiliser la fonctionnalité de fichier de correctif.
```bash
azdata arc sql mi config replace --path -p 
                                 --json-values -j
```
### <a name="examples"></a>Exemples
Ex 1 : remplacez le port d’un point de terminaison unique.
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Ex 2 : remplacez un stockage.
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin d’accès à la spécification de ressource personnalisée, ici custom/spec.json
#### `--json-values -j`
Liste de paires clé/valeur de chemins d’accès json aux valeurs : key1.subkey1=value1,key2.subkey2=value2. Vous pouvez fournir des valeurs json incluses, telles que : key='{"kind":"cluster","name":"test-cluster"}' ou fournir un chemin d'accès au fichier, tel que key=./values.json. Remplacer prend en charge des conditions dans la bibliothèque jsonpath.  Pour ce faire, démarrez votre chemin d’accès par un $. Cela vous permettra d’effectuer une condition telle que -j $.key1.key2[?(@.key3=="someValue"].key4=value. Si la valeur incluse que vous fournissez est une paire clé-valeur elle-même avec des caractères « = » et « , », mettez ces caractères dans une séquence d’échappement.  Par exemple, key1="key2\=val2\,key3\=val3". Consultez les exemples ci-dessous. Pour obtenir de l’aide supplémentaire, consultez : https://jsonpath.com/
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
## <a name="azdata-arc-sql-mi-config-patch"></a>azdata arc sql mi config patch
Corrige le fichier config en fonction du fichier correctif donné. Veuillez consulter http://jsonpatch.com/ pour mieux comprendre comment les chemins d’accès doivent être composés. L’opération de remplacement peut utiliser des conditions dans son chemin d’accès en raison de la bibliothèque jsonpath https://jsonpath.com/. Tous les fichiers json des correctifs doivent commencer par une clé « correctif » qui contient un tableau de correctifs avec les opérations (ajouter, remplacer, supprimer), le chemin d’accès et la valeur correspondants. L’opération « supprimer » ne requiert pas de valeur, juste un chemin d’accès. Consultez les exemples ci-dessous.
```bash
azdata arc sql mi config patch --path -p 
                               --patch-file
```
### <a name="examples"></a>Exemples
Ex 1 : remplacez le port d’un point de terminaison unique par un fichier du correctif.
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Ex 2 : remplacez un stockage par un fichier du correctif.
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin d’accès à la spécification de ressource personnalisée, ici custom/spec.json
#### `--patch-file`
Chemin d'accès à un fichier json du correctif basé sur la bibliothèque jsonpatch : http://jsonpatch.com/. Vous devez démarrer votre fichier json du correctif par une clé appelée « correctif », dont la valeur est un tableau d’opérations PATCH que vous envisagez de créer. Pour le chemin d’accès d’une opération PATCH, vous pouvez utiliser la notation, telle que key1.key2 pour la plupart des opérations. Si vous souhaitez effectuer une opération de remplacement et que vous remplacez une valeur dans un tableau qui requiert une condition, utilisez la notation jsonpath en commençant votre chemin d’accès par un $. Cela vous permettra d’effectuer une condition telle que $.key1.key2[?(@.key3=="someValue"].key4. Consultez les exemples ci-dessous. Pour obtenir une aide supplémentaire sur les conditions, consultez : https://jsonpath.com/.
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

