---
title: Exécuter une procédure stockée | Microsoft Docs
description: Découvrez comment exécuter une procédure stockée dans SQL Server 2019 (15.x) à l’aide de SQL Server Management Studio ou de Transact-SQL.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
f1_keywords:
- sql13.swb.executeprocedure.general.f1
- sql13.swb.executeprocedure.f1
helpviewer_keywords:
- stored procedures [SQL Server], parameters
- extended stored procedures [SQL Server], executing
- system stored procedures [SQL Server], executing
- stored procedures [SQL Server], executing
- user-defined stored procedures [SQL Server]
ms.assetid: a0b1337d-2059-4872-8c62-3f967d8b170f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 115220ef0fabbe98a0c4a1bf5f185409c5a82829
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250266"
---
# <a name="execute-a-stored-procedure"></a>Exécuter une procédure stockée
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette rubrique explique comment exécuter une procédure stockée dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Il existe deux façons différentes d'exécuter une procédure stockée. La première approche, et aussi la plus courante, est qu'une application ou un utilisateur appelle la procédure. La deuxième méthode consiste à définir la procédure pour qu'elle s'exécute automatiquement lorsqu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre. Lorsqu'une procédure est appelée par une application ou un utilisateur, le mot clé [!INCLUDE[tsql](../../includes/tsql-md.md)] EXECUTE ou EXEC est explicitement établi dans l'appel. Sinon, la procédure peut être appelée et exécutée sans le mot clé si elle est la première instruction dans le traitement [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour exécuter une procédure stockée à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   Le classement de la base de données d'appel est utilisé pour mettre en correspondance les noms des procédures système. Par conséquent, utilisez systématiquement la casse exacte des noms des procédures système dans vos appels de procédure. Par exemple, le code suivant ne fonctionnera pas s'il est exécuté dans le contexte d'une base de données dotée d'un classement qui respecte la casse :  
  
    ```sql  
    EXEC SP_heLP; -- Will fail to resolve because SP_heLP does not equal sp_help  
    ```  
  
     Pour afficher les noms exacts des procédures système, interrogez les affichages catalogue [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md) et [sys.system_parameters](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md) .  
  
-   Si une procédure définie par l'utilisateur a le même nom qu'une procédure système, elle peut ne jamais s'exécuter.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Exécution de procédures stockées système  
  
     Les procédures système commencent par le préfixe **sp_** . Étant donné qu'elles figurent logiquement dans toutes les bases de données d'utilisateur et les bases de données définies par le système, elles peuvent être exécutées à partir de n'importe quelle base de données sans devoir qualifier entièrement le nom de la procédure. Cependant, nous vous conseillons de qualifier tous les noms de procédures système à l’aide du nom de schéma **sys** pour éviter les conflits de nom. L'exemple suivant illustre la méthode recommandée pour l'appel d'une procédure système.  
  
    ```sql  
    EXEC sys.sp_who;  
    ```  
  
-   Exécution de procédures stockées définies par l'utilisateur  
  
     En exécutant une procédure définie par l'utilisateur, il est recommandé de qualifier le nom de la procédure avec le nom du schéma. Cette pratique améliore légèrement les performances car le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'a pas à rechercher dans plusieurs schémas. Elle évite également d'exécuter la procédure incorrecte si une base de données a des procédures de même nom dans plusieurs schémas.  
  
     L'exemple suivant illustre la méthode recommandée pour l'exécution d'une procédure définie par l'utilisateur. Notez que la procédure accepte un paramètre d'entrée. Pour plus d’informations sur la spécification des paramètres d’entrée et de sortie, consultez [Spécifier les paramètres](../../relational-databases/stored-procedures/specify-parameters.md).  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    EXEC dbo.uspGetEmployeeManagers @BusinessEntityID = 50;  
    ```  
  
     -Ou-  
  
    ```sql  
    EXEC AdventureWorks2012.dbo.uspGetEmployeeManagers 50;  
    GO  
    ```  
  
     Si une procédure non qualifiée définie par l'utilisateur est spécifiée, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] la recherche dans l'ordre suivant :  
  
    1.  Schéma **sys** de la base de données active.  
  
    2.  Schéma par défaut de l'appelant s'il est exécuté dans un traitement ou en SQL dynamique, ou bien, si le nom non qualifié de la procédure apparaît dans le corps d'une autre définition de procédure, le schéma contenant cette autre procédure est recherché par la suite.  
  
    3.  Schéma **dbo** dans la base de données active.  
  
-   Exécution automatique des procédures stockées  
  
     Les procédures marquées pour l'exécution automatique sont exécutées à chaque fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] démarre et que la base de données **master** est récupérée pendant le processus de démarrage. Configurer des procédures pour qu'elles s'exécutent automatiquement peut être utile pour effectuer des opérations de maintenance de base de données ou pour exécuter les procédures en continu en tant que processus d'arrière-plan. Cette solution revêt également de l'importance dans le cas de procédures exécutant des tâches système ou de maintenance dans **tempdb**, par exemple la création d'une table temporaire globale. De cette façon, la table temporaire existe toujours après que **tempdb** est recréée durant le démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Une procédure exécutée automatiquement opère avec les mêmes droits que les membres du rôle de serveur **sysadmin** . Les messages d'erreur produits par une procédure sont enregistrés dans le journal des erreurs de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Le nombre de procédures au démarrage est illimité, mais gardez à l'esprit que chacune utilise un thread de travail lors de son exécution. Si vous devez exécuter plusieurs procédures au démarrage mais si vous ne devez pas les exécuter de manière parallèle, utilisez comme procédure de démarrage une des procédures qui appellera les autres. Cette démarche permet de n'utiliser qu'un seul thread de travail.  
  
    > [!TIP]  
    >  Une procédure en exécution automatique ne retourne pas de jeu de résultats. En effet, la procédure est exécutée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et non par une application ou un utilisateur et il n'y a pas d'emplacement où envoyer le jeu de résultats.  
  
-   Définition, désactivation et contrôle de l'exécution automatique  
  
     Seul l’administrateur système (**sa**) peut marquer une procédure de sorte qu’elle s’exécute automatiquement. En outre, la procédure doit se trouver dans la base de données **master** , être la propriété de **sa** et ne pas posséder de paramètres d'entrée ou de sortie.  
  
     Utilisez [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) pour :  
  
    1.  désigner une procédure existante comme procédure de démarrage ;  
  
    2.  supprimer l'exécution automatique d'une procédure au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Pour plus d’informations, consultez [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md) et [Clause EXECUTE AS &#40;Transact-SQL& #41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Pour plus d’informations, consultez la section « Autorisations » dans [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-execute-a-stored-procedure"></a>Pour exécuter une procédure stockée  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], développez cette instance, puis développez **Bases de données**.  
  
2.  Développez la base de données que vous souhaitez, développez **Programmabilité**, puis développez **Procédures stockées**.  
  
3.  Cliquez avec le bouton droit sur la procédure stockée définie par l’utilisateur à executer, puis cliquez sur **Exécuter la procédure stockée**.  
  
4.  Dans la boîte de dialogue **Exécuter la procédure** , entrez une valeur pour chaque paramètre et indiquez si le paramètre doit passer une valeur Null.  
  
     **Paramètre**  
     Indique le nom du paramètre.  
  
     **Type de données**  
     Indique le type de données du paramètre.  
  
     **Paramètre de sortie**  
     Indique si le paramètre est un paramètre de sortie.  
  
     **Passer les valeurs de type NULL**  
     Permet le passage d'une valeur NULL en tant que valeur du paramètre.  
  
     **Valeur**  
     Tapez la valeur du paramètre lors de l'appel de la procédure.  
  
5.  Pour exécuter la procédure stockée, cliquez sur **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-execute-a-stored-procedure"></a>Pour exécuter une procédure stockée  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment exécuter une procédure stockée qui attend un seul paramètre. L'exemple exécute la procédure stockée `uspGetEmployeeManagers` avec la valeur  `6` spécifiée pour le paramètre `@EmployeeID` .  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC dbo.uspGetEmployeeManagers 6;  
GO  
```  
  
#### <a name="to-set-or-clear-a-procedure-for-executing-automatically"></a>Pour valider ou désactiver l'exécution automatique d'une procédure  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) pour définir l’exécution automatique d’une procédure.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_procoption @ProcName = '<procedure name>'   
    , @OptionName = 'startup'   
    , @OptionValue = 'on';  
```  
  
#### <a name="to-stop-a-procedure-from-executing-automatically"></a>Pour arrêter l'exécution automatique d'une procédure  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_procoption](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md) pour arrêter l’exécution automatique d’une procédure.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_procoption @ProcName = '<procedure name>'   
    , @OptionValue = 'off';  
```  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Exemple (Transact-SQL)  
  
## <a name="see-also"></a>Voir aussi  
 [Spécifier les paramètres](../../relational-databases/stored-procedures/specify-parameters.md)   
 [Configurer l'option de configuration du serveur scan for startup procs](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Procédures stockées &#40;moteur de base de données &#41;](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)  
  
  
