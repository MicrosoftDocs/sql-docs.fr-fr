---
title: Informations de référence sur azdata
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d29ac23842c8aae84ed92d37022c5f67420f42d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052260"
---
# <a name="azdata"></a>azdata

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
|[azdata notebook](reference-azdata-notebook.md) | Commandes permettant d’afficher, d’exécuter et de gérer des notebooks à partir d’un terminal. |
|[azdata extension](reference-azdata-extension.md) | Gérez et mettez à jour les extensions CLI. |
|[azdata arc](reference-azdata-arc.md) | Commandes pour l’utilisation d’Azure Arc pour Azure Data Services. |
|[azdata app](reference-azdata-app.md) | Créer, supprimer, exécuter et gérer des applications. |
|[azdata bdc](reference-azdata-bdc.md) | Sélectionner, gérer et utiliser des clusters Big Data SQL Server. |
|[azdata sql](reference-azdata-sql.md) | L’interface CLI SQL DB permet à l’utilisateur d’interagir avec SQL Server via T-SQL. |
[azdata login](#azdata-login) | Connectez-vous au point de terminaison du contrôleur du cluster et définissez son espace de noms en tant que contexte actif. Pour utiliser un mot de passe pour la connexion, vous devez définir la variable d’environnement AZDATA_PASSWORD.
[azdata logout](#azdata-logout) | Se déconnecter du cluster.
|[azdata context](reference-azdata-context.md) | Commandes de gestion du contexte. |
|[azdata postgres](reference-azdata-postgres.md) | Exécuteur de requêtes postgres et interpréteur de commandes interactif. |
## <a name="azdata-login"></a>azdata login
Quand votre cluster est déployé, il indique le point de terminaison du contrôleur lors du déploiement, que vous devez utiliser pour vous connecter.  Si vous ne connaissez pas le point de terminaison du contrôleur, vous pouvez vous connecter en faisant en sorte que la configuration Kube de votre cluster se trouve sur votre système dans l’emplacement par défaut <user home>/.kube/config ou en utilisant la variable d’environnement KUBECONFIG, par exemple exportez KUBECONFIG=path/to/.kube/configg.  Lorsque vous vous connectez, cet espace de noms de cluster est défini sur votre contexte actif.
```bash
azdata login [--auth] 
             [--endpoint -e]  
             
[--accept-eula -a]  
             
[--namespace -ns]  
             
[--username -u]  
             
[--principal -p]
```
### <a name="examples"></a>Exemples
Connectez-vous en utilisant l’authentification de base.
```bash
azdata login --auth basic --username johndoe --endpoint https://<ip or domain name>:30080            
```
Connectez-vous en utilisant Active Directory.
```bash
azdata login --auth ad --endpoint https://<ip or domain name>:30080                
```
Connectez-vous en utilisant Active Directory avec un principal explicite.
```bash
azdata login --auth ad --principal johndoe@COSTOSO.COM --endpoint https://<ip or domain name>:30080
```
Se connecter de façon interactive. Le nom du cluster est toujours demandé s’il n’est pas spécifié comme argument. Si les variables d’environnement AZDATA_USERNAME, AZDATA_PASSWORD et ACCEPT_EULA sont définies sur votre système, elles ne vous sont pas demandées. Si vous avez la configuration Kube sur votre système ou si vous utilisez la variable d’environnement KUBECONFIG pour spécifier le chemin de la configuration, l’expérience interactive tente d’abord d’utiliser la configuration, puis vous demande les informations si la configuration échoue.
```bash
azdata login
```
Se connecter (de façon non interactive). Connectez-vous avec le nom du cluster, le nom d’utilisateur du contrôleur, le point de terminaison du contrôleur et l’acceptation du CLUF définis comme arguments. La variable d’environnement AZDATA_PASSWORD doit être définie.  Si vous ne voulez pas spécifier le point de terminaison du contrôleur, placez la configuration Kube sur votre machine dans l’emplacement par défaut <user home>/.kube/config ou utilisez la variable d’environnement KUBECONFIG, par exemple exportez KUBECONFIG=path/to/.kube/configg.
```bash
azdata login --namespace ClusterName --username johndoe@contoso.com  --endpoint https://<ip or domain name>:30080 --accept-eula yes
```
Connectez-vous avec la configuration Kube sur la machine, et les variables d’environnement définies pour AZDATA_USERNAME, AZDATA_PASSWORD et ACCEPT_EULA.
```bash
azdata login -n ClusterName
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--auth`
La stratégie d’authentification. Authentification de base ou Active Directory. L’authentification « de base » est définie par défaut.
#### `--endpoint -e`
Point de terminaison du contrôleur du cluster « https://host:port  ». Si vous ne voulez pas utiliser cet argument, vous pouvez utiliser la configuration Kube sur votre machine. Vérifiez que la configuration se trouve à l’emplacement par défaut <user home>/.kube/config ou utilisez la variable d’environnement KUBECONFIG.
#### `--accept-eula -a`
Acceptez-vous les termes du contrat de licence ? [oui/non]. Si vous ne voulez pas utiliser cet argument, vous pouvez définir la variable d’environnement ACCEPT_EULA sur « oui ». Les termes du contrat de licence pour ce produit sont visibles à l’adresse https://aka.ms/eula-azdata-en.
#### `--namespace -ns`
Espace de noms du plan de contrôle du cluster.
#### `--username -u`
Utilisateur du compte. Si vous ne voulez pas utiliser cet argument, vous pouvez définir la variable d’environnement AZDATA_USERNAME.
#### `--principal -p`
Votre domaine Kerberos. Dans la plupart des cas, votre domaine Kerberos correspond à votre nom de domaine, en majuscules.
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
## <a name="azdata-logout"></a>azdata logout
Se déconnecter du cluster.
```bash
azdata logout 
```
### <a name="examples"></a>Exemples
Déconnectez cet utilisateur.
```bash
azdata logout
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

