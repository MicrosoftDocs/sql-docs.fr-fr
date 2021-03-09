---
title: Détecter un problème sur SQL Server sur Linux
description: Détectez un problème sur Microsoft SQL Server s’exécutant sur Linux ou dans un conteneur Docker. Découvrez où trouver des informations sur les fonctionnalités prises en charge et les limitations connues.
author: VanMSFT
ms.author: vanto
ms.date: 05/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 99636ee8-2ba6-4316-88e0-121988eebcf9S
ms.openlocfilehash: 1f2df7f7cb0660cb879334a1099f6b7ce639d211
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464635"
---
# <a name="troubleshoot-sql-server-on-linux"></a>Détecter un problème sur SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce document décrit comment détecter un problème sur Microsoft SQL Server s’exécutant sur Linux ou dans un conteneur Docker. Lorsque vous détectez un problème sur SQL Server sur Linux, n’oubliez pas de consulter les fonctionnalités prises en charge et les limitations connues dans les [Notes de publication de SQL Server sur Linux](sql-server-linux-release-notes.md).

> [!TIP]
> Pour obtenir des réponses aux questions fréquemment posées, consultez la [FAQ de SQL Server sur Linux](sql-server-linux-faq.yml).

## <a name="troubleshoot-connection-failures"></a><a id="connection"></a> Détecter un problème relatif aux échecs de connexion
Si vous avez des difficultés à vous connecter à votre SQL Server Linux, vous devez vérifier quelques points.

- Si vous ne parvenez pas à vous connecter localement à l’aide de **localhost**, essayez d’utiliser l’adresse IP 127.0.0.1 à la place. Il est possible que **localhost** ne soit pas correctement mappé à cette adresse.

