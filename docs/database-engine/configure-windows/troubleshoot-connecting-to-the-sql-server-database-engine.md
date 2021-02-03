---
title: Résoudre les problèmes de connexion au moteur de base de données SQL Server
description: Découvrez comment résoudre des problèmes de connexion. Consultez les étapes à suivre lorsque vous ne pouvez pas utiliser TCP/IP pour vous connecter à un moteur de base de données SQL Server sur un serveur unique.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: troubleshooting
helpviewer_keywords:
- troubleshooting, connecting to Database Engine
- connecting to Database Engine, troubleshooting
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: sqlfreshmay19
ms.date: 11/25/2019
ms.openlocfilehash: cd4dae49fb19532e9aee082e4046c4d79dbf0a82
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237994"
---
# <a name="troubleshoot-connecting-to-the-sql-server-database-engine"></a>Résoudre les problèmes de connexion au moteur de base de données SQL Server

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cet article répertorie les techniques de dépannage à utiliser quand vous ne pouvez pas vous connecter à une instance du Moteur de base de données SQL Server sur un serveur unique.

>[!NOTE]
>Pour plus de scénarios, consultez :
>- [Écouteur de groupe de disponibilité](../availability-groups/windows/listeners-client-connectivity-application-failover.md)
>- [Instances de cluster de basculement](../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)

Ces étapes ne sont pas dans l’ordre des problèmes les plus probables que vous connaissez peut-être déjà. Ces étapes sont classées dans l’ordre des problèmes les plus simples aux plus complexes. Ces étapes supposent que vous vous connectez à l’instance SQL Server à partir d’un autre ordinateur à l’aide du protocole TCP/IP, ce qui est le cas le plus courant.

Ces instructions sont particulièrement utiles pour la résolution de l’erreur de « **connexion au serveur** », qui peut être `Error Number: 11001 (or 53), Severity: 20, State: 0`. L’exemple suivant montre un message d’erreur :

> `A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.`
>
> `(provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server) (Microsoft SQL Server, Error: 53)`
>
> `(provider: TCP Provider, error: 0 - No such host is known.) (Microsoft SQL Server, Error: 11001)`

Cette erreur signifie habituellement que le client ne parvient pas à trouver l’instance SQL Server. Cela se produit généralement quand au moins l’un des problèmes suivants se présente :

- Le nom de l’ordinateur qui héberge l’instance SQL Server
- L’instance ne résout pas la bonne adresse IP
- Le numéro de port TCP n’est pas spécifié correctement

> [!TIP]
> Une page de résolution interactive est disponible à partir des services de support technique de [!INCLUDE[msCoName_md](../../includes/msconame-md.md)] sur [Solving Connectivity errors to SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server).

## <a name="not-included"></a>Non inclus

