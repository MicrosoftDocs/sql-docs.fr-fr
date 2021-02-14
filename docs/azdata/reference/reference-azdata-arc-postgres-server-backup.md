---
title: Informations de référence sur azdata arc postgres server backup
titleSuffix: SQL Server big data clusters
description: Article d’informations de référence sur les commandes azdata arc postgres server backup.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 00519fa62e0813bc25cee6599cd2faca9ed1489d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052650"
---
# <a name="azdata-arc-postgres-server-backup"></a>azdata arc postgres server backup

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata arc postgres server backup create](#azdata-arc-postgres-server-backup-create) | Créez une sauvegarde d’un groupe de serveurs PostgreSQL.
[azdata arc postgres server backup delete](#azdata-arc-postgres-server-backup-delete) | Supprimez une sauvegarde d’un groupe de serveurs PostgreSQL.
[azdata arc postgres server backup restore](#azdata-arc-postgres-server-backup-restore) | Restaurez une sauvegarde d’un groupe de serveurs PostgreSQL.
[azdata arc postgres server backup restorestatus](#azdata-arc-postgres-server-backup-restorestatus) | Obtenez l’état de la dernière opération de restauration, le cas échéant.
[azdata arc postgres server backup show](#azdata-arc-postgres-server-backup-show) | Affichez les détails relatifs à une sauvegarde d’un groupe de serveurs PostgreSQL.
## <a name="azdata-arc-postgres-server-backup-create"></a>azdata arc postgres server backup create
Créez une sauvegarde d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server backup create 
```
### <a name="examples"></a>Exemples
Crée une sauvegarde pour le service « pg ».
```bash
azdata arc postgres server backup create -sn pg
```
Crée une sauvegarde nommée pour le service « pg ».
```bash
azdata arc postgres server backup create -sn pg -n backup1
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
## <a name="azdata-arc-postgres-server-backup-delete"></a>azdata arc postgres server backup delete
Supprimez une sauvegarde d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server backup delete 
```
### <a name="examples"></a>Exemples
Supprimez une sauvegarde d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
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
## <a name="azdata-arc-postgres-server-backup-restore"></a>azdata arc postgres server backup restore
Restaurez une sauvegarde d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server backup restore 
```
### <a name="examples"></a>Exemples
Restaurez la sauvegarde la plus récente.
```bash
azdata arc postgres server restore -sn pg```
Restore a backup by ID
```bash
azdata arc postgres server restore -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-restorestatus"></a>azdata arc postgres server backup restorestatus
Obtenez l’état de la dernière opération de restauration, le cas échéant.
```bash
azdata arc postgres server backup restorestatus 
```
### <a name="examples"></a>Exemples
Obtenez l’état de restauration récent pour le service « pg » par ID.
```bash
azdata arc postgres server backup restorestatus -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-show"></a>azdata arc postgres server backup show
Affichez les détails relatifs à une sauvegarde d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server backup show 
```
### <a name="examples"></a>Exemples
Obtient une sauvegarde pour le service « pg » par ID
```bash
azdata arc postgres server backup show -sn pg -id 123e4567e89b12d3a456426655440000
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

