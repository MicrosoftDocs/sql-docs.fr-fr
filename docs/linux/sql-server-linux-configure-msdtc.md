---
title: Comment configurer MSDTC sur Linux
description: Dans cet article, découvrez comment configurer Microsoft Distributed Transaction Coordinator (MSDTC) sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 08/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: a2013fb1e32d14bed4b92aa0cc2394ad3a5c9174
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349121"
---
# <a name="how-to-configure-the-microsoft-distributed-transaction-coordinator-msdtc-on-linux"></a>Configurer Microsoft Distributed Transaction Coordinator (MSDTC) sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Cet article explique comment configurer Microsoft Distributed Transaction Coordinator (MSDTC) sur Linux.

> [!NOTE]
> MSDTC sur Linux est pris en charge sur SQL Server 2017 à compter de la mise à jour cumulative 16.

## <a name="overview"></a>Vue d’ensemble

Les transactions distribuées sont activées pour SQL Server sur Linux en introduisant la fonctionnalité MSDTC et RPC dans SQL Server. Par défaut, un processus de mappage des points de terminaison RPC écoute sur le port 135 les requêtes RPC entrantes, et fournit aux requêtes distantes des informations sur les composants enregistrés. Les requêtes à distance peuvent utiliser les informations renvoyées par le mappeur de points de terminaison afin de communiquer avec les composants RPC enregistrés, par exemple les services MSDTC. Un processus nécessite des privilèges de super utilisateur pour se lier à des ports bien connus (numéros de port inférieurs à 1024) sur Linux. Afin d’éviter de démarrer SQL Server avec les privilèges root pour le processus de mappage des points de terminaison RPC, les administrateurs système doivent utiliser des règles iptable et créer la conversion d'adresses réseau afin de router le trafic sur le port 135 vers le processus de mappage des points de terminaison RPC de SQL Server.

MSDTC utilise deux paramètres de configuration pour l'utilitaire mssql-conf :

| paramaètre mssql-conf | Description |
|---|---|
| **network.rpcport** | Le port TCP auquel se lie le processus de mappage de points de terminaison RPC. |
| **distributedtransaction.servertcpport** | Le port sur lequel le serveur MSDTC écoute. S'il n’est pas configuré, le service MSDTC utilise un port éphémère aléatoire au redémarrage du service, et les exceptions du pare-feu devront être reconfigurées pour garantir que le service MSDTC continue à communiquer. |