- Cette rubrique n’inclut pas d’informations sur les erreurs SSPI. Pour plus d’informations sur les erreurs SSPI, consultez [Comment faire pour résoudre le message d’erreur « Impossible de générer le contexte SSPI »](https://support.microsoft.com/kb/811889).
- Cette rubrique n’inclut pas d’informations sur les erreurs Kerberos. Pour obtenir de l’aide, consultez [Microsoft Kerberos Configuration Manager pour SQL Server](https://www.microsoft.com/download/details.aspx?id=39046).
- Cette rubrique n’inclut pas d’informations sur la connectivité Azure SQL Database. Pour obtenir de l’aide, consultez [Dépannage des problèmes de connectivité avec Microsoft Azure SQL Database](/azure/sql-database/troubleshoot-connectivity-issues-microsoft-azure-sql-database).

## <a name="get-instance-name-from-configuration-manger"></a>Obtenir le nom de l’instance du Gestionnaire de configuration

Sur le serveur qui héberge l’instance SQL Server, vérifiez le nom de l’instance. Utilisez le [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).

Le Gestionnaire de configuration est automatiquement installé sur l’ordinateur quand SQL Server est installé. Les instructions sur le démarrage du Gestionnaire de configuration varient légèrement selon la version de SQL Server et Windows. Pour plus d’informations spécifiques sur la version, consultez [Gestionnaire de configuration SQL Server](../../relational-databases/sql-server-configuration-manager.md).)

1. Se connecter à l’ordinateur qui héberge l’instance de SQL Server.
2. Démarrez le Gestionnaire de configuration SQL Server.
3. Dans le volet gauche, sélectionnez **Services SQL Server**.
4. Dans le volet de droite, vérifiez le nom de l’instance du moteur de base de données.

   - `SQL SERVER (MSSQLSERVER)` dénote une instance par défaut de SQL Server. Le nom de l’instance par défaut est `<computer name>`.
   - `SQL SERVER (<instance name>)` dénote une instance nommée de SQL Server. Le nom de l'instance du nom est `<computer name>\<instance name>`

## <a name="verify---the-instance-is-running"></a>Vérifiez que le service est en cours d'exécution

Pour vérifier que l’instance est en cours d’exécution, dans le Gestionnaire de configuration, examinez le symbole par l’instance SQL Server.

- Une flèche verte indique qu’une instance est en cours d’exécution.
- Un carré rouge, indique qu’une instance est arrêtée.

Si l’instance est arrêtée, cliquez avec le bouton de droite sur l’instance, puis cliquez sur **Démarrer**. L’instance de serveur démarre et l’indicateur devient une flèche verte.

## <a name="verify---sql-server-browser-service-is-running"></a><a name = "startbrowser"></a> Vérifier que le service SQL Server Browser est en cours d’exécution

Pour vous connecter à une instance nommée, le service SQL Server Browser doit être en cours d’exécution. Dans le Gestionnaire de configuration, recherchez le service **SQL Server Browser** et vérifiez qu’il s’exécute. Si elle n’est pas en cours d’exécution, démarrez-la. Le service SQL Server Browser n’est pas requis pour les instances par défaut.

Une instance par défaut de SQL Server ne nécessite pas le service SQL Server Browser.

## <a name="testing-a-local-connection"></a>Test d’une connexion locale

Avant de résoudre un problème de connexion à partir d’un autre ordinateur, commencez par tester votre capacité à vous connecter à partir d’une application cliente installée localement sur l’ordinateur qui exécute SQL Server. La connexion locale évite les problèmes avec les réseaux et les pare-feux. 

Cette procédure utilise SQL Server Management Studio. Si Management Studio n’est pas installé, consultez [Télécharger SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md). Si vous n’êtes pas en mesure d’installer Management Studio, vous pouvez tester la connexion en utilisant l’utilitaire `sqlcmd.exe`. `sqlcmd.exe` est installé avec le Moteur de base de données. Pour plus d’informations sur `sqlcmd.exe`, consultez [Utilitaire sqlcmd](../../tools/sqlcmd-utility.md).)

1. Connectez-vous sur l’ordinateur où SQL Server est installé à l’aide d’une connexion qui a l’autorisation d’accéder à SQL Server. (Lors de l’installation, SQL Server nécessite au moins une connexion à spécifier en tant qu’administrateur SQL Server. Si vous ne connaissez pas d’administrateur, consultez [Se connecter à SQL Server lorsque les administrateurs système n’y ont plus accès](connect-to-sql-server-when-system-administrators-are-locked-out.md).)
2. Dans la page de démarrage, tapez **SQL Server Management Studio** ou, sur des versions plus anciennes de Windows, dans le menu Démarrer, pointez sur **Tous les programmes**, sur **Microsoft SQL Server**, puis cliquez sur **SQL Server Management Studio**.
3. Dans la zone **Type de serveur** de la boîte de dialogue **Se connecter au serveur** , sélectionnez **Moteur de base de données**. Dans la zone **Authentification** , sélectionnez **Authentification Windows**. Dans la boîte **Nom du serveur**, tapez l’un des types de connexion suivants :

   |Connexion à|Type|Exemple|
   |:-----------------|:---------------|:-----------------|
   |Instance par défaut|`<computer name>`|`ACCNT27`|
   |Instance nommée|`<computer name\instance name>`|`ACCNT27\PAYROLL`|

   > [!NOTE]
   > Lorsque vous vous connectez à un serveur SQL Server à partir d’une application cliente sur le même ordinateur, le protocole de mémoire partagée est utilisé. La mémoire partagée étant un type de canal nommé local, des erreurs relatives aux canaux se produisent parfois.

   Si vous recevez une erreur à ce stade, vous devez la résoudre avant de continuer. Il existe de nombreuses sources de problèmes possibles. Votre connexion peut être refusée. Votre base de données par défaut peut-être absente.

   > [!NOTE]
   > Certains messages d’erreur transmis au client intentionnellement ne donnent pas suffisamment d’informations pour résoudre le problème. Il s’agit d’une fonctionnalité de sécurité pour éviter de fournir à un attaquant des informations sur SQL Server. Pour afficher les informations complètes sur l’erreur, consultez le journal des erreurs SQL Server. Les détails y sont fournis. 

