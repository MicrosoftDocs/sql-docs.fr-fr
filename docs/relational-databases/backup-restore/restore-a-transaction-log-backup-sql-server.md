---
title: Restaurer une sauvegarde du journal des transactions (SQL Server) | Microsoft Docs
description: Cet article explique comment restaurer une sauvegarde de fichier journal dans SQL Server, à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restoretlog.general.f1
- sql13.swb.restoretlog.options.f1
helpviewer_keywords:
- restore log
- backing up transaction logs [SQL Server], restoring
- transaction log backups [SQL Server], restoring
- restoring transaction logs [SQL Server], restoring backups
- transaction log restores [SQL Server], SQL Server Management Studio
ms.assetid: 1de2b888-78a6-4fb2-a647-ba4bf097caf3
author: cawrites
ms.author: chadam
ms.openlocfilehash: f4f4e5448a01ce9e4d644aec6dd50eaea2c31297
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/13/2021
ms.locfileid: "100346772"
---
# <a name="restore-a-transaction-log-backup-sql-server"></a>Restaurer une sauvegarde de journal des transactions (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment restaurer une sauvegarde du journal des transactions dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Composants requis](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour restaurer une sauvegarde de journal des transactions à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tâches associées](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Ces sauvegardes doivent être restaurées dans l'ordre de leur création. Avant de pouvoir restaurer une sauvegarde donnée du journal des transactions, vous devez restaurer les sauvegardes antérieures suivantes sans annuler les transactions non validées, autrement dit avec l'option WITH NORECOVERY.  
  
    -   La sauvegarde complète de la base de données et la dernière sauvegarde différentielle, s'il en existe, effectuée avant la sauvegarde donnée du journal des transactions. Avant de créer la sauvegarde complète ou différentielle de base de données la plus récente, la base de données devait utiliser le mode de restauration complète ou le mode de récupération utilisant les journaux de transactions.  
  
    -   Toutes les sauvegardes du journal des transactions effectuées après la sauvegarde complète de la base de données ou la sauvegarde différentielle (si vous la restaurez), mais avant la sauvegarde donnée du journal des transactions. Les sauvegardes du journal doivent être appliquées dans l'ordre dans lequel elles ont été créées, sans rupture dans la séquence de journaux.  
  
         Pour plus d’informations sur les sauvegardes du journal des transactions, consultez [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md) et [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations RESTORE sont attribuées aux rôles dont les informations d'appartenance sont toujours immédiatement accessibles à partir du serveur. Étant donné que l’appartenance au rôle de base de données fixe ne peut être contrôlée que quand la base de données est accessible et non endommagée, ce qui n’est pas toujours le cas lorsque RESTORE est exécuté, les membres du rôle de base de données fixe **db_owner** ne détiennent pas d’autorisations RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
> [!WARNING]  
>  La procédure de restauration normale consiste à sélectionner les sauvegardes des journaux dans la boîte de dialogue **Restaurer la base de données** en même temps que les sauvegardes de données et les sauvegardes différentielles.  
  
#### <a name="to-restore-a-transaction-log-backup"></a>Pour restaurer une sauvegarde de journal des transactions  
  
1.  Après vous être connecté à l’instance appropriée du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)], dans l’Explorateur d’objets, cliquez sur le nom du serveur pour développer son arborescence.  
  
2.  Développez **Bases de données** puis, selon la base de données, sélectionnez une base de données utilisateur ou développez **Bases de données système** et sélectionnez une base de données système.  
  
3.  Cliquez avec le bouton droit sur la base de données, pointez sur **Tâches**, pointez sur **Restaurer**, puis cliquez sur **Journal des transactions**, pour ouvrir la boîte de dialogue **Restaurer le journal des transactions** .  
  
    > [!NOTE]  
    >  Si **Journal des transactions** est grisé, dans un premier temps, vous devrez peut-être restaurer une sauvegarde complète ou différentielle. Utilisez la boîte de dialogue **Sauvegarde de la base de données** .  
  
4.  Dans la zone de liste **Base de données** de la page **Général** , sélectionnez le nom d'une base de données. Seules les bases de données en état de restauration sont répertoriées.  
  
