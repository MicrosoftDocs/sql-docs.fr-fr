---
description: Mise en route en exécutant l’Assistant Activer la base de données pour Stretch
title: Mise en route en exécutant l’Assistant Activer la base de données pour Stretch
ms.date: 08/05/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: quickstart
f1_keywords:
- sql13.swb.stretchwizard.f1
- sql13.swb.stretchwizard.createdatabasecredentials.f1
- sql13.swb.stretchwizard.selectdatabasetables.f1
- sql13.swb.stretchwizard.validatesqlserver.f1
- sql13.swb.stretchwizard.selectazuredeployment.f1
- sql13.swb.stretchwizard.configureazuredeployment.f1
- sql13.swb.stretchwizard.Summary.f1
- sql13.swb.stretchwizard.Results.f1
- sql13.swb.stretchwizard.introduction.f1
helpviewer_keywords:
- Stretch Database, wizard
- Enable Database for Stretch Wizard
ms.assetid: 855dd9fc-f80c-4dbc-bf46-55a9736bfe15
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: 74435746bc2ef72741cf25a610cb37d4ac1893b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080990"
---
# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Mise en route en exécutant l’Assistant Activer la base de données pour Stretch
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


 Pour configurer une base de données pour Stretch Database, exécutez l’Assistant Activer la base de données pour Stretch.  Cet article décrit les informations que vous devez entrer et les choix que vous avez à faire dans l’Assistant.  
  
 Pour en savoir plus sur Stretch Database, consultez [Stretch Database](../../sql-server/stretch-database/stretch-database.md). 
 
 > [!NOTE] 
 > Ultérieurement, n’oubliez pas que la désactivation de Stretch Database pour une table ou une base de données ne supprime pas l’objet distant. Si vous souhaitez supprimer la table distante ou la base de données distante, vous devez la supprimer à l'aide du portail de gestion Azure. Les objets distants continuent d’entraîner des coûts Azure tant qu’ils n’ont pas été supprimés manuellement. 
  
## <a name="launch-the-wizard"></a>Lancer l'Assistant  
  
1.  Dans SQL Server Management Studio, dans l'Explorateur d'objets, sélectionnez la base de données pour laquelle vous souhaitez activer Stretch.  
  
2.  Cliquez avec le bouton droit et sélectionnez **Tâches**, puis **Stretch**. Ensuite, sélectionnez **Activer** pour lancer l’Assistant.  
  
##  <a name="introduction"></a><a name="Intro"></a> Introduction  
 Passez en revue l'objectif de l'assistant et les conditions préalables.  
 
 Les conditions préalables importantes sont les suivantes.
 -   Vous devez être administrateur pour modifier les paramètres de base de données.
 -   Vous devez disposer d’un abonnement Microsoft Azure.
 -   Votre serveur SQL Server doit être en mesure de communiquer avec le serveur Azure distant.
  
 ![Page Introduction de l’Assistant Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-1.png "Page Introduction de l’Assistant Stretch Database")  
  
##  <a name="select-tables"></a><a name="Tables"></a> Sélectionner des tables  
 Sélectionnez les tables que vous souhaitez activer pour Stretch.  
 
Les tables qui contiennent un grand nombre de lignes apparaissent en haut de la liste triée. Avant d’afficher la liste des tables, l’Assistant analyse les tables à la recherche de types de données non pris en charge par Stretch Database. 
  
 ![Page Sélectionner des tables de l’Assistant Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-2.png "Page Sélectionner des tables de l’Assistant Stretch Database")  
  
|Colonne|Description|  
|------------|-----------------|  
|(sans titre)|Cochez la case de cette colonne pour activer la table sélectionnée pour Stretch.|  
|**Nom**|Spécifie le nom de la table dans la base de données.|  
|(sans titre)|Un symbole dans cette colonne peut représenter un avertissement qui ne vous empêche pas d’activer la table sélectionnée pour Stretch. Il peut également représenter un problème de blocage qui empêche l’activation de la table sélectionnée pour Stretch, par exemple, si la table utilise un type de données non pris en charge. Placez le curseur sur le symbole pour afficher plus d'informations dans une info-bulle. Pour plus d’informations, consultez [Limitations concernant Stretch Database](../../sql-server/stretch-database/limitations-for-stretch-database.md).|  
|**Étendu**|Indique si la table est déjà activée pour Stretch.|  
|**Migrer**|Vous pouvez migrer une table entière (**Table entière**) ou spécifier un filtre sur une colonne existante de la table. Si vous voulez utiliser une fonction de filtre différente pour sélectionner les lignes à migrer, exécutez l’instruction ALTER TABLE pour spécifier la fonction de filtre lorsque vous quittez l’Assistant. Pour plus d’informations sur la fonction de filtre, consultez [Sélectionner les lignes à migrer à l’aide d’une fonction de filtre](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md). Pour plus d’informations sur l’application de la fonction, consultez [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) ou [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).|  
|**Lignes**|Spécifie le nombre de lignes dans la table.|  
|**Taille (Ko)**|Spécifie la taille de la table en Ko.|  
  
