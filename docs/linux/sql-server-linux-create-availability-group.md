---
title: Créer et configurer un groupe de disponibilité pour SQL Server sur Linux
description: Ce tutoriel montre comment créer et configurer des groupes de disponibilité pour SQL Server sur Linux, et comment créer des points de terminaison et des certificats de groupe de disponibilité.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/28/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ecb634a770b4990bfbbc094b15498b03bdea4a57
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172001"
---
# <a name="create-and-configure-an-availability-group-for-sql-server-on-linux"></a>Créer et configurer un groupe de disponibilité pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce tutoriel traite de la création et de la configuration d’un groupe de disponibilité pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux. Contrairement à [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] et aux versions antérieures sur Windows, vous pouvez activer des groupes de disponibilité en commençant ou non par créer le cluster Pacemaker sous-jacent. Si elle est nécessaire, l’intégration au cluster est effectuée plus tard.

Le tutoriel inclut les tâches suivantes :
 
> [!div class="checklist"]
> * Activer des groupes de disponibilité.
> * Créer des points de terminaison de groupe de disponibilité et des certificats.
> * Utiliser [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) ou Transact-SQL pour créer un groupe de disponibilité.
> * Créer la connexion [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] et les autorisations pour Pacemaker.
> * Créer des ressources de groupe de disponibilité dans un cluster Pacemaker (type externe uniquement).

## <a name="prerequisite"></a>Configuration requise
- Déployez le cluster de haute disponibilité de Pacemaker comme décrit dans [Déployer un cluster Pacemaker pour SQL Server sur Linux](sql-server-linux-deploy-pacemaker-cluster.md).


## <a name="enable-the-availability-groups-feature"></a>Activez la fonctionnalité Groupes de disponibilité

Vous ne pouvez pas utiliser PowerShell ou Configuration Manager [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] pour activer la fonctionnalité Groupes de disponibilité comme sur Windows. Sous Linux, vous devez utiliser `mssql-conf` pour activer la fonctionnalité. Il y a deux façons d’activer la fonctionnalité Groupes de disponibilité : utilisez l’utilitaire `mssql-conf` ou modifiez manuellement le fichier `mssql.conf`.

> [!IMPORTANT]
> La fonctionnalité Groupe de disponibilité doit être activée pour les réplicas de configuration uniquement, même sur [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)].

### <a name="use-the-mssql-conf-utility"></a>Utilisez l’utilitaire mssql-conf

À l’invite, exécutez la commande suivante :

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
```

### <a name="edit-the-mssqlconf-file"></a>Modifiez le fichier mssql-conf

Vous pouvez également modifier le fichier `mssql.conf`, situé dans le dossier `/var/opt/mssql`, pour ajouter les lignes suivantes :

```
[hadr]

hadr.hadrenabled = 1
```

### <a name="restart-ssnoversion-md"></a>Redémarrez [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
Après avoir activé les groupes de disponibilité, comme sur Windows, vous devez redémarrer [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Cela peut être effectué de la façon suivante :

```bash
sudo systemctl restart mssql-server
```

## <a name="create-the-availability-group-endpoints-and-certificates"></a>Créez les points de terminaison du groupe de disponibilité et les certificats

Un groupe de disponibilité utilise des points de terminaison TCP pour la communication. Sous Linux, les points de terminaison d’un groupe de disponibilité ne sont pris en charge que si des certificats sont utilisés pour l’authentification. Cela signifie que le certificat d’une instance doit être restauré sur toutes les autres instances, qui seront des réplicas participant au même groupe de disponibilité. Le processus de certificat est requis même pour un réplica de configuration uniquement. 

La création de points de terminaison et la restauration de certificats ne peuvent être effectuées qu’à l’aide de Transact-SQL. Vous pouvez également utiliser des certificats non générés par [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Vous avez également besoin d’un processus de gestion et de remplacement des certificats qui expirent.

> [!IMPORTANT]
> Si vous envisagez l’Assistant [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] pour créer le groupe de disponibilité, vous devez toujours créer et restaurer les certificats à l’aide de Transact-SQL sur Linux.

Pour obtenir une syntaxe complète des options disponibles pour les différentes commandes (telles que la sécurité supplémentaire), consultez :

-   [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
-   [CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)
-   [CREATE ENDPOINT](../t-sql/statements/create-endpoint-transact-sql.md)

> [!NOTE]
> Bien que vous soyez en train de créer un groupe de disponibilité, le type de point de terminaison utilise *FOR DATABASE_MIRRORING*, car certains aspects sous-jacents ont préalablement été partagés avec cette fonctionnalité maintenant dépréciée.

Cet exemple crée des certificats pour une configuration à trois nœuds. Les noms des instances sont LinAGN1, LinAGN2 et LinAGN3.

1.  Exécutez la commande suivante sur LinAGN1 pour créer la clé principale, le certificat et le point de terminaison, ainsi que pour sauvegarder le certificat. Pour cet exemple, le port TCP standard 5022 est utilisé pour le point de terminaison.
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN1_Cert
    WITH SUBJECT = 'LinAGN1 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN1_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN1_Cert,
        ROLE = ALL);
    
    GO
    ```
    