5.  Pour préciser la source et l'emplacement des jeux de sauvegarde à restaurer, cliquez sur l'une des options suivantes :  
  
    -   **À partir de sauvegardes précédentes de la base de données**  
  
         Sélectionnez la base de données à restaurer dans la liste déroulante. La liste contient uniquement les bases de données qui ont été sauvegardées selon l'historique de sauvegarde **msdb** .  
  
    -   **À partir d'un fichier ou d'une bande**  
  
         Cliquez sur le bouton Parcourir ( **...** ) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . Dans la zone **Type du média de sauvegarde** , sélectionnez l'un des types d'unités proposés. Pour sélectionner une ou plusieurs unités pour la zone **Support de sauvegarde** , cliquez sur **Ajouter**.  
  
         Après avoir ajouté les unités souhaitées à la zone de liste **Support de sauvegarde** , cliquez sur **OK** pour revenir à la page **Général** .  
  
6.  Dans la grille **Sélectionner les sauvegardes du journal des transactions à restaurer** , sélectionnez les sauvegardes à restaurer. Cette grille répertorie les sauvegardes du journal des transactions disponibles pour la base de données sélectionnée. Une sauvegarde du journal n'est disponible que si son **Premier NSE** est supérieur au **Dernier NSE** de la base de données. Les sauvegardes des journaux sont affichées dans l'ordre des numéros séquentiels dans le journal (NSE) qu'elles contiennent et elles doivent être restaurées dans cet ordre.  
  
     Le tableau suivant répertorie les en-têtes de colonnes de la grille et décrit leur valeur.  
  
    |En-tête|Valeur|  
    |------------|-----------|  
    |**Restauration**|Les cases à cocher indiquent les jeux de sauvegarde à restaurer.|  
    |**Nom**|Nom du jeu de sauvegardes.|  
    |**Composant**|Composant de sauvegarde : **Base de données**, **Fichier** ou \<blank> (pour les journaux des transactions).|  
    |**Sauvegarde de la base de données**|Nom de la base de données impliquée dans la sauvegarde.|  
    |**Date de début**|Date et heure de début de la sauvegarde, d'après les paramètres régionaux du client.|  
    |**Date de fin**|Date et heure de fin de la sauvegarde, d'après les paramètres régionaux du client.|  
    |**Premier NSE**|Numéro séquentiel dans le journal correspondant à la première transaction dans le jeu de sauvegarde. Vide pour les sauvegardes de fichiers.|  
    |**Dernier NSE**|Numéro séquentiel dans le journal correspondant à la dernière transaction dans le jeu de sauvegarde. Vide pour les sauvegardes de fichiers.|  
    |**NSE du point de contrôle**|Numéro séquentiel dans le journal correspondant au point de contrôle le plus récent au moment où la sauvegarde a été créée.|  
    |**Tous les NSE**|Numéro séquentiel dans le journal correspondant à la sauvegarde complète la plus récente de la base de données.|  
    |**Serveur**|Nom de l'instance du moteur de base de données qui a effectué l'opération de sauvegarde.|  
    |**Nom d’utilisateur**|Nom de l'utilisateur qui a effectué la restauration.|  
    |**Taille**|Taille du jeu de sauvegarde en octets.|  
    |**Position**|Position du jeu de sauvegarde dans le volume|  
    |**Expiration**|Date et heure d'expiration du jeu de sauvegardes.|  
  
7.  Sélectionnez l’un des suivants :  
  
    -   **Limite dans le temps**  
  
         Conservez la valeur par défaut (**Le plus récent possible**) ou sélectionnez une date et une heure données en cliquant sur le bouton d’exploration pour ouvrir la boîte de dialogue **Limite de restauration dans le temps** .  
  
    -   **Transaction marquée**  
  
         Restaurez la base de données jusqu'à une transaction marquée auparavant. Cette option ouvre la boîte de dialogue **Sélectionner une transaction marquée** . Celle-ci affiche une grille qui répertorie les transactions marquées disponibles dans les sauvegardes du journal des transactions sélectionnées.  
  
         Par défaut, la restauration s'effectue jusqu'à la transaction marquée, en l'excluant. Pour restaurer également la transaction marquée, sélectionnez **Inclure la transaction marquée**.  
  
         Le tableau suivant répertorie les en-têtes de colonnes de la grille et décrit leur valeur.  
  
        |En-tête|Valeur|  
        |------------|-----------|  
        |\<blank>|Affiche une case à cocher pour la sélection de la marque.|  
        |**Marque de transaction**|Nom de la transaction marquée spécifiée par l'utilisateur lors de la validation de la transaction.|  
        |**Date**|Date et heure de la validation de la transaction. La date et l’heure sont affichées telles qu’enregistrées dans la table **msdbgmarkhistory** , et non dans l’option Date et heure de l’ordinateur client.|  
        |**Description**|Description de la transaction marquée spécifiée par l'utilisateur lorsque la transaction a été validée (le cas échéant).|  
        |**LSN**|Numéro séquentiel dans le journal de la transaction marquée.|  
        |**Sauvegarde de la base de données**|Nom de la base de données où la transaction marquée a été validée.|  
        |**Nom d’utilisateur**|Nom de l'utilisateur de la base de données où la transaction marquée a été validée.|  
  
