---
title: Analysis Services avec les groupes de disponibilité
description: Si vous utilisez des groupes de disponibilité Always On pour votre solution haute disponibilité, vous pouvez utiliser une base de données membre d’un de ces groupes comme source de données dans une solution Analysis Services tabulaire ou multidimensionnelle.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 14d16bfd-228c-4870-b463-a283facda965
author: cawrites
ms.author: chadam
manager: erikre
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7d64fdbc557f5b985d27818847091f7eea1816f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641657"
---
# <a name="analysis-services-with-always-on-availability-groups"></a>Analysis Services avec les groupes de disponibilité Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Un groupe de disponibilité Always On est une collection prédéfinie de bases de données relationnelles SQL Server qui basculent ensemble quand les conditions déclenchent un basculement dans l’une des bases de données, redirigeant les requêtes vers une base de données mise en miroir sur une autre instance dans le même groupe de disponibilité. Si vous utilisez des groupes de disponibilité pour votre solution haute disponibilité, vous pouvez utiliser une base de données de ce groupe comme source de données dans une solution Analysis Services tabulaire ou multidimensionnelle. Toutes les opérations d'Analysis Services s'exécutent comme prévu lorsque vous utilisez une base de données de disponibilité : le traitement ou l'importation des données, l'interrogation directe des données relationnelles (à l'aide du stockage ROLAP ou du mode DirectQuery) et l'écriture différée.  
  
 Le traitement et l'exécution des requêtes sont des charges de travail en lecture seule. Vous pouvez améliorer les performances en déchargeant ces charges de travail vers un réplica secondaire lisible. Une configuration supplémentaire est requise pour ce scénario. Utilisez la liste de contrôle de cette rubrique pour vous assurer que vous suivez toutes les étapes.  
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Conditions préalables  
 Vous devez avoir compte de connexion SQL Server sur tous les réplicas. Vous devez être un **sysadmin** pour configurer des groupes de disponibilité, des écouteurs et des bases de données, mais les utilisateurs n’ont besoin que d’autorisations **db_datareader** pour accéder à la base de données d’un client Analysis Services.  
  
 Utilisez un fournisseur de données qui prend en charge la version 7.4 du protocole TDS (Tabular Data Stream) ou une version plus récente, telle que SQL Server Native Client 11.0 ou le Fournisseur de données pour SQL Server dans .NET Framework 4.02.  
  
 **(Pour les charges de travail en lecture seule)** . Le rôle de réplica secondaire doit être configuré pour les connexions en lecture seule, le groupe de disponibilité doit avoir une liste de routage et la connexion dans la source de données Analysis Services doit spécifier l'écouteur du groupe de disponibilité. Les instructions sont fournies dans cette rubrique.  
  
##  <a name="checklist-use-a-secondary-replica-for-read-only-operations"></a><a name="bkmk_UseSecondary"></a> Liste de contrôle : Utiliser un réplica secondaire pour les opérations en lecture seule  
 À moins que votre solution Analysis Services inclue l'écriture différée, vous pouvez configurer une connexion de source de données pour utiliser un réplica secondaire lisible. Si votre connexion réseau est rapide, le réplica secondaire a une latence de données très faible et fournit des données pratiquement identiques au réplica principal. En utilisant le réplica secondaire pour les opérations Analysis Services, vous pouvez réduire les conflits de lecture-écriture sur le réplica principal et optimiser l'utilisation des réplicas secondaires dans votre groupe de disponibilité.  
  
 Par défaut, l'accès en lecture/écriture et d'intention de lecture sont autorisés sur le réplica principal et aucune connexion n'est autorisée sur les réplicas secondaires. Une configuration supplémentaire est requise pour configurer une connexion cliente en lecture seule sur un réplica secondaire. La configuration requiert la configuration de propriétés sur le réplica et l'exécution d'un script T-SQL qui définit une liste de routage en lecture seule. Utilisez les procédures suivantes pour vous assurer que vous avez effectué les deux étapes.  
  
> [!NOTE]  
>  Les étapes suivantes présument l’existence d’un groupe de disponibilité et de bases de données Always On. Si vous configurez un nouveau groupe, utilisez l'Assistant Nouveau groupe de disponibilité pour créer le groupe et pour joindre les bases de données. L'Assistant vérifie les composants requis, fournit de l'aide pour chaque étape et exécute la synchronisation initiale. Pour plus d’informations, consultez [Utiliser l’Assistant Groupe de disponibilité &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md).  
  
