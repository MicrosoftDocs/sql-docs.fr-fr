---
title: Restaurer une clé principale de base de données | Microsoft Docs
description: Découvrez comment restaurer la clé principale de base de données dans SQL Server à l’aide de SQL Server Management Studio avec Transact-SQL.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- database master key [SQL Server], importing
ms.assetid: 16897cc5-db8f-43bb-a38e-6855c82647cf
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 65441ecd309abbe9b1545de622f0f7d9d4c2d4bc
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250511"
---
# <a name="restore-a-database-master-key"></a>Restaurer une clé principale de base de données
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique décrit la restauration de la clé principale de base de données dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a>Limitations et restrictions  
  
- Une fois la clé principale restaurée, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] déchiffre toutes les clés chiffrées au moyen de la clé principale active, puis chiffre ces clés au moyen de la clé principale restaurée. Cette opération gourmande en ressources doit être planifiée au cours d'une période de faible demande. Si la clé principale de base de données en cours n'est pas ouverte ou ne peut pas être ouverte, ou si une clé chiffrée à l'aide de cette clé principale ne peut pas être déchiffrée, l'opération de restauration échoue.  
  
- Si l'un des déchiffrements échoue, la restauration échoue. Vous pouvez utiliser l'option FORCE pour ignorer les erreurs, mais cette option entraîne la perte de toutes les données ne pouvant pas être déchiffrées.  
  
- Si la clé principale a été chiffrée à l'aide de la clé principale du service, la clé principale restaurée sera également chiffrée à l'aide de la clé principale du service.  
  
- S'il n'existe aucune clé principale dans la base de données en cours, l'instruction RESTORE MASTER KEY crée une clé principale. La nouvelle clé principale n'est pas automatiquement chiffrée au moyen de la clé principale du service.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations
Requiert l'autorisation CONTROL sur la base de données.  
  
## <a name="using-sql-server-management-studio-with-transact-sql"></a>Utilisation de SQL Server Management Studio avec Transact-SQL  
  
### <a name="to-restore-the-database-master-key"></a>Pour restaurer la clé principale de base de données  
  
1. Récupérez une copie de la clé principale de base de données sauvegardée, à partir d'un support de sauvegarde physique ou d'un répertoire sur le système de fichiers local.  
  
2. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
3. Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
4. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  

    ```sql
    -- Restores the database master key of the AdventureWorks2012 database.  
    USE AdventureWorks2012;  
    GO  
    RESTORE MASTER KEY   
        FROM FILE = 'c:\backups\keys\AdventureWorks2012_master_key'   
        DECRYPTION BY PASSWORD = '3dH85Hhk003#GHkf02597gheij04'   
        ENCRYPTION BY PASSWORD = '259087M#MyjkFkjhywiyedfgGDFD';  
    GO  
    ```  
  
    > [!NOTE]  
    > Le chemin d'accès à la clé et le mot de passe de la clé (s'il existe) seront différents de ce qui est indiqué ci-dessus. Assurez-vous que les deux sont spécifiques à votre installation de serveur et de clé.  
  
 Pour plus d’informations, consultez [RESTORE MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/restore-master-key-transact-sql.md)  
