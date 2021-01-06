---
title: Créer un groupe de disponibilité indépendant du domaine
description: Étapes à suivre pour créer un groupe de disponibilité qui utilise un cluster de groupe de travail. Cela permet à SQL Server 2016 (et versions ultérieures) de déployer un groupe de disponibilité Always On sur un cluster WSFC qui ne nécessite pas Active Directory Domain Services et qui n’exige donc pas que chaque serveur fasse partie du même domaine.
ms.custom: seodec18
ms.date: 09/25/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], domain independent
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6c809abd5c333013b197dd48ca2fb4832bf6b615
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643605"
---
# <a name="create-a-domain-independent-availability-group"></a>Créer un groupe de disponibilité indépendant du domaine
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Les groupes de disponibilité AlwaysOn (AGs) exigent un cluster de basculement Windows Server (cluster WSFC) sous-jacent. Le déploiement d’un cluster WSFC via Windows Server 2012 R2 a toujours nécessité que les serveurs participant à un cluster WSFC, également connu sous le nom de nœuds, soient joints au même domaine. Pour plus d’informations sur Active Directory Domain Services (AD DS), reportez-vous [ici](/previous-versions/windows/it-pro/windows-server-2003/cc759073(v=ws.10)).

La dépendance entre WSFC et AD DS est plus complexe que ce qui a été précédemment déployé avec une configuration de mise en miroir de bases de données, étant donné que la mise en miroir de bases de données peut être déployée sur plusieurs centres de données à l’aide de certificats, sans de telles dépendances.  Un groupe de disponibilité traditionnel couvrant plusieurs centres de données requiert que tous les serveurs soient joints au même domaine Active Directory. Des domaines différents, même des domaines approuvés, ne fonctionnent pas. Tous les serveurs doivent être des nœuds du même cluster WSFC. Cette configuration est représentée dans la figure suivante. SQL Server 2016 a également des groupes de disponibilité distribués qui peuvent aussi atteindre cet objectif d’une manière différente.


![Cluster WSFC couvrant deux centres de données connectés au même domaine][1]

Windows Server 2012 R2 a introduit un [cluster détaché d’Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970(v=ws.11)), une forme spécialisée de cluster de basculement Windows Server utilisable avec les groupes de disponibilité. Ce type de cluster WSFC exige quand même que les nœuds soient joints au même domaine Active Directory, mais dans ce cas, le cluster WSFC utilise DNS, mais pas le domaine. Puisqu’un domaine est toujours impliqué, un cluster détaché d’Active Directory n’offre pas encore une expérience entièrement libre de domaine.

Windows Server 2016 a introduit un nouveau type de cluster de basculement Windows Server basé sur les principes d’un cluster détaché d’Active Directory, un cluster de groupe de travail. Un cluster de groupe de travail permet à SQL Server 2016 de déployer un groupe de disponibilité par-dessus un cluster WSFC qui ne requiert pas AD DS. SQL Server requiert l’utilisation de certificats pour la sécurité du point de terminaison, à l’instar du scénario de mise en miroir de bases de données.  Ce type de groupe de disponibilité est appelé groupe de disponibilité indépendant du domaine. Le déploiement d’un groupe de disponibilité avec un cluster de groupe de travail sous-jacent prend en charge les combinaisons suivantes pour les nœuds qui composent le cluster WSFC :
- Aucun nœud n’est joint à un domaine.
- Tous les nœuds sont joints à des domaines différents.
- Les nœuds sont mixtes, avec une combinaison de nœuds joints à un domaine et non joints à un domaine.

La figure suivante montre un exemple de groupe de disponibilité indépendant du domaine où les nœuds du centre de données 1 sont joints à un domaine alors que ceux du centre de données 2 utilisent uniquement DNS. Dans ce cas, définissez le suffixe DNS sur tous les serveurs qui sont des nœuds du cluster WSFC. Chaque application et chaque serveur qui accèdent au groupe de disponibilité doivent voir les mêmes informations DNS.


![Cluster de groupe de travail avec deux nœuds joints à un domaine][2]

Un groupe de disponibilité indépendant du domaine ne sert pas seulement dans les scénarios de récupération d’urgence ou de sites multiples. Il peut être déployé dans un centre de données unique et même utilisé avec un [groupe de disponibilité de base](basic-availability-groups-always-on-availability-groups.md) (également appelé groupe de disponibilité Standard Edition) pour fournir une architecture similaire à ce qui était atteint en utilisant une mise en miroir de bases de données avec des certificats comme indiqué.


![Vue générale d’un groupe de disponibilité Standard Edition][3]