#### <a name="step-1-configure-access-on-an-availability-replica"></a>Étape 1 : Configurer l'accès sur un réplica de disponibilité  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal et développez l'arborescence du serveur.  
  
    > [!NOTE]  
    >  Ces étapes sont tirées de la rubrique [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md), qui fournit des informations supplémentaires et d’autres instructions pour effectuer cette tâche.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez sur le groupe de disponibilité dont vous souhaitez modifier le réplica. Développez **Réplicas de disponibilité**.  
  
4.  Cliquez avec le bouton droit sur le réplica secondaire, puis cliquez sur **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du réplica de disponibilité** , modifiez l'accès à la connexion pour le rôle secondaire, comme suit :  
  
    -   Dans la liste déroulante **Secondaire accessible en lecture** , sélectionnez **Intention de lecture uniquement**.  
  
    -   Dans la liste déroulante **Connexions en rôle principal** , sélectionnez **Autoriser toutes les connexions**. Il s’agit de la valeur par défaut.  
  
    -   Éventuellement, dans la liste déroulante **Mode de disponibilité** , sélectionnez **Validation synchrone**. Cette étape n'est pas obligatoire, mais elle permet de garantir qu'il existe une parité des données entre le réplica primaire et le réplica secondaire.  
  
         Cette propriété est également exigée pour le basculement planifié. Si vous souhaitez effectuer un basculement planifié manuel à des fins de test, définissez le **Mode de disponibilité** sur **Validation synchrone** à la fois pour le réplica primaire et le réplica secondaire.  
  
#### <a name="step-2-configure-read-only-routing"></a>Étape 2 : Configurer le routage en lecture seule  
  
1.  Connectez-vous au réplica principal.  
  
    > [!NOTE]  
    >  Ces étapes sont tirées de la rubrique [Configurer le routage en lecture seule pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md), qui fournit des informations supplémentaires et d’autres instructions pour effectuer cette tâche.  
  
2.  Ouvrez une fenêtre de requête et collez le script suivant. Ce script effectue trois opérations : il active les connexions lisibles sur un réplica secondaire (option désactivée par défaut), il définit l'URL de routage en lecture seule et il crée la liste de routage qui définit la priorité de direction des demandes de connexion.  La première instruction, autorisant les connexions lisibles, est redondante si vous avez déjà défini les propriétés dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], mais elle est incluse par souci d'exhaustivité.  
  
    ```  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER01.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (ALLOW_CONNECTIONS = READ_ONLY));  
  
    ALTER AVAILABILITY GROUP [AG1]  
     MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (SECONDARY_ROLE (READ_ONLY_ROUTING_URL = N'TCP://COMPUTER02.contoso.com:1433'));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER01' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER02','COMPUTER01')));  
  
    ALTER AVAILABILITY GROUP [AG1]   
    MODIFY REPLICA ON  
    N'COMPUTER02' WITH   
    (PRIMARY_ROLE (READ_ONLY_ROUTING_LIST=('COMPUTER01','COMPUTER02')));  
    GO  
    ```  
  
3.  Modifiez le script en remplaçant les espaces réservés par des valeurs valides pour votre déploiement :  
  
    -   Remplacez « Computer01 » par le nom de l’instance de serveur qui héberge le réplica principal.  
  
    -   Remplacez « Computer02 » par le nom de l’instance de serveur qui héberge le réplica secondaire.  
  
    -   Remplacez « contoso.com » par le nom de votre domaine, ou omettez-le du script si tous les ordinateurs se trouvent dans le même domaine. Conservez le numéro de port si l'écouteur utilise le port par défaut. Le port réellement utilisé par l'écouteur est répertorié dans la page des propriétés dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  Exécutez le script.  
  
     Créez ensuite une source de données dans un modèle Analysis Services qui utilise une base de données à partir du groupe que vous venez de configurer.  
  
