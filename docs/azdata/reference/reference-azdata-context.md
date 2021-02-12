---
title: Informations de référence sur azdata context
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata context.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 689457bc99856b3f5b9b071dd68b2bf4f283096f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048719"
---
# <a name="azdata-context"></a>azdata context

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata context list](#azdata-context-list) | Dresse la liste des contextes disponibles dans le profil utilisateur.
[azdata context delete](#azdata-context-delete) | Supprime le contexte avec l’espace de noms donné du profil utilisateur.
[azdata context set](#azdata-context-set) | Définit le contexte avec l’espace de noms donné comme contexte actif dans le profil utilisateur.
## <a name="azdata-context-list"></a>azdata context list
Vous pouvez définir ou supprimer n’importe lequel de ces éléments avec `azdata context set` ou `azdata context delete`. Pour vous connecter à un nouveau contexte, utilisez `azdata login`.
```bash
azdata context list [--active -a] 
                    
```
### <a name="examples"></a>Exemples
Dresse la liste de tous les contextes disponibles dans le profil utilisateur.
```bash
azdata context list
```
Répertorie le contexte actif dans le profil utilisateur.
```bash
azdata context list --active
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--active -a`
Répertorie uniquement le contexte actuellement actif.
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
## <a name="azdata-context-delete"></a>azdata context delete
Si le contexte supprimé est actif, l’utilisateur doit définir un nouveau contexte actif. Pour afficher les contextes pouvant être définis ou supprimés, exécutez `azdata context list`.
```bash
azdata context delete --namespace -ns 
                      
```
### <a name="examples"></a>Exemples
Supprime contextNamespace du profil utilisateur.
```bash
azdata context delete -n contextNamespace
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--namespace -ns`
Espace de noms du contexte que vous souhaitez supprimer.
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
## <a name="azdata-context-set"></a>azdata context set
Pour afficher les contextes pouvant être définis, exécutez `azdata context list`. Si aucun contexte n’est listé, vous devez vous connecter pour créer un contexte dans votre profil utilisateur `azdata login`. Ce à quoi vous vous connectez devient votre contexte actif. Si vous vous connectez à plusieurs entités, vous pouvez basculer entre les contextes actifs à l’aide de cette commande. Pour afficher votre contexte actuellement actif, exécutez `azdata context list --active`.
```bash
azdata context set --namespace -ns 
                   
```
### <a name="examples"></a>Exemples
Définit contextNamespace en tant que contexte actif dans le profil utilisateur.
```bash
azdata context set -n contextNamespace
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--namespace -ns`
Espace de noms du contexte que vous souhaitez définir.
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