Le déploiement d’un groupe de disponibilité indépendant du domaine implique quelques inconvénients connus :
- Les seuls types de témoins disponibles pour une utilisation avec le quorum sont le disque et le [cloud](/windows-server/failover-clustering/deploy-cloud-witness), ce qui est une nouveauté dans Windows Server 2016. Le disque pose problème car il n’existe très probablement aucune utilisation de disque partagé par le groupe de disponibilité.
- La variante du cluster de groupe de travail sous-jacente d’un cluster WSFC ne peut être créée qu’à l’aide de PowerShell, mais elle peut alors être administré à l’aide du Gestionnaire du cluster de basculement.
- Si Kerberos est requis, vous devez déployer un cluster WSFC standard attaché à un domaine Active Directory. Un groupe de disponibilité indépendant du domaine n’est alors probablement pas possible.
- Quand bien même vous pouvez configurer un écouteur, celui-ci doit être inscrit dans DNS pour être utilisable. Comme indiqué ci-dessus, il n’existe aucune prise en charge de Kerberos pour l’écouteur.
- Les applications qui se connectent à SQL Server doivent principalement utiliser l’authentification SQL Server, puisque les domaines n’existent peut-être pas ou ne sont peut-être pas configurés pour fonctionner ensemble. 
- Des certificats sont utilisés dans la configuration du groupe de disponibilité.

## <a name="set-and-verify-the-dns-suffix-on-all-replica-servers"></a>Définir et vérifier le suffixe DNS sur tous les serveurs réplicas

Un suffixe DNS commun est nécessaire pour le cluster de groupe de travail d’un groupe de disponibilité indépendant du domaine. Pour définir et vérifier le suffixe DNS sur chaque serveur Windows Server qui héberge un réplica du groupe de disponibilité, suivez ces instructions :

1. À l’aide du raccourci touche Windows+X, sélectionnez Système.
2. Si le nom de l’ordinateur et le nom complet de l’ordinateur sont identiques, le suffixe DNS n’a pas été défini. Par exemple, si le nom de l’ordinateur est ALLAN, la valeur du nom complet de l’ordinateur ne doit pas être simplement ALLAN. Elle doit ressembler à quelque chose comme ALLAN.SQLHA.LAB. SQLHA.LAB est le suffixe DNS. La valeur du groupe de travail doit indiquer WORKGROUP. Si vous avez besoin de définir le suffixe DNS, sélectionnez Modifier les paramètres.
3. Dans la boîte de dialogue Propriétés système, cliquez sur Modifier sous l’onglet Nom de l’ordinateur.
4. Dans la boîte de dialogue Modification du nom ou du domaine de l’ordinateur, cliquez sur Autres.
5. Dans la boîte de dialogue Nom d’ordinateur NetBIOS et suffixe DNS, entrez le suffixe DNS commun en tant que suffixe DNS principal. 
6. Cliquez sur OK pour fermer la boîte de dialogue Nom d’ordinateur NetBIOS et suffixe DNS.
7. Cliquez sur OK pour fermer la boîte de dialogue Modification du nom ou du domaine de l’ordinateur.
8. Vous êtes invité à redémarrer le serveur pour que les modifications prennent effet. Cliquez sur OK pour fermer la boîte de dialogue Modification du nom ou du domaine de l’ordinateur.
9. Cliquez sur Fermer pour fermer la boîte de dialogue Propriétés système.
10. Vous êtes invité à redémarrer. Si vous ne voulez pas redémarrer immédiatement, cliquez sur Redémarrer ultérieurement, sinon cliquez sur Redémarrer maintenant.
11. Une fois que le serveur a redémarré, vérifiez que le suffixe DNS commun est configuré en réexaminant Système.

![Configuration réussie du suffixe DNS][4]

  > [!NOTE]
  > Si vous utilisez plusieurs sous-réseaux et avez un nom DNS statique, vous devez disposer d’un processus en place pour mettre à jour l’enregistrement DNS associé à l’écouteur avant d’effectuer un basculement, car sinon le nom de réseau n’est pas mis en ligne.

## <a name="create-a-domain-independent-availability-group"></a>Créer un groupe de disponibilité indépendant du domaine

La création d’un groupe de disponibilité indépendant du domaine n’est pas réalisable entièrement avec SQL Server Management Studio. Quand bien même la création du groupe de disponibilité indépendant du domaine ressemble trait pour trait à celle d’un groupe de disponibilité normal, certains aspects (tels que la création des certificats) sont uniquement possibles avec Transact-SQL. L’exemple ci-dessous illustre une configuration de groupe de disponibilité avec deux réplicas : un réplica principal et un réplica secondaire. 