##  <a name="create-an-analysis-services-data-source-using-an-always-on-availability-database"></a><a name="bkmk_ssasAODB"></a> Créer une source de données Analysis Services à l’aide d’une base de données de disponibilité Always On  
 Cette section explique comment créer une source de données Analysis Services qui se connecte à une base de données dans un groupe de disponibilité. Vous pouvez utiliser cette instruction pour configurer une connexion à un réplica principal (valeur par défaut) ou un réplica secondaire lisible, configurés en suivant les étapes de l'une des sections précédentes. Les paramètres de configuration Always On, plus les propriétés de connexion définies dans le client, déterminent si un réplica primaire ou secondaire est utilisé.  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)], dans un projet de modèle d’exploration de données et multidimensionnel Analysis Services, cliquez avec le bouton droit sur **Sources de données** et sélectionnez **Nouvelle source de données**. Cliquez sur **Nouvelle** pour créer une nouvelle source de données.  
  
     Sinon, pour un projet de modèle tabulaire, cliquez sur le menu Modèle, puis cliquez sur **Importer à partir de la source de données**.  
  
2.  Dans le Gestionnaire de connexions, dans le Fournisseur, choisissez un fournisseur qui prend en charge le protocole TDS (Tabular Data Stream). SQL Server Native Client 11.0 prend en charge ce protocole.  
  
3.  Dans le Gestionnaire de connexions, dans Nom du serveur, entrez le nom de l' *écouteur du groupe de disponibilité*, puis choisissez une base de données qui est disponible dans le groupe.  
  
     L'écouteur du groupe de disponibilité redirige une connexion cliente sur un réplica principal pour les demandes en lecture-écriture ou sur un réplica secondaire si vous spécifiez l'intention de lecture dans la chaîne de connexion. Étant donné que les rôles de réplica changent lors d'un basculement (lorsque le principal devient le secondaire et un secondaire devient un principal), vous devez toujours spécifier l'écouteur afin que la connexion cliente soit redirigée en conséquence.  
  
     Pour déterminer le nom de l’écouteur du groupe de disponibilité, vous pouvez demander à un administrateur de base de données ou vous connecter à une instance du groupe de disponibilité et afficher sa configuration de disponibilité Always On.   
  
4.  Toujours dans le Gestionnaire de connexions, cliquez sur **Tout** dans le volet de navigation gauche pour afficher la grille des propriétés du fournisseur de données.  
  
     Affectez la valeur **READONLY** à **Intention de l’application** si vous configurez une connexion cliente en lecture seule sur un réplica secondaire. Sinon, conservez la valeur par défaut **READWRITE** pour rediriger la connexion au réplica principal.  
  
5.  Dans les Informations d’emprunt d’identité, sélectionnez **Utiliser un nom d’utilisateur et un mot de passe Windows spécifiques**, puis entrez un compte d’utilisateur de domaine Windows qui a au minimum des autorisations **db_datareader** sur la base de données.  
  
     Ne sélectionnez pas **Utiliser les infos d'identification de l'utilisateur actuel** ou **Hériter**. Vous pouvez choisir **Utiliser le compte de service**, mais uniquement si ce compte a des autorisations de lecture sur la base de données.  
  
     Terminez la source de données et fermez l'Assistant Source de données.  
  
6.  Ajoutez **MultiSubnetFailover=Yes** à la chaîne de connexion pour fournir une détection et une connexion plus rapides au serveur actif. Pour plus d'informations sur cette propriété, consultez [Prise en charge des fonctionnalités de récupération d'urgence, haute disponibilité par SQL Server Native Client](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md).  
  
     Cette propriété n'est pas visible dans la grille des propriétés. Pour ajouter la propriété, cliquez avec le bouton droit sur la source de données et choisissez **Afficher le code**. Ajoutez `MultiSubnetFailover=Yes` à la chaîne de connexion.  
  
 La source de données est maintenant définie. Vous pouvez maintenant créer un modèle, en commençant par la vue de source de données ou, dans le cas de modèles tabulaires, en créant des relations. Lorsque vous arrivez au stade où les données doivent être récupérées de la base de données de disponibilité (par exemple lorsque vous êtes prêt à traiter ou déployer la solution), vous pouvez tester la configuration pour vérifier que les données sont accessibles à partir du réplica secondaire.  
  
