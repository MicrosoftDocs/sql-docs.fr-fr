---
title: Désactiver la compression sur une table ou un index | Microsoft Docs
description: Découvrez comment désactiver la compression sur une table ou un index dans SQL Server en utilisant SQL Server Management Studio ou Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- data compression [SQL Server], disabling
ms.assetid: bda1e452-397b-4757-82a4-181217361589
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 994c22db58c0d06b5a763769688d911cd739b367
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250708"
---
# <a name="disable-compression-on-a-table-or-index"></a>Désactiver la compression sur une table ou un index

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cette rubrique explique comment désactiver la compression sur une table ou un index dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour désactiver la compression sur une table ou un index à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Si la table est un segment de mémoire, l'opération de reconstruction pour le mode ONLINE sera monothread. Utilisez le mode OFFLINE pour une opération de reconstruction de segment de mémoire multithread. Pour plus d’informations sur la compression de données, consultez [Compression des données](../../relational-databases/data-compression/data-compression.md).  
  
-   Pour évaluer la façon dont la modification de l’état de compression affecte une table, un index ou une partition, utilisez la procédure stockée [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .  
  
-   Vous ne pouvez pas modifier le paramètre de compression d'une partition unique si la table possède des index non alignés.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Nécessite une autorisation ALTER sur la table ou l'index.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-disable-compression-on-a-table"></a>Pour désactiver la compression sur une table  
  
1.  Dans l'Explorateur d'objets, développez la base de données contenant la table sur laquelle vous souhaitez désactiver la compression, puis développez le dossier **Tables** .  
  
2.  Cliquez avec le bouton droit sur la table ou l’index sur lequel vous souhaitez désactiver la compression, pointez sur **Stockage** et sélectionnez **Gérer la compression...** .  
  
3.  Pour désactiver la compression sur un index, développez la table contenant l'index puis développez le dossier **Index** .  
  
4.  Dans l'Assistant Compression de données, sur la page **Assistant Compression de données** , cliquez sur **Suivant**.  
  
5.  Dans la page **Sélectionner le type de compression** , sélectionnez le type de compression **None** pour qu'aucune compression ne soit appliquée aux partitions de l'index. Une fois que vous avez terminé, cliquez sur **Suivant**.  
  
     Les options suivantes sont disponibles dans la page **Sélectionner le type de compression** :  
  
     Case **Utiliser le même type de compression pour toutes les partitions**  
     Sélectionnez cette option pour configurer le même paramètre de compression pour toutes les partitions. La zone de sélection est alors activée et la colonne **Type de compression** de la grille est désactivée. Lorsqu'une option est sélectionnée, les options dans la liste adjacente sont **None**, **Row** et **Page**.  
  
     **Numéro de partition**  
     Répertorie chaque partition de la table ou de l'index. Cette colonne est en lecture seule.  
  
     **Type de compression**  
     Sélectionnez l'option de compression de chaque partition. Non disponible lorsque **Utiliser le même type de compression pour toutes les partitions** est sélectionné. Les options de la liste sont **None**, **Row** et **Page**.  
  
     **Limite**  
     Affiche la limite de la partition. Cette colonne est en lecture seule.  
  
     **Nombre de lignes**  
     Affiche le nombre de lignes de cette partition. Cette colonne est en lecture seule.  
  
     **Espace actuel**  
     Affiche l'espace actuellement occupé par cette partition en mégaoctets (Mo). Cette colonne est en lecture seule.  
  
     **Espace compressé requis**  
     Après avoir cliqué sur **Calculer**, cette colonne affiche la taille estimée de chaque partition après la compression en utilisant le paramètre spécifié dans la colonne **Type de compression** . Cette colonne est en lecture seule.  
  
     **Calculer**  
     Cliquez sur ce bouton pour estimer la taille de chaque partition après la compression en utilisant le paramètre spécifié dans la colonne **Type de compression** .  
  
6.  Sur la page **Sélectionner une option de sortie** , spécifiez de quelle manière vous souhaitez effectuer cette tâche. Sélectionnez **Créer un script** pour créer un script SQL basé sur les pages précédentes de l'Assistant. Sélectionnez **Exécuter immédiatement** pour créer la nouvelle table partitionnée après avoir complété toutes les pages restantes de l'Assistant. Sélectionnez **Planification** pour créer la nouvelle table partitionnée à un moment prédéterminé dans le futur.  
  
     Si vous sélectionnez **Créer un script**, les options suivantes sont disponibles sous **Options de script**:  
  
     **Générer un script sur fichier**  
     Génère le script sous la forme d'un fichier .sql. Entrez un nom de fichier et un emplacement dans la boîte de dialogue **Nom de fichier** ou cliquez sur **Parcourir** pour ouvrir la boîte de dialogue **Emplacement du fichier de script** . Pour **Enregistrer sous**, sélectionnez **Texte Unicode** ou **Texte ANSI**.  
  
     **Générer un script sur le Presse-papiers**  
     Enregistre le script dans le Presse-papiers.  
  
     **Générer un script dans une nouvelle fenêtre de requête**  
     Génère le script dans une nouvelle fenêtre de l'éditeur de requêtes. Il s'agit de la sélection par défaut.  
  
     Si vous sélectionnez **Planification**, cliquez sur **Modifier la planification**.  
  
    1.  Dans la boîte de dialogue **Nouvelle planification du travail**, dans la zone **Nom**, entrez le nom de la planification du travail.  
  
    2.  Dans la liste **Type de planification** , sélectionnez le type de la planification :  
  
        -   **Lancer automatiquement au démarrage de SQL Server Agent**  
  
        -   **Démarrer dès que les processeurs sont inactifs**  
  
        -   **Périodique**. Sélectionnez cette option si votre nouvelle table partitionnée est mise à jour régulièrement avec de nouvelles informations.  
  
        -   **Une fois**. Il s'agit de la sélection par défaut.  
  
    3.  Activez ou désactivez la case à cocher **Activé** pour activer ou désactiver la planification.  
  
    4.  Si vous sélectionnez **Périodique**:  
  
        1.  Sous **Fréquence**, dans la liste **Périodicité** , spécifiez la fréquence d'occurrence :  
  
            -   Si vous sélectionnez **Quotidienne**, dans la zone **Répéter toutes les** , entrez la fréquence de répétition du travail de planification en jours.  
  
            -   Si vous sélectionnez **Hebdomadaire**, dans la zone **Répéter toutes les** , entrez la fréquence de répétition du travail de planification en semaines. Sélectionnez le jour ou les jours de la semaine pendant lesquels la planification du travail est exécutée.  
  
            -   Si vous sélectionnez **Mensuelle**, sélectionnez **Jour** ou **Le**.  
  
                -   Si vous sélectionnez **Jour**, entrez la date du mois à laquelle vous souhaitez que la planification du travail s'exécute, ainsi que la fréquence de répétition de la planification du travail en mois. Par exemple, si vous souhaitez que la planification du travail s’exécute le 15 du mois un mois sur deux, sélectionnez **Jour**, puis entrez « 15 » dans la première zone et « 2 » dans la deuxième zone. Notez que le nombre maximum autorisé dans la deuxième zone est « 99 ».  
  
                -   Si vous sélectionnez **Le**, sélectionnez le jour spécifique de la semaine et du mois pendant lequel vous voulez que la planification du travail s'exécute et la fréquence à laquelle la planification du travail doit se répéter en mois. Par exemple, si vous souhaitez que la planification du travail s’exécute le dernier jour de la semaine un mois sur deux, sélectionnez **Jour**, puis **dernier** dans la première liste, **jour ouvrable** dans la deuxième liste et « 2 » dans la dernière zone. Vous pouvez également sélectionner **premier**, **deuxième**, **troisième** ou **quatrième**, ainsi que des jours de la semaine spécifiques (par exemple, dimanche ou mercredi) dans les deux premières listes. Notez que le nombre maximum autorisé dans la dernière zone est « 99 ».  
  
        2.  Sous **Fréquence quotidienne**, spécifiez la fréquence à laquelle la planification du travail se répète le jour de son exécution :  
  
            -   Si vous sélectionnez **Une fois à**, entrez l'heure spécifique à laquelle la planification du travail doit s'exécuter dans la zone **Une fois à** . Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
            -   Si vous sélectionnez **Toutes les**, spécifiez la fréquence à laquelle la planification du travail s'exécute pendant la journée choisie sous **Fréquence**. Par exemple, si vous souhaitez que la planification du travail se répète toutes les 2 heures le jour d’exécution de la planification du travail, sélectionnez **Toutes les**, entrez « 2 » dans la première zone, puis sélectionnez **heure(s)** dans la liste. Dans cette liste, vous pouvez également sélectionner **minute(s)** et **seconde(s)** . Notez que le nombre maximum autorisé dans la première zone est « 100 ».  
  
                 Dans la zone **Début** , entrez l'heure à laquelle l'exécution de la planification du travail doit démarrer. Dans la zone **Fin** , entrez l'heure à laquelle la planification du travail doit s'arrêter. Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
        3.  Sous **Durée**, dans la zone **Date de début**, entrez la date à laquelle vous souhaitez que l'exécution de la planification du travail commence. Sélectionnez **Date de fin** ou **Aucune date de fin** pour indiquer à quel moment l'exécution de la planification du travail doit s'arrêter. Si vous sélectionnez **Date de fin**, entrez la date à laquelle l'exécution de la planification du travail doit s'arrêter.  
  
    5.  Si vous sélectionnez **Une fois** sous **Une seule occurrence**, dans la zone **Date** , entrez la date à laquelle la planification du travail est exécutée. Dans la zone **Heure** , entrez l'heure à laquelle la planification du travail sera exécutée. Entrez l'heure, les minutes et les secondes du jour, ainsi que AM ou PM.  
  
    6.  Sous **Résumé**, dans **Description**, vérifiez que tous les paramètres de planification du travail sont corrects.  
  
    7.  Cliquez sur **OK**.  
  
     Après avoir complété cette page, cliquez sur **Suivant**.  
  
7.  Dans la page **Vérifier le résumé** , sous **Vérifier vos sélections**, développez toutes les options disponibles pour vérifier que tous les paramètres sont corrects. Si tout est conforme à vos attentes, cliquez sur **Terminer**.  
  
8.  Sur la page **Progression de l'Assistant Compression de données** , surveillez les informations d'état relatives aux actions de l'Assistant Création de partition. Selon les options sélectionnées dans l'Assistant, la page de progression peut contenir une ou plusieurs actions. La zone supérieure affiche l'état global de l'Assistant et le nombre des messages d'état, d'erreur et d'avertissement qu'il a reçus.  
  
     Les options suivantes sont disponibles sur la page **Progression de l'Assistant Compression de données** :  
  
     **Détails**  
     Indique l'action, l'état et tous les messages retournés suite à l'action entreprise par l'Assistant.  
  
     **Action**  
     Indique le type et le nom de chaque action.  
  
     **État**  
     Indique si l’action de l’Assistant dans son ensemble a retourné la valeur **Réussite** ou **Échec**.  
  
     **Message**  
     Indique les messages d'erreur ou d'avertissement retournés par le processus.  
  
     **Report**  
     Crée un rapport qui contient les résultats de l'Assistant Création de partition. Les options sont **Afficher le rapport**, **Enregistrer le rapport dans un fichier**, **Copier le rapport dans le Presse-papiers** et **Envoyer le rapport sous forme de courrier électronique**.  
  
     **Afficher le rapport**  
     Ouvre la boîte de dialogue **Afficher le rapport** , qui contient un rapport au format texte de la progression de l’Assistant Création de partition.  
  
     **Enregistrer le rapport dans un fichier**  
     Ouvre la boîte de dialogue **Enregistrer le rapport sous** .  
  
     **Copier le rapport dans le Presse-papiers**  
     Copie les résultats du rapport de progression de l’Assistant dans le Presse-papiers.  
  
     **Envoyer le rapport sous forme de courrier électronique**  
     Copie les résultats du rapport de progression de l’Assistant dans un e-mail.  
  
     Une fois terminé, cliquez sur **Fermer**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-disable-compression-on-a-table"></a>Pour désactiver la compression sur une table  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    ALTER TABLE Person.Person REBUILD PARTITION = ALL  
    WITH (DATA_COMPRESSION = NONE);  
    GO  
    ```  
  
#### <a name="to-disable-compression-on-an-index"></a>Pour désactiver la compression sur un index  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    ALTER INDEX AK_Person_rowguid ON Person.Person REBUILD PARTITION = ALL WITH (DATA_COMPRESSION = NONE);  
    GO  
    ```  
  
 Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md) et [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
  