2.  Procédez de la même façon sur LinAGN2 :
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    WITH SUBJECT = 'LinAGN2 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN2_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN2_Cert,
        ROLE = ALL);
    
    GO
    ```
    
3.  Enfin, exécutez la même séquence sur LinAGN3 :
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    WITH SUBJECT = 'LinAGN3 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN3_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN3_Cert,
        ROLE = ALL);
    
    GO
    ```
    
4.  À l’aide de `scp` ou d’un autre utilitaire, copiez les sauvegardes du certificat sur chaque nœud qui fera partie du groupe de disponibilité.
    
    Pour cet exemple :
    
    - Copiez LinAGN1_Cert.cer dans LinAGN2 et LinAGN3
    - Copiez LinAGN2_Cert.cer dans LinAGN1 et LinAGN3.
    - Copiez LinAGN3_Cert.cer dans LinAGN1 et LinAGN2.
    
5.  Modifiez la propriété et le groupe associé aux fichiers de certificat copiés sur `mssql`.
    
    ```bash
    sudo chown mssql:mssql <CertFileName>
    ```
    
6.  Créez les connexions au niveau de l’instance et les utilisateurs associés à LinAGN2 et LinAGN3 sur LinAGN1.
    
    ```SQL
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
7.  Restaurez LinAGN2_Cert et LinAGN3_Cert sur LinAGN1. Le fait d’avoir les certificats des autres réplicas est un aspect important de la communication et de la sécurité du groupe de disponibilité.
    
    ```SQL
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
8.  Grant the logins associated with LinAG2 and LinAGN3 permission to connect to the endpoint on LinAGN1.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
9.  Créez les connexions au niveau de l’instance et les utilisateurs associés à LinAGN1 et LinAGN3 sur LinAGN2.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
10. Restaurez LinAGN1_Cert et LinAGN3_Cert sur LinAGN2.
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    ```
    
11. Accordez aux connexions associées à LinAG1 et à LinAGN3 l’autorisation de se connecter au point de terminaison sur LinAGN2.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
12. Créez les connexions au niveau de l’instance et les utilisateurs associés à LinAGN1 et LinAGN2 sur LinAGN3.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    ```
    
13. Restaurez LinAGN1_Cert et LinAGN2_Cert sur LinAGN3. 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    ```
    
14. Accordez aux connexions associées à LinAG1 et à LinAGN2 l’autorisation de se connecter au point de terminaison sur LinAGN3.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    ```

## <a name="create-the-availability-group"></a>Créez le groupe de disponibilité

Cette section explique comment utiliser [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) ou Transact-SQL pour créer le groupe de disponibilité pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

### <a name="use-ssmanstudiofull-md"></a>Utilisez [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)].

Cette section montre comment créer un groupe de disponibilité avec un type de cluster externe à l’aide de SSMS avec l’Assistant Nouveau groupe de disponibilité.

1.  Dans SSMS, développez **Haute disponibilité Always On**, cliquez avec le bouton droit sur **Groupes de disponibilité** et sélectionnez **Assistant Nouveau groupe de disponibilité**.

2.  Dans la boîte de dialogue Introduction, cliquez sur **Suivant**.

3.  Dans la boîte de dialogue Spécifier les options du groupe de disponibilité, entrez un nom pour le groupe de disponibilité et sélectionnez un type de cluster EXTERNAL (externe) ou NONE (aucun) dans la liste déroulante. Externe doit être utilisé lorsque Pacemaker est déployé. Aucun est destiné à des scénarios spécialisés, tels que le scale-out de lecture. La sélection de l’option de détection d’intégrité au niveau de la base de données est facultative. Pour plus d’informations sur cette option, consultez [Option de détection de l’intégrité au niveau base de données du groupe de disponibilité pour le basculement](../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). Cliquez sur **Suivant**.

    ![Créer un groupe de disponibilité 03](./media/sql-server-linux-create-availability-group/image3.png)