##  <a name="test-the-configuration"></a><a name="bkmk_test"></a> Tester la configuration  
 Après avoir configuré le réplica secondaire et créez une connexion à la source de données dans Analysis Services, vous pouvez vérifier que le traitement et les commandes de requête sont redirigées vers le réplica secondaire. Vous pouvez également effectuer un basculement manuel planifié afin de vérifier votre plan de récupération pour ce scénario.  
  
#### <a name="step-1-confirm-the-data-source-connection-is-redirected-to-the-secondary-replica"></a>Étape 1 : Vérifier que la connexion à la source de données est redirigé vers le réplica secondaire  
  
1.  Démarrez le Générateur de profils SQL Server et connectez-vous à l'instance SQL Server qui héberge le réplica secondaire.  
  
     À mesure que la trace s'exécute, les événements **SQL:BatchStarting** et **SQL:BatchCompleting** affichent les requêtes émises à partir d'Analysis Services qui s'exécutent sur l'instance du moteur de base de données. Ces événements sont sélectionnés par défaut, par conséquent il vous suffit de démarrer la trace.  
  
2.  Dans [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)], ouvrez le projet ou la solution Analysis Services contenant la connexion à la source de données que vous souhaitez tester. Assurez-vous que la source de données spécifie l'écouteur du groupe de disponibilité et non une instance du groupe.  
  
     Cette étape est importante. Le routage au réplica secondaire ne se produit pas si vous spécifiez un nom d'instance de serveur.  
  
3.  Réorganisez les fenêtres d'application afin que vous puissiez afficher le Générateur de profils SQL Server et [!INCLUDE[ssBIDevStudio](../../../includes/ssbidevstudio-md.md)] côte à côte.  
  
4.  Déployez la solution puis, lorsque cela est terminé, arrêtez la trace.  
  
     Dans la fenêtre de trace, vous devez voir les événements de l'application **Microsoft SQL Server Analysis Services**. Vous devez voir les instructions **SELECT** qui extraient les données d'une base de données sur l'instance de serveur qui héberge le réplica secondaire, ce qui prouve que la connexion a été établie via l'écouteur du réplica secondaire.  
  
#### <a name="step-2-perform-a-planned-failover-to-test-the-configuration"></a>Étape 2 : Effectuer un basculement planifié pour tester la configuration  
  
1.  Dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] , vérifiez les réplicas primaire et secondaire pour vous assurer qu'ils sont tous deux configurés pour le mode de validation synchrone et qu'ils sont actuellement synchronisés.  
  
     Les étapes suivantes impliquent qu'un réplica secondaire est configuré pour la validation synchrone.  
  
     Pour vérifier la synchronisation, ouvrez une connexion à chaque instance qui héberge les réplicas primaire et secondaire, développez le dossier Bases de données et vérifiez que **(Synchronisé)** et **(Synchronisation)** sont ajoutés au nom de la base de données dans chaque réplica.  
  
    > [!NOTE]  
    >  Ces étapes sont tirées de la rubrique [Effectuer un basculement manuel planifié d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md), qui fournit des informations supplémentaires et d’autres instructions pour effectuer cette tâche.  
  
2.  Dans le Générateur de profils SQL Server, démarrez les traces pour chaque réplica et affichez les traces côte à côte. Dans les étapes suivantes, vous allez comparer des traces, pour vérifier que les requêtes SQL utilisées pour le traitement ou l'interrogation depuis Analysis Services basculent d'un réplica à l'autre.  
  
3.  Exécutez un traitement ou une commande de requête depuis Analysis Services. Étant donné que vous avez configuré la source de données pour une connexion en lecture seule, vous devez voir la commande s'exécuter sur le réplica secondaire.  
  
4.  Dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], connectez-vous au réplica secondaire.  
  
5.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
6.  Cliquez avec le bouton droit sur le groupe de disponibilité à basculer et sélectionnez la commande **Basculement** . Cette commande démarre l'Assistant Basculer le groupe de disponibilité. Utilisez l'Assistant pour choisir le réplica qui sera le nouveau réplica principal.  
  
7.  Vérifiez que le basculement a réussi :  
  
    -   Dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)], développez les groupes de disponibilité pour afficher les désignations (principales) et (secondaires). L'instance qui était précédemment un réplica principal doit maintenant être un réplica secondaire.  
  
    -   Affichez le tableau de bord pour déterminer si des problèmes d'intégrité ont été détectés. Cliquez avec le bouton droit sur le groupe de disponibilité et sélectionnez **Afficher le tableau de bord**.  
  