8.  Pour afficher ou sélectionner les options avancées, cliquez sur **Options** dans le volet **Sélectionner une page** .  
  
9. Dans la section **Options de restauration** , les options sont les suivantes :  
  
    -   **Conserver les paramètres de la réplication (WITH KEEP_REPLICATION)**  
  
         Conserve les paramètres de réplication lors de la restauration d'une base de données publiée sur un serveur autre que celui sur lequel la base de données a été créée.  
  
         Cette option est disponible uniquement avec l’option **Laisser la base de données opérationnelle en restaurant les transactions non validées** (décrite plus loin). Cela équivaut à la restauration d’une base de données avec l’option **RECOVERY** .  
  
         L’activation de cette option revient à utiliser l’option **KEEP_REPLICATION** dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** .  
  
    -   **Demander confirmation avant chaque restauration de sauvegarde**  
  
         Avant de restaurer chaque jeu de sauvegarde (après la première), cette option affiche la boîte de dialogue **Continuer la restauration** qui vous invite à préciser si vous voulez continuer la séquence de restauration. Ce dialogue affiche le nom du support de sauvegarde suivant (s'il est disponible), ainsi que le nom et la description du jeu de sauvegarde.  
  
         Cette option est particulièrement utile lorsque vous devez changer des bandes de différents supports de sauvegarde. Vous pouvez par exemple l'utiliser lorsque le serveur n'a qu'un périphérique à bandes. Attendez d'être prêt à continuer avant de cliquer sur **OK**.  
  
         Cliquez sur **Non** pour laisser la base de données dans l'état de restauration. Si vous le souhaitez, vous pouvez poursuivre la séquence de restauration lorsque la dernière restauration est terminée. Si la sauvegarde suivante est une sauvegarde de données ou différentielle, utilisez à nouveau la tâche **Restaurer la base de données** . S'il s'agit d'une sauvegarde de journal, utilisez la tâche **Restaurer le journal des transactions** .  
  
    -   **Restreindre l'accès à la base de données restaurée (WITH RESTRICTED_USER)**  
  
         Limite l’accès à la base de données restaurée aux membres de **db_owner**, **dbcreator** ou **sysadmin**.  
  
         L’activation de cette option revient à utiliser l’option **RESTRICTED_USER** dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** .  
  
10. Pour les options **État de récupération** , spécifiez l'état de la base de données après la restauration.  
  
    -   **Laisser la base de données opérationnelle en restaurant les transactions non validées. Les journaux des transactions supplémentaires ne peuvent pas être restaurés. (RESTORE WITH RECOVERY)**  
  
         Récupère la base de données. Cette option équivaut à l’option **RECOVERY** dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** .  
  
         Choisissez cette option uniquement si vous ne voulez restaurer aucun fichier journal.  
  
    -   **Laisser la base de données non opérationnelle, et ne pas restaurer les transactions non validées. Les journaux des transactions supplémentaires peuvent être restaurés. (RESTORE WITH NORECOVERY)**  
  
         Laisse la base de données non récupérée dans l'état **RESTORING** . Cette option est équivalente à l’option **NORECOVERY** dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** .  
  
         Lorsque vous la choisissez, l'option **Conserver les paramètres de réplication** n'est pas disponible.  
  
        > [!IMPORTANT]  
        >  Pour une base de données secondaire ou miroir, sélectionnez toujours cette option.  
  
    -   **Laisser la base de données en lecture seule. Annulez les transactions non validées, mais enregistrez les actions d'annulation dans un fichier d'annulation afin de rendre réversibles les effets de la récupération. (RESTORE WITH STANDBY)**  
  
         Laisse la base de données dans un état de secours. Cette option est équivalente à l’option **STANDBY** dans une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)]**RESTORE** .  
  
         Cette option nécessite de définir un fichier d'annulation.  
  
11. Éventuellement, spécifiez un nom de fichier d'annulation dans la zone de texte **Fichiers d'annulation** . Cette option est indispensable si vous laissez la base de données en lecture seule. Vous pouvez rechercher le fichier d'annulation ou taper son chemin d'accès dans la zone de texte.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
> [!IMPORTANT]  
>  Nous vous recommandons de toujours préciser de façon claire WITH NORECOVERY ou WITH RECOVERY dans chaque instruction RESTORE pour éliminer toute ambiguïté. Cela est particulièrement important lors de l'écriture de scripts.  
  
