---
title: Informations de référence sur azdata arc postgres server
titleSuffix: SQL Server big data clusters
description: Article d’informations de référence sur les commandes azdata arc postgres server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 79d2cad23c6aed747837635e4d19d42a5daf41dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052640"
---
# <a name="azdata-arc-postgres-server"></a>azdata arc postgres server

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata arc postgres server create](#azdata-arc-postgres-server-create) | Créez un groupe de serveurs PostgreSQL.
[azdata arc postgres server edit](#azdata-arc-postgres-server-edit) | Modifiez la configuration d’un groupe de serveurs PostgreSQL.
[azdata arc postgres server delete](#azdata-arc-postgres-server-delete) | Supprimez un groupe de serveurs PostgreSQL.
[azdata arc postgres server show](#azdata-arc-postgres-server-show) | Affichez les détails d’un groupe de serveurs PostgreSQL.
[azdata arc postgres server list](#azdata-arc-postgres-server-list) | Répertoriez les groupes de serveurs PostgreSQL.
[azdata arc postgres server config](reference-azdata-arc-postgres-server-config.md) | Commandes de configuration.
[azdata arc postgres server backup](reference-azdata-arc-postgres-server-backup.md) | Gérez les sauvegardes de groupe de serveurs PostgreSQL.
## <a name="azdata-arc-postgres-server-create"></a>azdata arc postgres server create
Pour définir le mot de passe du groupe de serveurs, définissez la variable d’environnement AZDATA_PASSWORD
```bash
azdata arc postgres server create --name -n 
                                  [--path]  
                                  
[--cores-limit -cl]  
                                  
[--cores-request -cr]  
                                  
[--memory-limit -ml]  
                                  
[--memory-request -mr]  
                                  
[--storage-class-data -scd]  
                                  
[--storage-class-logs -scl]  
                                  
[--storage-class-backups -scb]  
                                  
[--extensions]  
                                  
[--volume-size-data -vsd]  
                                  
[--volume-size-logs -vsl]  
                                  
[--volume-size-backups -vsb]  
                                  
[--workers -w]  
                                  
[--engine-version -ev]  
                                  
[--no-external-endpoint]  
                                  
[--dev]  
                                  
[--port]  
                                  
[--no-wait]  
                                  
[--engine-settings -e]
```
### <a name="examples"></a>Exemples
Créez un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server create -n pg1
```
Créez un groupe de serveurs PostgreSQL avec les paramètres du moteur. Les deux exemples ci-dessous sont valides.
```bash
azdata arc postgres server create -n pg1 --engine-settings "key1=val1"
azdata arc postgres server create -n pg1 --engine-settings "key2=val2"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom de l'instance.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--path`
Chemin d’accès au fichier JSON source pour le groupe de serveurs PostgreSQL. Cette option est facultative.
#### `--cores-limit -cl`
Nombre maximum de cœurs du processeur pour l’instance postgres qui peuvent être utilisés par nœud. Les cœurs fractionnels sont pris en charge.
#### `--cores-request -cr`
Nombre minimum de cœurs du processeur qui doivent être disponibles par nœud pour planifier le service. Les cœurs fractionnels sont pris en charge.
#### `--memory-limit -ml`
Limite de mémoire de l’instance postgres sous la forme d’un nombre suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--memory-request -mr`
Requête de mémoire de l’instance postgres sous la forme d’un nombre suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--storage-class-data -scd`
Classe de stockage à utiliser pour les volumes persistants de données.
#### `--storage-class-logs -scl`
Classe de stockage à utiliser pour les volumes persistants de journaux.
#### `--storage-class-backups -scb`
Classe de stockage à utiliser pour les volumes persistants de sauvegarde.
#### `--extensions`
Liste séparée par des virgules des extensions Postgres à charger au démarrage. Reportez-vous à la documentation Postgres pour les valeurs prises en charge.
#### `--volume-size-data -vsd`
Taille du volume de stockage à utiliser pour les données sous la forme d’un nombre positif suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--volume-size-logs -vsl`
Taille du volume de stockage à utiliser pour les journaux sous la forme d’un nombre positif suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--volume-size-backups -vsb`
Taille du volume de stockage à utiliser pour les sauvegardes sous la forme d’un nombre positif suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets).
#### `--workers -w`
Nombre de nœuds Worker à approvisionner dans un cluster partitionné, ou zéro (valeur par défaut) pour Postgres à nœud unique.
#### `--engine-version -ev`
Doit être 11 ou 12. La valeur par défaut est 12.
#### `--no-external-endpoint`
Si elle est spécifiée, aucun service externe n’est créé. Dans le cas contraire, un service externe est créé en utilisant le même type de service que le contrôleur de données.
#### `--dev`
Si elle est spécifiée, elle est considérée comme une instance dev et n’est pas facturée.
#### `--port`
Optionnel.
#### `--no-wait`
Si elle est spécifiée, la commande n’attend pas que l’instance soit à l’état prêt avant un retour.
#### `--engine-settings -e`
Liste séparée par des virgules des paramètres du moteur Postgres au format « key1=val1, key2=val2 ».
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
## <a name="azdata-arc-postgres-server-edit"></a>azdata arc postgres server edit
Modifiez la configuration d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server edit --name -n 
                                [--path]  
                                
[--workers -w]  
                                
[--engine-version -ev]  
                                
[--cores-limit -cl]  
                                
[--cores-request -cr]  
                                
[--memory-limit -ml]  
                                
[--memory-request -mr]  
                                
[--extensions]  
                                
[--dev]  
                                
[--port]  
                                
[--no-wait]  
                                
[--engine-settings -e]  
                                
[--replace-engine-settings -re]  
                                
[--admin-password]
```
### <a name="examples"></a>Exemples
Modifiez la configuration d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server edit --src ./spec.json -n pg1
```
Modifiez un groupe de serveurs PostgreSQL avec les paramètres du moteur.
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key2=val2"
```
Modifie un groupe de serveurs PostgreSQL et remplace les paramètres du moteur existants par le nouveau paramètre « key1=val1 ».
```bash
azdata arc postgres server edit -n pg1 --engine-settings "key1=val1" --replace-engine-settings
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom du groupe de serveurs PostgreSQL en cours de modification. Le nom sous lequel votre instance est déployée ne peut pas être modifié.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--path`
Chemin d’accès au fichier JSON source pour le groupe de serveurs PostgreSQL. Cette option est facultative.
#### `--workers -w`
Nombre de nœuds Worker à approvisionner dans un cluster partitionné, ou zéro (valeur par défaut) pour Postgres à nœud unique.
#### `--engine-version -ev`
La version du moteur ne peut pas être modifiée. --engine-version peut être utilisé avec --name pour identifier un groupe de serveurs PostgreSQL hyperscale lorsque deux groupes de serveurs d’une version de moteur différente portent le même nom. --engine-version est facultatif et, lorsqu’il est utilisé pour identifier un groupe de serveurs, doit être 11 ou 12.
#### `--cores-limit -cl`
Nombre maximum de cœurs du processeur pour l’instance postgres qui peuvent être utilisés par nœud ; les cœurs fractionnels sont pris en charge. Pour supprimer la cores_limit, spécifiez sa valeur en tant que chaîne vide.
#### `--cores-request -cr`
Nombre minimum de cœurs du processeur qui doivent être disponibles par nœud pour planifier le service ; les cœurs fractionnels sont pris en charge. Pour supprimer la cores_request, spécifiez sa valeur en tant que chaîne vide.
#### `--memory-limit -ml`
Limite de mémoire pour l’instance postgres sous la forme d’un nombre suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets). Pour supprimer la memory_limit, spécifiez sa valeur en tant que chaîne vide.
#### `--memory-request -mr`
Requête de mémoire pour l’instance postgres sous la forme d’un nombre suivi de Ki (kilo-octets), Mi (mégaoctets) ou Gi (gigaoctets). Pour supprimer la memory_request, spécifiez sa valeur en tant que chaîne vide.
#### `--extensions`
Liste séparée par des virgules des extensions Postgres à charger au démarrage. Reportez-vous à la documentation Postgres pour les valeurs prises en charge.
#### `--dev`
Si elle est spécifiée, elle est considérée comme une instance dev et n’est pas facturée.
#### `--port`
Optionnel.
#### `--no-wait`
Si elle est spécifiée, la commande n’attend pas que l’instance soit à l’état prêt avant un retour.
#### `--engine-settings -e`
Liste séparée par des virgules des paramètres du moteur Postgres au format « key1=val1, key2=val2 ». Les paramètres fournis sont fusionnés avec les paramètres existants. Pour supprimer un paramètre, indiquez une valeur vide telle que « removedKey= ». Si vous modifiez un paramètre du moteur qui requiert un redémarrage, le service est redémarré pour appliquer les paramètres immédiatement.
#### `--replace-engine-settings -re`
Lorsqu’elle est spécifiée avec --engine-settings, elle remplace tous les paramètres du moteur personnalisés existants par un nouvel ensemble de paramètres et de valeurs.
#### `--admin-password`
Si elle est spécifiée, le mot de passe d’administrateur du serveur Postgres est défini sur la valeur de la variable d’environnement AZDATA_PASSWORD, si elle est présente, et sur une valeur demandée dans le cas contraire.
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
## <a name="azdata-arc-postgres-server-delete"></a>azdata arc postgres server delete
Supprimez un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server delete --name -n 
                                  [--engine-version -ev]
```
### <a name="examples"></a>Exemples
Supprimez un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server delete -n pg1
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom du groupe de serveurs PostgreSQL.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--engine-version -ev`
--engine-version peut être utilisé avec --name pour identifier un groupe de serveurs PostgreSQL hyperscale lorsque deux groupes de serveurs d’une version de moteur différente portent le même nom. --engine-version est facultatif et, lorsqu’il est utilisé pour identifier un groupe de serveurs, doit être 11 ou 12.
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
## <a name="azdata-arc-postgres-server-show"></a>azdata arc postgres server show
Affichez les détails d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server show --name -n 
                                [--engine-version -ev]  
                                
[--path -p]
```
### <a name="examples"></a>Exemples
Affichez les détails d’un groupe de serveurs PostgreSQL.
```bash
azdata arc postgres server show -n pg1
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom du groupe de serveurs PostgreSQL.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--engine-version -ev`
--engine-version peut être utilisé avec --name pour identifier un groupe de serveurs PostgreSQL hyperscale lorsque deux groupes de serveurs d’une version de moteur différente portent le même nom. --engine-version est facultatif et, lorsqu’il est utilisé pour identifier un groupe de serveurs, doit être 11 ou 12.
#### `--path -p`
Chemin d’accès où la spécification complète du groupe de serveurs PostgreSQL doit être écrite. S’il est omis, la spécification est écrite dans la sortie standard.
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
## <a name="azdata-arc-postgres-server-list"></a>azdata arc postgres server list
Répertoriez les groupes de serveurs PostgreSQL.
```bash
azdata arc postgres server list 
```
### <a name="examples"></a>Exemples
Répertoriez les groupes de serveurs PostgreSQL.
```bash
azdata arc postgres server list
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

