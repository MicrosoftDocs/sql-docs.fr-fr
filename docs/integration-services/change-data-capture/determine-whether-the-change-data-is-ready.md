---
description: Déterminer si les données modifiées sont prêtes
title: Déterminer si les données modifiées sont prêtes | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- incremental load [Integration Services],determining readiness
ms.assetid: 04935f35-96cc-4d70-a250-0fd326f8daff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9fd82f56cb1edb459e662e31778fcb8341a6887c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353030"
---
# <a name="determine-whether-the-change-data-is-ready"></a>Déterminer si les données modifiées sont prêtes

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Dans le flux de contrôle d’un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui effectue une charge incrémentielle des données modifiées, la deuxième tâche consiste à s’assurer que les données modifiées pour l’intervalle sélectionné sont prêtes. Cette étape est nécessaire car le processus de capture asynchrone n'a peut-être pas encore pu traiter toutes les modifications jusqu'au point de terminaison sélectionné.  
  
> [!NOTE]  
>  La première tâche pour le flux de contrôle consiste à calculer les points de terminaison de l'intervalle de modification. Pour plus d’informations sur cette tâche, consultez [Spécifier un intervalle de données modifiées](../../integration-services/change-data-capture/specify-an-interval-of-change-data.md). Pour obtenir une description du processus d’ensemble de la conception du flux de contrôle, consultez [Capture de données modifiées &#40;SSIS&#41;](../../integration-services/change-data-capture/change-data-capture-ssis.md).  
  
## <a name="understanding-the-components-of-the-solution"></a>Présentation des composants de la solution  
 La solution décrite dans cette rubrique utilise quatre composants [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] :  
  
-   Un conteneur de boucles For qui évalue à plusieurs reprises la sortie d'une tâche d'exécution SQL.  
  
-   Une tâche d'exécution SQL qui interroge les tables spéciales gérées par le processus de capture de données modifiées, puis utilise ces informations pour déterminer si les données sont prêtes.  
  
-   Un composant qui implémente un délai de traitement lorsque les données ne sont pas prêtes. Il peut s'agir d'une tâche de script ou d'une tâche d'exécution SQL.  
  
-   En option, un composant qui signale une erreur ou un délai d'attente lorsque la tâche d'exécution SQL retourne une valeur qui indique une erreur ou une condition de délai d'attente.  
  
 Ces composants définissent ou lisent les valeurs de plusieurs variables de package pour contrôler le flux d'exécution à l'intérieur de la boucle et, ultérieurement, dans le package.  
  
#### <a name="to-set-up-package-variables"></a>Pour configurer des variables de package  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], dans la fenêtre **Variables** , créez les variables suivantes :  
  
    1.  Créez une variable du type de données integer pour contenir la valeur d'état retournée par la tâche d'exécution SQL.  
  
         Cet exemple utilise la variable nommée DataReady avec une valeur initiale de 0.  
  
    2.  Créez une variable pour contenir la durée du délai lorsque les données ne sont pas prêtes. Si vous prévoyez d'utiliser une tâche de script pour implémenter le délai, la variable doit être du type de données integer. Si vous prévoyez d'utiliser une tâche d'exécution SQL avec une instruction WAITFOR, la variable doit être du type de données string pour accepter des valeurs telles que « 00:00:10 ».  
  
         Cet exemple utilise la variable nommée DelaySeconds avec une valeur initiale de 10.  
  
    3.  Créez une variable du type de données integer pour contenir l'itération actuelle de la boucle.  
  
         Cet exemple utilise la variable nommée TimeoutCount avec une valeur initiale de 0.  
  
    4.  Créez une variable du type de données integer pour spécifier le nombre de fois que la boucle doit tester les données avant de signaler une condition de délai d'attente.  
  
         Cet exemple utilise la variable nommée TimeoutCeiling avec une valeur initiale de 20.  
  
    5.  (Facultatif) Créez une variable du type de données integer que vous pouvez utiliser pour indiquer le premier chargement des données modifiées.  
  
         Cet exemple utilise la variable nommée IntervalID et vérifie uniquement l'existence de la valeur 0 pour indiquer le chargement initial.  
  
