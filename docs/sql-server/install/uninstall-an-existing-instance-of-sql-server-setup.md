---
title: Désinstallation d’une instance existante
description: Cet article explique comment désinstaller une instance autonome de SQL Server, qui prépare également le système afin que vous puissiez réinstaller SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- removing instances of SQL Server
- uninstalling instances of SQL Server
- removing SQL Server
- instances of SQL Server, uninstalling
- uninstalling SQL Server
ms.assetid: 3c64b29d-61d7-4b86-961c-0de62261c6a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6a5b77c8e67d6512e691f748265e7901319a5e4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336091"
---
# <a name="uninstall-an-existing-instance-of-sql-server-setup"></a>Désinstaller une instance existante de SQL Server (programme d'installation)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Cet article explique comment désinstaller une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En suivant les étapes de cet article, vous préparez également le système afin de pouvoir réinstaller [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 > [!NOTE]
 > Pour désinstaller un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez la fonctionnalité Supprimer un nœud fournie par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour supprimer chaque nœud individuellement. Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server &#40;programme d’installation&#41;](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)  

## <a name="considerations"></a>Considérations

- Pour désinstaller SQL Server, vous devez être administrateur local et disposer des autorisations pour vous connecter en tant que service. 
- Si votre ordinateur a la quantité *minimale* de mémoire physique nécessaire, augmentez la taille du fichier d’échange à deux fois la quantité de mémoire physique. Une mémoire virtuelle insuffisante peut entraîner la suppression incomplète de SQL Server. 
- Sur un système avec plusieurs instances de SQL Server, le service SQL Server Browser n’est désinstallé qu’une fois la dernière instance de SQL Server supprimée. Le service SQL Server Browser peut être supprimé manuellement à partir de **Programmes et fonctionnalités** dans le **Panneau de configuration**. 
- Quand vous désinstallez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les fichiers de données tempdb qui ont été ajoutés pendant le processus d’installation sont supprimés. Les fichiers avec le modèle de nom tempdb_mssql_*.ndf sont supprimés s’ils existent dans le répertoire de base de données système. 
  

  
## <a name="prepare"></a>Préparation  
  
1.  **Sauvegardez vos données.** Créez des [sauvegardes complètes](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md) de toutes les bases de données, y compris les bases de données système, ou copiez manuellement les fichiers .mdf et .ldf à un emplacement distinct. La base de données **master** contient toutes les informations système relatives au serveur, telles que les connexions et les schémas. La base de données **msdb** contient des informations sur les travaux, comme que les travaux de SQL Server Agent, l’historique des sauvegardes et les plans de maintenance. Pour plus d’informations sur les bases de données système, consultez [Bases de données système](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). 
  
    Les fichiers que vous devez enregistrer incluent les fichiers de base de données suivants :  

    * master.mdf
    * msdbdata.mdf
    * Tempdb.mdf
    * mastlog.ldf
    * msdblog.ldf
    * Templog.ldf
    * model.mdf
    * Mssqlsystemresource.mdf
    * ReportServer[$InstanceName]
    * modellog.ldf
    * Mssqlsustemresource.ldf
    * ReportServer[$InstanceName]TempDB

    > [!NOTE]
    > Les bases de données ReportServer sont incluses avec SQL Server Reporting Services.   

 
1.  **Arrêtez tous** les **services** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nous vous recommandons d'arrêter tous les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant de désinstaller les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La désinstallation peut échouer s'il existe des connexions actives.  
  
1.  **Utiliser un compte bénéficiant des autorisations appropriées** Connectez-vous au serveur à l'aide du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à l'aide d'un compte doté d'autorisations équivalentes. Par exemple, vous pouvez vous connecter au serveur à l'aide d'un compte qui est membre du groupe Administrateurs local.  
  
## <a name="uninstall"></a>Désinstaller l’interface 

# <a name="windows-10--2016-"></a>[Windows 10 / 2016 +](#tab/Windows10)