4.  Dans la boîte de dialogue Sélectionner les bases de données, sélectionnez la ou les bases de données qui participeront au groupe de disponibilité. Chaque base de données doit avoir une sauvegarde complète avant de pouvoir être ajoutée à un groupe de disponibilité. Cliquez sur **Suivant**.

5.  Dans la boîte de dialogue Spécifier les réplicas, cliquez sur **Ajouter un réplica**.

6.  Dans la boîte de dialogue Se connecter au serveur, entrez le nom de l'instance Linux de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] qui sera le réplica secondaire ainsi que les informations d’identification pour se connecter. Cliquez sur **Connecter**.

7.  Répétez les deux étapes précédentes pour l’instance qui contiendra un réplica de configuration uniquement ou un autre réplica secondaire.

8.  Les trois instances doivent maintenant être répertoriées dans la boîte de dialogue Spécifier les réplicas. Si vous utilisez un type de cluster externe, pour le réplica secondaire qui sera vraiment secondaire, assurez-vous que le mode de disponibilité correspond à celui du réplica principal et que le mode de basculement est défini sur Externe. Pour le réplica de configuration uniquement, sélectionnez un mode de disponibilité de configuration uniquement.

    L’exemple suivant montre un groupe de disponibilité avec deux réplicas, un type de cluster externe et un réplica de configuration uniquement.

    ![Créer un groupe de disponibilité 04](./media/sql-server-linux-create-availability-group/image4.png)

    L’exemple suivant montre un groupe de disponibilité avec deux réplicas, un type de cluster None et un réplica de configuration uniquement.

    ![Créer un groupe de disponibilité 05](./media/sql-server-linux-create-availability-group/image5.png)

9.  Si vous souhaitez modifier les préférences de sauvegarde, cliquez sur l’onglet Préférences de sauvegarde. Pour plus d’informations sur les préférences de sauvegarde, consultez [Configurer la sauvegarde sur des réplicas de disponibilité](../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).

10. Si vous utilisez des réplicas secondaires lisibles ou créez un groupe de disponibilité avec un type de cluster None pour l’échelle lecture, vous pouvez créer un écouteur en sélectionnant l’onglet Écouteur. Un écouteur peut également être ajouté ultérieurement. Pour créer un écouteur, choisissez l’option **Créer un écouteur de groupe de disponibilité**, entrez un nom, un port TCP/IP et indiquez si vous souhaitez utiliser une adresse IP DHCP affectée automatiquement ou statique. N’oubliez pas que pour un groupe de disponibilité avec un type de cluster None, l’adresse IP doit être statique et définie sur l’adresse IP du principal.

    ![Créer un groupe de disponibilité 06](./media/sql-server-linux-create-availability-group/image6.png)

11. Si un écouteur est créé pour des scénarios lisibles, SSMS 17.3 ou version ultérieure autorise la création du routage en lecture seule dans l’Assistant. Il peut également être ajouté ultérieurement via SSMS ou Transact-SQL. Pour activer le routage en lecture seule maintenant :

    a.  Sélectionnez l’onglet Routage en lecture seule.

    b.  Entrez les URL pour les réplicas en lecture seule. Ces URL sont similaires aux points de terminaison, sauf qu’elles utilisent le port de l’instance et non le point de terminaison.

    c.  Sélectionnez chaque URL et, dans la partie inférieure, sélectionnez les réplicas lisibles. Pour sélectionner plusieurs réplicas, maintenez la touche MAJ enfoncée ou effectuer un cliquer-glisser.

12. Cliquez sur **Suivant**.

13. Choisissez comment le ou les réplicas secondaires seront initialisés. La valeur par défaut consiste à utiliser l'[amorçage automatique](../database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group.md), qui requiert le même chemin sur tous les serveurs participant au groupe de disponibilité. L’Assistant peut également sauvegarder, copier et restaurer (deuxième option) ; joindre si vous avez sauvegardé, copié et restauré manuellement la base de données sur le ou les réplicas (troisième option) ; ou ajouter ultérieurement la base de données (dernière option). Quant aux certificats, si vous faites manuellement des sauvegardes et que vous les copiez, les autorisations sur les fichiers de sauvegarde doivent être définies sur le ou les autres réplicas. Cliquez sur **Suivant**.