## <a name="configuring-a-for-loop-container"></a>Configuration d'un conteneur de boucles For  
 Une fois les variables définies, le conteneur de boucles For est le premier composant à être ajouté.  
  
#### <a name="to-configure-a-for-loop-container-to-wait-until-change-data-is-ready"></a>Pour configurer un conteneur de boucles For pour attendre que les données modifiées soient prêtes  
  
1.  Sous l’onglet **Flux de contrôle** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , ajoutez un conteneur de boucles For au flux de contrôle.  
  
2.  Connectez la tâche d'exécution SQL qui calcule les points de terminaison de l'intervalle au conteneur de boucles For.  
  
3.  Dans **l’Éditeur de boucle For**, sélectionnez les options suivantes :  
  
    1.  Pour **InitExpression**, entrez `@DataReady = 0`.  
  
         Cette expression définit la valeur initiale de la variable de boucle.  
  
    2.  Pour **EvalExpression** m, entrez `@DataReady == 0`.  
  
         Quand cette expression renvoie **False**, l’exécution sort de la boucle et la charge incrémentielle démarre.  
  
## <a name="configuring-the-execute-sql-task-that-queries-for-change-data"></a>Configuration de la tâche d'exécution SQL qui recherche les données modifiées  
 Dans le conteneur de boucles For, vous ajoutez une tâche d'exécution SQL. Cette tâche interroge les tables que le processus de capture de données modifiées gère dans la base de données. Le résultat de cette requête est une valeur d'état qui indique si les données modifiées sont prêtes.  
  
 Dans la table suivante, la première colonne montre les valeurs retournées de la tâche d'exécution SQL par l'exemple de requête Transact-SQL. La deuxième colonne montre comment les autres composants répondent à ces valeurs.  
  
|Valeur renvoyée|Signification|response|  
|------------------|-------------|--------------|  
|0|Indique que les données modifiées ne sont pas prêtes.<br /><br /> Il n'existe aucun enregistrement de capture de données modifiées postérieur au point de fin de l'intervalle sélectionné.|L'exécution se poursuit avec le composant qui implémente un délai. Le contrôle retourne ensuite au conteneur de boucles For qui continue à vérifier la tâche d'exécution SQL tant que la valeur retournée est 0.|  
|1|Peut indiquer que les données modifiées n'ont pas été capturées pour l'intervalle complet ou qu'elles ont été supprimées. Cela est traité comme une condition d'erreur.<br /><br /> Il n'existe aucun enregistrement de capture de données modifiées antérieur au point de départ de l'intervalle sélectionné.|L'exécution se poursuit avec le composant facultatif qui enregistre l'erreur.|  
|2|Indique que les données sont prêtes.<br /><br /> Il existe des enregistrements de capture de données modifiées qui sont antérieurs au point de départ et postérieurs au point de fin de l'intervalle sélectionné.|L'exécution sort du conteneur de boucles For et le chargement incrémentiel démarre.|  
|3|Indique le chargement initial de toutes les données modifiées disponibles.<br /><br /> La logique conditionnelle obtient cette valeur à partir d'une variable de package spéciale qui est utilisée uniquement dans ce but.|L'exécution sort du conteneur de boucles For et le chargement incrémentiel démarre.|  
|5|Indique que la variable TimeoutCeiling a été atteinte.<br /><br /> La boucle a testé les données le nombre spécifié de fois et les données ne sont toujours pas disponibles. Sans ce test ou un test semblable, le package peut s'exécuter indéfiniment.|L'exécution se poursuit avec le composant facultatif qui enregistre le délai d'attente.|  
  
#### <a name="to-configure-an-execute-sql-task-to-query-whether-change-data-is-ready"></a>Pour configurer une tâche d'exécution SQL pour déterminer si les données modifiées sont prêtes  
  
1.  Dans le conteneur de boucles For, ajoutez une tâche d'exécution SQL.  
  