Pour plus d'informations sur ces paramètres et d'autres paramètres MSDTC connexes, voir [Configurer SQL Server sur Linux avec l'outil mssql-conf](sql-server-linux-configure-mssql-conf.md).

## <a name="supported-transaction-standards"></a>Normes de transaction prises en charge

Les configurations MSDTC prises en charge sont les suivantes :

| Norme de transaction | Sources de données | Pilote ODBC | Pilote JDBC|
|---|---|---|---|
| Transactions OLE-TX | SQL Server sur Linux | Oui | Non|
| Transactions distribuées XA | SQL Server, autres bases de données ODBC et sources de données JDBC qui prennent en charge XA | Oui (nécessite la version 17.3 ou supérieure) | Oui |
| Transactions distribuées sur le serveur Linked | SQL Server | Oui | Non

Pour plus d'informations, consultez [Compréhension des transactions XA](../connect/jdbc/understanding-xa-transactions.md#configuration-instructions).

## <a name="msdtc-configuration-steps"></a>Étapes de configuration MSDTC

Il existe trois étapes pour configurer la communication et la fonctionnalité MSDTC. Si les étapes de configuration nécessaires ne sont pas effectuées, SQL Server n'activera pas la fonctionnalité MSDTC.

- Configurez **network.rpcport** et **distributedtransaction.servertcpport** avec mssql-conf.
- Configurez le pare-feu pour autoriser la communication sur **distributedtransaction.servertcpport** et le port 135.
- Configurez le routage du serveur Linux pour que la communication RPC sur le port 135 soit redirigée vers **network.rpcport** sur SQL Server.

Les sections qui suivent fournissent des informations détaillées sur chaque étape.

## <a name="configure-rpc-and-msdtc-ports"></a>Configurer les ports RPC et MSDTC

Configurez d’abord **network.rpcport** et **distributedtransaction.servertcpport** avec mssql-conf. Cette étape est spécifique à SQL Server et commune à toutes les distributions prises en charge.

1. Utilisez mssql-conf pour définir la valeur **network.rpcport**. L'exemple suivant utilise la valeur 13500.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set network.rpcport 13500
   ```

2. Définissez la valeur **distributedtransaction.servertcpport**. L'exemple suivant utilise la valeur 51999.

   ```bash
   sudo /opt/mssql/bin/mssql-conf set distributedtransaction.servertcpport 51999
   ```

3. Redémarrez SQL Server.

   ```bash
   sudo systemctl restart mssql-server
   ```

## <a name="configure-the-firewall"></a>Configurer le pare-feu

La deuxième étape consiste à configurer le pare-feu pour autoriser la communication sur **servertcpport** et le port 135.  Cela permet au processus de mappage des points de terminaison RPC et au processus MSDTC de communiquer de façon externe avec d'autres gestionnaires et coordonnateurs de transactions. Les étapes réelles de cette opération varient selon votre distribution Linux et votre pare-feu. 

L’exemple suivant montre comment créer ces règles sur **Ubuntu**.

```bash
sudo ufw allow from any to any port 51999 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

L’exemple suivant montre comment effectuer cette opération sur **Red Hat Enterprise Linux (RHEL)** :

```bash
sudo firewall-cmd --zone=public --add-port=51999/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

Il est important de configurer le pare-feu avant de configurer le routage des ports dans la section suivante. L’actualisation du pare-feu peut effacer les règles de routage des ports dans certains cas.

## <a name="configure-port-routing"></a>Configurer le routage du port

Configurez la table de routage du serveur Linux pour que la communication RPC sur le port 135 soit redirigée vers **network.rpcport** sur SQL Server. Le mécanisme de configuration pour le transfert de port sur une autre distribution peut varier. Les sections suivantes fournissent des conseils pour Ubuntu, SUS Enterprise Linux (SLES) et Red Hat Enterprise Linux (RHEL).

### <a name="port-routing-in-ubuntu-and-sles"></a>Routage de port dans Ubuntu et SLES

Ubuntu et SLES n'utilisent pas le service **firewalld**, les règles **iptable** constituent donc un mécanisme efficace pour le routage des ports. Comme les règles **iptable** peuvent ne pas persister pendant le redémarrage, les commandes suivantes fournissent également des instructions pour restaurer les règles après un redémarrage.

1. Créez des règles de routage pour le port 135. Dans l'exemple suivant, le port 135 est dirigé vers le port RPC, 13500, défini dans la section précédente. Remplacez `<ipaddress>` par l'adresse IP de votre serveur.

   ```bash
   sudo iptables -t nat -A PREROUTING -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL  \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   sudo iptables -t nat -A OUTPUT -d <ip> -p tcp --dport 135 -m addrtype --dst-type LOCAL \
      -j DNAT --to-destination <ip>:13500 -m comment --comment RpcEndPointMapper
   ```

   Le paramètre `--comment RpcEndPointMapper` des commandes précédentes facilite la gestion de ces règles dans les commandes ultérieures.

2. Affichez les règles de routage que vous avez créées avec la commande suivante :

   ```bash
   sudo iptables -S -t nat | grep "RpcEndPointMapper"
   ```

3. Enregistrez les règles de routage dans un fichier sur votre machine.

   ```bash
   sudo iptables-save > /etc/iptables.conf
   ```

4. Pour recharger les règles après un redémarrage, ajoutez la commande suivante à `/etc/rc.local` (pour Ubuntu) ou à `/etc/init.d/after.local` (pour SLES) :

   ```bash
   iptables-restore < /etc/iptables.conf
   ```

   > [!NOTE]
   > Vous devez disposer de privilèges super utilisateur (sudo) pour modifier les fichiers **rc.local** ou **après.local**.

Les commandes **iptables-save** et **iptables-restore**, ainsi que la configuration de démarrage `rc.local`/`after.local` fournissent un mécanisme de base pour sauvegarder et restaurer les entrées iptables. Votre distribution Linux peut inclure des options plus avancées ou automatisées. Par exemple, une alternative Ubuntu est le package **iptables-persistent** qui rend les entrées persistantes.

> [!IMPORTANT]
> Les étapes précédentes supposent une adresse IP fixe. Si l'adresse IP de votre instance SQL Server change (suite à une intervention manuelle ou DHCP), vous devez supprimer et recréer les règles de routage si elles ont été créées avec des règles iptable. Si vous devez recréer ou supprimer des règles de routage existantes, vous pouvez utiliser la commande suivante pour supprimer les anciennes règles `RpcEndPointMapper` :
> 
> ```bash
> sudo iptables -S -t nat | grep "RpcEndPointMapper" | sed 's/^-A //' | while read rule; do iptables -t nat -D $rule; done
> ```

### <a name="port-routing-in-rhel"></a>Routage de port dans RHEL

Sur les distributions qui utilisent le service **firewalld**, comme Red Hat Enterprise Linux, le même service peut être utilisé pour ouvrir le port sur le serveur et pour le transfert de port interne. Par exemple, sur Red Hat Enterprise Linux, vous devez utiliser le service **firewalld** (via l’utilitaire de configuration **firewall-cmd** avec `-add-forward-port` ou des options similaires) pour créer et gérer des règles de redirection de ports persistantes au lieu d'utiliser des règles iptable.

```bash
sudo firewall-cmd --permanent --add-forward-port=port=135:proto=tcp:toport=13500
sudo firewall-cmd --reload
```

## <a name="verify"></a>Vérification

À ce stade, SQL Server devrait être en mesure de participer aux transactions distribuées. Pour vérifier que SQL Server est à l'écoute, exécutez la commande **netstat** (si vous utilisez RHEL, vous devrez peut-être d'abord installer le package **net-tools**) :

```bash
sudo netstat -tulpn | grep sqlservr
```

Vous devez voir une sortie de ce type :

```bash
tcp 0 0 0.0.0.0:1433 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 127.0.0.1:1434 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:13500 0.0.0.0:* LISTEN 13911/sqlservr
tcp 0 0 0.0.0.0:51999 0.0.0.0:* LISTEN 13911/sqlservr
tcp6 0 0 :::1433 :::* LISTEN 13911/sqlservr
tcp6 0 0 ::1:1434 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::13500 :::* LISTEN 13911/sqlservr
tcp6 0 0 :::51999 :::* LISTEN 13911/sqlservr
```

Cependant, après un redémarrage, SQL Server ne commence pas à écouter sur **servertcpport** avant la première transaction distribuée. Dans ce cas, vous ne verrez pas SQL Server écouter sur le port 51999 dans cet exemple jusqu'à la première transaction distribuée.

## <a name="configure-authentication-on-rpc-communication-for-msdtc"></a>Configurer l'authentification sur la communication RPC pour MSDTC

MSDTC pour SQL Server sur Linux n'utilise pas l'authentification sur la communication RPC par défaut. Cependant, lorsque la machine hôte est connectée à un domaine Active Directory (AD), il est possible de configurer MSDTC pour utiliser une communication RPC authentifiée à l’aide des paramètres **mssql-conf** suivants :

| Paramètre | Description |
|---|---|
| **distributedtransaction.allowonlysecurerpccalls**          | Configurer les appels sécurisés RPC pour les transactions distribuées. La valeur par défaut est 0. |
| **distributedtransaction.fallbacktounsecurerpcifnecessary** | Configurer les appels de sécurité RPC pour les transactions distribuées. La valeur par défaut est 0. |
| **distributedtransaction.turnoffrpcsecurity**               | Activer ou désactiver la sécurité RPC pour les transactions distribuées. La valeur par défaut est 0. |

## <a name="additional-guidance"></a>Conseils supplémentaires

### <a name="active-directory"></a>Active Directory

Microsoft recommande d’utiliser MSDTC avec RPC activé si SQL Server est inscrit dans une configuration Active Directory (AD). Si SQL Server est configuré pour utiliser l’authentification AD, MSDTC utilise la sécurité RPC de l’authentification mutuelle par défaut.

### <a name="windows-and-linux"></a>Windows et Linux

Si un client sur un système d’exploitation Windows doit s’inscrire dans une transaction distribuée avec SQL Server sur Linux, il doit avoir installé la version minimale suivante du système d’exploitation Windows :

| Système d’exploitation | Version minimale | Build de système d’exploitation |
|---|---|---|
| [Windows Server](/windows-server/get-started/windows-server-release-info) | 1903 | 18362.30.190401-1528 |
| [Windows 10](/windows/release-information/) | 1903 | 18362.267 |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur SQL Server sur Linux, consultez [SQL Server sur Linux](sql-server-linux-overview.md).