1. [En suivant les instructions indiquées par ce lien](https://techcommunity.microsoft.com/t5/Failover-Clustering/Workgroup-and-Multi-domain-clusters-in-Windows-Server-2016/ba-p/372059), déployez un cluster de groupe de travail composé de tous les serveurs inclus dans le groupe de disponibilité. Vérifiez que le suffixe DNS commun est déjà configuré avant de configurer le cluster de groupe de travail.
2. [Activez la fonctionnalité Groupes de disponibilité Always On](./enable-and-disable-always-on-availability-groups-sql-server.md) sur chaque instance membre du groupe de disponibilité. Un redémarrage de chaque instance SQL Server est alors nécessaire.
3. Chaque instance qui héberge le réplica principal a besoin d’une clé principale de base de données. Si aucune clé principale n’existe déjà, exécutez la commande suivante :

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'Strong Password';
   GO
   ```

4. Sur l’instance correspondant au réplica principal, créez le certificat qui sera utilisé à la fois pour les connexions entrantes sur les réplicas secondaires et pour sécuriser le point de terminaison sur le réplica principal.

   ```sql
   CREATE CERTIFICATE InstanceA_Cert 
   WITH SUBJECT = 'InstanceA Certificate';
   GO
   ``` 

5. Sauvegardez le certificat. Vous pouvez également le sécuriser davantage avec une clé privée si vous le souhaitez. Cet exemple n’utilise pas de clé privée.

   ```sql
   BACKUP CERTIFICATE InstanceA_Cert 
   TO FILE = 'Backup_path\InstanceA_Cert.cer';
   GO
   ```

6. Répétez les étapes 4 et 5 pour créer et sauvegarder des certificats pour chaque réplica secondaire, à l’aide de noms appropriés pour les certificats, comme InstanceB_Cert.
7. Sur le réplica principal, vous devez créer une connexion pour chaque réplica secondaire du groupe de disponibilité. Cette connexion reçoit l’autorisation de se connecter au point de terminaison utilisé par le groupe de disponibilité indépendant du domaine. Par exemple, pour un réplica nommé InstanceB :

   ```sql
   CREATE LOGIN InstanceB_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

8. Sur chaque réplica secondaire, créez une connexion pour le réplica principal. Cette connexion reçoit l’autorisation de se connecter au point de terminaison. Par exemple, sur un réplica nommé InstanceB :

   ```sql
   CREATE LOGIN InstanceA_Login WITH PASSWORD = 'Strong Password';
   GO
   ```

9. Sur toutes les instances, créez un utilisateur pour chaque connexion créée. Il sera utilisé lors de la restauration des certificats. Par exemple, pour créer un utilisateur pour le réplica principal :

   ```sql
   CREATE USER InstanceA_User FOR LOGIN InstanceA_Login;
   GO
   ```

10. Pour tout réplica pouvant être un réplica principal, créez une connexion et un utilisateur sur tous les réplicas secondaires pertinents.
11. Sur chaque instance, restaurez les certificats pour les autres instances pour lesquelles une connexion et un utilisateur ont été créés. Sur le réplica principal, restaurez tous les certificats de réplica secondaire. Sur chaque réplica secondaire, restaurez le certificat du réplica principal, mais aussi sur tout autre réplica pouvant être un réplica principal. Par exemple :

   ```sql
   CREATE CERTIFICATE [InstanceB_Cert]
   AUTHORIZATION InstanceB_User
   FROM FILE = 'Restore_path\InstanceB_Cert.cer'
   ```

12. Créez le point de terminaison qui sera utilisé par le groupe de disponibilité sur chaque instance qui sera un réplica. Pour les groupes de disponibilité, le point de terminaison doit être de type DATABASE_MIRRORING. Le point de terminaison utilise le certificat créé à l’étape 4 pour cette instance à des fins d’authentification. Un exemple de syntaxe est indiqué ci-dessous pour créer un point de terminaison à l’aide d’un certificat. Utilisez la méthode de chiffrement appropriée et d’autres options adaptées à votre environnement. Pour plus d’informations sur les options disponibles, consultez [CREATE ENDPOINT (Transact-SQL)](../../../t-sql/statements/create-endpoint-transact-sql.md).

   ```sql
   CREATE ENDPOINT DIAG_EP
   STATE = STARTED
   AS TCP (   
    LISTENER_PORT = 5022,
    LISTENER_IP = ALL
         )
   FOR DATABASE_MIRRORING (
    AUTHENTICATION = CERTIFICATE InstanceX_Cert,
    ROLE = ALL
         )
   ```

13. Affectez des droits à chaque utilisateur créé sur cette instance à l’étape 9 pour leur permettre de se connecter au point de terminaison. 

   ```sql
   GRANT CONNECT ON ENDPOINT::DIAG_EP TO [InstanceX_User];
   GO
   ```

14. Une fois que les certificats sous-jacents et la sécurité du point de terminaison sont configurés, créez le groupe de disponibilité à l’aide de la méthode de votre choix. Il est recommandé de manuellement sauvegarder, copier et restaurer la sauvegarde utilisée pour initialiser le réplica secondaire. Vous pouvez aussi utiliser [l’amorçage automatique](automatically-initialize-always-on-availability-group.md). L’utilisation de l’Assistant pour initialiser les réplicas secondaires implique d’utiliser des fichiers Server Message Block (SMB), ce qui risque de ne pas fonctionner quand vous utilisez un cluster de groupe de travail non joint à un domaine.
15. Si vous créez un écouteur, vérifiez que son nom et son adresse IP sont inscrits dans DNS.

### <a name="next-steps"></a>Étapes suivantes 

- [Utiliser l’Assistant Groupe de disponibilité (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

- [Utiliser la boîte de dialogue Nouveau groupe de disponibilité (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
- [Créer un groupe de disponibilité avec Transact-SQL](create-an-availability-group-transact-sql.md)

<!--Image references-->
[1]: ./media/diag-wsfc-two-data-centers-same-domain.png
[2]: ./media/diag-workgroup-cluster-two-nodes-joined.png
[3]: ./media/diag-high-level-view-ag-standard-edition.png
[4]: ./media/diag-successful-dns-suffix.png