2.  Dans **l’Éditeur de tâche d’exécution de requêtes SQL**, dans la page **Général** , sélectionnez les options suivantes :  
  
    1.  Pour **ResultSet**, sélectionnez **Ligne unique**.  
  
    2.  Configurez une connexion valide à la base de données source.  
  
    3.  Pour **SQLSourceType**, sélectionnez **Entrée directe**.  
  
    4.  Pour **SQLStatement**, entrez l’instruction SQL suivante :  
  
        ```  
        declare @DataReady int, @TimeoutCount int  
  
        if not exists (select tran_end_time from cdc.lsn_time_mapping  
                where tran_end_time > ?  )  
            select @DataReady = 0  
        else  
            if ? = 0  
                select @DataReady = 3   
        else  
            if not exists (select tran_end_time from cdc.lsn_time_mapping  
                    where tran_end_time <= ? )  
                select @DataReady = 1   
        else  
            select @DataReady = 2  
  
        select @TimeoutCount = ?  
        if (@DataReady = 0)  
            select @TimeoutCount = @TimeoutCount + 1  
        else  
            select @TimeoutCount = 0  
  
        if (@TimeoutCount > ?)  
            select @DataReady = 5  
  
        select @DataReady as DataReady, @TimeoutCount as TimeoutCount  
  
        ```  
  
3.  Dans la page **Mappage de paramètre** de **l’Éditeur de tâche d’exécution de requêtes SQL**, effectuez les mappages suivants :  
  
    1.  Mappez la variable ExtractEndTime au paramètre 0.  
  
    2.  Mappez la variable IntervalID au paramètre 1.  
  
    3.  Mappez la variable ExtractStartTime au paramètre 2.  
  
    4.  Mappez la variable TimeoutCount au paramètre 3.  
  
    5.  Mappez la variable TimeoutCeiling au paramètre 4.  
  
4.  Dans la page **Ensemble de résultats** de **Éditeur de tâche d’exécution de requêtes SQL**, mappez le résultat de DataReady à la variable DataReady, et le résultat de TimeoutCount à la variable TimeoutCount.  
  
## <a name="waiting-until-the-change-data-is-ready"></a>Attendre jusqu'à ce que les données modifiées soient prêtes  
 Vous pouvez utiliser l'une des diverses méthodes disponibles pour implémenter un délai lorsque les données modifiées ne sont pas prêtes. Les deux procédures suivantes illustrent comment utiliser une tâche de script ou une tâche d'exécution SQL pour implémenter le délai.  
  
> [!NOTE]  
>  Un script précompilé nécessite un temps de traitement inférieur à une tâche d'exécution SQL.  
  
#### <a name="to-implement-a-delay-by-using-a-script-task"></a>Pour implémenter un délai en utilisant une tâche de script  
  
1.  Dans le conteneur de boucles For, ajoutez une tâche de script.  
  
2.  Connectez la tâche d'exécution SQL utilisée pour déterminer si les données modifiées sont prêtes à la nouvelle tâche de script.  
  
3.  Pour la contrainte de précédence qui connecte la tâche d’exécution de requêtes SQL à la tâche de script, ouvrez **Éditeur de contrainte de précédence** , puis sélectionnez les options suivantes :  
  
    1.  Pour **Opération d’évaluation**, sélectionnez **Expression et contrainte**.  
  
    2.  Pour **Valeur**, sélectionnez **Succès**.  
  
         La valeur de contrainte **Succès** fait référence au succès de la tâche précédente. Dans le cas présent, il s’agit du succès de la tâche d’exécution de requêtes SQL.  
  
    3.  Pour **Expression**, entrez `@DataReady == 0 && @TimeoutCount <= @TimeoutCeiling`.  
  
    4.  Sélectionnez **Opérateur logique AND. Toutes les contraintes doivent avoir la valeur True**, si ce n’est déjà fait.  
  
4.  Dans **l’Éditeur de tâche de script**, dans la page **Script** , pour **ReadOnlyVariables**, sélectionnez la variable de type entier **User::DelaySeconds** dans la liste.  
  
5.  Dans **l’Éditeur de tâche de script**, dans la page **Script** , cliquez sur **Modifier le script** pour ouvrir l’environnement de développement de script.  
  
