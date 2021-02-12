---
title: Informations de référence sur azdata arc dc
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata arc dc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b223b3b69ba3eda1be5f882b4cabd868c345b20c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049009"
---
# <a name="azdata-arc-dc"></a>azdata arc dc

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata arc dc create](#azdata-arc-dc-create) | Créez un contrôleur de données.
[azdata arc dc delete](#azdata-arc-dc-delete) | Supprimez un contrôleur de données.
[azdata arc dc endpoint](reference-azdata-arc-dc-endpoint.md) | Commandes relatives aux points de terminaison.
[azdata arc dc status](reference-azdata-arc-dc-status.md) | Commandes relatives à l’état.
[azdata arc dc config](reference-azdata-arc-dc-config.md) | Commandes de configuration.
[azdata arc dc debug](reference-azdata-arc-dc-debug.md) | Commandes de débogage.
[azdata arc dc export](#azdata-arc-dc-export) | Exportez les métriques, les journaux ou l’utilisation.
[azdata arc dc upload](#azdata-arc-dc-upload) | Téléchargez le fichier de données exporté.
## <a name="azdata-arc-dc-create"></a>azdata arc dc create
Créez un contrôleur de données. La configuration Kubernetes est requise sur votre système avec les variables d’environnement suivantes ['AZDATA_USERNAME', 'AZDATA_PASSWORD'].
```bash
azdata arc dc create --namespace -ns 
                     --name -n  
                     
--connectivity-mode  
                     
--resource-group -g  
                     
--location -l  
                     
--subscription -s  
                     
[--profile-name]  
                     
[--path -p]  
                     
[--storage-class -sc]
```
### <a name="examples"></a>Exemples
Déploiement d’un contrôleur de données.
```bash
azdata arc dc create
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--namespace -ns`
Espace de noms Kubernetes dans lequel déployer le contrôleur de données. S’il existe déjà, il sera utilisé. S’il n’existe pas, le système tentera d’abord de le créer.
#### `--name -n`
Nom du contrôleur de données.
#### `--connectivity-mode`
Connectivité à Azure (indirecte ou directe) dans laquelle le contrôleur de données doit fonctionner.
#### `--resource-group -g`
Groupe de ressources Azure dans lequel la ressource de contrôleur de données doit être ajoutée.
#### `--location -l`
Emplacement Azure dans lequel les métadonnées du contrôleur de données seront stockées (par exemple, Eastus).
#### `--subscription -s`
ID de l’abonnement Azure dans lequel la ressource du contrôleur de données doit être ajoutée.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--profile-name`
Nom d’un profil de configuration existant. Exécutez `azdata arc dc config list` pour voir les options disponibles. Une des valeurs suivantes : ['azure-arc-aks-premium-storage', 'azure-arc-ake', 'azure-arc-openshift', 'azure-arc-gke', 'azure-arc-aks-default-storage', 'azure-arc-kubeadm', 'azure-arc-eks', 'azure-arc-azure-openshift', 'azure-arc-aks-hci'].
#### `--path -p`
Chemin d’accès à un répertoire contenant un profil de configuration personnalisé à utiliser. Exécutez `azdata arc dc config init` pour créer un profil de configuration personnalisé.
#### `--storage-class -sc`
Classe de stockage à utiliser pour les volumes persistants de l’ensemble des données et journaux pour tous les pods du contrôleur de données qui en ont besoin.
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
## <a name="azdata-arc-dc-delete"></a>azdata arc dc delete
Supprimez le contrôleur de données. La configuration de Kube est requise sur votre système.
```bash
azdata arc dc delete --name -n 
                     --namespace -ns  
                     
[--force -f]
```
### <a name="examples"></a>Exemples
Déploiement d’un contrôleur de données.
```bash
azdata arc dc delete
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--name -n`
Nom du contrôleur de données.
#### `--namespace -ns`
Espace de noms Kubernetes dans lequel le contrôleur de données existe.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--force -f`
Forcez la supprimession d’un contrôleur de données.
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
## <a name="azdata-arc-dc-export"></a>azdata arc dc export
Exportez les métriques, les journaux ou l’utilisation dans un fichier.
```bash
azdata arc dc export --type -t 
                     --path -p  
                     
[--force -f]
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--type -t`
Type de données à exporter. Options : journaux, métriques et utilisation.
#### `--path -p`
Chemin d’accès complet ou relatif comprenant le nom du fichier à exporter.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--force -f`
Forcez la création du fichier de sortie. Remplace tout fichier existant au même chemin d’accès.
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
## <a name="azdata-arc-dc-upload"></a>azdata arc dc upload
Téléchargez le fichier de données exporté à partir d’un contrôleur de données vers Azure.
```bash
azdata arc dc upload --path -p 
                     
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin d’accès complet ou relatif comprenant le nom du fichier à télécharger.
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

