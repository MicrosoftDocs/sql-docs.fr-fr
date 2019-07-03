---
title: référence de montage mssqlctl bdc pool de stockage
titleSuffix: SQL Server big data clusters
description: Article de référence pour les commandes de montage du pool de stockage mssqlctl bdc.
author: rothja
ms.author: jroth
manager: jroth
ms.date: 06/26/2019
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: cf6012b6c700afd6ee0eca763df0961f088a3be4
ms.sourcegitcommit: ce5770d8b91c18ba5ad031e1a96a657bde4cae55
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/25/2019
ms.locfileid: "67394241"
---
# <a name="mssqlctl-bdc-storage-pool-mount"></a>mssqlctl bdc storage-pool mount

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

L’article suivant fournit la référence pour le **montage du pool de stockage bdc** commandes dans le **mssqlctl** outil. Pour plus d’informations sur les autres **mssqlctl** commandes, consultez [mssqlctl référence](reference-mssqlctl.md).

## <a name="commands"></a>Commandes
|     |     |
| --- | --- |
[mssqlctl bdc storage-pool mount create](#mssqlctl-bdc-storage-pool-mount-create) | Créer des montages de magasins distants dans HDFS.
[mssqlctl bdc storage-pool mount delete](#mssqlctl-bdc-storage-pool-mount-delete) | Supprimer les montages de magasins distants dans HDFS.
[mssqlctl bdc storage-pool mount status](#mssqlctl-bdc-storage-pool-mount-status) | État de mount(s).
## <a name="mssqlctl-bdc-storage-pool-mount-create"></a>créer de montage de pool de stockage mssqlctl bdc
Créer des montages de magasins distants dans HDFS. Les informations d’identification pour accéder à la banque à distance, cas échéant, doivent être spécifiées à l’aide de la variable d’environnement MOUNT_CREDENTIALS sous forme de liste séparée par des virgules de clé = paires de valeur. Les virgules de clés ou valeurs doivent être échappés.
```bash
mssqlctl bdc storage-pool mount create --remote-uri 
                                       --mount-path
```
### <a name="examples"></a>Exemples
Pour monter le conteneur « données » dans le compte de ADLS Gen 2 « adlsv2example » sur /mounts/adlsv2/data de chemin d’accès HDFS à l’aide de la clé partagée
```bash
Set the MOUNT_CREDENTIALS environment variable as "fs.azure.abfs.account.name=<your-storage-account-name>.dfs.core.windows.net,fs.azure.account.key.<your-storage-account-name>.dfs.core.windows.net=<storage-account-access-key>".
mssqlctl bdc storage-pool mount create --remote-uri abfs://data@adlsv2example.dfs.core.windows.net/
    --mount-path /mounts/adlsv2/data
```
Pour monter un BDC HDFS à distance (hdfs://namenode1:8080 /) on local HDFS path /mounts/hdfs /
```bash
mssqlctl bdc storage-pool mount create --remote-uri hdfs://namenode1:8080/ --mount-path /mounts/hdfs/
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--remote-uri`
URI de la banque à distance qui doit être monté (source de montage).
#### `--mount-path`
Chemin d’accès HDFS dans lequel le montage doit être créé (destination de montage).
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmente le détail de journalisation pour afficher que tous les journaux de débogage.
#### `--help -h`
Afficher ce message d’aide et de sortie.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Par défaut : json.
#### `--query -q`
Chaîne de requête JMESPath. Consultez [ http://jmespath.org/ ](http://jmespath.org/]) pour plus d’informations et des exemples.
#### `--verbose`
Augmente le détail de journalisation. Utilisez--debug pour les journaux de débogage complets.
## <a name="mssqlctl-bdc-storage-pool-mount-delete"></a>mssqlctl bdc storage-pool mount delete
Supprimer les montages de magasins distants dans HDFS.
```bash
mssqlctl bdc storage-pool mount delete --mount-path 
                                       
```
### <a name="examples"></a>Exemples
Supprimer le montage créé au /mounts/adlsv2/data pour un compte de stockage ADLS Gen 2.
```bash
mssqlctl bdc storage-pool mount delete --mount-path /mounts/adlsv2/data
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--mount-path`
Le chemin d’accès HDFS correspondant pour le montage qui doit être supprimé.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmente le détail de journalisation pour afficher que tous les journaux de débogage.
#### `--help -h`
Afficher ce message d’aide et de sortie.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Par défaut : json.
#### `--query -q`
Chaîne de requête JMESPath. Consultez [ http://jmespath.org/ ](http://jmespath.org/]) pour plus d’informations et des exemples.
#### `--verbose`
Augmente le détail de journalisation. Utilisez--debug pour les journaux de débogage complets.
## <a name="mssqlctl-bdc-storage-pool-mount-status"></a>mssqlctl bdc storage-pool mount status
État de mount(s).
```bash
mssqlctl bdc storage-pool mount status [--mount-path] 
                                       
```
### <a name="examples"></a>Exemples
Obtenir l’état de montage par chemin d’accès
```bash
mssqlctl bdc storage-pool mount status --mount-path /mounts/hdfs
```
Obtenir l’état de tous les montages.
```bash
mssqlctl bdc storage-pool mount status
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--mount-path`
Chemin d’accès de montage.
### <a name="global-arguments"></a>Arguments globaux
#### `--debug`
Augmente le détail de journalisation pour afficher que tous les journaux de débogage.
#### `--help -h`
Afficher ce message d’aide et de sortie.
#### `--output -o`
Format de sortie.  Valeurs autorisées : json, jsonc, table, tsv.  Par défaut : json.
#### `--query -q`
Chaîne de requête JMESPath. Consultez [ http://jmespath.org/ ](http://jmespath.org/]) pour plus d’informations et des exemples.
#### `--verbose`
Augmente le détail de journalisation. Utilisez--debug pour les journaux de débogage complets.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les autres **mssqlctl** commandes, consultez [mssqlctl référence](reference-mssqlctl.md). Pour plus d’informations sur la façon d’installer le **mssqlctl** , consultez [installer mssqlctl pour gérer les clusters de données volumineuses de SQL Server 2019](deploy-install-mssqlctl.md).