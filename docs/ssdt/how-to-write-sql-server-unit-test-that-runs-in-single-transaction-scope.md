---
title: Écrire un test unitaire SQL Server qui s'exécute sur l'étendue d'une seule transaction
description: Apprenez à démarrer le service Distributed Transaction Coordinator, à écrire un test unitaire de SQL Server à transaction unique et à restaurer les modifications de test.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: cb241e94-d81c-40e9-a7ae-127762a6b855
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 95b91ffe542b4085dcdeca9f3e0dcfc4928b6564
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597014"
---
# <a name="how-to-write-a-sql-server-unit-test-that-runs-within-the-scope-of-a-single-transaction"></a>Procédure : Écrire un test unitaire SQL Server qui s'exécute dans l'étendue d'une seule transaction

Modifiez les tests unitaires de façon à ce qu'ils s'exécutent dans l'étendue d'une transaction. Si vous adoptez cette approche, vous pouvez restaurer les modifications apportées par le test une fois le test terminé. Les procédures suivantes expliquent comment effectuer les tâches suivantes :  
  
-   Créer une transaction dans votre script de test Transact\-SQL qui utilise **BEGIN TRANSACTION** et **ROLLBACK TRANSACTION**.  
  
-   Créer une transaction pour une seule méthode de test dans une classe de test.  
  
-   Créer une transaction pour toutes les méthodes de test dans une classe de test donnée.  
  
**Composants requis**  
  
Pour certaines procédures de cette rubrique, le service Distributed Transaction Coordinator doit être en cours d'exécution sur l'ordinateur sur lequel vous exécutez des tests unitaires. Pour plus d'informations, consultez la procédure figurant à la fin de cette rubrique.  
  
## <a name="to-create-a-transaction-using-transact-sql"></a>Pour créer une transaction à l'aide de Transact\-SQL  
  
#### <a name="to-create-a-transaction-using-transact-sql"></a>Pour créer une transaction à l'aide de Transact\-SQL  
  
1.  Ouvrez un test unitaire dans le Concepteur de test unitaire SQL Server. (Double-cliquez sur le fichier de code source du test unitaire pour afficher le concepteur.)  
  
2.  Spécifiez le type de script pour lequel vous souhaitez créer la transaction. Par exemple, vous pouvez spécifier le prétest, le test ou le post-test.  
  
3.  Entrez un script de test dans l'éditeur Transact\-SQL.  
  
4.  Insérez les instructions `BEGIN TRANSACTION` et `ROLLBACK TRANSACTION`, comme l'illustre cet exemple simple. L'exemple utilise une table de base de données appelée OrderDetails et qui contient 50 lignes de données :  
  
    ```  
    BEGIN TRANSACTION TestTransaction  
    UPDATE "OrderDetails" set Quantity = Quantity + 10  
    IF @@ROWCOUNT!=50  
    RAISERROR('Row count does not equal 50',16,1)  
    ROLLBACK TRANSACTION TestTransaction  
    ```  
  
    > [!NOTE]  
    > Vous ne pouvez pas restaurer une transaction après l'exécution d'une instruction COMMIT TRANSACTION.  
  
    Pour plus d'informations sur le fonctionnement de l'instruction ROLLBACK TRANSACTION avec les procédures stockées et les déclencheurs, consultez cette page sur le site Web Microsoft : [ROLLBACK TRANSACTION (Transact-SQL)](../t-sql/language-elements/rollback-transaction-transact-sql.md).  
  
## <a name="to-create-a-transaction-for-a-single-test-method"></a>Pour créer une transaction pour une seule méthode de test  
Dans cet exemple, vous utilisez une transaction ambiante lorsque vous utilisez le type [System.Transactions.TransactionScope](/dotnet/api/system.transactions.transactionscope). Par défaut, les connexions d'exécution et privilégiées n'utilisent pas la transaction ambiante, car les connexions ont été créées avant que la méthode ne soit exécutée. SqlConnection possède une méthode [System.Data.SqlClient.SqlConnection.EnlistTransaction](/dotnet/api/system.data.sqlclient.sqlconnection.enlisttransaction), qui associe une connexion active à une transaction. Lorsqu'une transaction ambiante est créée, elle est inscrite en tant que transaction active, et la propriété [System.Transactions.Transaction.Current](/dotnet/api/system.transactions.transaction.current) vous permet d'y accéder. Dans cet exemple, la transaction est restaurée lorsque la transaction ambiante est supprimée. Si vous souhaitez valider toutes les modifications apportées lorsque vous avez exécuté le test unitaire, vous devez appeler la méthode [System.Transactions.TransactionScope.Complete](/dotnet/api/system.transactions.transactionscope.complete).  
  
#### <a name="to-create-a-transaction-for-a-single-test-method"></a>Pour créer une transaction pour une seule méthode de test  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le nœud **Références** du projet de test, puis cliquez sur **Ajouter une référence**.  
  
    La boîte de dialogue **Ajouter une référence** s’affiche.  
  
2.  Cliquez sur l'onglet **.NET**.  
  
3.  Dans la liste des assemblys, cliquez sur **System.Transactions**, puis cliquez sur **OK**.  
  
4.  Ouvrez le fichier Visual Basic ou C# pour le test unitaire.  
  