#### <a name="to-restore-a-transaction-log-backup"></a>Pour restaurer une sauvegarde de journal des transactions  
  
1.  Exécutez l'instruction RESTORE LOG pour appliquer la sauvegarde du journal des transactions, en spécifiant :  
  
    -   le nom de la base de données à laquelle sera appliqué le journal des transactions ;  
  
    -   l'unité de sauvegarde à partir de laquelle sera restaurée la sauvegarde du journal des transactions ;  
  
    -   la clause NORECOVERY.  
  
     La syntaxe de base pour cette instruction est la suivante :  
  
     RESTORE LOG *nom_base_de_données* FROM <unité_de_sauvegarde> WITH NORECOVERY.  
  
     Où *nom_base_de_données* représente le nom de la base de données et <unité_de_sauvegarde> correspond au nom de l’unité qui contient la sauvegarde du journal en cours de restauration.  
  
2.  Répétez l'étape 1 pour chaque sauvegarde du journal des transactions à appliquer.  
  
3.  Après avoir restauré la dernière sauvegarde de votre séquence de restauration, utilisez l'une des instructions ci-dessous :  
  
    -   Récupérer la base de données dans le cadre de la dernière instruction RESTORE LOG :  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH RECOVERY;  
        GO  
        ```  
  
    -   attendre de récupérer la base de données à l'aide d'une instruction séparée RESTORE DATABASE :  
  
        ```  
        RESTORE LOG <database_name> FROM <backup_device> WITH NORECOVERY;   
        RESTORE DATABASE <database_name> WITH RECOVERY;  
        GO  
        ```  
  
         Attendre de récupérer la base de données vous permet de vérifier si vous avez restauré toutes les sauvegardes de journaux nécessaires. Cette méthode est souvent conseillée si vous effectuez une restauration limitée dans le temps.  
  
    > [!IMPORTANT]  
    >  Si vous créez une base de données miroir, omettez l'étape de récupération. Une base de données miroir doit rester dans l'état RESTORING.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Exemples (Transact-SQL)  
 Par défaut, la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] utilise le mode de récupération simple. Les exemples suivants nécessitent la modification de la base de données pour utiliser le mode de restauration complète, comme suit :  
  
```sql  
ALTER DATABASE AdventureWorks2012 SET RECOVERY FULL;  
```  
  
#### <a name="a-applying-a-single-transaction-log-backup"></a>R. Application d'une sauvegarde unique du journal des transactions  
 Dans cet exemple, nous commençons par restaurer la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] à l'aide d'une sauvegarde complète de base de données qui réside sur une unité de sauvegarde nommée `AdventureWorks2012_1`. Nous appliquons ensuite la première sauvegarde du journal des transactions qui réside sur une unité de sauvegarde nommée `AdventureWorks2012_log`. Enfin, nous récupérons la base de données.  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
#### <a name="b-applying-multiple-transaction-log-backups"></a>B. Application de plusieurs sauvegardes du journal des transactions  
 Dans cet exemple, nous commençons par restaurer la base de données [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] à l'aide d'une sauvegarde complète de base de données qui réside sur une unité de sauvegarde nommée `AdventureWorks2012_1`. Nous appliquons ensuite, successivement, les premières sauvegardes du journal des transactions qui résident sur une unité de sauvegarde nommée `AdventureWorks2012_log`. Enfin, nous récupérons la base de données.  
  
```sql  
RESTORE DATABASE AdventureWorks2012  
   FROM AdventureWorks2012_1  
   WITH NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 1,  
   NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 2,  
   NORECOVERY;  
GO  
RESTORE LOG AdventureWorks2012  
   FROM AdventureWorks2012_log  
   WITH FILE = 3,  
   NORECOVERY;  
GO  
RESTORE DATABASE AdventureWorks2012  
   WITH RECOVERY;  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
  
-   [Sauvegarder un journal des transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Restaurer une sauvegarde de base de données à l’aide de SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Restaurer une base de données jusqu’au point d’échec en mode de récupération complète &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Restaurer une base de données SQL Server jusqu’à une limite dans le temps &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
-   [Restaurer une base de données jusqu’à une transaction marquée &#40;SQL Server Management Studio&#41;](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
## <a name="see-also"></a>Voir aussi  
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Appliquer les sauvegardes du journal de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)  
  
  