Pour désinstaller SQL Server de Windows 10, Windows Server 2016, et Windows Server 2019 et ultérieur, procédez comme suit : 

1. Pour commencer le processus de suppression, accédez à **Paramètres** à partir du menu Démarrer, puis choisissez **Applications**. 
1. Recherchez `sql` dans la zone de recherche. 
1. Sélectionnez **Microsoft SQL Server (Version) (Bits)** . Par exemple : `Microsoft SQL Server 2017 (64-bit)`.
1. Sélectionner **Désinstaller**.
 
    ![Désinstaller SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-10.png)

1. Sélectionnez **Supprimer** dans la fenêtre boîte de dialogue contextuelle de SQL Server pour lancer l’Assistant Installation de Microsoft SQL Server. 

    ![Supprimer SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2017.png)
  
1.  Dans la page **Sélectionner une instance**, utilisez la zone de liste déroulante pour spécifier une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à supprimer ou spécifiez l’option pour supprimer seulement les fonctionnalités partagées et outils d’administration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour continuer, sélectionnez **suivant**.  
  
1.  Dans la page **Sélectionner les composants**, spécifiez les fonctionnalités à supprimer de l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
1.  Dans la page **Prêt pour la suppression** , consultez la liste des composants et des fonctionnalités à désinstaller. Cliquez sur **Supprimer** pour commencer la désinstallation  
 
1. Actualisez la fenêtre **Applications et fonctionnalités** pour vérifier que l’instance de SQL Server a été supprimée avec succès, et déterminez, le cas échéant, les composants de SQL Server qui existent encore. Si vous le souhaitez, supprimez ces composants à partir de cette fenêtre. 

# <a name="windows-2008---2012-r2"></a>[Windows 2008 - 2012 R2](#tab/windows2012)

Pour désinstaller SQL Server de Windows Server 2008, Windows Server 2012 et Windows Server 2012 R2, procédez comme suit : 

1. Pour commencer le processus de suppression, accédez au **Panneau de configuration**, puis sélectionnez **Programmes et fonctionnalités**.
1. Cliquez avec le bouton droit sur **Microsoft SQL Server (Version) (Bits)** , puis sélectionnez **Désinstaller**. Par exemple : `Microsoft SQL Server 2012 (64-bit)`.  
  
    ![Désinstaller SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-2012.png)

1. Sélectionnez **Supprimer** dans la fenêtre boîte de dialogue contextuelle de SQL Server pour lancer l’Assistant Installation de Microsoft SQL Server. 

    ![Supprimer SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2012.png)
  
1.  Dans la page **Sélectionner une instance**, utilisez la zone de liste déroulante pour spécifier une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à supprimer ou spécifiez l’option pour supprimer seulement les fonctionnalités partagées et outils d’administration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour continuer, sélectionnez **suivant**.  
  
1.  Dans la page **Sélectionner les composants**, spécifiez les fonctionnalités à supprimer de l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
1.  Dans la page **Prêt pour la suppression** , consultez la liste des composants et des fonctionnalités à désinstaller. Cliquez sur **Supprimer** pour commencer la désinstallation  
 
1. Actualisez la fenêtre **Programmes et fonctionnalités** pour vérifier que l’instance de SQL Server a été supprimée avec succès, et déterminez, le cas échéant, les composants de SQL Server qui existent encore. Si vous le souhaitez, supprimez ces composants à partir de cette fenêtre. 

---

  
## <a name="in-the-event-of-failure"></a>En cas de défaillance  

Si le processus de suppression échoue, passez en revue les [fichiers journaux d’installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md) pour déterminer la cause racine. 

L’article de la base de connaissances [Comment faire pour identifier les problèmes d’installation de SQL Server dans les fichiers journaux d’installation](https://support.microsoft.com/kb/955396/en-us) peut vous aider dans cette investigation. Bien qu’elle concerne SQL Server 2008, la méthodologie décrite est applicable à toutes les versions de SQL Server. 

  
## <a name="see-also"></a>Voir aussi  
 [Afficher et lire les fichiers journaux d'installation de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