## <a name="optionally-provide-a-row-filter"></a>Fournir un filtre de lignes (facultatif)  
 Si vous voulez fournir une fonction de filtre pour sélectionner les lignes à migrer, effectuez l’une des opérations suivantes dans la page **Sélectionner des tables** .  
  
1.  Dans la liste **Sélectionnez les tables à étendre** , cliquez sur **Table entière** sur la ligne correspondant à la table. La boîte de dialogue **Sélectionner les lignes à étendre** s’ouvre.  
  
     ![Définir un prédicat de filtre basé sur une date](../../sql-server/stretch-database/media/stretch-wizard-2a.png "Définir un prédicat de filtre basé sur une date")  
  
2.  Dans la boîte de dialogue **Sélectionner les lignes à étendre** , sélectionnez **Sélectionner des lignes**.  
  
3.  Dans le champ **Nom**, fournissez un nom pour la fonction de filtre.  
  
4.  Pour la clause **Where** , choisissez une colonne de la table, sélectionnez un opérateur, puis fournissez une valeur.  
  
5.  Cliquez sur **Vérifier** pour tester la fonction. Si la fonction renvoie des résultats de la table (autrement dit, s’il y a des lignes à migrer qui répondent à la condition), le test affiche **Succès**.  

> [!NOTE] 
> La zone de texte qui affiche la requête de filtre est en lecture seule. Vous ne pouvez pas modifier la requête dans la zone de texte.
  
6.  Cliquez sur Terminé pour revenir à la page **Sélectionner des tables** .  

La fonction de filtre n’est créée dans SQL Server qu’une fois l’Assistant terminé. Avant cela, vous pouvez revenir à la page **Sélectionner des tables** pour modifier ou renommer la fonction de filtre.

![Page Sélectionner des tables après la définition d’un prédicat de filtre](../../sql-server/stretch-database/media/stretch-wizard-2b.png "Page Sélectionner des tables après la définition d’un prédicat de filtre")

Si vous souhaitez utiliser un autre type de fonction de filtre pour sélectionner les lignes à migrer, effectuez l’une des opérations suivantes.  
  
-   Quittez l’Assistant et exécutez l’instruction ALTER TABLE pour activer Stretch pour la table et pour spécifier une fonction de filtre. Pour plus d’informations, consultez [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md).  
  
-   Exécutez l’instruction ALTER TABLE pour spécifier une fonction de filtre après avoir quitté l’Assistant. Pour connaître les étapes nécessaires, consultez [Ajouter une fonction de filtre après avoir exécuté l’Assistant](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md#addafterwiz).  
  
##  <a name="configure-azure"></a><a name="Configure"></a> Configuration d'Azure  
  
1.  Connectez-vous à Microsoft Azure avec un compte Microsoft.  
  
     ![Se connecter à Azure - Assistant Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-3.png "Se connecter à Azure - Assistant Stretch Database")  
  
2.  Sélectionnez l’abonnement Azure existant à utiliser pour Stretch Database. 

> [!NOTE] 
> Pour activer Stretch sur une base de données, vous devez disposer des droits d’administrateur sur l’abonnement que vous utilisez. L’assistant Stretch Database affiche uniquement les abonnements pour lesquels l’utilisateur dispose de droits d’administrateur.
  
3.  Sélectionnez la région Azure à utiliser pour Stretch Database.
    -   Si vous créez un serveur, il est créé dans cette région.  
    -   Si vous disposez de serveurs existants dans la région sélectionnée, l’Assistant les répertorie lorsque vous choisissez **Serveur existant**.
  
     Pour réduire la latence, choisissez la région Azure dans laquelle se trouve votre serveur SQL Server. Pour plus d'informations sur les régions, consultez [Régions Azure](https://azure.microsoft.com/regions/).  
  
4.  Spécifiez si vous souhaitez utiliser un serveur existant ou créer un nouveau serveur Azure.  
  
     Si Active Directory sur votre serveur SQL Server est fédéré avec Azure Active Directory, vous pouvez éventuellement utiliser un compte de service fédéré pour SQL Server afin de communiquer avec le serveur Azure distant. Pour plus d’informations sur la configuration requise pour cette option, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
    -   **Créer un serveur**  
  
        1.  Créez un nom de connexion et un mot de passe pour l'administrateur du serveur.  
  
        2.  Vous pouvez éventuellement utiliser un compte de service fédéré pour SQL Server afin de communiquer avec le serveur Azure distant.  
  
         ![Créer un serveur Azure - Assistant Stretch Database](../../relational-databases/tables/media/stretch-wizard-4.png "Créer un serveur Azure - Assistant Stretch Database")  
  
    -   **Serveur existant**  
  
        1.  Sélectionnez le serveur Azure existant.  
  
        2.  Sélectionnez la méthode d'authentification.  
  
            -   Si vous sélectionnez **Authentification SQL Server**, entrez un nom de connexion et un mot de passe d’administrateur.  
  
            -   Sélectionnez **Authentification intégrée Active Directory** pour utiliser un compte de service fédéré pour SQL Server afin de communiquer avec le serveur Azure distant. Si le serveur sélectionné n’est pas intégré à Azure Active Directory, cette option n’apparaît pas.
  
         ![Sélectionner un serveur Azure existant - Assistant Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-5.png "Sélectionner un serveur Azure existant - Assistant Stretch Database")  
  
##  <a name="secure-credentials"></a><a name="Credentials"></a> Informations d'identification sécurisées  
 Vous devez disposer d’une clé principale de base de données pour sécuriser les informations d’identification que Stretch Database utilise pour se connecter à la base de données distante.  
  
 Si une clé principale de base de données existe déjà, entrez son mot de passe.  
  
 ![Capture d’écran montrant la page Informations d’identification sécurisées de l’Assistant Stretch Database, avec la zone de mot de passe vide.](../../sql-server/stretch-database/media/stretch-wizard-6b.PNG "Page d’informations d’identification sécurisées de l’Assistant Stretch Database")  
  
 Si la base de données ne dispose pas d’une clé principale existante, entrez un mot de passe fort pour créer une clé principale de base de données.  
  
 ![Capture d’écran montrant la page Informations d’identification sécurisées de l’Assistant Stretch Database, avec les champs Nouveau mot de passe et Confirmer le mot de passe renseignés.](../../relational-databases/tables/media/stretch-wizard-6.png "Page d’informations d’identification sécurisées de l’Assistant Stretch Database")  
  
 Pour plus d’informations sur la clé principale de base de données, consultez [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md) et [Créer une clé principale de base de données](../../relational-databases/security/encryption/create-a-database-master-key.md). Pour plus d’informations sur les informations d’identification créées par l’Assistant, consultez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).  
  
