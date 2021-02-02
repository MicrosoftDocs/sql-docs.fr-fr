---
title: Configurer un gestionnaire de logique métier pour un article de fusion
description: Utilisez la programmation de réplication ou Replication Management Objects afin de configurer un gestionnaire de logique métier pour la synchronisation de la réplication de fusion.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], business logic handlers
- merge replication business logic handlers [SQL Server replication]
- conflict resolution [SQL Server replication], merge replication
- business logic handlers [SQL Server replication]
- BusinessLogicModule class
ms.assetid: ed477595-6d46-4fa2-b0d3-a5358903ec05
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 45ed99072fb5abad47b9f63a22f879c0f4e438eb
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076827"
---
# <a name="implement-a-business-logic-handler-for-a-merge-article"></a>Implémenter un gestionnaire de logique métier pour un article de fusion
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit comment implémenter un gestionnaire de logique métier pour un article de fusion dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de la programmation de réplication ou d'objets RMO (Replication Management Objects).  
  
 L'espace de noms <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport> implémente une interface qui vous permet d'écrire une logique métier complexe afin de gérer les événements qui se produisent au cours du processus de synchronisation de la réplication de fusion. Les méthodes dans le gestionnaire de logique métier peuvent être appelées par le processus de réplication pour chaque ligne modifiée qui est répliquée pendant la synchronisation.  
  
 La procédure globale permettant d'implémenter un gestionnaire de logique métier est la suivante :  
  
1.  Créez l'assembly du gestionnaire de logique métier.  
  
2.  Inscrivez l'assembly sur le serveur de distribution.  
  
3.  Déployez l'assembly sur le serveur où l'Agent de fusion s'exécute. Pour un abonnement par extraction, l'agent s'exécute sur l'Abonné, alors que pour un abonnement par émission de données, il s'exécute sur le serveur de distribution. Lorsque vous utilisez la synchronisation Web, l'agent s'exécute sur le serveur Web.  
  
4.  Créez un article qui utilise le gestionnaire de logique métier ou modifiez un article existant pour utiliser le gestionnaire de logique métier.  
  
 Le gestionnaire de logique métier que vous spécifiez est exécuté pour chaque ligne faisant l'objet d'une synchronisation. Une logique complexe et des appels à d'autres applications ou services réseau peuvent avoir une incidence sur les performances. Pour plus d’informations sur les gestionnaires de logique métier, consultez [Exécuter une logique pendant la synchronisation de fusion](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md).  
  
 **Dans cette rubrique**  
  