6.  Dans la procédure Main, entrez l'une des lignes de code suivantes :  
  
    -   Si vous programmez en C#, entrez la ligne de code suivante :  
  
        ```  
        System.Threading.Thread.Sleep((int)Dts.Variables["DelaySeconds"].Value * 1000);  
        ```  
  
         \- ou -  
  
    -   Si vous programmez en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], entrez la ligne de code suivante :  
  
        ```  
        System.Threading.Thread.Sleep(Ctype(Dts.Variables("DelaySeconds").Value, Integer) * 1000)  
  
        ```  
  
        > [!NOTE]  
        >  La méthode **Thread.Sleep** attend un argument spécifié en millisecondes.  
  
7.  Laissez la ligne de code par défaut qui retourne **DtsExecResult.Success** suite à l’exécution du script.  
  
8.  Fermez l’environnement de développement de script et **l’Éditeur de tâche de script**.  
  
#### <a name="to-implement-a-delay-by-using-an-execute-sql-task"></a>Pour implémenter un délai en utilisant une tâche d'exécution SQL  
  
1.  Dans le conteneur de boucles For, ajoutez une tâche d'exécution SQL.  
  
2.  Connectez la tâche d'exécution SQL utilisée pour déterminer si les données modifiées sont prêtes à la nouvelle tâche d'exécution SQL.  
  
3.  Pour la contrainte de précédence qui connecte les deux tâches d’exécution de requêtes SQL, ouvrez l’ **Éditeur de contrainte de précédence** sélectionnez les options suivantes :  
  
    1.  Pour **Opération d’évaluation**, sélectionnez **Expression et contrainte**.  
  
    2.  Pour **Valeur**, sélectionnez **Succès**.  
  
         La valeur de contrainte **Succès** fait référence au succès de la tâche d’exécution de requêtes SQL précédente.  
  
    3.  Pour **Expression**, entrez `@DataReady == 0`.  
  
    4.  Sélectionnez **Opérateur logique AND. Toutes les contraintes doivent avoir la valeur True**, si ce n’est déjà fait.  
  
         Cette sélection requiert que les deux conditions, la contrainte et l'expression, soient vraies.  
  
4.  Dans **l’Éditeur de tâche d’exécution de requêtes SQL**, dans la page **Général** , sélectionnez les options suivantes :  
  
    1.  Pour **ResultSet**, sélectionnez **Ligne unique**.  
  
    2.  Configurez une connexion valide à la base de données source.  
  
    3.  Pour **SQLSourceType**, sélectionnez **Entrée directe**.  
  
    4.  Pour **SQLStatement**, entrez l’instruction SQL suivante :  
  
        ```  
        WAITFOR DELAY ?  
  
        ```  
  
5.  Dans la page **Mappage de paramètre** de l’éditeur, mappez la variable de type chaîne DelaySeconds au paramètre 0.  
  
## <a name="handling-an-error-condition"></a>Traitement d'une condition d'erreur  
 Vous pouvez éventuellement configurer un composant supplémentaire dans la boucle pour enregistrer une erreur ou une condition de délai d'attente :  
  
-   Ce composant peut enregistrer une condition d'erreur lorsque la variable DataReady a pour valeur 1. Cette valeur indique qu'il n'y a pas de données modifiées de disponibles avant le début de l'intervalle sélectionné.  
  
-   Ce composant peut également enregistrer une condition de délai d'attente lorsque la valeur de la variable TimeoutCeiling est atteinte. Cette valeur indique que la boucle a testé les données le nombre spécifié de fois et que les données ne sont toujours pas disponibles. Sans ce test ou un test semblable, le package peut s'exécuter indéfiniment.  
  
#### <a name="to-configure-an-optional-script-task-to-log-an-error-condition"></a>Pour configurer une tâche de script facultative pour enregistrer une condition d'erreur  
  
1.  Si vous souhaitez signaler l'erreur ou le délai d'attente en écrivant un message dans le journal, configurez l'enregistrement pour le package. Pour plus d’informations, consultez [Activer la journalisation des packages dans les outils de données SQL Server](../../integration-services/performance/integration-services-ssis-logging.md#ssdt).  
  
2.  Dans le conteneur de boucles For, ajoutez une tâche de script.  
  
3.  Connectez la tâche d'exécution SQL utilisée pour déterminer si les données modifiées sont prêtes à la nouvelle tâche de script.  
  