5.  Encapsulez les actions d'avant test, de test et d'après test, tel que l'illustre cet exemple de code Visual Basic :  
  
    ```  
    <TestMethod()> _  
    Public Sub dbo_InsertTable1Test()  
  
        Using ts as New System.Transactions.TransactionScope( System.Transactions.TransactionScopeOption.Required)  
            ExecutionContext.Connection.EnlistTransaction(Transaction.Current)  
            PrivilegedContext.Connection.EnlistTransaction(Transaction.Current)  
  
            Dim testActions As DatabaseTestActions = Me.dbo_InsertTable1TestData  
            'Execute the pre-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PretestAction) Is Nothing), "Executing pre-test script...")  
            Dim pretestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PretestAction)  
            'Execute the test script  
  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.TestAction) Is Nothing), "Executing test script...")  
            Dim testResults() As ExecutionResult = TestService.Execute(ExecutionContext, Me.PrivilegedContext, testActions.TestAction)  
  
            'Execute the post-test script  
            '  
            System.Diagnostics.Trace.WriteLineIf((Not (testActions.PosttestAction) Is Nothing), "Executing post-test script...")  
            Dim posttestResults() As ExecutionResult = TestService.Execute(Me.PrivilegedContext, Me.PrivilegedContext, testActions.PosttestAction)  
  
            'Because the transaction is not explicitly committed, it  
            'is rolled back when the ambient transaction is   
            'disposed.  
            'To commit the transaction, remove the comment delimiter  
            'from the following statement:  
            'ts.Complete()  
  
    End Sub  
    Private dbo_InsertTable1TestData As DatabaseTestActions  
    ```  
  
    > [!NOTE]  
    > Si vous utilisez Visual Basic, vous devez ajouter `Imports System.Transactions` (en plus de `Imports Microsoft.VisualStudio.TestTools.UnitTesting`, `Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTesting` et`Imports Microsoft.VisualStudio.TeamSystem.Data.UnitTest.Conditions`). Si vous utilisez Visual C#, vous devez ajouter `using System.Transactions` (en plus des instructions `using` pour Microsoft.VisualStudio.TestTools, Microsoft.VisualStudio.TeamSystem.Data.UnitTesting et Microsoft.VisualStudio.TeamSystem.Data.UnitTesting.Conditions). Vous devez également ajouter une référence à ces assemblys dans votre projet.  
  
## <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>Pour créer une transaction pour toutes les méthodes de test dans une classe de test donnée  
  
#### <a name="to-create-a-transaction-for-all-test-methods-in-a-test-class"></a>Pour créer une transaction pour toutes les méthodes de test dans une classe de test donnée  
  
1.  Ouvrez le fichier Visual Basic ou C# pour le test unitaire.  
  
2.  Créez la transaction dans TestInitialize, et supprimez-la dans TestCleanup, tel que l'illustre cet exemple de code Visual C# :  
  
    ```  
    TransactionScope _trans;  
  
            [TestInitialize()]  
            public void Init()  
            {  
                _trans = new TransactionScope();  
                base.InitializeTest();  
            }  
  
            [TestCleanup()]  
            public void Cleanup()  
            {  
                base.CleanupTest();  
                _trans.Dispose();  
            }  
  
            [TestMethod()]  
            public void TransactedTest()  
            {  
                DatabaseTestActions testActions = this.DatabaseTestMethod1Data;  
                // Execute the pre-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PretestAction != null), "Executing pre-test script...");  
                ExecutionResult[] pretestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PretestAction);  
                // Execute the test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.TestAction != null), "Executing test script...");  
                ExecutionResult[] testResults = TestService.Execute(this.ExecutionContext, this.PrivilegedContext, testActions.TestAction);  
                // Execute the post-test script  
                //   
                System.Diagnostics.Trace.WriteLineIf((testActions.PosttestAction != null), "Executing post-test script...");  
                ExecutionResult[] posttestResults = TestService.Execute(this.PrivilegedContext, this.PrivilegedContext, testActions.PosttestAction);  
  
            }  
    ```  
  
## <a name="to-start-the-distributed-transaction-coordinator-service"></a>Pour démarrer le service Distributed Transaction Coordinator  
Certaines procédures de cette rubrique utilisent des types dans l'assembly System.Transactions. Avant que vous suiviez les procédures, vous devez vérifier que le service Distributed Transaction Coordinator est en cours d'exécution sur l'ordinateur sur lequel vous exécutez les tests unitaires. Sinon, les tests échouent, et le message d'erreur suivant s'affiche : « La méthode de test *ProjectName*.*TestName*.*MethodName* a levé une exception : System.Data.SqlClient.SqlException : MSDTC n'est pas disponible sur le serveur *ComputerName* ».  
  
#### <a name="to-start-the-distributed-transaction-coordinator-service"></a>Pour démarrer le service Distributed Transaction Coordinator  
  
1.  Ouvrez le **Panneau de configuration**.  
  
2.  Dans le **Panneau de configuration**, ouvrez **Outils d'administration**.  
  
3.  Dans **Outils d'administration**, ouvrez **Services**.  
  
4.  Dans le volet **Services**, cliquez avec le bouton droit sur le service **Contrôleur de transaction distribuée**, puis cliquez sur **Démarrer**.  
  
    L'état du service doit être mis à jour vers **Démarré**. Vous devez maintenant être en mesure d'exécuter les tests unitaires qui utilisent System.Transactions.  
  
> [!IMPORTANT]  
> L'erreur suivante peut apparaître, même si vous avez démarré le service Distributed Transaction Controller : `System.Transactions.TransactionManagerCommunicationException: Network access for Distributed Transaction Manager (MSDTC) has been disabled. Please enable DTC for network access in the security configuration for MSDTC using the Component Services Administrative tool. ---> System.Runtime.InteropServices.COMException: The transaction manager has disabled its support for remote/network transactions. (Exception from HRESULT: 0x8004D024)`. Si cette erreur se produit, vous devez configurer le service Distributed Transaction Controller pour l'accès réseau. Pour plus d'informations, consultez [Activation de l’accès DTC réseau](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753510(v=ws.10)).  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
