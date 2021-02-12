---
title: azdata bdc config reference
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc config.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 28c9820cd147d5ce4b93fe7605a1f36187f4dfd7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048929"
---
# <a name="azdata-bdc-config"></a>azdata bdc config

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata bdc config init](#azdata-bdc-config-init) | Initialise un profil de configuration de cluster Big Data qui peut être utilisé avec bdc create.
[azdata bdc config list](#azdata-bdc-config-list) | Répertorie les choix de profils de configuration disponibles.
[azdata bdc config show](#azdata-bdc-config-show) | Affiche la configuration actuelle du cluster BDC ou la configuration d’un fichier local que vous spécifiez, par exemple custom/bdc.json.
[azdata bdc config add](#azdata-bdc-config-add) | Ajoutez une valeur pour un chemin d’accès json dans un fichier de configuration.
[azdata bdc config remove](#azdata-bdc-config-remove) | Supprimez une valeur pour un chemin d’accès json dans un fichier de configuration.
[azdata bdc config replace](#azdata-bdc-config-replace) | Remplacez une valeur pour un chemin d’accès json dans un fichier de configuration.
[azdata bdc config patch](#azdata-bdc-config-patch) | Corrige un fichier de configuration basé sur un fichier de correctif json.
## <a name="azdata-bdc-config-init"></a>azdata bdc config init
Initialise un profil de configuration de cluster Big Data qui peut être utilisé avec bdc create. La source spécifique du profil de configuration peut être spécifiée dans les arguments.
```bash
azdata bdc config init [--path -p] 
                       [--source -s]  
                       
[--force -f]  
                       
[--accept-eula -a]
```
### <a name="examples"></a>Exemples
Expérience d’initialisation de configuration de BDC guidée : vous serez invité à entrer les valeurs nécessaires.
```bash
azdata bdc config init
```
L’initialisation de la configuration BDC avec des arguments crée un profil de configuration aks-dev-test dans ./custom.
```bash
azdata bdc config init --source aks-dev-test --target custom
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--path -p`
Chemin de fichier de l’emplacement où vous souhaitez placer le profil de configuration. Il est défini par défaut sur <cwd>/custom.
#### `--source -s`
Source du profil de configuration : ['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--force -f`
Forcez le remplacement du fichier cible.
#### `--accept-eula -a`
Acceptez-vous les termes du contrat de licence ? [oui/non]. Si vous ne voulez pas utiliser cet argument, vous pouvez définir la variable d’environnement ACCEPT_EULA sur « oui ». Les termes du contrat de licence pour ce produit sont visibles à l’adresse https://aka.ms/eula-azdata-en.
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
## <a name="azdata-bdc-config-list"></a>azdata bdc config list
Répertorie les choix de profils de configuration disponibles dans `bdc config init`
```bash
azdata bdc config list [--config-profile -c] 
                       [--type -t]  
                       
[--accept-eula -a]
```
### <a name="examples"></a>Exemples
Affiche les noms de profils de configuration disponibles.
```bash
azdata bdc config list
```
Affiche json d’un profil de configuration spécifique.
```bash
azdata bdc config list --config-profile aks-dev-test
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--config-profile -c`
Profil de configuration par défaut : ['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--type -t`
Le type de configuration que vous souhaitez afficher.
#### `--accept-eula -a`
Acceptez-vous les termes du contrat de licence ? [oui/non]. Si vous ne voulez pas utiliser cet argument, vous pouvez définir la variable d’environnement ACCEPT_EULA sur « oui ». Les termes du contrat de licence pour ce produit sont visibles à l’adresse https://aka.ms/eula-azdata-en.
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
## <a name="azdata-bdc-config-show"></a>azdata bdc config show
Affiche la configuration actuelle du cluster BDC ou la configuration d’un fichier local que vous spécifiez, par exemple custom/bdc.json. La commande peut également utiliser un chemin d’accès json si vous souhaitez obtenir une section uniquement.  Vous pouvez également spécifier un fichier cible vers lequel effectuer la sortie.  Si un fichier cible n’est pas spécifié, il sera simplement sorti sur le terminal.
```bash
azdata bdc config show [--config-file -c] 
                       [--target -t]  
                       
[--json-path -j]  
                       
[--force -f]
```
### <a name="examples"></a>Exemples
Afficher la configuration du BDC dans votre console
```bash
azdata bdc config show
```
Dans un fichier config local, obtenez une valeur à la fin d’un chemin d’accès à la clé json simple.
```bash
azdata bdc config show --config-file custom-config/bdc.json --json-path "metadata.name" --target section.json
```
Dans un fichier de configuration local, obtient les ressources au sein d’un service.
```bash
azdata bdc config show --config-file custom-config/bdc.json --json-path "$.spec.services.sql.resources" --target section.json
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--config-file -c`
Chemin de fichier de configuration du cluster Big Data si vous ne voulez pas la configuration du cluster auquel vous êtes actuellement connecté, par exemple, custom/bdc.json
#### `--target -t`
Fichier de sortie dans lequel stocker le résultat. Valeur par défaut : dirigée vers stdout.
#### `--json-path -j`
Chemin d’accès à la clé json qui mène à la section ou à la valeur que vous souhaitez dans la configuration, par exemple clé1, clé2, clé3. Utilise le langage de requête jsonpath, https://jsonpath.com/, par exemple : -j '$.spec.pools[?(@.spec.type == "Master")]..points de terminaison'
#### `--force -f`
Forcez le remplacement du fichier cible.
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
## <a name="azdata-bdc-config-add"></a>azdata bdc config add
Ajoute la valeur au chemin d’accès json dans le fichier config.  Tous les exemples ci-dessous sont fournis dans Bash.  Si vous utilisez une autre ligne de commande, sachez que vous devrez peut-être échapper des devis de manière appropriée.  Vous pouvez également utiliser la fonctionnalité de fichier de correctif.
```bash
azdata bdc config add --path -p 
                      --json-values -j
```
### <a name="examples"></a>Exemples
Ex 1 : ajoutez un stockage de plan de contrôle.
```bash
azdata bdc config add --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin de fichier de configuration du cluster Big Data de la configuration que vous souhaitez définir, par exemple custom/bdc.json
#### `--json-values -j`
Liste de paires clé/valeur de chemins d’accès json aux valeurs : key1.subkey1=value1,key2.subkey2=value2. Vous pouvez fournir des valeurs json incluses, telles que : key='{"kind":"cluster","name":"test-cluster"}' ou fournir un chemin d'accès au fichier, tel que key=./values.json. L’ajout ne prend PAS en charge les conditions.  Si la valeur incluse que vous fournissez est une paire clé-valeur elle-même avec des caractères « = » et « , », mettez ces caractères dans une séquence d’échappement.  Par exemple, key1="key2\=val2\,key3\=val3". Veuillez consulter http://jsonpatch.com/ pour obtenir des exemples d’apparence de votre chemin d’accès.  Si vous souhaitez accéder à un tableau, vous devez le faire en indiquant l’index, tel que key.0=value
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
## <a name="azdata-bdc-config-remove"></a>azdata bdc config remove
Supprime la valeur au chemin d’accès json dans le fichier config.  Tous les exemples ci-dessous sont fournis dans Bash.  Si vous utilisez une autre ligne de commande, sachez que vous devrez peut-être échapper des devis de manière appropriée.  Vous pouvez également utiliser la fonctionnalité de fichier de correctif.
```bash
azdata bdc config remove --path -p 
                         --json-path -j
```
### <a name="examples"></a>Exemples
Ex 1 : supprimez un stockage de plan de contrôle.
```bash
azdata bdc config remove --path custom/control.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin de fichier de configuration du cluster Big Data de la configuration que vous souhaitez définir, par exemple custom/bdc.json
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
## <a name="azdata-bdc-config-replace"></a>azdata bdc config replace
Remplace la valeur au niveau du chemin d’accès json dans le fichier config.  Tous les exemples ci-dessous sont fournis dans Bash.  Si vous utilisez une autre ligne de commande, sachez que vous devrez peut-être échapper des devis de manière appropriée.  Vous pouvez également utiliser la fonctionnalité de fichier de correctif.
```bash
azdata bdc config replace --path -p 
                          --json-values -j
```
### <a name="examples"></a>Exemples
Ex 1 : remplacez le port d’un point de terminaison unique (point de terminaison de contrôleur).
```bash
azdata bdc config replace --path custom/control.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Ex 2 : remplacez un stockage de plan de contrôle.
```bash
azdata bdc config replace --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
Ex 3 : remplacez la spécification de ressource storage-0, y compris les réplicas.
```bash
azdata bdc config replace --path custom/bdc.json --json-values "$.spec.resources.storage-0.spec={"replicas": 2,"storage": {"className": "managed-premium","size": "10Gi","accessMode": "ReadWriteOnce"},"type": "Storage"}"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin de fichier de configuration du cluster Big Data de la configuration que vous souhaitez définir, par exemple custom/bdc.json
#### `--json-values -j`
Liste de paires clé/valeur de chemins d’accès json aux valeurs : key1.subkey1=value1,key2.subkey2=value2. Vous pouvez fournir des valeurs json incluses, telles que : key='{"kind":"cluster","name":"test-cluster"}' ou fournir un chemin d'accès au fichier, tel que key=./values.json. Remplacer prend en charge des conditions dans la bibliothèque jsonpath.  Pour ce faire, démarrez votre chemin d’accès par un $. Cela vous permettra d’effectuer une condition telle que -j $.key1.key2[?(@.key3=='someValue'].key4=value. Si la valeur incluse que vous fournissez est une paire clé-valeur elle-même avec des caractères « = » et « , », mettez ces caractères dans une séquence d’échappement.  Par exemple, key1="key2\=val2\,key3\=val3". Consultez les exemples ci-dessous. Pour obtenir de l’aide supplémentaire, consultez : https://jsonpath.com/
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
## <a name="azdata-bdc-config-patch"></a>azdata bdc config patch
Corrige le fichier config en fonction du fichier correctif donné. Veuillez consulter http://jsonpatch.com/ pour mieux comprendre comment les chemins d’accès doivent être composés. L’opération de remplacement peut utiliser des conditions dans son chemin d’accès en raison de la bibliothèque jsonpath https://jsonpath.com/. Tous les fichiers json des correctifs doivent commencer par une clé « correctif » qui contient un tableau de correctifs avec les opérations (ajouter, remplacer, supprimer), le chemin d’accès et la valeur correspondants. L’opération « supprimer » ne requiert pas de valeur, juste un chemin d’accès. Consultez les exemples ci-dessous.
```bash
azdata bdc config patch --path 
                        --patch-file -p
```
### <a name="examples"></a>Exemples
Ex 1 : remplacez le port d’un point de terminaison unique (point de terminaison de contrôleur) par un fichier du correctif.
```bash
azdata bdc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Ex 2 : remplacez un stockage de plan de contrôle par un fichier du correctif.
```bash
azdata bdc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
Ex 3 : remplacez le stockage du pool, y compris les réplicas (pool de stockage) par un fichier du correctif.
```bash
azdata bdc config patch --path custom/bdc.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.resources.storage-0.spec","value":{"replicas": 2,"storage": {"className": "managed-premium","size": "10Gi","accessMode": "ReadWriteOnce"},"type": "Storage"}}]}
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path`
Chemin de fichier de configuration du cluster Big Data de la configuration que vous souhaitez définir, par exemple custom/bdc.json
#### `--patch-file -p`
Chemin d'accès à un fichier json du correctif basé sur la bibliothèque jsonpatch : http://jsonpatch.com/. Vous devez démarrer votre fichier json du correctif par une clé appelée « correctif », dont la valeur est un tableau d’opérations PATCH que vous envisagez de créer. Pour le chemin d’accès d’une opération PATCH, vous pouvez utiliser la notation, telle que key1.key2 pour la plupart des opérations. Si vous souhaitez effectuer une opération de remplacement et que vous remplacez une valeur dans un tableau qui requiert une condition, utilisez la notation jsonpath en commençant votre chemin d’accès par un $. Cela vous permettra d’effectuer une condition telle que $.key1.key2[?(@.key3=='someValue'].key4. Consultez les exemples ci-dessous. Pour obtenir une aide supplémentaire sur les conditions, consultez : https://jsonpath.com/.
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