8.  Attendez une ou deux minutes que le basculement se termine sur le service principal.  
  
9. Répétez le traitement ou la commande de requête dans la solution Analysis Services, puis affichez les traces côte à côte dans le Générateur de profils SQL Server. Vous devez voir le traitement sur l'autre instance, qui est désormais le nouveau réplica secondaire.  
  
##  <a name="what-happens-after-a-failover-occurs"></a><a name="bkmk_whathappens"></a> Que se passe-t-il après un basculement  
 Lors d'un basculement, un réplica secondaire adopte le rôle principal et l'ancien réplica principal joue le rôle secondaire. Toutes les connexions clientes sont terminées, la propriété de l’écouteur du groupe de disponibilité passe avec le rôle de réplica principal à une nouvelle instance de SQL Server, et le point de terminaison de l’écouteur est lié aux adresses IP virtuelles et aux ports TCP de la nouvelle instance. Pour plus d’informations, consultez [À propos de l’accès de la connexion client aux réplicas de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md).  
  
 Si le basculement se produit lors du traitement, l’erreur suivante apparaît dans Analysis Services dans le fichier journal ou la fenêtre de sortie : « Erreur OLE DB : Erreur OLE DB ou ODBC : Échec de la liaison de communication ; 08S01 ; Fournisseur de TPC : Une connexion existante a dû être fermée par l’hôte distant. ; 08S01. »  
  
 Cette erreur devrait être résolue si vous attendez une minute avant de recommencer. Si le groupe de disponibilité est configuré correctement pour le réplica secondaire lisible, le traitement continue sur le nouveau réplica secondaire lorsque vous réexécutez le traitement.  
  
 Les erreurs persistantes sont généralement dues à un problème de configuration. Vous pouvez essayer de réexécuter le script T-SQL pour résoudre les problèmes liés à la liste de routage, aux URL de routage en lecture seule et à l'intention de lecture sur le réplica secondaire. Vous devez également vérifier que le réplica principal autorise toutes les connexions.  
  
##  <a name="writeback-when-using-an-always-on-availability-database"></a><a name="bkmk_writeback"></a> Écriture différée lors de l’utilisation d’une base de données de disponibilité Always On  
 L'écriture différée est une fonctionnalité Analysis Services qui prend en charge l'analyse Scénario dans Excel. Elle est généralement utilisée pour budgéter et prévoir des tâches dans des applications personnalisées.  
  
 La prise en charge de l'écriture différée nécessite une connexion cliente READWRITE. Dans Excel, si vous tentez d’écrire en différé sur une connexion en lecture seule, l’erreur suivante se produit : « Impossible de récupérer les données de la source de données externe. ». « Impossible de récupérer les données de la source de données externe. »  
  
 Si vous avez configuré une connexion pour accéder toujours à un réplica secondaire lisible, vous devez maintenant configurer une connexion qui utilise une connexion READWRITE au réplica principal.  
  
 Pour cela, créez une source de données supplémentaire dans un modèle Analysis Services pour prendre en charge la connexion en lecture-écriture. Lors de la création de la source de données supplémentaire, utilisez le nom d’écouteur et la base de données que vous avez spécifiés dans la connexion en lecture seule, mais au lieu de modifier **Intention de l’application**, conservez la valeur par défaut qui prend en charge les connexions READWRITE. Vous pouvez à présent ajouter de nouvelles tables de faits ou de dimension à votre vue de source de données, basées sur la source de données en lecture-écriture, puis activer l'écriture différée sur les nouvelles tables.  
  
## <a name="see-also"></a>Voir aussi  
 [Écouteurs de groupe de disponibilité, connectivité client et basculement d’application &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Secondaires actifs : réplicas secondaires lisibles &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)   
 [Stratégies Always On pour les problèmes opérationnels avec les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-policies-for-operational-issues-always-on-availability.md)   
 [Créer une source de données &#40;SSAS Multidimensionnel&#41;](/analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional)   
 [Activer l’écriture différée de la dimension](/analysis-services/multidimensional-models/bi-wizard-enable-dimension-writeback)  
  