-   **Pour implémenter un gestionnaire de logique métier pour un article de fusion à l'aide de :**  
  
     [Programmation de la réplication](#ReplProg)  
  
     [Objets RMO (Replication Management Objects)](#RMOProcedure)  
  
##  <a name="using-replication-programming"></a><a name="ReplProg"></a> Utilisation de la programmation de la réplication  
  
#### <a name="to-create-and-deploy-a-business-logic-handler"></a>Pour créer et déployer un gestionnaire de logique métier  
  
1.  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio, créez un projet pour l'assembly .NET contenant le code qui implémente le gestionnaire de logique métier.  
  
2.  Ajoutez des références au projet pour les espaces de noms ci-dessous.  
  
    |Référence d'assembly|Location|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM (installation par défaut)|  
    |<xref:System.Data>|GAC (composant du .NET Framework)|  
    |<xref:System.Data.Common>|GAC (composant du .NET Framework)|  
  
3.  Ajoutez une classe qui remplace la classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
4.  Implémentez la propriété <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> pour indiquer les types de modifications qui sont gérés.  
  
5.  Remplacez une ou plusieurs des méthodes suivantes de la classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> :  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> – méthode appelée lorsqu'une modification de données est validée pendant la synchronisation ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> – méthode appelée lorsqu'une erreur se produit quand une instruction DELETE est téléchargée vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> – méthode appelée lorsque des instructions DELETE sont téléchargées vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> – méthode appelée lorsqu'une erreur se produit quand une instruction INSERT est téléchargée vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> – méthode appelée lorsque des instructions INSERT sont téléchargées vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> – méthode appelée lorsque des instructions UPDATE en conflit se produisent sur le serveur de publication et l'Abonné ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> – méthode appelée lorsque des instructions UPDATE entrent en conflit avec des instructions DELETE sur le serveur de publication et l'Abonné ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> – méthode appelée lorsqu'une erreur se produit quand une instruction UPDATE est téléchargée vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> – méthode appelée lorsque des instructions UPDATE sont téléchargées vers ou à partir du serveur.  
  
6.  Générez le projet pour créer l'assembly du gestionnaire de logique métier.  
  
7.  Déployez l'assembly dans le répertoire contenant le fichier exécutable de l'Agent de fusion (replmerg.exe), qui est pour une installation par défaut [!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM, ou installez-le dans le .NET Global Assembly Cache (GAC). Vous devez installer l'assembly dans le GAC seulement si des applications autres que l'Agent de fusion requièrent l'accès à l'assembly. L'assembly peut être installé dans le GAC à l'aide de l'outil Global Assembly Cache (**Gacutil.exe)** fourni dans le Kit de développement .NET Framework SDK.  
  
    > [!NOTE]  
    >  Un gestionnaire de logique métier doit être déployé sur chaque serveur sur lequel l'Agent de fusion s'exécute, ce qui inclut le serveur IIS qui héberge replisapi.dll lors de l'utilisation de la synchronisation Web.  
  
#### <a name="to-register-a-business-logic-handler"></a>Pour inscrire un gestionnaire de logique métier  
  
1.  Sur le serveur de publication, exécutez [sp_enumcustomresolvers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-enumcustomresolvers-transact-sql.md) pour vérifier que l’assembly n’a pas été inscrit en tant que gestionnaire de logique métier.  
  
2.  Sur le serveur de distribution, exécutez [sp_registercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md), en spécifiant un nom convivial pour le gestionnaire de logique métier pour **\@article_resolver**, la valeur **true** pour **\@is_dotnet_assembly**, le nom de l’assembly pour **\@dotnet_assembly_name** et le nom complet de la classe qui remplace <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> pour **\@dotnet_class_name**.  
  
    > [!NOTE]  
    >  Si l’assembly n’est pas déployé dans le même répertoire que l’exécutable de l’Agent de fusion, dans le même répertoire que l’application qui démarre de façon synchrone l’Agent de fusion ni dans le GAC (Global Assembly Cache), vous devez spécifier le chemin complet avec le nom de l’assembly pour **\@dotnet_assembly_name**. Lorsque vous utilisez la synchronisation Web, vous devez spécifier l'emplacement de l'assembly sur le serveur Web.  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>Pour utiliser un gestionnaire de logique métier avec un nouvel article de table  
  
1.  Exécutez [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) pour définir un article, en spécifiant le nom convivial du gestionnaire de logique métier pour **\@article_resolver**. Pour plus d’informations, consultez [définir un Article](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>Pour utiliser un gestionnaire de logique métier avec un article de table existant  
  
1.  Exécutez [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) en spécifiant **\@publication**, **\@article**, en affectant la valeur **article_resolver** à **\@property** et en affectant le nom convivial du gestionnaire de logique métier à **\@value**.  
  
###  <a name="examples-replication-programming"></a><a name="TsqlExample"></a> Exemples (programmation de la réplication)  
 Cet exemple illustre un gestionnaire de logique métier qui crée un journal d'audit.  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 L'exemple ci-dessous inscrit un assembly du gestionnaire de logique métier sur le serveur de distribution et modifie un article de fusion existant afin d'utiliser cette logique métier personnalisée.  
  
 [!code-sql[HowTo#sp_RegisterBLH_10](../../relational-databases/replication/codesnippet/tsql/implement-a-business-log_3.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Utilisation d'objets RMO (Replication Management Objects)  
  
#### <a name="to-create-a-business-logic-handler"></a>Pour créer un gestionnaire de logique métier  
  
1.  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio, créez un projet pour l'assembly .NET contenant le code qui implémente le gestionnaire de logique métier.  
  
2.  Ajoutez des références au projet pour les espaces de noms ci-dessous.  
  
    |Référence d'assembly|Location|  
    |------------------------|--------------|  
    |<xref:Microsoft.SqlServer.Replication.BusinessLogicSupport>|[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]COM (installation par défaut)|  
    |<xref:System.Data>|GAC (composant du .NET Framework)|  
    |<xref:System.Data.Common>|GAC (composant du .NET Framework)|  
  
3.  Ajoutez une classe qui remplace la classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> .  
  
4.  Implémentez la propriété <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.HandledChangeStates%2A> pour indiquer les types de modifications qui sont gérés.  
  
5.  Remplacez une ou plusieurs des méthodes suivantes de la classe <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> :  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.CommitHandler%2A> – méthode appelée lorsqu'une modification de données est validée pendant la synchronisation ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteErrorHandler%2A> – méthode appelée si une erreur se produit pendant qu'une instruction DELETE est téléchargée vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.DeleteHandler%2A> – méthode appelée lorsque des instructions DELETE sont téléchargées vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertErrorHandler%2A> – méthode appelée si une erreur se produit pendant qu'une instruction INSERT est téléchargée vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.InsertHandler%2A> – méthode appelée lorsque des instructions INSERT sont téléchargées vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateConflictsHandler%2A> – méthode appelée lorsque des instructions UPDATE en conflit se produisent sur le serveur de publication et l'Abonné ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateDeleteConflictHandler%2A> – méthode appelée lorsque des instructions UPDATE entrent en conflit avec des instructions DELETE sur le serveur de publication et l'Abonné ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateErrorHandler%2A> – méthode appelée si une erreur se produit pendant qu'une instruction UPDATE est téléchargée vers ou à partir du serveur ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule.UpdateHandler%2A> – méthode appelée lorsque des instructions UPDATE sont téléchargées vers ou à partir du serveur.  
  
    > [!NOTE]  
    >  Tous les conflits d'un article qui ne sont pas gérés explicitement par votre logique métier personnalisée sont gérés par le programme de résolution par défaut pour l'article.  
  
6.  Générez le projet pour créer l'assembly du gestionnaire de logique métier.  
  
#### <a name="to-register-a-business-logic-handler"></a>Pour inscrire un gestionnaire de logique métier  
  
1.  Créez une connexion au serveur de distribution en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Passez l'objet <xref:Microsoft.SqlServer.Management.Common.ServerConnection> créé à l'étape 1.  
  
3.  Appelez <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumBusinessLogicHandlers%2A> et vérifiez l'objet <xref:System.Collections.ArrayList> retourné pour vous assurer que l'assembly n'a pas déjà été inscrit en tant que gestionnaire de logique métier.  
  
4.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler>. Spécifiez les propriétés suivantes :  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetAssemblyName%2A> – nom de l'assembly .NET. Si l'assembly n'est pas déployé dans le même répertoire que l'exécutable de l'Agent de fusion, dans le même répertoire que l'application qui démarre de façon synchrone l'Agent de fusion ou dans le GAC, vous devez inclure le chemin d'accès complet avec le nom de l'assembly. Vous devez inclure le chemin d'accès complet avec le nom de l'assembly lorsque vous utilisez un gestionnaire de logique métier avec la synchronisation Web ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.DotNetClassName%2A> – nom complet de la classe qui remplace <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> et implémente le gestionnaire de logique métier ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> – nom convivial que vous utilisez lorsque vous accédez au gestionnaire de logique métier ;  
  
    -   <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.IsDotNetAssembly%2A> – valeur **true**.  
  
#### <a name="to-deploy-a-business-logic-handler"></a>Pour déployer un gestionnaire de logique métier  
  
1.  Déployez l'assembly sur le serveur où l'Agent de fusion s'exécute, dans l'emplacement de fichier qui a été spécifié quand le gestionnaire de logique métier a été inscrit sur le serveur de distribution. Pour un abonnement par extraction, l'agent s'exécute sur l'Abonné, alors que pour un abonnement par émission de données, il s'exécute sur le serveur de distribution. Lorsque vous utilisez la synchronisation Web, l'agent s'exécute sur le serveur Web. Si le chemin d'accès complet n'a pas été inclus avec le nom de l'assembly lorsque le gestionnaire de logique métier a été inscrit, déployez l'assembly dans le même répertoire que l'exécutable de l'Agent de fusion, dans le même répertoire que l'application qui démarre de façon synchrone l'Agent de fusion. Vous pouvez installer l'assembly dans le GAC si plusieurs applications utilisent le même assembly.  
  
#### <a name="to-use-a-business-logic-handler-with-a-new-table-article"></a>Pour utiliser un gestionnaire de logique métier avec un nouvel article de table  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeArticle>. Définissez les propriétés suivantes :  
  
    -   Le nom de l'article pour <xref:Microsoft.SqlServer.Replication.Article.Name%2A>.  
  
    -   Le nom de la publication pour <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>.  
  
    -   le nom de la base de données de publication pour <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A>;  
  
    -   le nom convivial du gestionnaire de logique métier (<xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A>) pour <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>.  
  
3.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.Article.Create%2A> . Pour plus d’informations, consultez [définir un Article](../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-use-a-business-logic-handler-with-an-existing-table-article"></a>Pour utiliser un gestionnaire de logique métier avec un article de table existant  
  
1.  Créez une connexion au serveur de publication en utilisant la classe <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Créez une instance de la classe <xref:Microsoft.SqlServer.Replication.MergeArticle>.  
  
3.  Définissez les propriétés <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>et <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Définissez la connexion créée à l'étape 1 pour la propriété <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Appelez la méthode <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> pour obtenir les propriétés de l'objet. Si cette méthode retourne **false**, soit les propriétés de l'article ont été définies de manière incorrecte à l'étape 3, soit l'article n'existe pas. Pour plus d'informations, voir [View and Modify Article Properties](../../relational-databases/replication/publish/view-and-modify-article-properties.md).  
  
6.  Définissez le nom convivial du gestionnaire de logique métier pour <xref:Microsoft.SqlServer.Replication.MergeArticle.ArticleResolver%2A>. Il s'agit de la valeur de la propriété <xref:Microsoft.SqlServer.Replication.BusinessLogicHandler.FriendlyName%2A> spécifiée lors de l'inscription du gestionnaire de logique métier.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Exemples (RMO)  
 Cet exemple illustre un gestionnaire de logique métier qui enregistre les informations sur les insertions, les mises à jour et les suppressions sur l'Abonné.  
  
 [!code-cs[HowTo#rmo_BusinessLogicCode](../../relational-databases/replication/codesnippet/csharp/rmohowto/businesslogic.cs#rmo_businesslogiccode)]  
  
 [!code-vb[HowTo#rmo_vb_BusinessLogicCode](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/businesslogic.vb#rmo_vb_businesslogiccode)]  
  
 Cet exemple illustre l'inscription d'un gestionnaire de logique métier sur le serveur de distribution.  
  
 [!code-cs[HowTo#rmo_RegisterBLH_10](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_registerblh_10)]  
  
 [!code-vb[HowTo#rmo_vb_RegisterBLH_10](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_registerblh_10)]  
  
 Cet exemple illustre la modification d'un article existant pour utiliser le gestionnaire de logique métier.  
  
 [!code-cs[HowTo#rmo_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergearticle_blh)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergeArticle_BLH](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergearticle_blh)]  
  
## <a name="see-also"></a>Voir aussi  
 [Implémenter un outil personnalisé de résolution des conflits pour un article de fusion](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [Déboguer un gestionnaire de logique métier &#40;programmation de la réplication&#41;](../../relational-databases/replication/debug-a-business-logic-handler-replication-programming.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Concepts liés à Replication Management Objects](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)  
  
  
