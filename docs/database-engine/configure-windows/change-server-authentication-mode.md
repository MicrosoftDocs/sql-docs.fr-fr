---
title: Changer le mode d'authentification du serveur
description: Découvrez comment modifier le mode d’authentification du serveur dans SQL Server. Vous pouvez utiliser SQL Server Management Studio ou Transact-SQL pour effectuer cette tâche.
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- authentication [SQL Server], changing modes
- server authentication mode [SQL Server]
- modifying server authentication mode
ms.assetid: 79babcf8-19fd-4495-b8eb-453dc575cac0
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 02/18/2020
ms.openlocfilehash: a4cdc32bd0514be768ac8ed1723fc88840a7e930
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783412"
---
# <a name="change-server-authentication-mode"></a>Changer le mode d’authentification du serveur

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cette rubrique explique comment modifier le mode d'authentification du serveur dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Au cours de l’installation, le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] est paramétré sur **Mode d’authentification Windows** ou sur **Mode d’authentification SQL Server et Windows**. Après l'installation, vous pouvez modifier le mode d'authentification à tout moment.

Si vous sélectionnez le **Mode d’authentification Windows** au cours de l’installation, la connexion sa est désactivée et un mot de passe est attribué par le programme d’installation. Si vous remplacez ensuite le mode d’authentification par **Mode d’authentification SQL Server et Windows**, la connexion sa reste désactivée. Pour utiliser la connexion sa, utilisez l’instruction ALTER LOGIN de façon à activer la connexion et attribuer un nouveau mot de passe. La connexion sa au serveur est possible uniquement via l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="before-you-begin"></a>Avant de commencer

Le compte sa est un compte [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bien connu qui est souvent la cible d'utilisateurs malveillants. N'activez pas le compte sa à moins que votre application l'exige. Il est par conséquent important d'utiliser un mot de passe fort pour la connexion sa.

## <a name="change-authentication-mode-with-ssms"></a>Changer le mode d’authentification avec SSMS

1. Dans l’Explorateur d’objets de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , cliquez avec le bouton droit sur le serveur, puis sélectionnez **Propriétés**.

2. Dans la page **Sécurité** , sous **Authentification du serveur**, sélectionnez le nouveau mode d'authentification du serveur, puis cliquez sur **OK**.

3. Dans la boîte de dialogue [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , cliquez sur **OK** pour accepter le message indiquant la nécessité de redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

4. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur votre serveur, puis cliquez sur **Redémarrer**. S'il est en cours d'exécution, l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit également être redémarré.

## <a name="enable-sa-login"></a>Activer la connexion sa

Vous pouvez activer la connexion **sa** avec SSMS ou T-SQL.

### <a name="use-ssms"></a>Utiliser SSMS

1. Dans l’Explorateur d’objets, développez **Sécurité**, puis Connexions, cliquez avec le bouton droit sur **sa**, puis sélectionnez **Propriétés**.

2. Dans la page **Général**, vous devrez peut-être créer et confirmer un mot de passe pour la connexion **sa**.

3. Sur la page **État** , dans la section **Connexion** , cliquez sur **Activée**, puis sur **OK**.

### <a name="using-transact-sql"></a>Utilisation de Transact-SQL

L’exemple suivant active la connexion sa et définit un nouveau mot de passe. Remplacez `<enterStrongPasswordHere>` par un mot de passe fort avant d’exécuter ce code.

```sql  
ALTER LOGIN sa ENABLE ;  
GO  
ALTER LOGIN sa WITH PASSWORD = '<enterStrongPasswordHere>' ;  
GO  
```

## <a name="change-authentication-mode-t-sql"></a>Changer le mode d’authentification (T-SQL)

L’exemple suivant change l’authentification du serveur du mode mixte (Windows + SQL) à Windows uniquement.

> [!CAUTION]
> L’exemple suivant utilise une procédure stockée étendue pour la modification du registre du serveur. En effet, toute erreur de modification peut être à l’origine de problèmes graves. Ces problèmes peuvent vous obliger à réinstaller le système d’exploitation. Microsoft ne garantit pas que ces problèmes peuvent être résolus. Toute modification du registre relève de votre propre responsabilité.

```sql
USE [master]
GO
EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', 
     N'Software\Microsoft\MSSQLServer\MSSQLServer',
     N'LoginMode', REG_DWORD, 1
GO
```

> [!Note]
> Les autorisations requises pour modifier le mode d’authentification sont [sysadmin](../../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles) ou [Control Server](../../relational-databases/security/permissions-database-engine.md)

## <a name="see-also"></a>Voir aussi

- [Mots de passe forts](../../relational-databases/security/strong-passwords.md)
- [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)
- [Se connecter à SQL Server lorsque les administrateurs système n'y ont plus accès](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)
