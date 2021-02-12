---
title: Informations de référence sur azdata sql
titleSuffix: SQL Server big data clusters
description: Article de référence sur les commandes azdata sql.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a7d142ca6afb77539b1425caaf097aca3959baeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048699"
---
# <a name="azdata-sql"></a>azdata sql

S'applique à l'[!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

L’article suivant fournit des informations de référence sur les commandes **sql** dans l’outil **azdata**. Pour plus d’informations sur les commandes **azdata**, consultez [azdata reference](reference-azdata.md)

## <a name="commands"></a>Commandes

|Commande|Description|
| --- | --- |
[azdata sql shell](#azdata-sql-shell) | L’interface de ligne de commande SQL permet à l’utilisateur d’interagir avec SQL Server et Azure SQL via T-SQL.
[azdata sql query](#azdata-sql-query) | L’interface de ligne de commande SQL permet à l’utilisateur d’interagir avec SQL Server et Azure SQL via T-SQL.
## <a name="azdata-sql-shell"></a>azdata sql shell
L’interface de ligne de commande SQL permet à l’utilisateur d’interagir avec SQL Server et Azure SQL via T-SQL.
```bash
azdata sql shell [--username -u] 
                 [--database -d]  
                 
[--server -s]  
                 
[--integrated -e]  
                 
[--mssqlclirc]  
                 
[--row-limit]  
                 
[--less-chatty]  
                 
[--auto-vertical-output]  
                 
[--encrypt -n]  
                 
[--trust-server-certificate -c]  
                 
[--connect-timeout -l]  
                 
[--application-intent -k]  
                 
[--multi-subnet-failover -m]  
                 
[--packet-size]  
                 
[--dac-connection -a]  
                 
[--input-file -i]  
                 
[--output-file]  
                 
[--enable-sqltoolsservice-logging]  
                 
[--prompt]
```
### <a name="examples"></a>Exemples
Exemple de ligne de commande pour démarrer l’expérience interactive.
```bash
azdata sql shell
```
Exemple de ligne de commande utilisant un serveur, un utilisateur et une base de données fournis
```bash
azdata sql shell --server localhost --username sa --database master         
```
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--username -u`
Nom d’utilisateur permettant de se connecter à la base de données.
#### `--database -d`
Nom de la base de données à laquelle se connecter.
#### `--server -s`
Nom ou adresse de l’instance SQL Server.
#### `--integrated -e`
Utilisez l’authentification intégrée sur Windows.
#### `--mssqlclirc`
Emplacement du fichier de configuration mssqlclirc.
#### `--row-limit`
Définit le seuil de l’invite de limite de lignes. Utilisez 0 pour désactiver l’invite.
#### `--less-chatty`
Ignorez le message d’introduction au démarrage et le message d’au revoir à la sortie.
#### `--auto-vertical-output`
Basculez automatiquement en mode de sortie verticale si le résultat est plus large que la largeur du terminal.
#### `--encrypt -n`
SQL Server utilise le chiffrement SSL pour toutes les données si le serveur a un certificat installé.
#### `--trust-server-certificate -c`
Le canal sera chiffré en contournant la chaîne de certificats pour valider l’approbation.
#### `--connect-timeout -l`
Délai d’attente d’une connexion au serveur avant l’arrêt de la requête, en secondes.
#### `--application-intent -k`
Déclare le type de charge de travail de l’application pendant la connexion à une base de données dans un groupe de disponibilité SQL Server.
#### `--multi-subnet-failover -m`
Si l’application se connecte aux groupes de disponibilité AlwaysOn sur des sous-réseaux différents, cette option permet d’accélérer la détection et la connexion au serveur actuellement actif.
#### `--packet-size`
Taille en octets des paquets réseau utilisés pour communiquer avec SQL Server.
#### `--dac-connection -a`
Connectez-vous à SQL Server par le biais d’une connexion administrateur dédiée.
#### `--input-file -i`
Indique quel fichier contient un lot d’instructions SQL à traiter.
#### `--output-file`
Indique quel fichier reçoit une sortie d’une requête.
#### `--enable-sqltoolsservice-logging`
Active la journalisation des diagnostics pour SqlToolsService.
#### `--prompt`
Format de l’invite (par défaut : \d >
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
## <a name="azdata-sql-query"></a>azdata sql query
L’interface de ligne de commande SQL permet à l’utilisateur d’interagir avec SQL Server et Azure SQL via T-SQL.
```bash
azdata sql query -q 
                 [--database -d]  
                 
[--username -u]  
                 
[--server -s]  
                 
[--integrated -e]
```
### <a name="examples"></a>Exemples
Exemple de ligne de commande pour sélectionner la liste des noms de tableaux.
```bash
azdata sql query --server localhost --username sa --database master -q "SELECT name FROM SYS.TABLES"
```
### <a name="required-parameters"></a>Paramètres obligatoires
#### `-q`
Requête T-SQL à exécuter.
### <a name="optional-parameters"></a>Paramètres facultatifs
#### `--database -d`
Nom de la base de données à laquelle se connecter.
`master`
#### `--username -u`
Nom d’utilisateur permettant de se connecter à la base de données.
#### `--server -s`
Nom ou adresse de l’instance SQL Server.
#### `--integrated -e`
Utilisez l’authentification intégrée sur Windows.
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

