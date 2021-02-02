---
title: Mesurer la latence et valider les connexions (Transactionnel)
description: Découvrez comment mesurer la latence et valider les connexions pour une publication de transaction dans SQL Server en utilisant le moniteur de réplication dans SQL Server Management Studio (SSMS), Transact-SQL (T-SQL) ou Replication Management Objects (RMO).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Replication Monitor, performance
- tracer tokens [SQL Server replication]
- latency [SQL Server replication]
- transactional replication, tracer tokens
- monitoring performance [SQL Server replication], tracer tokens
ms.assetid: 4addd426-7523-4067-8d7d-ca6bae4c9e34
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 99e2551cef1208a2fe2f0f4c14280fdf2bfe83df
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077157"
---
# <a name="measure-latency-and-validate-connections-for-transactional-replication"></a>Mesurer la latence et valider les connexions pour la réplication transactionnelle
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment mesurer la latence et valider les connexions pour la réplication transactionnelle dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide du Moniteur de réplication, de [!INCLUDE[tsql](../../../includes/tsql-md.md)]ou d'objets RMO (Replication Management Objects). La réplication transactionnelle offre la fonctionnalité de jeton de suivi, moyen facile de mesurer la latence dans les topologies de réplication transactionnelle et de valider les connexions entre le serveur de publication, le serveur de distribution et les Abonnés. Un jeton (une petite quantité de données) est écrit dans le journal des transactions de la base de données de publication et est marqué comme s'il s'agissait d'une transaction standard répliquée, puis est envoyé dans le système, permettant ainsi le calcul :  
  
-   du temps écoulé entre la validation d'une transaction par le serveur de publication et l'insertion de la commande qui en découle dans la base de données de distribution sur le serveur de distribution ;  
  
-   du temps écoulé entre l'insertion d'une commande dans la base de données de distribution et la validation de la transaction qui en découle au niveau d'un Abonné.  
  
 De par ces calculs, vous pouvez ainsi répondre à diverses questions, notamment :  
  
-   Quels Abonnés mettent le plus longtemps à recevoir une modification du serveur de publication ?  
  
