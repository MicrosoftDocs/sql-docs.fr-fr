---
description: Travaux de l'Agent SQL Server pour les packages
title: Travaux de SQL Server Agent pour les packages | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- jobs [Integration Services]
- automatic package execution
- scheduling packages [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: ecf7a5f9-b8a7-47f1-9ac0-bac07cb89e31
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 353acdd28f3611bcc387bd4122ca377a2d8d0ca5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346860"
---
# <a name="sql-server-agent-jobs-for-packages"></a>Travaux de l'Agent SQL Server pour les packages

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Automatisez et planifiez l’exécution des packages [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Vous pouvez planifier les packages qui sont déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et sont stockés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] et le système de fichiers.  
 
> [!NOTE]
> Cet article décrit comment planifier des packages SSIS de manière générale et comment les planifier au niveau local. Vous pouvez également exécuter et planifier des packages SSIS sur les plateformes suivantes :
> - **Cloud Microsoft Azure**. Pour plus d’informations, consultez [Effectuer un « lift-and-shift » des charges de travail SQL Server Integration Services vers le cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) et [Planifier l’exécution d’un package SSIS dans Azure](../lift-shift/ssis-azure-schedule-packages.md).
> - **Linux**. Pour plus d’informations, consultez [Extraire, transformer et charger des données sur Linux avec SSIS](../../linux/sql-server-linux-migrate-ssis.md) et [Planifier l’exécution du package SQL Server Integration Services sur Linux avec cron](../../linux/sql-server-linux-schedule-ssis-packages.md).

## <a name="sections-in-this-topic"></a>Rubriques de cette section  
 Cette rubrique contient les sections suivantes :  
  
-   [Planification de travaux dans l'Agent SQL Server](#jobs)  
  
-   [Planification de packages Integration Services](#packages)  
  
-   [Dépannage de packages planifiés](#trouble)  
  
##  <a name="scheduling-jobs-in-sql-server-agent"></a><a name="jobs"></a> Scheduling Jobs in SQL Server Agent  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est le service installé par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui vous permet d’automatiser et de planifier des tâches en exécutant les travaux de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit s’exécuter avant que les travaux puissent s’exécuter automatiquement. Pour plus d’informations, consultez [Configure SQL Server Agent](../../ssms/agent/configure-sql-server-agent.md).  
  
 Le nœud **SQL Server Agent** s’affiche dans l’Explorateur d’objets dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] quand vous vous connectez à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
 Pour automatiser une tâche récurrente, créez un travail à l’aide de la boîte de dialogue **Nouveau travail** . Pour plus d’informations, consultez [Implémenter des travaux](../../ssms/agent/implement-jobs.md).  
  
 Après avoir créé le travail, vous devez ajouter au moins une étape. Un travail peut inclure plusieurs étapes, et chaque étape peut effectuer une tâche différente. Pour plus d’informations, consultez [Gérer les étapes de travail](../../ssms/agent/manage-job-steps.md).  
  
 Après avoir créé le travail et les étapes du travail, vous pouvez créer une planification d'exécution du travail. Cependant, vous pouvez également créer un travail non planifié que vous exécutez manuellement. Pour plus d’informations, consultez [Créer des planifications et les attacher à des travaux](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
 Vous pouvez améliorer le travail en définissant des options de notification, par exemple en spécifiant l'envoi de messages électroniques à un opérateur à la fin du travail ou en ajoutant des alertes. Pour plus d’informations, consultez [Alertes](../../ssms/agent/alerts.md).  
  
##  <a name="scheduling-integration-services-packages"></a><a name="packages"></a> Scheduling Integration Services Packages  
 Quand vous créez un travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour planifier des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous devez ajouter au moins une étape et affecter à son type la valeur **Package SQL Server Integration Services**. Un travail peut inclure plusieurs étapes, et chaque étape peut exécuter un package différent.  
  
 L’exécution d’un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à partir d’une étape du travail revient à exécuter un package à l’aide des utilitaires **dtexec** (dtexec.exe) et **DTExecUI** (dtexecui.exe). Au lieu de définir les options d’exécution pour un package à l’aide des options de ligne de commande ou de la boîte de dialogue **Utilitaire d’exécution de package** , vous définissez les options d’exécution dans la boîte de dialogue **Nouvelle étape de travail** . Pour plus d’informations sur les options de ligne de commande pour l’exécution d’un package, consultez [Utilitaire dtexec](../../integration-services/packages/dtexec-utility.md).  
  
 Pour plus d’informations, consultez [Planifier un package à l’aide de SQL Server Agent](#schedule).  
  
 Pour obtenir une vidéo qui montre comment utiliser l’agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exécuter un package, consultez la page d’accueil vidéo [Guide pratique pour automatiser l’exécution du package à l’aide de SQL Server Agent (vidéo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100)) dans MSDN Library.  
  
##  <a name="troubleshooting"></a><a name="trouble"></a> Dépannage  
 Une étape de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent peut ne pas démarrer un package même si le package est exécuté correctement dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et à partir de la ligne de commande. Ce problème est connu et plusieurs solutions sont recommandées. Pour plus d'informations, consultez les ressources ci-dessous.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Article de la Base de connaissances intitulé [Un package SSIS n’est pas exécuté quand vous appelez le package SSIS à partir d’une étape de travail de SQL Server Agent](https://support.microsoft.com/kb/918760)  
  
-   Vidéo, [Résolution des problèmes : exécution du package à l’aide de SQL Server Agent (vidéo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100)) dans MSDN Library.  
  
 Lorsqu'une étape de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent démarre un package, l'exécution du package peut échouer ou le package peut s'exécuter correctement mais avec des résultats inattendus. Vous pouvez utiliser les outils suivants pour résoudre ces problèmes.  
  
-   Pour les packages enregistrés dans la base de données MSDB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , dans le magasin de packages d’ [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou dans un dossier sur votre ordinateur local, vous pouvez utiliser la **Visionneuse du fichier journal** ainsi que tous les journaux et les fichiers de vidage du débogage générés pendant l’exécution du package.  
  
     **Pour utiliser la Visionneuse du fichier journal, procédez comme suit.**  
  
    1.  Cliquez avec le bouton droit sur le travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans l’Explorateur d’objets, puis cliquez sur **Afficher l’historique**.  
  
    2.  Dans la zone **Résumé du fichier journal** , recherchez l’exécution du travail qui présente le message **Travail échoué** dans la colonne **Message** .  
  
    3.  Développez le nœud de travail, puis cliquez sur l’étape de travail pour afficher les détails du message dans la zone sous le **Résumé du fichier journal** .  
  
-   Pour les packages enregistrés dans la base de données SSISDB, vous pouvez également utiliser la **Visionneuse du fichier journal** ainsi que tous les journaux et les fichiers de vidage du débogage générés pendant l’exécution du package. En outre, vous pouvez utiliser les rapports pour le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
     **Pour rechercher des informations dans les rapports sur l'exécution du package associée à une exécution de travail, procédez comme suit.**  
  
    1.  Suivez les étapes ci-dessus pour afficher les détails du message de l'étape de travail.  
  
    2.  Recherchez l'ID d'exécution répertorié dans le message.  
  
    3.  Développez le nœud du catalogue Integration Services dans l'Explorateur d'objets.  
  
    4.  Cliquez avec le bouton droit sur SSISDB, pointez sur Rapports, puis sur Rapports standard, et cliquez sur Toutes les exécutions.  
  
    5.  Dans le rapport **Toutes les exécutions** , recherchez l’ID d’exécution dans la colonne **ID** . Cliquez sur **Vue d’ensemble**, **Tous les messages**, ou **Performances de l’exécution** pour afficher des informations sur cette exécution du package.  
  
    Pour plus d’informations sur les rapports Vue d’ensemble, Tous les messages et Performances de l’exécution, consultez [Rapports du serveur Integration Services](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).  

## <a name="schedule-a-package-by-using-sql-server-agent"></a><a name="schedule"></a> Planifier un package à l’aide de SQL Server Agent
  La procédure suivante fournit les étapes pour automatiser l'exécution d'un package en utilisant une étape de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent pour exécuter le package.  
  
### <a name="to-automate-package-execution-by-using-sql-server-agent"></a>Pour automatiser l'exécution des packages à l'aide de l'Agent SQL Server  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle vous voulez créer un travail, ou l'instance contenant le travail auquel vous voulez ajouter une étape.  
  
2.  Développez le nœud de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans l'Explorateur d'objets et effectuez l'une des tâches suivantes :  
  
    -   Pour créer un travail, cliquez avec le bouton droit sur **Travaux** , puis cliquez sur **Nouveau travail**.  
  
    -   Pour ajouter une étape à un travail existant, développez **Travaux**, cliquez avec le bouton droit sur le travail, puis cliquez sur **Propriétés**.  
  
3.  Sur la page **Général** , si vous créez un nouveau travail, fournissez un nom de travail, sélectionnez un propriétaire et une catégorie de travail, et fournissez, si vous le souhaitez, une description du travail.  
  
4.  Pour rendre le travail disponible pour la planification, sélectionnez **Activé**.  
  
5.  Pour créer une étape de travail d'un package que vous souhaitez à planifier, cliquez sur **Étapes**, puis cliquez sur **Nouveau**.  
  
6.  Pour le type d'étape de travail, sélectionnez **Package Integration Services** .  
  
7.  Dans la liste **Exécuter en tant que** , sélectionnez **Compte de service SQL Server Agent** ou sélectionnez un compte proxy ayant les informations d'identification qui seront utilisées par le travail. Pour plus d'informations sur la création d'un compte proxy, consultez [Create a SQL Server Agent Proxy](../../ssms/agent/create-a-sql-server-agent-proxy.md).  

    En utilisant un compte proxy au lieu du **Compte de service SQL Server Agent** , vous pouvez résoudre les problèmes courants qui peuvent se produire lors de l'exécution d'un package à l'aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Pour plus d’informations sur ces problèmes, consultez l’article de la Base de connaissances [!INCLUDE[msCoName](../../includes/msconame-md.md)] intitulé [Un package SSIS n’est pas exécuté lorsque vous appelez le package SSIS à partir d’une étape de travail de SQL Server Agent](https://support.microsoft.com/kb/918760). 
     
    - Lors de l’exécution d’un travail avec un proxy, les éléments suivants de sécurité doivent être en place pour que le travail s’exécute correctement.

        Informations d’identification de connexion utilisées par le proxy, le compte qui exécute SQL Server Agent et le compte qui exécute le Service SQL Server nécessitent les autorisations suivantes :

        - Attribut de stratégie de sécurité locale : Remplacer un jeton de niveau processus
        - Contrôle total sur %SYSTEMROOT%\Temp 

        En cas d’échec de mise en place des éléments de sécurité, la tâche échoue et un message d’erreur semblable au suivant s’affiche : Le travail a échoué. Le client ne dispose pas d'un privilège qui est obligatoire.

        > **REMARQUE :** Si le mot de passe est différent de celui des informations d'identification que le compte proxy utilise, vous devez mettre à jour le mot de passe des informations d'identification. Autrement, l'étape de travail échouera.  

        Pour plus d’informations sur la configuration du compte de service de l’Agent SQL Server, consultez [Définir le compte de démarrage du service pour l’Agent SQL Server &#40;Gestionnaire de configuration SQL Server&#41;](../../ssms/agent/set-service-startup-account-sql-server-agent-sql-server-configuration-manager.md).  

8.  Dans la zone de liste **Source du package** , cliquez sur la source du package et définissez les options de l'étape de travail.  
  
    **Le tableau suivant décrit les sources de package possibles.**  
  
    |Source du package|Description|  
    |--------------------|-----------------|  
    |**Catalogue SSIS**|Packages stockés dans la base de données SSISDB. Les packages sont contenus dans les projets [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] déployés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
    |**SQL Server**|Packages stockés dans la base de données MSDB. Vous utilisez le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour gérer les packages.|  
    |**Magasin de packages SSIS**|Packages stockés dans le dossier par défaut sur votre ordinateur. Le dossier par défaut est *\<drive>*  : \Program files\Microsoft SQL Server\110\DTS\Packages. Vous utilisez le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour gérer les packages.<br /><br /> Remarque : Vous pouvez spécifier un dossier différent ou spécifier des dossiers supplémentaires dans le système de fichiers géré par le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], en modifiant le fichier de configuration pour [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Pour plus d’informations, consultez [Service Integration Services &#40;Service SSIS&#41;](../../integration-services/service/integration-services-service-ssis-service.md).|  
    |**Système de fichiers**|Packages stockés dans un dossier sur votre ordinateur local.|  
    |||
  
    **Les tableaux suivants décrivent les options de configuration disponibles pour l'étape de travail en fonction de la source du package que vous sélectionnez.**  
  
    > **IMPORTANT :** Si le package est protégé par mot de passe, lorsque vous cliquez sur l'un des onglets de la page **Général** de la boîte de dialogue **Nouvelle étape de travail** , à l'exception de l'onglet **Package** , vous devez entrer le mot de passe dans la boîte de dialogue **Mot de passe du package** qui s'affiche. Sinon, le travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent n'exécute pas le package.  
  
    **Source du package** : Catalogue SSIS  
  
    |Onglet|Options|  
    |---------|-------------|  
    |**Package**|**Serveur**<br /><br /> Tapez ou sélectionnez le nom de l'instance de serveur de base de données qui héberge le catalogue SSIS.<br /><br /> Lorsque **Catalogue SSIS** est la source du package, vous pouvez vous connecter au serveur à l'aide d'un compte d'utilisateur Microsoft Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'est pas disponible.|  
    ||**Package**<br /><br /> Cliquez sur le bouton de sélection (...) et sélectionnez un package.<br /><br /> Vous sélectionnez un package dans un dossier sous le nœud **Catalogues Integration Services** dans l' **Explorateur d'objets**.|  
    |**Paramètres**<br /><br /> Situés sur l'onglet **Configuration** .|L' **Assistant Conversion de projet Integration Services** vous permet de remplacer les configurations du package avec des paramètres.<br /><br /> L'onglet **Paramètres** affiche les paramètres que vous avez ajoutés lors de la conception du package, par exemple à l'aide de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. L'onglet affiche également les paramètres qui ont été ajoutés au package lors de la conversion du projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] du modèle de déploiement de package au modèle de déploiement de projet. Entrez les nouvelles valeurs des paramètres contenus dans le package. Vous pouvez entrer une valeur littérale ou utiliser la valeur contenue dans une variable d'environnement serveur que vous avez déjà mappée au paramètre.<br /><br /> Pour entrer la valeur littérale, cliquez sur le bouton de sélection en regard d'un paramètre. La boîte de dialogue **Modifier la valeur littérale pour l'exécution** s'affiche.<br /><br /> Pour utiliser une variable d'environnement, cliquez sur **Environnement** puis sélectionner l'environnement qui contient la variable que vous souhaitez utiliser.<br /><br /> **\*\* Important \*\*** Si vous avez mappé plusieurs paramètres et/ou propriétés du gestionnaire de connexions à des variables contenues dans plusieurs environnements, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent affiche un message d’erreur. Pour une exécution données, un package peut s'exécuter uniquement avec les valeurs contenues dans un seul environnement.<br /><br /> Pour plus d’informations sur la création d’un environnement serveur et le mappage d’une variable à un paramètre, consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).|  
    |**Gestionnaires de connexions**<br /><br /> Situés sur l'onglet **Configuration** .|Modifiez les valeurs des propriétés du gestionnaire de connexions. Par exemple, vous pouvez modifier le nom du serveur. Les paramètres sont automatiquement générés sur le serveur SSIS pour les propriétés du gestionnaire de connexions. Pour modifier une valeur de propriété, vous pouvez entrer une valeur littérale ou utiliser la valeur contenue dans une variable d'environnement serveur que vous avez déjà mappée à la propriété du gestionnaire de connexions.<br /><br /> Pour entrer la valeur littérale, cliquez sur le bouton de sélection en regard d'un paramètre. La boîte de dialogue **Modifier la valeur littérale pour l'exécution** s'affiche.<br /><br /> Pour utiliser une variable d'environnement, cliquez sur **Environnement** puis sélectionner l'environnement qui contient la variable que vous souhaitez utiliser.<br /><br /> **\*\* Important \*\*** Si vous avez mappé plusieurs paramètres et/ou propriétés du gestionnaire de connexions à des variables contenues dans plusieurs environnements, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent affiche un message d’erreur. Pour une exécution données, un package peut s'exécuter uniquement avec les valeurs contenues dans un seul environnement.<br /><br /> Pour plus d’informations sur la création d’un environnement serveur et le mappage d’une variable à une propriété du gestionnaire de connexions, consultez [Déployer des projets et des packages Integration Services (SSIS)](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).|  
    |**Avancée**<br /><br /> Situés sur l'onglet **Configuration** .|Configurez les paramètres supplémentaires suivants pour l’exécution du package :|  
    ||**Substitutions de propriété**:<br /><br /> Cliquez sur **Ajouter** pour entrer une nouvelle valeur de propriété de package, spécifiez le chemin d'accès de la propriété, et indiquez si la valeur de la propriété est sensible. Le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] chiffre les données sensibles. Pour modifier ou supprimer des paramètres de propriété, cliquez sur une ligne dans la zone de priorités **Propriété** puis cliquez sur **Modifier** ou sur **Supprimer**. Vous pouvez trouver le chemin de la propriété en procédant de l’une des façons suivantes :<br /><br /> \- Copiez le chemin de la propriété à partir du fichier de configuration XML (\*.dtsconfig). Le chemin d'accès est répertorié dans la section Configuration du fichier, comme valeur de l'attribut Path. Voici un exemple de chemin d’accès de la propriété MaximumErrorCount : \Package.Properties[MaximumErrorCount]<br /><br /> \- Exécutez **l’Assistant Configuration de package** et copiez les chemins des propriétés à partir de la dernière page **Fin de l’Assistant** . Vous pouvez ensuite quitter l'Assistant.<br /><br /> <br /><br /> Remarque : L'option **Substitutions de propriété** concerne des packages contenant des configurations que vous avez mises à niveau depuis une version précédente de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Les packages que vous créez à l'aide de [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] et que vous déployez sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilisent des paramètres à la place des configurations.|  
    ||**Niveau de journalisation**<br /><br /> Sélectionnez l'un des niveaux de journalisation suivants pour l'exécution du package. Notez que le niveau de journalisation **Performances** ou **Commentaires** sélectionné peut affecter les performances de l’exécution du package.<br /><br /> **Aucun** :<br />                          La journalisation est désactivée. Seul l'état d'exécution du package est enregistré.<br /><br /> **De base** :<br />                          Tous les événements sont enregistrés, sauf les événements personnalisés et de diagnostic. Il s'agit de la valeur par défaut du niveau de journalisation.<br /><br /> **Performances** :<br />                          Seules les statistiques de performances, et les événements OnError et OnWarning, sont enregistrés.<br /><br /> **Commentaires**:<br />                          Tous les événements sont enregistrés, y compris les événements personnalisés et de diagnostic.<br /><br /> Le niveau de journalisation que vous sélectionnez détermine quelles informations sont affichées dans les vues SSISDB et dans les rapports pour le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez [Journalisation d’Integration Services (SSIS)](../../integration-services/performance/integration-services-ssis-logging.md).|  
    ||**Vider en cas d'erreurs**<br /><br /> Déterminez si des fichiers de vidage du débogage sont générés lorsqu'une erreur se produit pendant l'exécution du package. Les fichiers contiennent des informations sur l'exécution du package qui peuvent vous aider à résoudre les problèmes d'exécution. Quand vous sélectionnez cette option et qu’une erreur se produit pendant l’exécution, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un fichier .mdmp (fichier binaire) et un fichier .tmp (fichier texte). Par défaut, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] stocke ces fichiers dans le dossier *\<drive> :* \Program Files\Microsoft SQL Server\110\Shared\ErrorDumps.|  
    ||**Runtime 32 bits**<br /><br /> Indiquez si le package est exécuté à l’aide de la version 32 bits de l’utilitaire dtexec sur un ordinateur 64 bits contenant une version 64 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.<br /><br /> Il peut être nécessaire d'exécuter le package à l'aide de la version 32 bits de dtexec si, par exemple, le package utilise un fournisseur OLE DB natif qui n'est pas disponible en version 64 bits. Pour plus d'informations, consultez [Considérations 64 bits pour Integration Services](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx).<br /><br /> Par défaut, lorsque vous sélectionnez le type d'étape de travail **Package SQL Server Integration Services** , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent exécute le package à l'aide de la version de l'utilitaire dtexec qui est appelée automatiquement par le système. Le système appelle la version 32 bits ou 64 bits de l'utilitaire selon le processeur de l'ordinateur, et de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent qui s'exécute sur l'ordinateur.|  
  
     **Source du package** :  SQL Server, Magasin de packages SSIS ou Système de fichiers  
  
     Plusieurs options que vous pouvez définir pour les packages stockées dans SQL Server, le magasin de packages SSIS ou le système de fichiers correspondent aux options de ligne de commande de l’utilitaire d’invite de commandes **dtexec** . Pour plus d’informations sur l’utilitaire et les options de la ligne de commande, consultez [Utilitaire dtexec](../../integration-services/packages/dtexec-utility.md).  
  
    |Onglet|Options|  
    |---------|-------------|  
    |**Package**<br /><br /> Voici les options de l'onglet pour les packages stockés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans le magasin de packages d' [!INCLUDE[ssIS](../../includes/ssis-md.md)] .|**Serveur**<br /><br /> Tapez ou sélectionnez le nom de l'instance de serveur de base de données pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou pour le service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
    ||**Utiliser l'authentification Windows**<br /><br /> Sélectionnez cette option pour la connexion au serveur à l'aide d'un compte d'utilisateur Microsoft Windows.|  
    ||**Utiliser l’authentification SQL Server**<br /><br /> Quand un utilisateur se connecte avec un nom d’accès et un mot de passe spécifiés à partir d’une connexion non autorisée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réalise l’authentification en vérifiant si un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été défini et si le mot de passe spécifié correspond à celui enregistré précédemment. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas trouver le compte de connexion, l'authentification échoue et l'utilisateur reçoit un message d'erreur.|  
    ||**Nom d’utilisateur**|  
    ||**Mot de passe**|  
    ||**Package**<br /><br /> Cliquez sur le bouton de sélection et sélectionnez le package.<br /><br /> Vous sélectionnez un package dans un dossier sous le nœud **Packages stockés** dans l' **Explorateur d'objets**.|  
    |**Package**<br /><br /> Voici les options de l'onglet pour les packages stockés dans le système de fichiers.|**Package**<br /><br /> Tapez le chemin d'accès complet du fichier de package, ou cliquez sur le bouton pour sélectionner le package.|  
    |**Configurations**|Ajoutez un fichier de configuration XML pour exécuter le package avec une configuration spécifique. Utilisez une configuration de package pour mettre à jour les valeurs des propriétés du package au moment de l'exécution.<br /><br /> Cette option correspond à l’option **/ConfigFile** de **dtexec**.<br /><br /> Pour comprendre le fonctionnement de l'application des configurations de package, consultez [Package Configurations](./legacy-package-deployment-ssis.md). Pour plus d'informations sur la création d'un configuration de package, consultez [Create Package Configurations](./legacy-package-deployment-ssis.md).|  
    |**Fichiers de commandes**|Spécifiez les autres options que vous souhaitez exécuter avec **dtexec**, dans un fichier distinct.<br /><br /> Par exemple, vous pouvez inclure un fichier qui contient l’option /Dump *errorcode* pour générer des fichiers de vidage du débogage quand un ou plusieurs événements spécifiques se produisent pendant l’exécution du package.<br /><br /> Vous pouvez exécuter un package avec des ensembles d'options différents en créant plusieurs fichiers et en spécifiant le fichier approprié à l'aide de l'option **Fichiers de commandes** .<br /><br /> L’option **Fichiers de commandes** correspond à l’option **/CommandFile** de **dtexec**.|  
    |**Data Sources** (Sources de données)|Affichez les gestionnaires de connexions contenus dans le package. Pour modifier une chaîne de connexion, cliquez sur le gestionnaire de connexions et cliquez sur la chaîne de connexion.<br /><br /> Cette option correspond à l’option **/Connection** de **dtexec**.|  
    |**Options d'exécution**|**Mettre le package en échec en cas d'avertissements de validation**<br /> Indique si un avertissement est considéré une erreur. Si vous sélectionnez cette option et un avertissement se produit pendant la validation, le package échoue pendant la validation. Cette option correspond à l’option **/WarnAsError** de **dtexec**.<br /><br /> **Valider le package sans l'exécuter**<br /> Indique si l'exécution du package s'arrête après la phase de validation, sans exécuter effectivement le package. Cette option correspond à l’option **/Validate** de **dtexec**.<br /><br /> **Remplacer la propriété MacConcurrentExecutables**<br /> Spécifie le nombre d'exécutables que le package peut exécuter simultanément. La valeur -1 signifie que le package peut exécuter simultanément un nombre maximal de fichiers exécutables égal au nombre total de processeurs sur l'ordinateur exécutant le package, plus deux. Cette option correspond à l’option **/MaxConcurrent** de **dtexec**.<br /><br /> **Activer les points de contrôle de package**<br /> Indique si le package utilise des points de contrôle pendant l'exécution du package. Pour plus d'informations, consultez [Redémarrer des packages à l'aide de points de contrôle](../../integration-services/packages/restart-packages-by-using-checkpoints.md).<br /><br /> Cette option correspond à l’option **/CheckPointing** de **dtexec**.<br /><br /> **Substituer les options de redémarrage**<br /> Indique si une nouvelle valeur est définie pour la propriété **CheckpointUsage** du package. Sélectionnez une valeur dans la zone de liste **Option de redémarrage** .<br /><br /> Cette option correspond à l’option **/Restart** de **dtexec**.<br /><br /> **Utiliser le runtime 32 bits**<br /> Indiquez si le package est exécuté à l’aide de la version 32 bits de l’utilitaire dtexec sur un ordinateur 64 bits contenant une version 64 bits de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.<br /><br /> Il peut être nécessaire d'exécuter le package à l'aide de la version 32 bits de dtexec si, par exemple, le package utilise un fournisseur OLE DB natif qui n'est pas disponible en version 64 bits. Pour plus d'informations, consultez [Considérations 64 bits pour Integration Services](https://msdn.microsoft.com/library/ms141766\(SQL.105\).aspx).<br /><br /> Par défaut, lorsque vous sélectionnez le type d'étape de travail **Package SQL Server Integration Services** , [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent exécute le package à l'aide de la version de l'utilitaire dtexec qui est appelée automatiquement par le système. Le système appelle la version 32 bits ou 64 bits de l'utilitaire selon le processeur de l'ordinateur, et de la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent qui s'exécute sur l'ordinateur.|  
    |**Logging**|Associez un fournisseur d'informations à l'exécution du package.<br /><br /> **Module fournisseur d'informations SSIS pour les fichiers texte**<br /> Écrit les entrées du journal dans des fichiers texte ASCII<br /><br /> **Module fournisseur d'informations SSIS pour SQL Server**<br /> Écrit les entrées du journal dans la table sysssislog de la base de données MSDB.<br /><br /> **Module fournisseur d'informations SSIS pour SQL Server Profiler**<br /> Écrit des traces que vous pouvez afficher à l'aide de SQL Server Profiler.<br /><br /> **Module fournisseur d'informations SSIS pour le journal d'événements Windows**<br /> Écrit des entrées dans le journal Application du journal des événements Windows.<br /><br /> **Module fournisseur d'informations SSIS pour les fichiers XML**<br /> Écrit des fichiers journaux dans un fichier XML.<br /><br /> Pour le fichier texte, le fichier XML et les fournisseurs d'informations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler, sélectionnez les gestionnaires de connexions des fichiers contenus dans le package. Pour le fournisseur d'informations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionnez un gestionnaire de connexions OLE DB contenu dans le package.<br /><br /> Cette option correspond à l’option **/Logger** de **dtexec**.|  
    |**Valeurs définies**|Substituez un paramètre de propriété du package. Dans la zone **Propriétés** , entrez des valeurs dans les colonnes **Chemin d'accès de la propriété** et **Valeur** . Après avoir entré les valeurs d'une propriété, une ligne vide apparaît dans la zone **Propriétés** et vous permet d'entrer des valeurs pour une autre propriété.<br /><br /> Pour supprimer une propriété de la zone Propriétés, cliquez sur la ligne puis cliquez sur **Supprimer**.<br /><br /> Vous pouvez trouver le chemin de la propriété en procédant de l’une des façons suivantes :<br /><br /> \- Copiez le chemin de la propriété à partir du fichier de configuration XML (\*.dtsconfig). Le chemin d'accès est répertorié dans la section Configuration du fichier, comme valeur de l'attribut Path. Voici un exemple de chemin d’accès de la propriété MaximumErrorCount : \Package.Properties[MaximumErrorCount]<br /><br /> \- Exécutez **l’Assistant Configuration de package** et copiez les chemins des propriétés à partir de la dernière page **Fin de l’Assistant** . Vous pouvez ensuite quitter l'Assistant.|  
    |**Vérification**|**Exécuter uniquement les packages signés**<br /> Indique si la signature du package est vérifiée. Si le package n'est pas signé ou si la signature n'est pas valide, le package échoue. Cette option correspond à l’option **/VerifySigned** de **dtexec**.<br /><br /> **Vérifier la version du package**<br /> Indique si le numéro de build du package est vérifié par rapport au numéro de build entré dans la zone **Build** en regard de cette option. En cas de non-concordance, le package ne s'exécute pas. Cette option correspond à l’option **/VerifyBuild** de **dtexec**.<br /><br /> **Vérifier l'ID de package**<br /> Indique si le GUID du package est valide, en le comparant à l'ID de package entré dans la zone **ID du package** en regard de cette option. Cette option correspond à l’option **/VerifyPackageID** de **dtexec**.<br /><br /> **Vérifier l'ID de version**<br /> Indique si le GUID de la version est valide, en le comparant à l'ID de version entré dans la zone **ID de version** en regard de cette option. Cette option correspond à l’option **/VerifyVersionID** de **dtexec**.|  
    |**Ligne de commande**|Modifiez les options de ligne de commande de l'utilitaire dtexec. Pour plus d'informations sur les options, consultez [dtexec Utility](../../integration-services/packages/dtexec-utility.md).<br /><br /> **Restaurer les options d'origine**<br /> Utilisez les options de ligne de commande que vous avez définies sous les onglets **Package**, **Configurations**, **Fichiers de commandes**, **Sources de données**, **Options d’exécution**, **Journalisation**, **Valeurs définies** et **Vérification** de la boîte de dialogue **Propriétés de travail** .<br /><br /> **Modifier la commande manuellement**<br /> Entrez les options de ligne de commande supplémentaires dans la zone **Ligne de commande** .<br /><br /> Avant de cliquer sur **OK** pour enregistrer les modifications apportées à l’étape du travail, vous pouvez supprimer toutes les autres options que vous avez tapées dans la zone **Ligne de commande** en cliquant sur **Restaurer les options d’origine**.<br /><br /> **\*\* Conseil \*\*** Copiez la ligne de commande dans une fenêtre d’invite de commandes, ajoutez `dtexec`, puis exécutez le package à partir de la ligne de commande. Il s’agit d’un moyen simple de générer le texte dans la ligne de commande.|  
  
9. Cliquez sur **OK** pour enregistrer les paramètres et fermer la boîte de dialogue **Nouvelle étape de travail** .  
  
    > [!NOTE]
    > Pour les packages enregistrés dans le **Catalogue SSIS**, le bouton **OK** est désactivé lorsqu'il existe un paramètre de propriété du gestionnaire de connexions, ou un autre paramètre, non résolu. Un paramètre est considéré comme non résolu lorsque vous utilisez une valeur contenue dans une variable d’environnement serveur pour définir le paramètre ou la propriété, et lorsque l’une des conditions suivantes est remplie :  
    >   
    >  La case à cocher **Environnement** sous l'onglet **Configuration** n'est pas sélectionnée.  
    >   
    >  L'environnement serveur qui contient la variable n'est pas sélectionné dans la zone de liste de l'onglet **Configuration** .  
  
10. Pour créer une planification pour une étape de travail, cliquez sur **Planifications** dans le volet **Sélectionner une page** . Pour plus d'informations sur la manière de configurer une planification, consultez [Schedule a Job](../../ssms/agent/schedule-a-job.md).  
  
    > [!TIP]  
    >  Lorsque vous nommez la planification, utilisez un nom unique et un descriptif, pour différencier plus facilement la planification des autres planifications de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.  

## <a name="see-also"></a>Voir aussi  
 [Exécution de projets et de packages](deploy-integration-services-ssis-projects-and-packages.md)  

## <a name="external-resources"></a>Ressources externes  
  
-   Article de la Base de connaissances intitulé [Un package SSIS n’est pas exécuté lorsque vous appelez le package SSIS à partir d’une étape de travail de SQL Server Agent](https://support.microsoft.com/kb/918760)sur le site web [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
-   Vidéo, [Résolution des problèmes : exécution du package à l’aide de SQL Server Agent (vidéo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440760(v=sql.100)) dans MSDN Library  
  
-   Vidéo, [Guide pratique pour automatiser l’exécution du package à l’aide de SQL Server Agent (vidéo de SQL Server)](/previous-versions/sql/sql-server-2008/dd440761(v=sql.100)) dans MSDN Library  
  
-   Article technique intitulé [Checking SQL Server Agent jobs using Windows PowerShell](https://go.microsoft.com/fwlink/?LinkId=165675)sur mssqltips.com  
  
-   Article technique intitulé [Auto alert for SQL Agent jobs when they are enabled or disabled](https://go.microsoft.com/fwlink/?LinkId=165676)sur mssqltips.com  
  
-   Entrée de blog intitulée [Configuring SQL Agent Jobs to Write to Windows Event Log](https://go.microsoft.com/fwlink/?LinkId=220745)sur mssqltips.com  
  
