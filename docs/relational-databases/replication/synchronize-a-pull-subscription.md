---
description: Synchroniser un abonnement par extraction
title: Synchroniser un abonnement par extraction | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- pull subscriptions [SQL Server replication], synchronizing
- synchronization [SQL Server replication], pull subscriptions
- subscriptions [SQL Server replication], pull
ms.assetid: 3ca24b23-fdc3-408e-8208-a2ace48fc8e3
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a8c254caf9d8142b9135d7424dda35095b2ff72a
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250584"
---
# <a name="synchronize-a-pull-subscription"></a>Synchroniser un abonnement par extraction
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Cette rubrique explique comment synchroniser un abonnement par extraction de données (pull) dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], d' [agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md)ou d'objets RMO (Replication Management Objects).  
  
 **Dans cette rubrique**  
  
-   **Pour synchroniser un abonnement par extraction de données (pull) à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Replication Agents](#ReplProg)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Les abonnements sont synchronisés par l'Agent de distribution (pour la réplication transactionnelle et d'instantané) ou l'Agent de fusion (pour la réplication de fusion). Les agents peuvent s'exécuter en continu, à la demande ou selon une planification. Pour plus d’informations sur la spécification de planifications de synchronisation, consultez [Spécifier des planifications de synchronisation](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
 Synchronisez un abonnement à la demande à partir du dossier **Publications locales** dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-synchronize-a-pull-subscription-on-demand-in-management-studio"></a>Pour synchroniser à la demande un abonnement par extraction de données dans Management Studio  
  
1.  Connectez-vous à l'Abonné dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], puis développez le nœud du serveur.  
  
2.  Développez le dossier **Réplication** , puis développez le dossier **Abonnements locaux** .  
  
3.  Cliquez avec le bouton droit sur l'abonnement à synchroniser, puis cliquez sur **Afficher l'état de synchronisation**.  
  
4.  Dans la boîte de dialogue **Afficher l'état de synchronisation - \<Subscriber>:\<SubscriptionDatabase>** cliquez sur **Démarrer**. Lorsque la synchronisation est terminée, le message **Synchronisation terminée** s'affiche.  
  
5.  Cliquez sur **Fermer**.  
  
##  <a name="replication-agents"></a><a name="ReplProg"></a> Replication Agents  
 Les abonnements par extraction de données (pull) peuvent être synchronisés par le biais de la programmation et à la demande en appelant le fichier exécutable de l'Agent de réplication approprié à partir de l'invite de commandes. Le fichier exécutable de l'Agent de réplication qui est appelé dépend du type de publication à laquelle l'abonnement par extraction de données (pull) appartient. Pour plus d'informations, voir [Replication Agents](../../relational-databases/replication/agents/replication-agents-overview.md).  
  
> [!NOTE]  
>  Les Agents de réplication se connectent au serveur local au moyen des informations d'identification d'authentification Windows de l'utilisateur qui a démarré l'agent à partir de l'invite de commandes. Ces informations d'identification Windows sont également utilisées lors de la connexion à des serveurs distants au moyen de l'authentification intégrée Windows.  
  
#### <a name="to-start-the-distribution-agent-from-the-command-prompt-or-from-a-batch-file"></a>Pour démarrer l'Agent de distribution à partir de l'invite de commandes ou à partir d'un fichier de commandes  
  
1.  À partir de l'invite de commandes ou dans un fichier de commandes, démarrez l' [Agent de distribution de réplication](../../relational-databases/replication/agents/replication-distribution-agent.md) en exécutant **distrib.exe** et en spécifiant les arguments suivant sur la ligne de commande :  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-Distributor**  
  
    -   **-DistributorSecurityMode** = **1**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriberSecurityMode** = **1**  
  
    -   **-SubscriptionType** = **1**  
  
     Si vous utilisez l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez également spécifier les arguments suivants :  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode** = **0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode** = **0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode** = **0**  
  
#### <a name="to-start-the-merge-agent-from-the-command-prompt-or-from-a-batch-file"></a>Pour démarrer l'Agent de fusion à partir de l'invite de commandes ou à partir d'un fichier de commandes  
  
1.  À partir de l'invite de commandes ou dans un fichier de commandes, démarrez l' [Agent de fusion de réplication](../../relational-databases/replication/agents/replication-merge-agent.md) en exécutant **replmerg.exe** et en spécifiant les arguments suivant sur la ligne de commande :  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-PublisherSecurityMode** = **1**  
  
    -   **-Publication**  
  
    -   **-Distributor**  
  
    -   **-DistributorSecurityMode** = **1**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberSecurityMode** = **1**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriptionType** = **1**  
  
     Si vous utilisez l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez également spécifier les arguments suivants :  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode** = **0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode** = **0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode** = **0**  
  
###  <a name="examples-replication-agents"></a><a name="TsqlExample"></a> Exemples (Agents de réplication)  
 L'exemple suivant démarre l'Agent de distribution pour synchroniser un abonnement par extraction de données (pull). Toutes les connexions sont effectuées au moyen de l'authentification Windows.  
  
```  
 -- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks  
SET SubscriptionDB=AdventureWorksReplica   
SET Publication=AdvWorksProductsTran  
  
-- Start the Distribution Agent.  
-- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\100\COM\DISTRIB.EXE" -Subscriber %Subscriber%   
-SubscriberDB %SubscriptionDB% -SubscriberSecurityMode 1 -Publication %Publication%   
-Publisher %Publisher% -PublisherDB %PublicationDB% -Distributor %Publisher%   
-DistributorSecurityMode 1 -Continuous -SubscriptionType 1;  
```  
  
 L'exemple suivant démarre l'Agent de fusion pour synchroniser un abonnement par extraction de données (pull). Toutes les connexions sont effectuées au moyen de l'authentification Windows.  
  
```  
-- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks  
SET SubscriptionDB=AdventureWorksReplica   
SET Publication=AdvWorksSalesOrdersMerge  
  
--Start the Merge Agent with concurrent upload and download processes.  
-- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\100\COM\REPLMERG.EXE" -Publication %Publication%    
-Publisher %Publisher%  -Subscriber  %Subscriber%  -Distributor %Publisher%    
-PublisherDB %PublicationDB%  -SubscriberDB %SubscriptionDB% -PublisherSecurityMode 1    
-OutputVerboseLevel 2  -SubscriberSecurityMode 1  -SubscriptionType 1 -DistributorSecurityMode 1    
-Validate 3  -ParallelUploadDownload 1 ;  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
 Vous pouvez synchroniser des abonnements par extraction de données (pull) au moyen d'objets RMO (Replication Management Objects) et d'un accès par code managé aux fonctionnalités de l'Agent de réplication. Les classes que vous utilisez pour synchroniser un abonnement par extraction de données (pull) sont fonction du type de publication à laquelle l'abonnement appartient.  
  
> [!NOTE]
>  Si vous voulez démarrer une synchronisation qui s'exécute de façon autonome sans affecter votre application, démarrez l'agent en mode asynchrone. Toutefois, si vous voulez analyser le résultat de la synchronisation et recevoir des rappels à partir de l'agent au cours du processus de synchronisation (par exemple, pour afficher une barre de progression), démarrez l'agent en mode synchrone. Pour Abonnés [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] , vous devez démarrer l’agent en mode synchrone.  
  
#### <a name="to-synchronize-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Pour synchroniser un abonnement par extraction vers une publication d'instantané ou transactionnelle  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransPullSubscription> et définissez les propriétés suivantes :  
  
    -   Le nom de la base de données d'abonnement pour <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>.  
  
    -   Le nom de la publication à laquelle l'abonnement appartient pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>.  
  
    -   le nom de la base de données de publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>;  
  
    -   Le nom du serveur de publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>.  
  
    -   La connexion créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés d'abonnement restantes. Si cette méthode retourne **false**, vérifiez que l'abonnement existe.  
  
4.  Démarrez l'Agent de distribution sur l'Abonné de l'une des façons suivantes :  
  
    -   Appelez la méthode <xref:Microsoft.SqlServer.Replication.TransPullSubscription.SynchronizeWithJob%2A> sur l'instance de <xref:Microsoft.SqlServer.Replication.TransPullSubscription> obtenue à l'étape 2. Cette méthode démarre l'Agent de distribution en mode asynchrone et votre application récupère immédiatement le contrôle pendant l'exécution du travail de l'agent. Vous ne pouvez pas appeler cette méthode pour les Abonnés [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] ou si l'abonnement a été créé avec la valeur **false** for <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> (la valeur par défaut).  
  
    -   Obtenez une instance de la classe <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent> à partir de la propriété <xref:Microsoft.SqlServer.Replication.TransPullSubscription.SynchronizationAgent%2A> et appelez la méthode <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Synchronize%2A> . Cette méthode démarre l'agent en mode synchrone, et le travail d'agent en cours d'exécution conserve le contrôle. Au cours de l'exécution synchrone, vous pouvez gérer l'événement <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Status> pendant que l'agent est en cours d'exécution.  
  
        > [!NOTE]  
        >  Si vous avez spécifié la valeur **false** for <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> (la valeur par défaut) lorsque vous avez créé l'abonnement par envoi de données (pull), vous devez également spécifier <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Distributor%2A>, d' <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.DistributorSecurityMode%2A>et, facultativement, <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.DistributorLogin%2A> et <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.DistributorPassword%2A> parce que les métadonnées liées au travail d'agent pour l'abonnement ne sont pas disponibles dans [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md).  
  
#### <a name="to-synchronize-a-pull-subscription-to-a-merge-publication"></a>Pour synchroniser un abonnement par extraction de données (pull) vers une publication de fusion  
  
1.  Créez une connexion à l'Abonné en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergePullSubscription> et définissez les propriétés suivantes :  
  
    -   Le nom de la base de données d'abonnement pour <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>.  
  
    -   Le nom de la publication à laquelle l'abonnement appartient pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>.  
  
    -   Le nom de la base de données publiée pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>.  
  
    -   Le nom du serveur de publication pour <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>.  
  
    -   La connexion créée à l'étape 1 pour <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés d'abonnement restantes. Si cette méthode retourne **false**, vérifiez que l'abonnement existe.  
  
4.  Démarrez l'Agent de fusion sur l'Abonné de l'une des façons suivantes :  
  
    -   Appelez la méthode <xref:Microsoft.SqlServer.Replication.MergePullSubscription.SynchronizeWithJob%2A> sur l'instance de <xref:Microsoft.SqlServer.Replication.MergePullSubscription> obtenue à l'étape 2. Cette méthode démarre l'Agent de fusion en mode asynchrone et votre application récupère immédiatement le contrôle pendant l'exécution du travail de l'agent. Vous ne pouvez pas appeler cette méthode pour les Abonnés [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] ou si l'abonnement a été créé avec la valeur **false** for <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> (la valeur par défaut).  
  
    -   Obtenez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent> à partir de la propriété <xref:Microsoft.SqlServer.Replication.MergePullSubscription.SynchronizationAgent%2A> et appelez la méthode <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Synchronize%2A> . Cette méthode démarre l'Agent de fusion en mode synchrone, et le travail d'agent en cours d'exécution conserve le contrôle. Au cours de l'exécution synchrone, vous pouvez gérer l'événement <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Status> pendant que l'agent est en cours d'exécution.  
  
        > [!NOTE]  
        >  Si vous avez spécifié la valeur **false** for <xref:Microsoft.SqlServer.Replication.PullSubscription.CreateSyncAgentByDefault%2A> (la valeur par défaut) lorsque vous avez créé l'abonnement par envoi de données (pull), vous devez également spécifier <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Distributor%2A>, d' <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.DistributorSecurityMode%2A>, d' <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.PublisherSecurityMode%2A>, d' <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.HostName%2A>, d' <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.SubscriptionType%2A>, d' <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.ExchangeType%2A>et, facultativement, <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.DistributorLogin%2A>, d' <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.DistributorPassword%2A>, d' <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.PublisherLogin%2A>et <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.PublisherPassword%2A> parce que les métadonnées liées au travail d'agent pour l'abonnement ne sont pas disponibles dans [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple synchronise un abonnement par envoi de données (pull) vers une publication transactionnelle, où l'agent est démarré en mode asynchrone au moyen du travail d'agent.  
  
```csharp  
// Define server, publication, and database names.  
String subscriberName = subscriberInstance;  
String publisherName = publisherInstance;  
String publicationName = "AdvWorksProductTran";  
String publicationDbName = "AdventureWorks";  
String subscriptionDbName = "AdventureWorksReplica";  
  
// Create a connection to the Subscriber.  
ServerConnection conn = new ServerConnection(subscriberName);  
  
TransPullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    conn.Connect();  
  
    // Define subscription properties.  
    subscription = new TransPullSubscription();  
    subscription.ConnectionContext = conn;  
    subscription.DatabaseName = subscriptionDbName;  
    subscription.PublisherName = publisherName;  
    subscription.PublicationDBName = publicationDbName;  
    subscription.PublicationName = publicationName;  
  
    // If the pull subscription and the job exists, start the agent job.  
    if (subscription.LoadProperties() && subscription.AgentJobId != null)  
    {  
        subscription.SynchronizeWithJob();  
    }  
    else  
    {  
        // Do something here if the subscription does not exist.  
        throw new ApplicationException(String.Format(  
            "A subscription to '{0}' does not exists on {1}",  
            publicationName, subscriberName));  
    }  
}  
catch (Exception ex)  
{  
    // Do appropriate error handling here.  
    throw new ApplicationException("The subscription could not be synchronized.", ex);  
}  
finally  
{  
    conn.Disconnect();  
}  
```  
  
```vb  
' Define server, publication, and database names.  
Dim subscriberName As String = subscriberInstance  
Dim publisherName As String = publisherInstance  
Dim publicationName As String = "AdvWorksProductTran"  
Dim publicationDbName As String = "AdventureWorks"  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
  
