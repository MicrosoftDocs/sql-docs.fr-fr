---
description: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
title: BEGIN DISTRIBUTED TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/29/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DISTRIBUTED
- BEGIN_DISTRIBUTED_TRANSACTION_TSQL
- DISTRIBUTED_TSQL
- BEGIN DISTRIBUTED TRANSACTION
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN DISTRIBUTED TRANSACTION statement
- distributed transactions [SQL Server], starting
- MS DTC, transaction management
- distributed transactions [SQL Server], BEGIN DISTRIBUTED TRANSACTION statement
- servers [SQL Server], distributed transactions
- starting distributed transactions
- remote servers [SQL Server], distributed transactions
- starting transactions
ms.assetid: c3bc2716-39d3-4061-8c6a-8734899231ac
author: cawrites
ms.author: chadam
ms.openlocfilehash: 15f3221d2e7848a64f3ba22d618bdc54927afeba
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095989"
---
# <a name="begin-distributed-transaction-transact-sql"></a>BEGIN DISTRIBUTED TRANSACTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Indique le début d'une transaction [!INCLUDE[tsql](../../includes/tsql-md.md)] distribuée gérée par [!INCLUDE[msCoName](../../includes/msconame-md.md)] DTC (Distributed Transaction Coordinator).  
    
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
BEGIN DISTRIBUTED { TRAN | TRANSACTION }   
     [ transaction_name | @tran_name_variable ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *transaction_name*  
 Nom de transaction, défini par l'utilisateur, qui est utilisé pour suivre la transaction distribuée dans les utilitaires MS DTC. *transaction_name* doit suivre les règles applicables aux identificateurs et comporter un nombre \<= à 32 caractères.  
  
 @*tran_name_variable*  
 Nom d'une variable définie par l'utilisateur qui contient un nom de transaction utilisé pour suivre la transaction distribuée dans les utilitaires MS DTC. La variable doit être déclarée avec un type de données **char**, **varchar**, **nchar** ou **nvarchar**.  
  
## <a name="remarks"></a>Remarques  
 L'instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] exécutant l'instruction BEGIN DISTRIBUTED TRANSACTION est le créateur de la transaction et contrôle également l'exécution jusqu'à son terme. Si une instruction COMMIT TRANSACTION ou ROLLBACK TRANSACTION est ensuite émise pour la session, l'instance de contrôle demande à MS DTC de gérer l'exécution de la transaction distribuée sur toutes les instances concernées.  
  
 L'isolement d'instantané au niveau de la transaction ne prend pas en charge les transactions distribuées.  
  
 Le plus souvent, les instances distantes du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sont inscrites dans une transaction distribuée lorsqu'une session déjà inscrite dans cette transaction exécute une requête distribuée référençant un serveur lié.  
  
 Par exemple, si BEGIN DISTRIBUTED TRANSACTION est émis sur ServerA, la session appelle une procédure stockée sur ServerB et une autre sur ServerC. La procédure stockée sur ServerC exécute alors une requête distribuée sur ServerD, et les quatre ordinateurs se trouvent impliqués dans la transaction distribuée. L'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur ServerA est l'instance qui crée la transaction et contrôle son exécution.  
  
 Les sessions intervenant dans les transactions [!INCLUDE[tsql](../../includes/tsql-md.md)] distribuées n'obtiennent pas d'objet de transaction qu'elles peuvent transmettre à une autre session pour que celle-ci soit explicitement inscrite dans la transaction distribuée. La seule façon pour un serveur distant de s'inscrire dans une transaction est d'être la cible d'une requête distribuée ou d'un appel de procédure stockée distante.  
  
 Quand une requête distribuée est exécutée dans une transaction locale, celle-ci est automatiquement promue en transaction distribuée si la source de données OLE DB cible prend en charge ITransactionLocal. Si la source de données OLE DB cible ne prend pas en charge ITransactionLocal, seules les opérations de lecture sont autorisées dans la requête distribuée.  
  
 Une session déjà inscrite dans la transaction distribuée effectue un appel de procédure distante faisant référence à un serveur distant.  
  
 L’option **sp_configure remote proc trans** détermine si les appels aux procédures stockées distantes dans une transaction locale entraînent automatiquement la promotion de celle-ci en transaction distribuée gérée par MS DTC. REMOTE_PROC_TRANSACTIONS est une option SET au niveau de la connexion utilisable pour remplacer la valeur par défaut de l’instance définie par **sp_configure remote proc trans**. Quand cette option est activée, un appel de procédure stockée à distance entraîne la promotion de la transaction locale en une transaction distribuée. Le serveur qui crée la transaction MS DTC devient l'émetteur de la transaction. COMMIT TRANSACTION déclenche une validation coordonnée MS DTC. Si l’option **sp_configure remote proc trans** est activée, les appels de procédures stockées à distance contenus dans les transactions locales sont automatiquement protégés dans le cadre des transactions distribuées, et cela sans qu’il soit nécessaire de réécrire les applications pour définir explicitement un BEGIN DISTRIBUTED TRANSACTION à la place d’un BEGIN TRANSACTION.  
  
 Pour plus d'informations sur l'environnement et le traitement des transactions distribuées, consultez la documentation de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MSDTC).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 Cet exemple supprime un candidat de la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] à la fois sur l'instance locale du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et sur une instance située sur un serveur distant. Ces deux bases locale et distante vont soit valider soit annuler la transaction.  
  
> [!NOTE]  
>  Cet exemple provoque un message d'erreur, sauf si MS DTC est actuellement installé sur l'ordinateur qui exécute l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour plus d'informations sur l'installation de MS DTC, consultez la documentation de Microsoft Distributed Transaction Coordinator.  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN DISTRIBUTED TRANSACTION;  
-- Delete candidate from local instance.  
DELETE AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
-- Delete candidate from remote instance.  
DELETE RemoteServer.AdventureWorks2012.HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT TRANSACTION;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)  
  
  