14. Dans la boîte de dialogue Validation, si tout ne réussit pas, examinez. Certains avertissements sont acceptables et ne sont pas irrécupérables, par exemple si vous ne créez pas d’écouteur. Cliquez sur **Suivant**.

15. Sur la boîte de dialogue **Terminer**. Le processus de création du groupe de disponibilité va maintenant commencer.

16. Lorsque la création du groupe de disponibilité est terminée, cliquez sur **Fermer** sur les résultats. Vous pouvez maintenant voir le groupe de disponibilité sur les réplicas dans les vues de gestion dynamique, ainsi que dans le dossier Haute disponibilité Always On dans SSMS.

### <a name="use-transact-sql"></a>Utiliser Transact-SQL

Cette section présente des exemples de création d’un groupe de disponibilité à l’aide de Transact-SQL. L’écouteur et le routage en lecture seule peuvent être configurés après la création du groupe de disponibilité. Le groupe de disponibilité proprement dit peut être modifié avec `ALTER AVAILABILITY GROUP`, mais la modification du type de cluster ne peut pas être effectuée dans [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. Si vous ne vouliez pas créer de groupe de disponibilité avec un type de cluster externe, vous devez le supprimer et le recréer avec un type de cluster None. Pour plus d’informations et d’autres options, consultez les liens suivants :

-   [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)
-   [ALTER AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/alter-availability-group-transact-sql.md)
-   [Configurer le routage en lecture seule pour un groupe de disponibilité (SQL Server)](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)
-   [Créer ou configurer un écouteur de groupe de disponibilité (SQL Server)](../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)

#### <a name="example-one---two-replicas-with-a-configuration-only-replica-external-cluster-type"></a>Exemple un : deux réplicas avec un réplica à configuration uniquement (type de cluster externe)

Cet exemple montre comment créer un groupe de disponibilité à deux réplicas qui utilise un réplica de configuration uniquement.

1.  Exécutez sur le nœud qui sera le réplica principal contenant la copie en lecture/écriture entière de la ou des bases de données. Cet exemple utilise l’amorçage automatique.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1' WITH (
       ENDPOINT_URL = N' TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       SEEDING_MODE = AUTOMATIC),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       AVAILABILITY_MODE = CONFIGURATION_ONLY);
       
    GO
    ```
    
2.  Dans une fenêtre de requête connectée à l’autre réplica, exécutez la commande suivante pour joindre le réplica au groupe de disponibilité et initier le processus d’amorçage du réplica principal au réplica secondaire.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3. Dans une fenêtre de requête connectée au réplica de configuration uniquement, joignez-le au groupe de disponibilité.
    
   ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
   ```

#### <a name="example-two---three-replicas-with-read-only-routing-external-cluster-type"></a>Exemple deux : trois réplicas avec routage en lecture seule (type de cluster externe)

Cet exemple montre trois réplicas complets et la manière dont le routage en lecture seule peut être configuré dans le cadre de la création initiale du groupe de disponibilité.

1.  Exécutez sur le nœud qui sera le réplica principal contenant la copie en lecture/écriture entière de la ou des bases de données. Cet exemple utilise l’amorçage automatique.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN2.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:1433')),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN2.FullyQualified.Name:1433')),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN3.FullyQualified.Name:1433'))
    LISTENER '<ListenerName>' (WITH IP = ('<IPAddress>', '<SubnetMask>'), Port = 1433);
    
    GO
    ```
    
    Voici quelques points à noter concernant cette configuration :
    
    - *AGName* correspond au nom du groupe de disponibilité.
    - *DBName* est le nom de la base de données qui sera utilisée avec le groupe de disponibilité. Il peut également s’agir d’une liste de noms séparés par des virgules.
    - *ListenerName* est un nom différent de celui des serveurs/nœuds sous-jacents. Il sera inscrit dans le DNS avec *IPAddress*.
    - *IPAddress* est une adresse IP associée à *ListenerName*. Elle est également unique et différente de celle des serveurs/nœuds. Les applications et les utilisateurs finaux utilisent soit *ListenerName*, soit *IPAddress* pour se connecter au groupe de disponibilité.
    - *SubnetMask* est le masque de sous-réseau d’*IPAddress*, par exemple, 255.255.255.0.

2.  Dans une fenêtre de requête connectée à l’autre réplica, exécutez la commande suivante pour joindre le réplica au groupe de disponibilité et initier le processus d’amorçage du réplica principal au réplica secondaire.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  Répétez l’étape 2 pour le troisième réplica.

#### <a name="example-three---two-replicas-with-read-only-routing-none-cluster-type"></a>Exemple trois : deux réplicas avec routage en lecture seule (type de cluster None)

Cet exemple illustre la création d’une configuration à deux réplicas à l’aide d’un type de cluster None. Il est utilisé pour le scénario de l’échelle de lecture, où aucun basculement n’est attendu. Cela crée l’écouteur qui est en fait le réplica principal, ainsi que le routage en lecture seule, à l’aide de la fonctionnalité de tourniquet (round robin).

1.  Exécutez sur le nœud qui sera le réplica principal contenant la copie en lecture/écriture entière de la ou des bases de données. Cet exemple utilise l’amorçage automatique.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = NONE)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name: <PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name'.'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:<PortOfInstance>'));
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:<PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL =N'TCP://LinAGN2.FullyQualified.Name:<PortOfInstance>'));
    LISTENER '<ListenerName>' (WITH IP = ('<PrimaryReplicaIPAddress>', '<SubnetMask>'), Port = <PortOfListener>);
    
    GO
    ```
    
    Where
    - *AGName* correspond au nom du groupe de disponibilité.
    - *DBName* est le nom de la base de données qui sera utilisée avec le groupe de disponibilité. Il peut également s’agir d’une liste de noms séparés par des virgules.
    - *PortOfEndpoint* est le numéro de port utilisé par le point de terminaison créé.
    - *PortOfInstance* est le numéro de port utilisé par l’instance de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].
    - *ListenerName* est un nom différent de celui des réplicas sous-jacents, mais ne sera pas vraiment utilisé.
    - *PrimaryReplicaIPAddress* est l’adresse IP du réplica principal.
    - *SubnetMask* est le masque de sous-réseau d’*IPAddress*. Par exemple, 255.255.255.0.
    
