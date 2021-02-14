---
title: Informations de référence sur azdata postgres
titleSuffix: SQL Server big data clusters
description: Article d’informations de référence sur les commandes azdata postgres.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6403db5f9b716684c3a791ade183a8f9c5edad47
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052270"
---
# <a name="azdata-postgres"></a>azdata postgres

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[interpréteur de commandes azdata postgres](#azdata-postgres-shell) | Interface de l’interpréteur de ligne de commande pour Postgres. Voir https://www.pgcli.com/
[requête azdata postgres](#azdata-postgres-query) | La commande de requête permet l’exécution de commandes PostgreSQL dans une session de base de données.
## <a name="azdata-postgres-shell"></a>interpréteur de commandes azdata postgres
Interface de l’interpréteur de ligne de commande pour Postgres. Voir https://www.pgcli.com/
```bash
azdata postgres shell [--dbname -d] 
                      [--host]  
                      
[--port -p]  
                      
[--password -w]  
                      
[--no-password]  
                      
[--single-connection]  
                      
[--username -u]  
                      
[--pgclirc]  
                      
[--dsn]  
                      
[--list-dsn]  
                      
[--row-limit]  
                      
[--less-chatty]  
                      
[--prompt]  
                      
[--prompt-dsn]  
                      
[--list -l]  
                      
[--auto-vertical-output]  
                      
[--warn]  
                      
[--no-warn]
```
### <a name="examples"></a>Exemples
Exemple de ligne de commande pour démarrer l’expérience interactive.
```bash
azdata postgres shell
```
Exemple de ligne de commande utilisant une base de données et un utilisateur fournis
```bash
azdata postgres shell --dbname <database> --username <username> --host <host>
```
Exemple de ligne de commande pour commencer à utiliser une chaîne de connexion complète.
```bash
azdata postgres shell --dbname postgres://user:passw0rd@example.com:5432/master 
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--dbname -d`
Nom de la base de données à laquelle se connecter.
#### `--host`
Adresse d’hôte de la base de données postgres.
#### `--port -p`
Numéro de port pour l’écoute de l’instance postgres.
#### `--password -w`
Forcez l’invite de mot de passe.
#### `--no-password`
Ne demandez jamais le mot de passe.
#### `--single-connection`
N’utilisez pas de connexion distincte pour les complétions.
#### `--username -u`
Nom d’utilisateur permettant de se connecter à la base de données postgres.
#### `--pgclirc`
Emplacement du fichier pgclirc.
#### `--dsn`
Utilisez le nom de source de données configuré dans la section [alias_dsn] du fichier pgclirc.
#### `--list-dsn`
Liste du nom de source de données configuré dans la section [alias_dsn] du fichier pgclirc.
#### `--row-limit`
Définissez le seuil de l’invite de limite de lignes. Utilisez 0 pour désactiver l’invite.
#### `--less-chatty`
Ignorez le message d’introduction au démarrage et le message d’au revoir à la sortie.
#### `--prompt`
Format de l’invite (par défaut : \u@\h:\d>).
#### `--prompt-dsn`
Format de l’invite pour les connexions utilisant des alias de nom de source de données (défaut : \u@\h:\d>).
#### `--list -l`
Répertoriez les bases de données disponibles, puis quittez cette page.
#### `--auto-vertical-output`
Basculez automatiquement en mode de sortie verticale si le résultat est plus large que la largeur du terminal.
#### `--warn`
Avertissement affiché avant l’exécution d’une requête destructrice.
#### `--no-warn`
Avertissement affiché avant l’exécution d’une requête destructrice.
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
## <a name="azdata-postgres-query"></a>requête azdata postgres
La commande de requête permet l’exécution de commandes PostgreSQL dans une session de base de données.
```bash
azdata postgres query --q -q 
                      [--host]  
                      
[--dbname -d]  
                      
[--port -p]  
                      
[--username -u]
```
### <a name="examples"></a>Exemples
Répertoriez tous les tableaux dans information_schema.
```bash
azdata postgres query --host <host> --username <username> -q "SELECT * FROM information_schema.tables"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `--q -q`
Requête PostgreSQL à exécuter.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--host`
Adresse d’hôte de la base de données postgres.
`localhost`
#### `--dbname -d`
Base de données où exécuter la requête.
#### `--port -p`
Numéro de port pour l’écoute de l’instance postgres.
`5432`
#### `--username -u`
Nom d’utilisateur permettant de se connecter à la base de données postgres.
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

