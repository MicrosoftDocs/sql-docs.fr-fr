---
title: Réduire un fichier | Microsoft Docs
description: Découvrez comment réduire un fichier de données ou journal dans SQL Server à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkfile.f1
helpviewer_keywords:
- shrinking files
- decreasing file size
- databases [SQL Server], shrinking
- reducing file size
- size [SQL Server], files
- file size [SQL Server]
ms.assetid: ce5c8798-c039-4ab2-81e7-90a8d688b893
author: stevestein
ms.author: sstein
ms.openlocfilehash: 077f7d98470c385b8333fc9db2f55d04bd13bb44
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812970"
---
# <a name="shrink-a-file"></a>Réduire un fichier
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment réduire un fichier de données ou un fichier journal dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 La réduction des fichiers de données permet de récupérer de l'espace en déplaçant des pages de données de la fin du fichier vers un espace inoccupé plus proche de l'avant du fichier. Quand une quantité d'espace libre suffisante est créée à la fin du fichier, des pages de données à la fin du fichier peuvent être désallouées et retournées au système de fichiers.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour réduire un fichier de données ou un fichier journal, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   La taille du fichier de données primaire ne peut pas être inférieure à celle du fichier primaire de la base de données model.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Les données qui sont déplacées pour réduire un fichier peuvent être dispersées à n'importe quel emplacement disponible dans le fichier. Cela provoque la fragmentation de l'index et peut ralentir les performances des requêtes qui recherchent une plage de l'index. Pour éliminer la fragmentation, reconstruisez les index dans le fichier après réduction.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-shrink-a-data-or-log-file"></a>Pour réduire un fichier de données ou un fichier journal  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Développez **Bases de données** , puis cliquez avec le bouton droit sur la base de données à réduire.  
  
3.  Dans le menu **Tâches**, pointez sur **Réduire**, puis cliquez sur **Fichiers**.  
  
     **Sauvegarde de la base de données**  
     Affiche le nom de la base de données sélectionnée.  
  
     **Type de fichier**  
     Sélectionnez le type du fichier. Les choix possibles sont **Données** et **Journal** . La sélection par défaut est **Données**. La sélection d'un autre type de groupe de fichiers modifie en conséquence les sélections des autres champs.  
  
     **Groupe de fichiers**  
     Sélectionnez un groupe de fichiers dans la liste des groupes de fichiers associés au **Type de fichier** sélectionné auparavant. La sélection d'un autre groupe de fichiers modifie en conséquence les sélections des autres champs.  
  
     **Nom de fichier**  
     Sélectionnez un fichier dans la liste des fichiers disponibles pour le groupe de fichiers et le type de fichier choisis.  
  
     **Lieu**  
     Affiche le chemin d'accès complet au fichier actuellement sélectionné. Le chemin d'accès n'est pas modifiable, mais il peut être copié dans le Presse-papiers.  
  
     **Espace actuellement alloué**  
     Pour les fichiers de données, affiche l'espace actuellement alloué. Pour les fichiers journaux, ce champ affiche l'espace actuellement alloué qui est calculé à partir de la sortie de DBCC SQLPERF(LOGSPACE).  
  
     **Espace libre disponible**  
     Pour les fichiers de données, ce champ affiche l'espace libre actuellement disponible qui est calculé à partir de la sortie de DBCC SHOWFILESTATS(fileid). Pour les fichiers journaux, ce champ affiche l'espace libre actuellement disponible qui est calculé à partir de la sortie de DBCC SQLPERF(LOGSPACE).  
  
     **Libérer l'espace inutilisé**  
     Tout espace inutilisé dans les fichiers est libéré pour le système d'exploitation et le fichier est réduit à la dernière extension allouée, ce qui en réduit la taille sans toucher aux données. Aucune tentative de réaffectation des lignes aux pages non allouées n'est entreprise.  
  
     **Réorganiser les pages avant de libérer l'espace inutilisé**  
     Équivaut à exécuter DBCC SHRINKFILE en spécifiant la taille du fichier cible. Lorsque cette option est activée, l'utilisateur doit spécifier la taille du fichier cible dans la zone **Réduire le fichier à** .  
  
     **Réduire le fichier à**  
     Spécifie la taille du fichier cible pour l'opération de réduction. La taille ne peut pas être inférieure à l'espace actuellement alloué ou supérieure à l'extension totale qui est allouée au fichier. Lorsqu'une valeur supérieure à la valeur minimale ou maximale est entrée, la valeur min ou max est rétablie lorsqu'un autre élément est sélectionné ou que vous cliquez sur un bouton de la barre d'outils.  
  
     **Vider le fichier en effectuant une migration des données vers d'autres fichiers dans le même groupe de fichiers**  
     Migre toutes les données du fichier spécifié. Cette option permet au fichier d'être supprimé à l'aide de l'instruction ALTER DATABASE. Elle équivaut à exécuter DBCC SHRINKFILE avec l'option EMPTYFILE.  
  
4.  Sélectionnez le type et le nom du fichier.  
  
5.  Éventuellement, activez la case à cocher **Libérer l'espace inutilisé** .  
  
     En activant cette case d'option, tout espace inutilisé dans les fichiers de données est libéré vers le système d'exploitation et le fichier est réduit à la dernière étendue allouée, sans déplacement de données.  
  
6.  Éventuellement, activez la case d'option **Réorganiser les pages avant de libérer de l'espace inutilisé** . Si cette case d'option est activée, la valeur **Réduire le fichier à** doit être spécifiée. Cette option est désactivée par défaut.  
  
     En activant cette case d'option, tout espace inutilisé dans les fichiers de données est libéré vers le système d'exploitation et, dans la mesure du possible, les lignes sont déplacées dans des pages non allouées.  
  
7.  Éventuellement, entrez le pourcentage maximal d'espace libre à laisser dans le fichier de base de données après la réduction de la base de données. Les valeurs autorisées sont comprises entre 0 et 99. Cette option est disponible uniquement lorsque **Réorganiser les pages avant de libérer de l'espace inutilisé** est activée.  
  
8.  Éventuellement, activez la case d'option **Vider le fichier en effectuant une migration des données vers d'autres fichiers dans le même groupe de fichiers** .  
  
     L'activation de cette case d'option déplace toutes les données du fichier spécifié dans d'autres fichiers du groupe de fichiers. Le fichier vide peut alors être supprimé. Cette case d'option revient à exécuter DBCC SHRINKFILE avec l'option EMPTYFILE.  
  
9. Cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-shrink-a-data-or-log-file"></a>Pour réduire un fichier de données ou un fichier journal  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple utilise [DBCC SHRINKFILE](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md) pour réduire la taille d'un fichier de données nommé `DataFile1` dans la base de données `UserDB` à 7 Mo.  
  
 [!code-sql[DBCC#DBCC_SHRINKFILE1](../../relational-databases/databases/codesnippet/tsql/shrink-a-file_1.sql)]  
  
## <a name="see-also"></a>Voir aussi  
 [DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)   
 [Réduire une base de données](../../relational-databases/databases/shrink-a-database.md)   
 [Supprimer des fichiers de données ou des fichiers journaux d'une base de données](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
  
  