2.  Joignez le réplica secondaire au groupe de disponibilité et lancez l’amorçage automatique.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = NONE);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```

## <a name="create-the-ssnoversion-md-login-and-permissions-for-pacemaker"></a>Créer la connexion [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] et les autorisations pour Pacemaker

Un cluster de haute disponibilité Pacemaker sous-jacent à [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux doit avoir accès à l’instance [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] ainsi qu’aux autorisations du groupe de disponibilité proprement dit. Ces étapes permettent de créer la connexion et les autorisations associées, ainsi qu’un fichier qui indique à Pacemaker comment se connecter à [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

1.  Dans une fenêtre de requête connectée au premier réplica, exécutez la commande suivante :

    ```SQL
    CREATE LOGIN PMLogin WITH PASSWORD ='<StrongPassword>';
    
    GO
    
    GRANT VIEW SERVER STATE TO PMLogin;
    
    GO
    
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::<AGThatWasCreated> TO PMLogin;
    
    GO
    ```
    
2.  Sur le nœud 1, entrez la commande 
    ```bash
    sudo emacs /var/opt/mssql/secrets/passwd
    ```
    
    L’éditeur Emacs s’ouvre.
    
3.  Dans l’éditeur, entrez les deux lignes suivantes :

    ```
    PMLogin
    <StrongPassword>
    ```
    
4.  Maintenez la touche CTRL enfoncée et appuyez sur X, puis sur C, pour quitter et enregistrer le fichier.

5.  Execute 
    ```bash
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```
    
    pour verrouiller le fichier.

6.  Répétez les étapes 1 à 5 sur les autres serveurs qui serviront de réplicas.

## <a name="create-the-availability-group-resources-in-the-pacemaker-cluster-external-only"></a>Créer les ressources de groupe de disponibilité dans le cluster Pacemaker (externe uniquement)

Après la création d’un groupe de disponibilité dans [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], les ressources correspondantes doivent être créées dans Pacemaker lorsqu’un type de cluster externe est spécifié. Il existe deux ressources associées à un groupe de disponibilité : le groupe de disponibilité proprement dit et l’adresse IP. La configuration de la ressource d’adresse IP est facultative si vous n’utilisez pas la fonctionnalité d’écouteur, mais elle est recommandée.

La ressource de groupe de disponibilité créée est un type spécial de ressource appelé clone. La ressource de groupe de disponibilité a essentiellement des copies sur chaque nœud, une ressource de contrôle étant appelée maître. Le maître est associé au serveur qui héberge le réplica principal. Les réplicas secondaires hôtes d’autres ressources (standard ou de configuration uniquement) peuvent être promus maître dans un basculement.

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

1.  Créez la ressource de groupe de disponibilité avec la syntaxe suivante :

    **Red Hat Enterprise Linux (RHEL) et Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failure-timeout=30s --master meta notify=true
    ```

    >[!NOTE]
    >Sur RHEL 7.4, vous pouvez rencontrer un avertissement lors de l’utilisation de --maître. Pour l’éviter, utilisez `sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failover-timeout=30s master notify=true`
   
    **SUSE Linux Enterprise Server (SLES)**
    
    ```bash
    primitive <NameForAGResource> \
    ocf:mssql:ag \
    params ag_name="<AGName>" \
    meta failure-timeout=60s \
    op start timeout=60s \
    op stop timeout=60s \
    op promote timeout=60s \
    op demote timeout=10s \
    op monitor timeout=60s interval=10s \
    op monitor timeout=60s interval=11s role="Master" \
    op monitor timeout=60s interval=12s role="Slave" \
    op notify timeout=60s
    ms ms-ag_cluster <NameForAGResource> \
    meta master-max="1" master-node-max="1" clone-max="3" \
    clone-node-max="1" notify="true" \
    commit
    ```
    
    où *NameForAGResource* est le nom unique donné à cette ressource de cluster pour le groupe de disponibilité et *AGName* le nom du groupe de disponibilité qui a été créé.
 
