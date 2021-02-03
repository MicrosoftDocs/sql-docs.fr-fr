---
title: Restaurer la clé principale du service | Microsoft Docs
description: Découvrez comment restaurer une clé principale de service dans SQL Server à l’aide de Transact-SQL. La clé principale du service est la racine de la hiérarchie de chiffrement de Microsoft SQL Server.
ms.custom: ''
ms.date: 01/02/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- service master key [SQL Server], importing
- service master key [SQL Server], restoring
ms.assetid: 14bdbbbe-d384-4692-b670-4243d2466fe1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 8df8f8a8a01b8ade5372147f274d329baf346ffd
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250169"
---
# <a name="restore-the-service-master-key"></a>Restaurer la clé principale du service
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment restaurer la clé principale de service dans [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
> [!WARNING]  
> Il est peu probable que vous ayez jamais à restaurer cette clé. Si vous deviez le faire, observez la plus grande prudence. Pour plus d’informations, consultez [Back Up the Service Master Key](../../../relational-databases/security/encryption/back-up-the-service-master-key.md).  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a>Limitations et restrictions  
  
- Lorsque la clé principale de service est restaurée, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] déchiffre toutes les clés et les secrets qui ont été chiffrés au moyen de la clé principale de service en cours, puis les chiffre au moyen de la clé principale de service chargée à partir du fichier de sauvegarde.  
  
- Si l'un des déchiffrements échoue, la restauration échoue. Vous pouvez utiliser l'option FORCE pour ignorer les erreurs, mais cette option entraîne la perte de toutes les données ne pouvant pas être déchiffrées.  
  
- La régénération de la hiérarchie de chiffrement est une opération qui consomme beaucoup de ressources. Par conséquent, vous devez planifier cette opération au cours d'une période de faible demande.  
  
> [!CAUTION]  
> La clé principale de service représente la racine de la hiérarchie de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . La clé principale de service sécurise de manière directe ou indirecte toutes les autres clés de l'arborescence. Si une clé dépendante ne peut pas être déchiffrée au cours d'une restauration forcée, les données sécurisées par cette clé sont perdues.  
  
## <a name="security"></a>Sécurité  
  
### <a name="permissions"></a>Autorisations  
Requiert l'autorisation CONTROL SERVER sur le serveur.  
  
## <a name="using-transact-sql"></a>Utilisation de Transact-SQL  
  
### <a name="to-restore-the-service-master-key"></a>Pour restaurer la clé principale du service  
  
1. Récupérez une copie de la clé principale du service sauvegardée, à partir d'un support de sauvegarde physique ou d'un répertoire sur le système de fichiers local.  
  
2. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
3. Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
4. Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql
    -- Restores the service master key from a backup file.  
    RESTORE SERVICE MASTER KEY   
        FROM FILE = 'c:\temp_backups\keys\service_master_key'   
        DECRYPTION BY PASSWORD = '3dH85Hhk003GHk2597gheij4';  
    GO  
    ```  
  
    > [!NOTE]  
    > Le chemin d'accès à la clé et le mot de passe de la clé (s'il existe) seront différents de ce qui est indiqué ci-dessus. Assurez-vous que les deux sont spécifiques à votre installation de serveur et de clé.