' Create a connection to the Subscriber.  
Dim conn As ServerConnection = New ServerConnection(subscriberName)  
  
Dim subscription As TransPullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    conn.Connect()  
  
    ' Define subscription properties.  
    subscription = New TransPullSubscription()  
    subscription.ConnectionContext = conn  
    subscription.DatabaseName = subscriptionDbName  
    subscription.PublisherName = publisherName  
    subscription.PublicationDBName = publicationDbName  
    subscription.PublicationName = publicationName  
  
    ' If the pull subscription and the job exists, start the agent job.  
    If subscription.LoadProperties() And Not subscription.AgentJobId Is Nothing Then  
        subscription.SynchronizeWithJob()  
    Else  
        ' Do something here if the subscription does not exist.  
        Throw New ApplicationException(String.Format( _  
         "A subscription to '{0}' does not exists on {1}", _  
         publicationName, subscriberName))  
    End If  
Catch ex As Exception  
    ' Do appropriate error handling here.  
    Throw New ApplicationException("The subscription could not be synchronized.", ex)  
Finally  
    conn.Disconnect()  
End Try  
```  
  
 Cet exemple synchronise un abonnement par envoi de données (pull) vers une publication transactionnelle, où l'agent est démarré en mode synchrone.  
  
```csharp  
// Define the server, publication, and database names.  
string subscriberName = subscriberInstance;  
string publisherName = publisherInstance;  
string publicationName = "AdvWorksProductTran";  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
// Create a connection to the Subscriber.  
ServerConnection conn = new ServerConnection(subscriberName);  
  
TransPullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    conn.Connect();  
  
    // Define the pull subscription.  
    subscription = new TransPullSubscription();  
    subscription.ConnectionContext = conn;  
    subscription.DatabaseName = subscriptionDbName;  
    subscription.PublisherName = publisherName;  
    subscription.PublicationDBName = publicationDbName;  
    subscription.PublicationName = publicationName;  
  
    // If the pull subscription exists, then start the synchronization.  
    if (subscription.LoadProperties())  
    {  
        // Check that we have enough metadata to start the agent.  
        if (subscription.PublisherSecurity != null)  
        {  
            // Synchronously start the Distribution Agent for the subscription.  
            subscription.SynchronizationAgent.Synchronize();  
        }  
        else  
        {  
            throw new ApplicationException("There is insufficent metadata to " +  
                "synchronize the subscription. Recreate the subscription with " +  
                "the agent job or supply the required agent properties at run time.");  
        }  
    }  
    else  
    {  
        // Do something here if the pull subscription does not exist.  
        throw new ApplicationException(String.Format(  
            "A subscription to '{0}' does not exist on {1}",  
            publicationName, subscriberName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement appropriate error handling here.  
    throw new ApplicationException("The subscription could not be " +  
        "synchronized. Verify that the subscription has " +  
        "been defined correctly.", ex);  
}  
finally  
{  
    conn.Disconnect();  
}  
```  
  
```vb  
' Define the server, publication, and database names.  
Dim subscriberName As String = subscriberInstance  
Dim publisherName As String = publisherInstance  
Dim publicationName As String = "AdvWorksProductTran"  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
' Create a connection to the Subscriber.  
Dim conn As ServerConnection = New ServerConnection(subscriberName)  
  
Dim subscription As TransPullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    conn.Connect()  
  
    ' Define the pull subscription.  
    subscription = New TransPullSubscription()  
    subscription.ConnectionContext = conn  
    subscription.DatabaseName = subscriptionDbName  
    subscription.PublisherName = publisherName  
    subscription.PublicationDBName = publicationDbName  
    subscription.PublicationName = publicationName  
  
    ' If the pull subscription exists, then start the synchronization.  
    If subscription.LoadProperties() Then  
        ' Check that we have enough metadata to start the agent.  
        If Not subscription.PublisherSecurity Is Nothing Then  
  
            ' Write agent output to a log file.  
            subscription.SynchronizationAgent.Output = "distagent.log"  
            subscription.SynchronizationAgent.OutputVerboseLevel = 2  
  
            ' Synchronously start the Distribution Agent for the subscription.  
            subscription.SynchronizationAgent.Synchronize()  
        Else  
            Throw New ApplicationException("There is insufficent metadata to " + _  
             "synchronize the subscription. Recreate the subscription with " + _  
             "the agent job or supply the required agent properties at run time.")  
        End If  
    Else  
        ' Do something here if the pull subscription does not exist.  
        Throw New ApplicationException(String.Format( _  
         "A subscription to '{0}' does not exist on {1}", _  
         publicationName, subscriberName))  
    End If  
Catch ex As Exception  
    ' Implement appropriate error handling here.  
    Throw New ApplicationException("The subscription could not be " + _  
     "synchronized. Verify that the subscription has " + _  
     "been defined correctly.", ex)  
Finally  
    conn.Disconnect()  
End Try  
```  
  
 Cet exemple synchronise un abonnement par envoi de données (pull) vers une publication de fusion, où l'agent est démarré en mode asynchrone au moyen du travail d'agent.  
  
```csharp  
// Define server, publication, and database names.  
String subscriberName = subscriberInstance;  
String publisherName = publisherInstance;  
String publicationName = "AdvWorksSalesOrdersMerge";  
String publicationDbName = "AdventureWorks";  
String subscriptionDbName = "AdventureWorksReplica";  
  
// Create a connection to the Subscriber.  
ServerConnection conn = new ServerConnection(subscriberName);  
  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    conn.Connect();  
  
    // Define subscription properties.  
    subscription = new MergePullSubscription();  
    subscription.ConnectionContext = conn;  
    subscription.DatabaseName = subscriptionDbName;  
    subscription.PublisherName = publisherName;  
    subscription.PublicationDBName = publicationDbName;  
    subscription.PublicationName = publicationName;  
  
    // If the pull subscription and the job exists, start the agent job.  
    if (subscription.LoadProperties() && subscription.AgentJobId != null)  
    {  
        subscription.SynchronizeWithJob();  
    }  
    else  
    {  
        // Do something here if the subscription does not exist.  
        throw new ApplicationException(String.Format(  
            "A subscription to '{0}' does not exists on {1}",  
            publicationName, subscriberName));  
    }  
}  
catch (Exception ex)  
{  
    // Do appropriate error handling here.  
    throw new ApplicationException("The subscription could not be synchronized.", ex);  
}  
finally  
{  
    conn.Disconnect();  
}  
```  
  
```vb  
' Define server, publication, and database names.  
Dim subscriberName As String = subscriberInstance  
Dim publisherName As String = publisherInstance  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim publicationDbName As String = "AdventureWorks"  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
  
' Create a connection to the Subscriber.  
Dim conn As ServerConnection = New ServerConnection(subscriberName)  
  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    conn.Connect()  
  
    ' Define subscription properties.  
    subscription = New MergePullSubscription()  
    subscription.ConnectionContext = conn  
    subscription.DatabaseName = subscriptionDbName  
    subscription.PublisherName = publisherName  
    subscription.PublicationDBName = publicationDbName  
    subscription.PublicationName = publicationName  
  
    ' If the pull subscription and the job exists, start the agent job.  
    If subscription.LoadProperties() And Not subscription.AgentJobId Is Nothing Then  
        subscription.SynchronizeWithJob()  
    Else  
        ' Do something here if the subscription does not exist.  
        Throw New ApplicationException(String.Format( _  
         "A subscription to '{0}' does not exists on {1}", _  
         publicationName, subscriberName))  
    End If  
Catch ex As Exception  
    ' Do appropriate error handling here.  
    Throw New ApplicationException("The subscription could not be synchronized.", ex)  
Finally  
    conn.Disconnect()  
End Try  
```  
  
 Cet exemple synchronise un abonnement par envoi de données (pull) vers une publication de fusion, où l'agent est démarré en mode synchrone.  
  
```csharp  
// Define the server, publication, and database names.  
string subscriberName = subscriberInstance;  
string publisherName = publisherInstance;  
string publicationName = "AdvWorksSalesOrdersMerge";  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
  
// Create a connection to the Subscriber.  
ServerConnection conn = new ServerConnection(subscriberName);  
  
MergePullSubscription subscription;  
  
try  
{  
    // Connect to the Subscriber.  
    conn.Connect();  
  
    // Define the pull subscription.  
    subscription = new MergePullSubscription();  
    subscription.ConnectionContext = conn;  
    subscription.DatabaseName = subscriptionDbName;  
    subscription.PublisherName = publisherName;  
    subscription.PublicationDBName = publicationDbName;  
    subscription.PublicationName = publicationName;  
  
    // If the pull subscription exists, then start the synchronization.  
    if (subscription.LoadProperties())  
    {  
        // Check that we have enough metadata to start the agent.  
        if (subscription.PublisherSecurity != null || subscription.DistributorSecurity != null)  
        {  
            // Synchronously start the Merge Agent for the subscription.  
            subscription.SynchronizationAgent.Synchronize();  
        }  
        else  
        {  
            throw new ApplicationException("There is insufficent metadata to " +  
                "synchronize the subscription. Recreate the subscription with " +  
                "the agent job or supply the required agent properties at run time.");  
        }  
    }  
    else  
    {  
        // Do something here if the pull subscription does not exist.  
        throw new ApplicationException(String.Format(  
            "A subscription to '{0}' does not exist on {1}",  
            publicationName, subscriberName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement appropriate error handling here.  
    throw new ApplicationException("The subscription could not be " +  
        "synchronized. Verify that the subscription has " +  
        "been defined correctly.", ex);  
}  
finally  
{  
    conn.Disconnect();  
}  
```  
  
```vb  
' Define the server, publication, and database names.  
Dim subscriberName As String = subscriberInstance  
Dim publisherName As String = publisherInstance  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
  
' Create a connection to the Subscriber.  
Dim conn As ServerConnection = New ServerConnection(subscriberName)  
  
Dim subscription As MergePullSubscription  
  
Try  
    ' Connect to the Subscriber.  
    conn.Connect()  
  
    ' Define the pull subscription.  
    subscription = New MergePullSubscription()  
    subscription.ConnectionContext = conn  
    subscription.DatabaseName = subscriptionDbName  
    subscription.PublisherName = publisherName  
    subscription.PublicationDBName = publicationDbName  
    subscription.PublicationName = publicationName  
  
    ' If the pull subscription exists, then start the synchronization.  
    If subscription.LoadProperties() Then  
        ' Check that we have enough metadata to start the agent.  
        If Not subscription.PublisherSecurity Is Nothing Or subscription.DistributorSecurity Is Nothing Then  
  
            ' Output agent messages to the console.   
            subscription.SynchronizationAgent.OutputVerboseLevel = 1  
            subscription.SynchronizationAgent.Output = ""  
  
            ' Synchronously start the Merge Agent for the subscription.  
            subscription.SynchronizationAgent.Synchronize()  
        Else  
            Throw New ApplicationException("There is insufficent metadata to " + _  
             "synchronize the subscription. Recreate the subscription with " + _  
             "the agent job or supply the required agent properties at run time.")  
        End If  
    Else  
        ' Do something here if the pull subscription does not exist.  
        Throw New ApplicationException(String.Format( _  
         "A subscription to '{0}' does not exist on {1}", _  
         publicationName, subscriberName))  
    End If  
Catch ex As Exception  
    ' Implement appropriate error handling here.  
    Throw New ApplicationException("The subscription could not be " + _  
     "synchronized. Verify that the subscription has " + _  
     "been defined correctly.", ex)  
Finally  
    conn.Disconnect()  
End Try  
```  
  
 Cet exemple synchronise un abonnement par extraction de données (pull) vers une publication de fusion au moyen de la synchronisation Web. Dans la mesure où l'abonnement a été créé sans le travail d'agent et les métadonnées d'abonnement associées, l'agent doit être démarré en mode synchrone et des informations d'abonnement supplémentaires sont fournies.  
  
```csharp  
// Define the server, publication, and database names.  
string subscriberName = subscriberInstance;  
string publisherName = publisherInstance;  
string distributorName = distributorInstance;  
string publicationName = "AdvWorksSalesOrdersMerge";  
string subscriptionDbName = "AdventureWorksReplica";  
string publicationDbName = "AdventureWorks";  
string hostname = @"adventure-works\garrett1";  
string webSyncUrl = "https://" + publisherInstance + "/SalesOrders/replisapi.dll";  
  
// Create a connection to the Subscriber.  
ServerConnection conn = new ServerConnection(subscriberName);  
  
MergePullSubscription subscription;  
MergeSynchronizationAgent agent;  
  
try  
{  
    // Connect to the Subscriber.  
    conn.Connect();  
  
    // Define the pull subscription.  
    subscription = new MergePullSubscription();  
    subscription.ConnectionContext = conn;  
    subscription.DatabaseName = subscriptionDbName;  
    subscription.PublisherName = publisherName;  
    subscription.PublicationDBName = publicationDbName;  
    subscription.PublicationName = publicationName;  
  
    // If the pull subscription exists, then start the synchronization.  
    if (subscription.LoadProperties())  
    {  
        // Get the agent for the subscription.  
        agent = subscription.SynchronizationAgent;  
  
        // Check that we have enough metadata to start the agent.  
        if (agent.PublisherSecurityMode == null)  
        {  
            // Set the required properties that could not be returned  
            // from the MSsubscription_properties table.   
            agent.PublisherSecurityMode = SecurityMode.Integrated;  
            agent.DistributorSecurityMode = SecurityMode.Integrated;  
            agent.Distributor = publisherName;  
            agent.HostName = hostname;  
  
            // Set optional Web synchronization properties.  
            agent.UseWebSynchronization = true;  
            agent.InternetUrl = webSyncUrl;  
            agent.InternetSecurityMode = SecurityMode.Standard;  
            agent.InternetLogin = winLogin;  
            agent.InternetPassword = winPassword;  
        }  
        // Enable agent output to the console.  
        agent.OutputVerboseLevel = 1;  
        agent.Output = "";  
  
        // Synchronously start the Merge Agent for the subscription.  
        agent.Synchronize();  
    }  
    else  
    {  
        // Do something here if the pull subscription does not exist.  
        throw new ApplicationException(String.Format(  
            "A subscription to '{0}' does not exist on {1}",  
            publicationName, subscriberName));  
    }  
}  
catch (Exception ex)  
{  
    // Implement appropriate error handling here.  
    throw new ApplicationException("The subscription could not be " +  
        "synchronized. Verify that the subscription has " +  
        "been defined correctly.", ex);  
}  
finally  
{  
    conn.Disconnect();  
}  
```  
  
```vb  
' Define the server, publication, and database names.  
Dim subscriberName As String = subscriberInstance  
Dim publisherName As String = publisherInstance  
Dim publicationName As String = "AdvWorksSalesOrdersMerge"  
Dim subscriptionDbName As String = "AdventureWorksReplica"  
Dim publicationDbName As String = "AdventureWorks"  
Dim hostname As String = "adventure-works\garrett1"  
Dim webSyncUrl As String = "https://" + publisherInstance + "/SalesOrders/replisapi.dll"  
  
' Create a connection to the Subscriber.  
Dim conn As ServerConnection = New ServerConnection(subscriberName)  
  
Dim subscription As MergePullSubscription  
Dim agent As MergeSynchronizationAgent  
  
Try  
    ' Connect to the Subscriber.  
    conn.Connect()  
  
    ' Define the pull subscription.  
    subscription = New MergePullSubscription()  
    subscription.ConnectionContext = conn  
    subscription.DatabaseName = subscriptionDbName  
    subscription.PublisherName = publisherName  
    subscription.PublicationDBName = publicationDbName  
    subscription.PublicationName = publicationName  
  
    ' If the pull subscription exists, then start the synchronization.  
    If subscription.LoadProperties() Then  
        ' Get the agent for the subscription.  
        agent = subscription.SynchronizationAgent  
  
        ' Check that we have enough metadata to start the agent.  
        If agent.PublisherSecurityMode = Nothing Then  
            ' Set the required properties that could not be returned  
            ' from the MSsubscription_properties table.   
            agent.PublisherSecurityMode = SecurityMode.Integrated  
            agent.Distributor = publisherInstance  
            agent.DistributorSecurityMode = SecurityMode.Integrated  
            agent.HostName = hostname  
  
            ' Set optional Web synchronization properties.  
            agent.UseWebSynchronization = True  
            agent.InternetUrl = webSyncUrl  
            agent.InternetSecurityMode = SecurityMode.Standard  
            agent.InternetLogin = winLogin  
            agent.InternetPassword = winPassword  
        End If  
  
        ' Enable agent logging to the console.  
        agent.OutputVerboseLevel = 1  
        agent.Output = ""  
  
        ' Synchronously start the Merge Agent for the subscription.  
        agent.Synchronize()  
    Else  
        ' Do something here if the pull subscription does not exist.  
        Throw New ApplicationException(String.Format( _  
         "A subscription to '{0}' does not exist on {1}", _  
         publicationName, subscriberName))  
    End If  
Catch ex As Exception  
    ' Implement appropriate error handling here.  
    Throw New ApplicationException("The subscription could not be " + _  
     "synchronized. Verify that the subscription has " + _  
     "been defined correctly.", ex)  
Finally  
    conn.Disconnect()  
End Try  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Synchroniser les données](../../relational-databases/replication/synchronize-data.md)   
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Bonnes pratiques en matière de sécurité de la réplication](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