4. Si vous recevez l’erreur `18456 Login failed for user`, la rubrique Documentation en ligne [MSSQLSERVER_18456](../../relational-databases/errors-events/mssqlserver-18456-database-engine-error.md) contient des informations supplémentaires sur les codes d’erreur. En outre, le blog d’Aaron Bertrand comprend une liste complète des codes d’erreur à la page [Troubleshooting Error 18456](https://sqlblog.org/2011/01/14/troubleshooting-error-18456). Vous pouvez afficher le journal des erreurs avec SSMS (si vous pouvez vous connecter), dans la section Gestion de l’Explorateur d’objets. Sinon, vous pouvez afficher le journal des erreurs avec le programme Bloc-notes de Windows. L’emplacement par défaut varie en fonction de votre version et peut être modifié pendant l’installation. L’emplacement par défaut pour [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] est `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\Log\ERRORLOG`. 

5. Si vous pouvez vous connecter à l’aide de la mémoire partagée, testez la connexion à l’aide de TCP. Vous pouvez forcer une connexion TCP en spécifiant `tcp:` avant le nom. Par exemple :

   |Connexion à :|Tapez :|Exemple :|
   |-----------------|---------------|-----------------|
   |Instance par défaut|`tcp:<computer name>`|`tcp:ACCNT27`|
   |Instance nommée|`tcp:<computer name/instance name>`|`tcp:ACCNT27\PAYROLL`|

6. Si vous pouvez vous connecter avec la mémoire partagée mais pas avec TCP, vous devez corriger le problème TCP. La cause la plus probable est que TCP n’est pas activé. Pour activer TCP, consultez la procédure [Activer les protocoles](#enableprotocols) ci-dessus.

7. Si votre objectif est de vous connecter avec un compte autre qu’un compte d’administrateur, une fois que vous pouvez vous connecter en tant qu’administrateur, essayez à nouveau la connexion d’authentification Windows ou la connexion d’authentification SQL Server qui sera utilisée par l’application cliente.

## <a name="get-the-ip-address-of-the-server"></a>Obtenir l’adresse IP du serveur

Obtenir l’adresse IP de l’ordinateur hébergeant l’instance de SQL Server.

1. Dans le menu Démarrer, cliquez sur **Exécuter**. Dans la fenêtre **Exécuter**, tapez **cmd**, puis cliquez sur **OK**.
2. Dans la fenêtre d’invite de commandes, tapez `ipconfig` et appuyez sur Entrée. Notez l’adresse **IPv4** et l’adresse **IPv6** .

  >SQL Server peut se connecter à l’aide du protocole IP version 4 ou du protocole IP version 6. Votre réseau peut autoriser l’un des deux, ou les deux. La plupart des personnes commencent par la résolution des problèmes de l’adresse **IPv4** . Elle est plus courte et plus simple à taper.

## <a name="get-the-sql-server-instance-tcp-port"></a><a name = "getTCP"></a>Obtenir le port TCP de l’instance SQL Server

Dans la plupart des cas, vous vous connectez au moteur de base de données à partir d’un autre ordinateur en utilisant le protocole TCP.

1. En utilisant SQL Server Management Studio sur l’ordinateur exécutant SQL Server, connectez-vous à l’instance de SQL Server. Dans l’Explorateur d’objets, développez **Gestion**, **Journaux SQL Server**, puis double-cliquez sur le journal actuel.
2. Dans la visionneuse du journal, cliquez sur le bouton **Filtrer** dans la barre d’outils. Dans la boîte **Le message contient du texte**, tapez `server is listening on`, cliquez sur **Appliquer le filtre** puis cliquez sur **OK**.
3. Un message similaire à `Server is listening on [ 'any' <ipv4> 1433]` doit être répertorié. 

Ce message indique que cette instance de SQL Server écoute sur toutes les adresses IP de cet ordinateur (pour le protocole IP version 4) et écoute le port TCP 1433. (Le port TCP 1433 est généralement le port utilisé par le moteur de base de données ou une instance par défaut de SQL Server. Une seule instance de SQL Server pouvant utiliser un port, certaines instances doivent utiliser d’autres numéros de port si plusieurs instances de SQL Server sont installées.) Notez le numéro de port utilisé par l’instance [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] à laquelle vous essayez de vous connecter.

  > [!NOTE]
  > `IP address 127.0.0.1` est probablement répertoriée. C’est ce qu’on appelle l’adresse de l’adaptateur de bouclage. Seuls les processus sur le même ordinateur peuvent l’utiliser pour se connecter. Elle peut être utile pour la résolution des problèmes, mais vous ne pouvez pas l’utiliser pour vous connecter à partir d’un autre ordinateur.

## <a name="enable-protocols"></a><a name = "enableprotocols"></a>Activer les protocoles

Dans certaines installations de SQL Server, la connexion au moteur de base de données à partir d’un autre ordinateur est désactivée, sauf si un administrateur utilise le Gestionnaire de configuration pour l’activer. Pour activer des connexions à partir d’un autre ordinateur :

1. Ouvrez le Gestionnaire de configuration SQL Server, comme décrit précédemment.
2. En utilisant le Gestionnaire de configuration, dans le volet gauche, développez **Configuration du réseau SQL Server**, puis sélectionnez l’instance de SQL Server à laquelle vous voulez vous connecter. Le volet droit répertorie les protocoles de connexion disponibles. La mémoire partagée est normalement activée. Comme elle peut uniquement être utilisée à partir du même ordinateur, la plupart des installations laissent la mémoire partagée activée. Pour vous connecter à SQL Server à partir d’un autre ordinateur, vous utilisez normalement TCP/IP. Si TCP/IP n’est pas activé, cliquez avec le bouton droit sur **TCP/IP**, puis cliquez sur **Activer**.
3. Si vous avez modifié le paramètre activé pour n’importe quel protocole, redémarrez le Moteur de base de données. Dans le volet gauche, sélectionnez **Services SQL Server**. Dans le volet droit, cliquez avec le bouton droit sur l’instance du moteur de base de données, puis cliquez sur **Redémarrer**.

## <a name="testing-tcpip-connectivity"></a><a name="testTCPIP"></a>Test de la connectivité TCP/IP

La connexion à SQL Server à l’aide de TCP/IP exige que Windows puisse établir la connexion. Utilisez l’outil `ping` pour tester TCP.

1. Dans le menu Démarrer, cliquez sur **Exécuter**. Dans la fenêtre **Exécuter** , tapez **cmd**, puis cliquez sur **OK**. 
2. Dans la fenêtre d’invite de commandes, tapez `ping <ip address>` , puis l’adresse IP de l’ordinateur qui exécute SQL Server. Par exemple :

    - IPv4 : `ping 192.168.1.101`
    - IPv6 : `ping fe80::d51d:5ab5:6f09:8f48%11`

3. Si votre réseau est correctement configuré, `ping` retourne `Reply from <IP address>` suivi de certaines informations supplémentaires. Si `ping` retourne `Destination host unreachable` ou `Request timed out`, TCP/IP n’est alors pas configuré correctement. Des erreurs à ce stade peuvent indiquer un problème avec l’ordinateur client, l’ordinateur serveur, ou relatif au réseau (par exemple, un routeur). Pour résoudre les problèmes de réseau, consultez[Résolution avancée des problèmes pour les problèmes TCP/IP](/windows/client-management/troubleshoot-tcpip).
4. Ensuite, si le test ping a réussi à l’aide de l’adresse IP, vérifiez que le nom de l’ordinateur peut être résolu en adresse TCP/IP. Sur l’ordinateur client, dans la fenêtre d’invite de commandes, tapez `ping` , puis le nom de l’ordinateur qui exécute SQL Server. Par exemple : `ping newofficepc`.
5. Si `ping` à l’adresse IP réussit, mais que `ping` à l’ordinateur retourne `Destination host unreachable` ou `Request timed out` vous avez peut-être des anciennes informations de résolution de nom (obsolètes) mises en cache sur l’ordinateur client. Tapez `ipconfig /flushdns` pour effacer le cache DNS (résolution de noms dynamique). Réexécutez ensuite une commande ping sur l’ordinateur selon le nom. Comme le cache DNS est vide, l’ordinateur client recherche les informations les plus récentes sur l’adresse IP pour l’ordinateur serveur.
6. Si votre réseau est correctement configuré, `ping` retourne `Reply from <IP address>` suivi de certaines informations supplémentaires. Si vous pouvez correctement effectuer un test ping de l’ordinateur du serveur par adresse IP, mais recevez une erreur telle que `Destination host unreachable.` ou `Request timed out.` quand vous effectuez un test Ping par nom de l’ordinateur, la résolution de noms n’est alors pas correctement configurée. (Pour plus d’informations, consultez l’article de 2006 précédemment mentionné, [Résolution des problèmes liés au TCP/IP](https://support.microsoft.com/kb/169790).) Une résolution correcte de noms n’est pas obligatoire pour se connecter à SQL Server mais, si le nom d’ordinateur ne peut pas résolu en adresse IP, les connexions doivent être établies en spécifiant l’adresse IP. La résolution de noms peut être corrigée plus tard.

## <a name="open-a-port-in-the-firewall"></a>Ouverture d’un port dans le pare-feu

Par défaut, le pare-feu Windows est activé et bloque toutes les connexions d’un autre ordinateur. Pour vous connecter à l’aide de TCP/IP à partir d’un autre ordinateur, vous devez configurer le pare-feu sur l’ordinateur SQL Server pour autoriser les connexions au port TCP utilisé par le moteur de base de données. L’instance par défaut est à l’écoute sur le port TCP 1433, par défaut. Si vous avez des instances nommées ou si vous avez modifié le port de l’instance par défaut, le port TCP [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] peut être à l’écoute sur un autre port. Consultez [Obtenir le port TCP de l’instance SQL Server](#getTCP).

Si vous vous connectez à une instance nommée ou un port autre que le port TCP 1433, vous devez également ouvrir le port UDP 1434 pour le service SQL Server Browser. Pour obtenir des instructions détaillées sur l’ouverture d’un port dans le Pare-feu Windows, consultez [Configurer un pare-feu Windows pour accéder au moteur de base de données](configure-a-windows-firewall-for-database-engine-access.md).

## <a name="test-the-connection"></a>Tester la connexion

Une fois que vous pouvez vous connecter à l’aide de TCP sur le même ordinateur, il est temps de tester la connexion à partir de l’ordinateur client. Vous pouvez théoriquement utiliser toute application cliente, mais pour éviter une complexité supplémentaire, installez les outils d’administration SQL Server sur le client et tentez l’opération à l’aide de SQL Server Management Studio.

1. Sur l’ordinateur client, avec SQL Server Management Studio, essayez de vous connecter à l’aide de l’adresse IP et du numéro de port TCP au format adresse IP virgule numéro de port. Par exemple : `192.168.1.101,1433`. Si cette connexion ne fonctionne pas, vous êtes probablement confronté à l’un des problèmes suivants :

   - `ping` de l’adresse IP ne fonctionne pas, ce qui indique un problème de configuration TCP d’ordre général. Revenez à la section [Test de la connectivité TCP/IP](#testTCPIP).
   - SQL Server n’écoute pas sur le protocole TCP. Revenez à la section [Activer les protocoles](#enableprotocols).
   - SQL Server est à l’écoute sur un port autre que le port spécifié. Revenez à la section [Obtenir le numéro de port TCP](#getTCP).
   - Le port TCP SQL Server est bloqué par le pare-feu. Revenez à la section [Ouverture d’un port dans le pare-feu](#open-a-port-in-the-firewall).

2. Une fois que vous pouvez vous connecter à l’aide de l’adresse IP et du numéro de port, essayez de vous connecter à l’aide de l’adresse IP sans numéro de port. Pour une instance par défaut, utilisez simplement l’adresse IP. Pour une instance nommée, utilisez l’adresse IP et le nom de l’instance dans le nom d’instance de la barre oblique de l’adresse au format IP, par exemple `192.168.1.101\<instance name>`. Si cela ne fonctionne pas, vous êtes probablement confronté à l’un des problèmes suivants :

   - Si vous vous connectez à l’instance par défaut, elle peut être à l’écoute sur un port autre que le port TCP 1433 et le client ne tente pas de se connecter au numéro de port correct.
   - Si vous vous connectez à une instance nommée, le numéro de port n’est pas retourné au client.

   Ces deux problèmes sont liés au service SQL Server Browser, qui fournit le numéro de port au client. Les solutions sont les suivantes :

   - Démarrez le service SQL Server Browser. Consultez les instructions pour [démarrer le navigateur dans le Gestionnaire de configuration SQL Server](#startbrowser).
   - Le service SQL Server Browser est bloqué par le pare-feu. Ouvrez le port UDP 1434 dans le pare-feu. Revenez à la section [Ouverture d’un port dans le pare-feu](#open-a-port-in-the-firewall). Assurez-vous que vous ouvrez un port UDP et non un port TCP.
   - Les informations du port UDP 1434 sont bloquées par un routeur. Les communications UDP (protocole UDP) ne sont pas conçues pour passer par des routeurs. Cela empêche le réseau d’être rempli par un trafic de faible priorité. Vous pouvez configurer votre routeur pour transférer le trafic UDP, ou vous pouvez choisir de toujours fournir le numéro de port quand vous vous connectez.
   - Si l’ordinateur client utilise Windows 7 ou Windows Server 2008 (ou un système d’exploitation plus récent), le trafic UDP peut être supprimé par le système d’exploitation client, car la réponse du serveur est retournée à partir d’une adresse IP différente de celle de la requête. Il s’agit d’une fonctionnalité de sécurité qui bloque le « mappage de source libre ». Pour plus d’informations, consultez la section **Adresses IP de serveurs multiples** de la rubrique de la documentation en ligne [Dépannage : expiration du délai d’attente](/previous-versions/sql/sql-server-2008-r2/ms190181(v=sql.105)). Il s’agit d’un article de SQL Server 2008 R2, mais les principes s’appliquent toujours. Vous pouvez configurer le client pour utiliser l’adresse IP correcte, ou vous pouvez choisir de toujours fournir le numéro de port quand vous vous connectez.

3. Une fois que vous pouvez vous connecter à l’aide de l’adresse IP (ou de l’adresse IP et du nom de l’instance pour une instance nommée), essayez de vous connecter en utilisant le nom de l’ordinateur (ou le nom de l’ordinateur et le nom de l’instance pour une instance nommée). Placez `tcp:` devant le nom de l’ordinateur pour forcer une connexion TCP/IP. Par exemple, pour l’instance par défaut sur un ordinateur nommé `ACCNT27`utilisez `tcp:ACCNT27` . Pour une instance nommée appelée `PAYROLL`sur cet ordinateur, utilisez `tcp:ACCNT27\PAYROLL` . Si vous pouvez vous connecter à l’aide de l’adresse IP, mais pas avec le nom de l’ordinateur, vous avez un problème de résolution de noms. Revenez à la section **Test de la connectivité TCP/IP**, section 4.

4. Une fois que vous pouvez vous connecter en utilisant le nom de l’ordinateur et en forçant une connexion TCP, tentez de vous connecter en utilisant le nom de l’ordinateur, mais sans forcer la connexion TCP. Par exemple, pour une instance par défaut, utilisez simplement le nom de l’ordinateur comme `CCNT27` . Pour une instance nommée, utilisez le nom de l’ordinateur et le nom de l’instance comme `ACCNT27\PAYROLL` . Si vous pouvez vous connecter en forçant une connexion TCP, mais pas sans forcer la connexion TCP, le client utilise probablement un autre protocole (par exemple, des canaux nommés).

    1. Sur l’ordinateur client, à l’aide du Gestionnaire de configuration SQL Server, dans le volet gauche, développez **Configuration** de **SQL Native Client** *version*, puis sélectionnez **Protocoles clients**.
    2. Dans le volet droit, vérifiez que TCP/IP est activé. Si TCP/IP est désactivé, cliquez avec le bouton droit sur **TCP/IP** , puis cliquez sur **Activer**.
    3. Vérifiez que l’ordre des protocoles pour TCP/IP est un nombre inférieur aux protocoles de canaux nommés (ou VIA sur des versions plus anciennes). En règle générale, vous devez laisser la mémoire partagée en 1 et TCP/IP en 2. La mémoire partagée est utilisée uniquement quand le client et SQL Server sont en cours d’exécution sur le même ordinateur. Tous les protocoles activés sont essayés dans l’ordre jusqu’à ce que l’un d’eux réussisse, sauf que la mémoire partagée est ignorée quand la connexion n’est pas sur le même ordinateur.