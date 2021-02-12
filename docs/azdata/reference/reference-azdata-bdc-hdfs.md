---
title: azdata bdc hdfs reference
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata bdc hdfs.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ed57027a0fa7a2f5003c67cafb5a7bb8eb3d876b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052420"
---
# <a name="azdata-bdc-hdfs"></a>azdata bdc hdfs

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata bdc hdfs shell](#azdata-bdc-hdfs-shell) | L’interpréteur de commandes HDFS est un interpréteur de commandes interactif simple pour le système de fichiers HDFS.
[azdata bdc hdfs ls](#azdata-bdc-hdfs-ls) | Répertoriez l’état du fichier ou du répertoire donné.
[azdata bdc hdfs exists](#azdata-bdc-hdfs-exists) | Déterminez si un fichier ou un répertoire existe.  Retourne True s’il existe, sinon False.
[azdata bdc hdfs mkdir](#azdata-bdc-hdfs-mkdir) | Créez un répertoire au niveau du chemin d’accès spécifié.
[azdata bdc hdfs mv](#azdata-bdc-hdfs-mv) | Déplacez le fichier ou le chemin d’accès spécifié vers l’emplacement spécifié.
[azdata bdc hdfs create](#azdata-bdc-hdfs-create) | Créez le fichier texte à l’emplacement spécifié.  Un contenu de texte simple peut être ajouté par le biais du paramètre Données.
[azdata bdc hdfs cat](#azdata-bdc-hdfs-cat) | Lisez le contenu d’un fichier.  Le décalage et la longueur en octets sont des paramètres facultatifs.
[azdata bdc hdfs rm](#azdata-bdc-hdfs-rm) | Supprimez un fichier ou un répertoire.
[azdata bdc hdfs rmr](#azdata-bdc-hdfs-rmr) | Supprimez de manière récursive un fichier ou un répertoire.
[azdata bdc hdfs chmod](#azdata-bdc-hdfs-chmod) | Modifiez l’autorisation sur le fichier ou le répertoire spécifié.
[azdata bdc hdfs chown](#azdata-bdc-hdfs-chown) | Modifiez le propriétaire ou le groupe du fichier spécifié.
[azdata bdc hdfs cp](#azdata-bdc-hdfs-cp) | Copiez un fichier ou un répertoire entre l’ordinateur local et HDFS.
[azdata bdc hdfs mount](reference-azdata-bdc-hdfs-mount.md) | Gérez des montages de magasins distants dans HDFS.
[azdata bdc hdfs status](reference-azdata-bdc-hdfs-status.md) | Commandes d’état du service HDFS.
## <a name="azdata-bdc-hdfs-shell"></a>azdata bdc hdfs shell
L’interpréteur de commandes HDFS est un interpréteur de commandes interactif simple pour le système de fichiers HDFS.
```bash
azdata bdc hdfs shell 
```
### <a name="examples"></a>Exemples
Lancez l’interpréteur de commandes.
```bash
azdata bdc hdfs shell
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
## <a name="azdata-bdc-hdfs-ls"></a>azdata bdc hdfs ls
Répertoriez l’état du fichier ou du répertoire donné.
```bash
azdata bdc hdfs ls --path -p 
                   
```
### <a name="examples"></a>Exemples
Répertorier les états
```bash
azdata bdc hdfs ls --path "tmp/"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Le chemin d’accès pour répertorier le statut.
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
## <a name="azdata-bdc-hdfs-exists"></a>azdata bdc hdfs exists
Déterminez si un fichier ou un répertoire existe.  Retourne True s’il existe, sinon False.
```bash
azdata bdc hdfs exists --path -p 
                       
```
### <a name="examples"></a>Exemples
Vérifiez l’existence d’un fichier ou d’un répertoire.
```bash
azdata bdc hdfs exists --path "tmp/"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Chemin d’accès pour vérifier l’existence.
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
## <a name="azdata-bdc-hdfs-mkdir"></a>azdata bdc hdfs mkdir
Créez un répertoire au niveau du chemin d’accès spécifié.
```bash
azdata bdc hdfs mkdir --path -p 
                      
```
### <a name="examples"></a>Exemples
Créez un répertoire.
```bash
azdata bdc hdfs mkdir --path "tmp/"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Nom du répertoire à créer.
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
## <a name="azdata-bdc-hdfs-mv"></a>azdata bdc hdfs mv
Déplacez le fichier ou le chemin d’accès spécifié vers l’emplacement spécifié.
```bash
azdata bdc hdfs mv --source-path -s 
                   --target-path -t
```
### <a name="examples"></a>Exemples
Déplacez le fichier ou le répertoire.
```bash
azdata bdc hdfs mv --source-path "tmp/" --target-path "dest/"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--source-path -s`
Le répertoire à déplacer.
#### `--target-path -t`
L’emplacement vers lequel le déplacer.
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
## <a name="azdata-bdc-hdfs-create"></a>azdata bdc hdfs create
Créez le fichier texte à l’emplacement spécifié.  Un contenu de texte simple peut être ajouté par le biais du paramètre Données.
```bash
azdata bdc hdfs create --path -p 
                       --data -d
```
### <a name="examples"></a>Exemples
Créez un fichier.
```bash
azdata bdc hdfs create --path "tmp/test.txt" --data "This is a test."
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Nom du fichier à créer.
#### `--data -d`
Contenu du fichier.  Destiné à un contenu de texte simple.
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
## <a name="azdata-bdc-hdfs-cat"></a>azdata bdc hdfs cat
Lisez le contenu d’un fichier.  Le décalage et la longueur en octets sont des paramètres facultatifs.
```bash
azdata bdc hdfs cat --path -p 
                    --offset  
                    
--length -l
```
### <a name="examples"></a>Exemples
Lisez le fichier.
```bash
azdata bdc hdfs cat --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Nom du fichier à lire.
#### `--offset`
Nombre d’octets décalés dans le fichier à lire.
#### `--length -l`
Longueur des données à lire.
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
## <a name="azdata-bdc-hdfs-rm"></a>azdata bdc hdfs rm
Supprimez un fichier ou un répertoire.
```bash
azdata bdc hdfs rm --path -p 
                   
```
### <a name="examples"></a>Exemples
Supprimez un fichier ou un répertoire.
```bash
azdata bdc hdfs rm --path "tmp/"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Nom du fichier à supprimer.
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
## <a name="azdata-bdc-hdfs-rmr"></a>azdata bdc hdfs rmr
Supprimez de manière récursive un fichier ou un répertoire.
```bash
azdata bdc hdfs rmr --path -p 
                    
```
### <a name="examples"></a>Exemples
Suppression récursive du répertoire.
```bash
azdata bdc hdfs rmr --path "tmp/"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Nom du fichier à supprimer de façon récursive.
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
## <a name="azdata-bdc-hdfs-chmod"></a>azdata bdc hdfs chmod
Modifiez l’autorisation sur le fichier ou le répertoire spécifié.
```bash
azdata bdc hdfs chmod --path -p 
                      --permission
```
### <a name="examples"></a>Exemples
Modifiez l’autorisation du fichier ou du répertoire.
```bash
azdata bdc hdfs chmod --permission 775 --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Nom du fichier ou du répertoire sur lequel définir les autorisations.
#### `--permission`
Octets d’autorisation à définir.  Exemple « 775 ».
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
## <a name="azdata-bdc-hdfs-chown"></a>azdata bdc hdfs chown
Modifiez le propriétaire ou le groupe du fichier spécifié.
```bash
azdata bdc hdfs chown --path -p 
                      --owner  
                      
--group -g
```
### <a name="examples"></a>Exemples
Modifiez le propriétaire et le groupe.
```bash
azdata bdc hdfs chown --owner hdfs --group superusergroup --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--path -p`
Nom du fichier ou du répertoire pour lequel modifier le propriétaire.
#### `--owner`
Le nom du propriétaire à définir.
#### `--group -g`
Nom du groupe à définir.
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
## <a name="azdata-bdc-hdfs-cp"></a>azdata bdc hdfs cp
Copiez un fichier ou un répertoire entre l’ordinateur local et HDFS.  Si l’entrée est un répertoire, la totalité de l’arborescence du répertoire est copiée.  Si le fichier ou le répertoire cible existe, la commande échoue.  Pour spécifier le répertoire HDFS distant, préfixez le chemin avec « hdfs: »
```bash
azdata bdc hdfs cp --from-path -f 
                   --to-path -t
```
### <a name="examples"></a>Exemples
Copiez un fichier ou un répertoire entre l’ordinateur local et HDFS.
```bash
azdata bdc hdfs cp --from_path "tmp/test.txt" --to-path "hdfs:/user/me/test.txt"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--from-path -f`
Nom du chemin de l’emplacement à partir duquel effectuer la copie.  Préfixez le chemin avec « hdfs: » pour indiquer un chemin HDFS.
#### `--to-path -t`
Nom du chemin de l’emplacement vers lequel effectuer la copie.  Préfixez le chemin avec « hdfs: » pour indiquer un chemin HDFS.
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

