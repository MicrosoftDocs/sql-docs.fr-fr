---
description: Désactiver des contraintes de clé étrangère pour la réplication
title: Désactiver des contraintes de clé étrangère pour la réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- constraints [SQL Server], foreign keys
- foreign keys [SQL Server], disabling constraints
- disabling constraints
ms.assetid: 4211f2fd-d16a-4081-995c-43f1f0827f0b
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 981983821435e17a8ab00b7c30ca1e676a960cb1
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766152"
---
# <a name="disable-foreign-key-constraints-for-replication"></a>Désactiver des contraintes de clé étrangère pour la réplication
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Vous pouvez désactiver les contraintes de clé étrangère lors de la réplication dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cela peut être utile si vous publiez des données issues d'une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Si une table est publiée à l'aide du processus de réplication, les contraintes de clé étrangère sont automatiquement désactivées lors des opérations effectuées par les Agents de réplication. Lorsqu'un Agent de réplication effectue une requête Insert, Update ou Delete vers un abonné, la contrainte n'est pas vérifiée ; si c'est un utilisateur qui effectue la requête Insert, Update ou Delete, la contrainte est vérifiée. La contrainte est désactivée pour l'Agent de réplication, car elle était déjà vérifiée au niveau de l'éditeur lorsque les données ont été insérées, mises à jour ou supprimées à l'origine.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour désactiver une contrainte de clé étrangère lors de la réplication, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert une autorisation ALTER sur la table.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-disable-a-foreign-key-constraint-for-replication"></a>Pour désactiver une contrainte de clé étrangère lors de la réplication  
  
1.  Dans l' **Explorateur d'objets**, développez la table avec la contrainte de clé étrangère que vous souhaitez modifier, puis développez le dossier **Clés** .  
  
2.  Cliquez avec le bouton droit sur la contrainte de clé étrangère et cliquez sur **Modifier**.  
  
3.  Dans la boîte de dialogue **Relations de clé étrangère** , sélectionnez la valeur **Non** pour **Appliquer la réplication**.  
  
4.  Cliquez sur **Fermer**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-disable-a-foreign-key-constraint-for-replication"></a>Pour désactiver une contrainte de clé étrangère lors de la réplication  
  
1.  Pour effectuer cette tâche dans [!INCLUDE[tsql](../../includes/tsql-md.md)], supprimez la contrainte de clé étrangère. Ensuite, ajoutez une nouvelle contrainte de clé étrangère et spécifiez l'option NOT FOR REPLICATION.  
  
 Pour plus d’informations, consultez [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="TsqlExample"></a>  