2.  Créez la ressource d’adresse IP pour le groupe de disponibilité qui sera associé à la fonctionnalité d’écouteur.

    **RHEL et Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForIPResource> ocf:heartbeat:IPaddr2 ip=<IPAddress> cidr_netmask=<Netmask>
    ```

    **SLES**
    
    ```bash
    crm configure \
    primitive <NameForIPResource> \
       ocf:heartbeat:IPaddr2 \
       params ip=<IPAddress> \
          cidr_netmask=<Netmask>
    ```
    
    où *NameForIPResource* est le nom unique de la ressource IP et *IPAddress* est l’adresse IP statique affectée à la ressource. Sur SLES, vous devez également fournir le masque réseau. Par exemple, 255.255.255.0 a une valeur de 24 pour le *masque réseau.*
    
3.  Pour vous assurer que l’adresse IP et la ressource de disponibilité de groupe s’exécutent sur le même nœud, une contrainte de colocalisation doit être configurée.

    **RHEL et Ubuntu**
    
    ```bash
    sudo pcs constraint colocation add <NameForIPResource> <NameForAGResource>-master INFINITY with-rsc-role=Master
    ```

    **SLES**
    
    ```bash
    crm configure <NameForConstraint> inf: \
    <NameForIPResource> <NameForAGResource>:Master 
    commit
    ```
    
    où *NameForIPResource* est le nom de la ressource IP, *NameForAGResource* le nom de la ressource de groupe de disponibilité, et sur SLES, *NameForConstraint* est le nom de la restriction.

4.  Créez une contrainte de classement pour vous assurer que la ressource de groupe de disponibilité est active et en cours d’exécution avant l’adresse IP. Alors que la contrainte de colocalisation implique une contrainte de classement, elle l’applique.

    **RHEL et Ubuntu**
    
    ```bash
    sudo pcs constraint order promote <NameForAGResource>-master then start <NameForIPResource>
    ```
    
    **SLES**
    
    ```bash
    crm configure \
    order <NameForConstraint> inf: <NameForAGResource>:promote <NameForIPResource>:start
    commit
    ```
    
    où *NameForIPResource* est le nom de la ressource IP, *NameForAGResource* le nom de la ressource de groupe de disponibilité, et sur SLES, *NameForConstraint* est le nom de la restriction.

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel vous avez appris comment créer et configurer un groupe de disponibilité pour [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] sur Linux. Vous avez appris à :
> [!div class="checklist"]
> * Activer des groupes de disponibilité.
> * Créer des points de terminaison et des certificats de groupes de disponibilité.
> * Utiliser [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) ou Transact-SQL pour créer un groupe de disponibilité.
> * Créer la connexion [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] et les autorisations pour Pacemaker.
> * Créer des ressources de groupe de disponibilité dans un cluster Pacemaker.

Pour la plupart des tâches d’administration du groupe de disponibilité, notamment les mises à niveau et le basculement, consultez :

> [!div class="nextstepaction"]
> [Faire fonctionner un groupe de haute disponibilité pour SQL Server sur Linux](sql-server-linux-availability-group-failover-ha.md)