-   De l'ensemble des Abonnés prévus pour recevoir le jeton de suivi, lesquels, le cas échéant, ne l'ont pas reçu ?  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   **Pour mesurer la latence et valider les connexions à l'aide de :**  
  
     [Moniteur de réplication de SQL Server](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
 Les jetons de suivi peuvent également être utiles lors de la suspension d'un système, qui consiste à arrêter toute l'activité pour vérifier que tous les nœuds ont reçu les changements en cours. Pour plus d’informations, consultez [Suspendre une topologie de réplication &#40;programmation Transact-SQL de la réplication&#41;](../../../relational-databases/replication/administration/quiesce-a-replication-topology-replication-transact-sql-programming.md).  
  
 Pour utiliser les jetons de suivi, vous avez besoin de certaines versions de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   Le serveur de distribution doit être [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ou une version ultérieure.  
  
-   Le serveur de publication doit être [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] ou version ultérieure, ou un serveur de publication Oracle.  
  
-   Pour les abonnements par envoi de données, les statistiques de jetons de suivi sont collectées à partir du serveur de publication, du serveur de distribution et des Abonnés si l’Abonné exécute [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 ou une version ultérieure.  
  
-   Pour les abonnements par extraction de données, les statistiques de jetons de suivi sont rassemblées uniquement à partir des Abonnés si l'Abonné exécute [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 7.0 ou version ultérieure. Si l’Abonné exécute [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 7.0 ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)], les statistiques sont collectées uniquement à partir du serveur de publication et du serveur de distribution.  
  
 Il existe aussi d'autres problèmes et restrictions à connaître :  
  
-   Les abonnements doivent être actifs pour recevoir un jeton de suivi. Un abonnement est actif s'il a été initialisé.  
  
-   La réinitialisation supprime tout jeton de suivi en suspens pour les abonnements concernés.  
  
-   Les Abonnés ne reçoivent que les jetons de suivi créés après leur première synchronisation.  
  
-   Les jetons de suivi ne sont pas retransmis par les Abonnés qui republient.  
  
-   Après un basculement vers un réplica secondaire, le Moniteur de réplication ne peut pas ajuster le nom de l'instance de publication de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et continue à afficher les informations de réplication sous le nom de l'instance principale [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]d'origine. Après le basculement, un jeton de suivi ne peut pas être écrit à l'aide du Moniteur de réplication, toutefois un jeton de suivi écrit sur le nouveau serveur de publication à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)]est visible dans le Moniteur de réplication.  
  
##  <a name="using-sql-server-replication-monitor"></a><a name="SSMSProcedure"></a> Utilisation du Moniteur de réplication SQL Server  
 Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-insert-a-tracer-token-and-view-information-on-the-token"></a>Pour insérer un jeton de suivi et afficher des informations sur le jeton  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Jetons de suivi** .  
  
3.  Cliquez sur **Insérer un suivi**.  
  
4.  Affichez le temps écoulé pour le jeton de suivi dans les colonnes suivantes : **Du serveur de publication vers le serveur de distribution**, **Du serveur de distribution vers l'Abonné** et **Latence totale**. Une valeur **En attente** indique que le jeton n'a pas atteint un point donné.  
  
#### <a name="to-view-information-on-a-tracer-token-inserted-previously"></a>Pour afficher les informations d'un jeton de suivi inséré précédemment  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Jetons de suivi** .  
  
3.  Sélectionnez une heure dans la liste déroulante **Heure de l'insertion** .  
  
4.  Affichez le temps écoulé pour le jeton de suivi dans les colonnes suivantes : **Du serveur de publication vers le serveur de distribution**, **Du serveur de distribution vers l'Abonné** et **Latence totale**. Une valeur **En attente** indique que le jeton n'a pas atteint un point donné.  
  
    > [!NOTE]  
    >  Les informations de jeton de suivi sont conservées pour la même durée que toute autre donnée d'historique, elles-mêmes étant régies par la période de rétention des historiques définie dans la base de données de distribution. Pour plus d’informations sur la modification des propriétés de base de données de distribution, consultez [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>Pour publier un jeton de suivi sur une publication transactionnelle  
  
1.  (Facultatif) Dans la base de données de publication sur le serveur de publication, exécutez [sp_helppublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). Vérifiez que la publication existe et que l'état est actif.  
  
2.  (Facultatif) Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpsubscription &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Vérifiez que l'abonnement existe et que l'état est actif.  
  
3.  Dans la base de données du serveur de publication sur le serveur de publication, exécutez [sp_posttracertoken &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-posttracertoken-transact-sql.md), en spécifiant **\@publication**. Notez la valeur du paramètre de sortie **\@tracer_token_id**.  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>Pour déterminer la latence et valider les connexions d'une publication transactionnelle  
  
1.  Publiez un jeton de suivi sur la publication à l'aide de la procédure précédente.  
  
2.  Dans la base de données du serveur de publication sur le serveur de publication, exécutez [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md), en spécifiant **\@publication**. La liste de tous les jetons de suivi publiés sur la publication est ainsi retournée. Notez le **tracer_id** désiré dans le jeu de résultats.  
  
3.  Dans la base de données du serveur de publication sur le serveur de publication, exécutez [sp_helptracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md), en spécifiant **\@publication** et l’ID du jeton de suivi obtenu à l’étape 2 pour **\@tracer_id**. Les informations de latence pour le jeton de suivi sélectionné sont ainsi retournées.  
  
#### <a name="to-remove-tracer-tokens"></a>Pour supprimer les jetons de suivi  
  
1.  Dans la base de données du serveur de publication sur le serveur de publication, exécutez [sp_helptracertokens &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md), en spécifiant **\@publication**. La liste de tous les jetons de suivi publiés sur la publication est ainsi retournée. Notez **tracer_id** pour le jeton de suivi à supprimer dans le jeu de résultats.  
  
2.  Dans la base de données du serveur de publication sur le serveur de publication, exécutez [sp_deletetracertokenhistory &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md), en spécifiant **\@publication** et l’ID du jeton de suivi à supprimer obtenu à l’étape 2 pour `@tracer_id`.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
 Cet exemple publie un enregistrement de jeton de suivi et utilise l'ID retourné du jeton de suivi publié pour consulter les informations de latence.  
  
 [!code-sql[HowTo#sp_tracertokens](../../../relational-databases/replication/codesnippet/tsql/measure-latency-and-vali_1.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
  
#### <a name="to-post-a-tracer-token-to-a-transactional-publication"></a>Pour publier un jeton de suivi sur une publication transactionnelle  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPublication>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> et <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> de la publication, et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la connexion créée à l'étape 1.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de la publication ont été définies de manière incorrecte à l’étape 3, soit la publication n’existe pas.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.TransPublication.PostTracerToken%2A> . Cette méthode insère un jeton de suivi dans le journal des transactions de la publication.  
  
#### <a name="to-determine-latency-and-validate-connections-for-a-transactional-publication"></a>Pour déterminer la latence et valider les connexions d'une publication transactionnelle  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.PublicationMonitor>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>, et <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> , et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la connexion créée à l'étape 1.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de surveillance de la publication ont été définies de manière incorrecte à l'étape 3, soit la publication n'existe pas.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> . Effectuez un cast de l'objet <xref:System.Collections.ArrayList> retourné en un tableau d'objets <xref:Microsoft.SqlServer.Replication.TracerToken> .  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokenHistory%2A> . Passez une valeur de <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> pour un jeton de suivi de l'étape 5. Les informations de latence pour le jeton de suivi sélectionné sont ainsi retournées comme objet <xref:System.Data.DataSet> . Si toutes les informations de jeton de suivi sont retournées, la connexion entre le serveur de publication et le serveur de distribution et la connexion entre le serveur de distribution et l'Abonné existent et la topologie de réplication fonctionne.  
  
#### <a name="to-remove-tracer-tokens"></a>Pour supprimer les jetons de suivi  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.PublicationMonitor>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.PublicationMonitor.Name%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.DistributionDBName%2A>, <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublisherName%2A>, et <xref:Microsoft.SqlServer.Replication.PublicationMonitor.PublicationDBName%2A> , et définissez la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> avec la connexion créée à l'étape 1.  
  
4.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de surveillance de la publication ont été définies de manière incorrecte à l'étape 3, soit la publication n'existe pas.  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PublicationMonitor.EnumTracerTokens%2A> . Effectuez un cast de l'objet <xref:System.Collections.ArrayList> retourné en un tableau d'objets <xref:Microsoft.SqlServer.Replication.TracerToken> .  
  
6.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.PublicationMonitor.CleanUpTracerTokenHistory%2A> . Passez l'une des valeurs suivantes :  
  
    -   <xref:Microsoft.SqlServer.Replication.TracerToken.TracerTokenId%2A> pour un jeton de suivi de l'étape 5. Les informations d'un jeton sélectionné sont alors supprimées.  
  
    -   Objet <xref:System.DateTime> . Les informations pour tous les jetons plus anciens que les date et heure spécifiées sont alors supprimées.  
  
  