##  <a name="select-ip-address"></a><a name="Network"></a> Sélectionner l'adresse IP  
 Utilisez la plage d’adresses IP de sous-réseau (recommandé), ou l’adresse IP publique de votre serveur SQL Server, pour créer une règle de pare-feu sur Azure qui permette à SQL Server de communiquer avec le serveur Azure distant.  
  
 La ou les adresses IP que vous fournissez sur cette page indiquent au serveur Azure qu’il doit autoriser les données entrantes, les requêtes et les opérations de gestion générées par SQL Server à franchir le pare-feu Azure. L'Assistant ne modifie en rien les paramètres du pare-feu sur le serveur SQL Server.  
  
 ![Page de sélection de l’adresse IP de l’Assistant Stretch Database](../../relational-databases/tables/media/stretch-wizard-7.png "Page de sélection de l’adresse IP de l’Assistant Stretch Database")  
  
##  <a name="summary"></a><a name="Summary"></a> Résumé  
 Passez en revue les valeurs que vous avez entrées et les options que vous avez sélectionnées dans l'Assistant, ainsi que les coûts estimés sur Azure. Puis sélectionnez **Terminer** pour activer Stretch.  
  
 ![Page Récapitulatif de l’Assistant Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-8.png "Page Récapitulatif de l’Assistant Stretch Database")  
  
##  <a name="results"></a><a name="Results"></a> Résultats  
 Examinez les résultats.  
  
 Pour surveiller l’état de la migration de données, consultez [Surveiller et résoudre les problèmes de migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
 ![Page Résultats de l’Assistant Stretch Database](../../sql-server/stretch-database/media/stretch-wizard-9.PNG "Page Résultats de l’Assistant Stretch Database")  
  
##  <a name="troubleshooting-the-wizard"></a><a name="KnownIssues"></a> Résolution des problèmes liés à l'Assistant  
 **Échec de l’Assistant Stretch Database.**  
 Si Stretch Database n'est pas encore activé au niveau du serveur et que vous exécutez l'Assistant sans les autorisations d'administrateur du système nécessaires pour l'activer, l'Assistant échoue. Demandez à l'administrateur système d’activer Stretch Database sur l'instance de serveur local, puis exécutez à nouveau l'Assistant. Pour plus d'informations, consultez [Condition préalable : Autorisation d’activer Stretch Database sur le serveur](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer).  
  
## <a name="next-steps"></a>Étapes suivantes  
 Activer des tables supplémentaires pour Stretch Database. Surveiller la migration des données et gérez les tables et les bases de données Stretch.  
  
-   [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) afin d’activer des tables supplémentaires.  
  
-   Pour connaître l’état de la migration de données, consultez [Surveiller et résoudre les problèmes de migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/monitor-and-troubleshoot-data-migration-stretch-database.md).  
  
-   [Suspension et reprise de la migration de données &#40;Stretch Database&#41;](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md)  
  
-   [Gérer et dépanner Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)  
  
-   [Sauvegarder des bases de données Stretch](../../sql-server/stretch-database/backup-stretch-enabled-databases-stretch-database.md)  
  
-   [Restaurer des bases de données Stretch](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Activer Stretch Database pour une base de données](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md)   
 [Activer Stretch Database pour une table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md)  
  
  