- Vérifiez que le nom du serveur ou l’adresse IP est accessible à partir de votre machine client.

   > [!TIP]
   > Pour trouver l’adresse IP de votre machine Ubuntu, vous pouvez exécuter la commande ifconfig comme dans l’exemple suivant :
   >
   >   ```bash
   >   sudo ifconfig eth0 | grep 'inet addr'
   >   ```
   > Pour Red Hat, vous pouvez utiliser l’adresse IP comme dans l’exemple suivant :
   >
   >   ```bash
   >   sudo ip addr show eth0 | grep "inet"
   >   ```
   > Une exception à cette technique est associée aux machines virtuelles Azure. Pour les machines virtuelles Azure, [recherchez l’adresse IP publique de la machine virtuelle dans la Portail Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#connect).

- Le cas échéant, vérifiez que vous avez ouvert le port SQL Server (par défaut 1433) sur le pare-feu.

- Pour les machines virtuelles Azure, vérifiez que vous disposez d’une [règle de groupe de sécurité réseau pour le port SQL Server par défaut](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine#remote).

- Vérifiez que le nom d’utilisateur et le mot de passe ne contiennent aucune faute de frappe ni d’espace supplémentaire ni de casse incorrecte.

- Essayez de définir explicitement le protocole et le numéro de port avec le nom du serveur, comme dans l’exemple suivant : **tcp:servername,1433**.

- Des problèmes de connectivité réseau peuvent également entraîner des délais d’attente et des erreurs de connexion. Une fois que vous avez vérifié vos informations de connexion et la connectivité réseau, réessayez de vous connecter.

## <a name="manage-the-sql-server-service"></a>Gérer le service SQL Server

Les sections suivantes indiquent comment démarrer, arrêter, redémarrer et vérifier l’état du service SQL Server.

### <a name="manage-the-mssql-server-service-in-red-hat-enterprise-linux-rhel-and-ubuntu"></a>Gérer le service mssql-server dans Red Hat Enterprise Linux (RHEL) et Ubuntu 

Vérifiez l’état du service SQL Server à l’aide de cette commande :

   ```bash
   sudo systemctl status mssql-server
   ```

Vous pouvez arrêter, démarrer ou redémarrer le service SQL Server en fonction des besoins à l’aide des commandes suivantes :

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

### <a name="manage-the-execution-of-the-mssql-docker-container"></a>Gérer l’exécution du conteneur Docker mssql

Vous pouvez accéder à l’état et à l’ID de conteneur du dernier conteneur Docker SQL Server créé en exécutant la commande suivante (l’ID se trouve sous la colonne **ID DE CONTENEUR**) :

   ```bash
   sudo docker ps -l
   ```
   
Vous pouvez arrêter ou redémarrer le service SQL Server en fonction des besoins à l’aide des commandes suivantes :
   
   ```bash
   sudo docker stop <container ID>
   sudo docker restart <container ID>
   ```

> [!TIP]
> Pour plus de conseils sur la résolution des problèmes liés à Docker, consultez [Résolution des problèmes de conteneurs Docker SQL Server](./sql-server-linux-docker-container-troubleshooting.md).

## <a name="access-the-log-files"></a>Accéder aux fichiers journaux
   
Le moteur de SQL Server se connecte au fichier /var/opt/mssql/log/errorlog à la fois dans les installations Linux et Docker. Vous devez être en mode « superutilisateur » pour parcourir ce répertoire.

Le programme d’installation se connecte ici : /var/opt/mssql/setup-< horodatage représentant l’heure de l’installation > Vous pouvez parcourir les fichiers ErrorLog avec n’importe quel outil compatible à UTF-16, par exemple « vim » ou « cat » comme suit : 

   ```bash
   sudo cat errorlog
   ```

Si vous préférez, vous pouvez également convertir les fichiers au format UTF-8 pour les lire avec « plus » ou « moins » à l’aide de la commande suivante :
   
   ```bash
   sudo iconv -f UTF-16LE -t UTF-8 <errorlog> -o <output errorlog file>
   ```
## <a name="extended-events"></a>Événements étendus

Les événements étendus peuvent être interrogés à l’aide d’une commande SQL.  Plus d’informations sur les événements étendus peuvent être obtenues [ici](../relational-databases/extended-events/extended-events.md) :

## <a name="crash-dumps"></a>Vidages sur incident 

Recherchez des vidages sur incident dans le répertoire de journaux Linux. Vérifiez dans le répertoire /var/opt/mssql/log les vidages Linux Core (extension .tar.gz2) ou les minidumps SQL (extension .mdmp)

Pour les vidages Core 
   ```bash
   sudo ls /var/opt/mssql/log | grep .tar.gz2 
   ```

Pour les vidages SQL 
   ```bash
   sudo ls /var/opt/mssql/log | grep .mdmp 
   ```
   
## <a name="start-sql-server-in-minimal-configuration-or-in-single-user-mode"></a>Démarrer SQL Server en mode de configuration minimale ou en mode mono-utilisateur

### <a name="start-sql-server-in-minimal-configuration-mode"></a>Démarrer SQL Server en mode de configuration minimale
Cette option est utile lorsqu'une valeur de configuration définie (espace mémoire insuffisant, par exemple) a empêché le serveur de démarrer.
  
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -f
   ```

### <a name="start-sql-server-in-single-user-mode"></a>Démarrer SQL Server en mode mono-utilisateur
Dans certaines circonstances, vous devrez peut-être démarrer une instance de SQL Server en mode mono-utilisateur à l’aide de l’option de démarrage -m. Vous pouvez par exemple vouloir modifier les options de configuration du serveur ou rétablir une base de données master ou une autre base de données système endommagées. Vous pouvez par exemple vouloir modifier les options de configuration du serveur ou rétablir une base de données MASTER ou une autre base de données système endommagée   

Démarrer SQL Server en mode mono-utilisateur
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m
   ```

Démarrer SQL Server en mode mono-utilisateur avec SQLCMD
   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr -m SQLCMD
   ```
  
> [!WARNING]  
>  Démarrez SQL Server sur Linux avec l’utilisateur « mssql » afin d’éviter les problèmes de démarrage futurs. Exemple « sudo -u mssql /opt/mssql/bin/sqlservr [OPTIONS DE DÉMARRAGE] » 

Si vous avez démarré accidentellement SQL Server avec un autre utilisateur, vous devez remplacer la propriété des fichiers de base de données SQL Server par l’utilisateur « mssql » avant de démarrer SQL Server avec SystemD. Par exemple, pour modifier la propriété de tous les fichiers de base de données sous /var/opt/mssql pour l’utilisateur « mssql », exécutez la commande suivante

   ```bash
   chown -R mssql:mssql /var/opt/mssql/
   ```

## <a name="rebuild-system-databases"></a>Reconstruire des bases de données système
En dernier recours, vous pouvez choisir de régénérer les bases de données MASTER et model vers les versions par défaut.

> [!WARNING]
> Ces étapes **SUPPRIMENT toutes les données système SQL Server** que vous avez configurées ! Cela comprend des informations sur vos bases de données utilisateur (mais pas sur les bases de données utilisateur elles-mêmes). Elles suppriment également les autres informations stockées dans les bases de données système, y compris les informations suivantes : de clé principale, les certificats chargés dans le MASTER, le mot de passe de connexion SA, les informations relatives aux tâches de msdb, les informations de messagerie de la base de données de msdb et les options sp_configure. Utilisez uniquement si vous comprenez les implications !

1. Arrêtez SQL Server.

   ```bash
   sudo systemctl stop mssql-server
   ```

1. Exécutez **sqlservr** avec le paramètre **force-setup**. 

   ```bash
   sudo -u mssql /opt/mssql/bin/sqlservr --force-setup
   ```
   
   > [!WARNING]
   > Consultez l’avertissement précédent ! En outre, vous devez l'exécuter en tant qu’utilisateur **mssql** comme indiqué ici.

1. Une fois que vous voyez le message « La récupération est terminée », appuyez sur CTRL + C. Cela arrête SQL Server

1. Reconfigurez le mot de passe.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```
   
1. Démarrez SQL Server et reconfigurez le serveur. Cela comprend la restauration ou la réassociation de toutes les bases de données utilisateur.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="improve-performance"></a>Améliorer les performances

De nombreux facteurs affectent les performances, notamment la conception de bases de données, le matériel et les demandes de charge de travail. Si vous cherchez à améliorer les performances, commencez par consulter les meilleures pratiques dans l'article [Meilleures pratiques en matière de performances et lignes directrices de configuration pour SQL Server sur Linux](sql-server-linux-performance-best-practices.md). Explorez ensuite certains des outils disponibles pour résoudre les problèmes de performances.

- [Magasin de requêtes](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Vues de gestion dynamique système (DMV)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [Tableau de bord des performances dans SQL Server Management Studio](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in)

## <a name="common-issues"></a>Problèmes courants

1. Vous ne pouvez pas vous connecter à votre instance distante.

   Consultez la section Résolution des problèmes de l'article [Se connecter à SQL Server sur Linux](#connection).

2. ERREUR : Le nom d’hôte doit comporter 15 caractères au maximum.

   Il s’agit d’un problème connu qui se produit chaque fois que le nom de la machine qui tente d’installer le package SQL Server Debian comporte plus de 15 caractères. Il n’existe actuellement aucune solution de contournement autre que la modification du nom de la machine. Une façon d’y parvenir consiste à modifier le fichier de nom d’hôte et à redémarrer la machine. Le [Guide du site Web](https://www.cyberciti.biz/faq/ubuntu-change-hostname-command/) suivant explique cela en détail.

3. Réinitialisation du mot de passe d’administration de système (SA).

   Si vous avez oublié le mot de passe de l’administrateur système ou si vous devez le réinitialiser pour une raison quelconque, procédez comme suit.

   > [!NOTE]
   > Les étapes suivantes arrêtent temporairement le service SQL Server.

   Connectez-vous au terminal hôte, exécutez les commandes suivantes et suivez les invites pour réinitialiser le mot de passe SA :

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf setup
   ```

4. Utilisation de caractères spéciaux dans un mot de passe.

   Si vous utilisez des caractères dans le mot de passe de connexion SQL Server, vous devrez peut-être y ajouter un caractère d’échappement avec une barre oblique inverse quand vous les utilisez dans une commande Linux dans le terminal. Par exemple, vous devez y ajouter le caractère d’échappement signe dollar ($) chaque fois que vous l’utilisez dans un script de commande/shell de terminal :

   Ne fonctionne pas :

   ```bash
   sudo sqlcmd -S myserver -U sa -P Test$$
   ```

   Fonctionne :

   ```bash
   sqlcmd -S myserver -U sa -P Test\$\$
   ```

   Ressources : [Caractères spéciaux](https://tldp.org/LDP/abs/html/special-chars.html)
   [Échappement](https://tldp.org/LDP/abs/html/escapingsection.html)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]