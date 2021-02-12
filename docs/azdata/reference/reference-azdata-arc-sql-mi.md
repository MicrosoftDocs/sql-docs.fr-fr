---
title: Informations de référence sur azdata arc sql mi
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata arc sql mi.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6a3d561f6b09ca6720a70a067e5985fbc96dceb1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048949"
---
# <a name="azdata-arc-sql-mi"></a>azdata arc sql mi

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata arc sql mi create](#azdata-arc-sql-mi-create) | Créez une instance SQL gérée.
[azdata arc sql mi edit](#azdata-arc-sql-mi-edit) | Modifiez la configuration d’une instance SQL gérée.
[azdata arc sql mi delete](#azdata-arc-sql-mi-delete) | Supprimez une instance SQL gérée.
[azdata arc sql mi show](#azdata-arc-sql-mi-show) | Affichez les détails d’une instance SQL gérée.
[azdata arc sql mi list](#azdata-arc-sql-mi-list) | Répertoriez les instances SQL gérées.
[azdata arc sql mi config](reference-azdata-arc-sql-mi-config.md) | Commandes de configuration.
## <a name="azdata-arc-sql-mi-create"></a>azdata arc sql mi create
Pour définir le mot de passe de l’instance SQL gérée, définissez la variable d’environnement AZDATA_PASSWORD
```bash
azdata arc sql mi create --name -n 
                         [--path]  
                         
[--cores-limit -cl]  
                         
[--cores-request -cr]  
                         
[--memory-limit -ml]  
                         
[--memory-request -mr]  
                         
[--storage-class-data -scd]  
                         
[--storage-class-logs -scl]  
                         
[--storage-class-data-logs -scdl]  
                         
[--storage-class-backups -scb]  
                         
[--volume-size-data -vsd]  
                         
[--volume-size-logs -vsl]  
                         
[--volume-size-data-logs -vsdl]  
                         
[--volume-size-backups -vsb]  
                         
[--no-external-endpoint]  
                         
[--dev]  
                         
[--no-wait]
```
### <a name="examples"></a>Exemples
Créez une instance SQL gérée.
```bash
azdata arc sql mi create -n sqlmi1
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l’instance SQL gérée.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--path`
Chemin d’accès au fichier SRC pour le fichier JSON de l’instance SQL gérée.
#### `--cores-limit -cl`
Limite de cœurs de l’instance gérée sous la forme d’un entier.
#### `--cores-request -cr`
Demande de cœurs de l’instance gérée sous la forme d’un entier.
#### `--memory-limit -ml`
Limite de capacité de l’instance gérée sous la forme d’un entier.
#### `--memory-request -mr`
Demande de capacité de l’instance gérée sous la forme d’une quantité de mémoire en Go.
#### `--storage-class-data -scd`
Classe de stockage à utiliser pour les données (.mdf). Si aucune valeur n’est indiquée, aucune classe de stockage n’est indiquée. Par conséquent, Kubernetes utilisera la classe de stockage par défaut.
#### `--storage-class-logs -scl`
Classe de stockage à utiliser pour les journaux (/var/log). Si aucune valeur n’est indiquée, aucune classe de stockage n’est indiquée. Par conséquent, Kubernetes utilisera la classe de stockage par défaut.
#### `--storage-class-data-logs -scdl`
Classe de stockage à utiliser pour les journaux de base de données (.ldf). Si aucune valeur n’est indiquée, aucune classe de stockage n’est indiquée. Par conséquent, Kubernetes utilisera la classe de stockage par défaut.
#### `--storage-class-backups -scb`
Classe de stockage à utiliser pour les sauvegardes (/var/opt/mssql/backups). Si aucune valeur n’est indiquée, aucune classe de stockage n’est indiquée. Par conséquent, Kubernetes utilisera la classe de stockage par défaut.
#### `--volume-size-data -vsd`
Taille du volume de stockage à utiliser pour les données sous la forme d’un nombre positif suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--volume-size-logs -vsl`
Taille du volume de stockage à utiliser pour les journaux sous la forme d’un nombre positif suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--volume-size-data-logs -vsdl`
Taille du volume de stockage à utiliser pour les journaux de données sous la forme d’un nombre positif suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--volume-size-backups -vsb`
Taille du volume de stockage à utiliser pour les sauvegardes sous la forme d’un nombre positif suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--no-external-endpoint`
Si elle est spécifiée, aucun service externe n’est créé. Dans le cas contraire, un service externe est créé en utilisant le même type de service que le contrôleur de données.
#### `--dev`
Si elle est spécifiée, elle est considérée comme une instance dev et n’est pas facturée.
#### `--no-wait`
Si elle est spécifiée, la commande n’attend pas que l’instance soit à l’état prêt avant un retour.
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
## <a name="azdata-arc-sql-mi-edit"></a>azdata arc sql mi edit
Modifiez la configuration d’une instance SQL gérée.
```bash
azdata arc sql mi edit --name -n 
                       [--path]  
                       
[--cores-limit -cl]  
                       
[--cores-request -cr]  
                       
[--memory-limit -ml]  
                       
[--memory-request -mr]  
                       
[--dev]  
                       
[--no-wait]
```
### <a name="examples"></a>Exemples
Modifiez la configuration d’une instance SQL gérée.
```bash
azdata arc sql mi edit --path ./spec.json -n sqlmi1
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l’instance SQL gérée en cours de modification. Le nom sous lequel votre instance est déployée ne peut pas être modifié.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--path`
Chemin d’accès au fichier SRC pour le fichier JSON de l’instance SQL gérée.
#### `--cores-limit -cl`
Limite de cœurs de l’instance gérée sous la forme d’un entier.
#### `--cores-request -cr`
Demande de cœurs de l’instance gérée sous la forme d’un entier.
#### `--memory-limit -ml`
Limite de capacité de l’instance gérée sous la forme d’un entier.
#### `--memory-request -mr`
Demande de capacité de l’instance gérée sous la forme d’une quantité de mémoire en Go.
#### `--dev`
Si elle est spécifiée, elle est considérée comme une instance dev et n’est pas facturée.
#### `--no-wait`
Si elle est spécifiée, la commande n’attend pas que l’instance soit à l’état prêt avant un retour.
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
## <a name="azdata-arc-sql-mi-delete"></a>azdata arc sql mi delete
Supprimez une instance SQL gérée.
```bash
azdata arc sql mi delete --name -n 
                         
```
### <a name="examples"></a>Exemples
Supprimez une instance SQL gérée.
```bash
azdata arc sql mi delete -n sqlmi1
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l’instance SQL gérée à supprimer.
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
## <a name="azdata-arc-sql-mi-show"></a>azdata arc sql mi show
Affichez les détails d’une instance SQL gérée.
```bash
azdata arc sql mi show --name -n 
                       [--path -p]
```
### <a name="examples"></a>Exemples
Affichez les détails d’une instance SQL gérée.
```bash
azdata arc sql mi show -n sqlmi1
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l’instance SQL gérée à afficher.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--path -p`
Chemin d’accès où l’ensemble de la spécification de l’instance SQL gérée doit être écrit. S’il est omis, la spécification est écrite dans la sortie standard.
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
## <a name="azdata-arc-sql-mi-list"></a>azdata arc sql mi list
Répertoriez les instances SQL gérées.
```bash
azdata arc sql mi list 
```
### <a name="examples"></a>Exemples
Répertoriez les instances SQL gérées.
```bash
azdata arc sql mi list
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