4.  Pour la contrainte de précédence qui connecte la tâche d’exécution de requêtes SQL à la tâche de script, ouvrez **Éditeur de contrainte de précédence** , puis sélectionnez les options suivantes :  
  
    1.  Pour **Opération d’évaluation**, sélectionnez **Expression et contrainte**.  
  
    2.  Pour **Valeur**, sélectionnez **Succès**.  
  
         La valeur de contrainte **Succès** fait référence au succès de la tâche précédente. Dans le cas présent, il s’agit du succès de la tâche d’exécution de requêtes SQL.  
  
    3.  Pour **Expression**, entrez `@DataReady == 1 || @DataReady == 5`.  
  
    4.  Sélectionnez **Opérateur logique AND. Toutes les contraintes doivent avoir la valeur True**, si ce n’est déjà fait.  
  
         Cette sélection requiert que les deux conditions, la contrainte et l'expression, soient vraies.  
  
5.  Dans **l’Éditeur de tâche de script**, dans la page **Script** de l’éditeur, pour **ReadOnlyVariables**, sélectionnez **User::DataReady** et **User::ExtractStartTime** dans la liste pour rendre leurs valeurs disponibles pour le script.  
  
     Si vous souhaitez inclure les informations de certaines variables système (par exemple, System::PackageName) dans les informations que vous écrivez dans le journal, sélectionnez également ces variables.  
  
6.  Dans **l’Éditeur de tâche de script**, dans la page **Script** , cliquez sur **Modifier le script** pour ouvrir l’environnement de développement de script.  
  
7.  Dans la procédure Main, entrez le code soit pour enregistrer une erreur en appelant la méthode **Dts.Log** , soit pour déclencher un événement en appelant l’une des méthodes de l’interface **Dts.Events** . Informez le package de l'erreur en retournant `Dts.TaskResult = Dts.Results.Failure`.  
  
     L'exemple suivant montre comment écrire un message dans le journal. Pour plus d’informations, consultez [Journalisation dans la tâche de script](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md), [Déclenchement d’événements dans la tâche de script](../../integration-services/extending-packages-scripting/task/raising-events-in-the-script-task.md)et [Retour de résultats de la tâche de script](../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md).  
  
    ```  
    ' User variables.  
    Dim dataReady As Integer = _  
      CType(Dts.Variables("DataReady").Value, Integer)  
    Dim extractStartTime As Date = _  
      CType(Dts.Variables("ExtractStartTime").Value, DateTime)  
  
    ' System variables.  
    Dim packageName As String = _  
      Dts.Variables("PackageName").Value.ToString()  
    Dim executionStartTime As Date = _  
      CType(Dts.Variables("StartTime").Value, DateTime)  
  
    Dim eventMessage As New System.Text.StringBuilder()  
  
    If dataReady = 1 OrElse dataReady = 5 Then  
  
      If dataReady = 1 Then  
        eventMessage.AppendLine("Start Time Error")  
      Else  
        eventMessage.AppendLine("Timeout Error")  
      End If  
  
      With eventMessage  
        .Append("The package ")  
        .Append(packageName)  
        .Append(" started at ")  
        .Append(executionStartTime.ToString())  
        .Append(" and ended at ")  
        .AppendLine(DateTime.Now().ToString())  
        If dataReady = 1 Then  
          .Append("The specified ExtractStartTime was ")  
          .AppendLine(extractStartTime.ToString())  
        End If  
      End With  
  
      System.Windows.Forms.MessageBox.Show(eventMessage.ToString())  
  
      Dts.Log(eventMessage.ToString(), 0, Nothing)  
  
      Dts.TaskResult = Dts.Results.Failure  
  
    Else  
  
      Dts.TaskResult = Dts.Results.Success  
  
    End If  
  
    ```  
  
8.  Fermez l’environnement de développement de script et **l’Éditeur de tâche de script**.  
  
## <a name="next-step"></a>étape suivante  
 Après avoir déterminé que les données modifiées sont prêtes, l'étape suivante consiste à préparer la recherche des données modifiées.  
  
 **Rubrique suivante :** [Préparer la recherche des données modifiées](../../integration-services/change-data-capture/prepare-to-query-for-the-change-data.md)  
  
  
