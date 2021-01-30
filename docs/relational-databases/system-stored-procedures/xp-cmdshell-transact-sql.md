---
description: xp_cmdshell (Transact-SQL)
title: xp_cmdshell (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_cmdshell
- xp_cmdshell_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_cmdshell
ms.assetid: 18935cf4-b320-4954-b6c1-e007fcefe358
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73bea06c7919c40b1080458f3e6982d2c10ebe47
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99125002"
---
# <a name="xp_cmdshell-transact-sql"></a>xp_cmdshell (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Engendre une commande d'environnement Windows et lui transmet une chaîne à traiter. Toute sortie est retournée sous forme de lignes de texte.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
xp_cmdshell { 'command_string' } [ , no_output ]  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *command_string* **'**  
 Chaîne qui contient une commande à transmettre au système d'exploitation. *command_string* est de type **varchar (8000)** ou **nvarchar (4000)**, sans valeur par défaut. *command_string* ne peut pas contenir plus d’un jeu de guillemets doubles. Une paire de guillemets simples est requise si des espaces sont présents dans les chemins d’accès aux fichiers ou dans les noms de programme référencés dans *command_string*. Si l'incorporation d'espaces est source de problèmes, recourez à des noms de fichiers FAT 8.3.  
  
 **no_output**  
 Paramètre facultatif indiquant qu'aucune sortie ne doit être retournée au client.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 L'exécution de l'instruction `xp_cmdshell` ci-dessous retourne le contenu du répertoire en cours.  
  
```  
EXEC xp_cmdshell 'dir *.exe';  
GO  
```  
  
 Les lignes sont retournées dans une colonne **nvarchar (255)** . Si l’option **no_output** est utilisée, seuls les éléments suivants sont retournés :  
  
```  
The command(s) completed successfully.  
```  
  
## <a name="remarks"></a>Notes  
 Le processus Windows généré par **xp_cmdshell** a les mêmes droits de sécurité que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service.  
 
> [!IMPORTANT]
>  **xp_cmdshell** est une fonctionnalité très puissante qui est désactivée par défaut. **xp_cmdshell** peuvent être activés et désactivés à l’aide de la gestion basée sur des stratégies ou en exécutant **sp_configure**. Pour plus d’informations, consultez Configuration de la [surface d’exposition](../../relational-databases/security/surface-area-configuration.md) et [Xp_cmdshell option de configuration de serveur](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 **xp_cmdshell** fonctionne de façon synchrone. Le contrôle n'est renvoyé à l'appelant que lorsque la commande de l'interpréteur de commandes est terminée. 
 
> [!IMPORTANT]
>  Si **xp_cmdshell** est exécutée dans un lot et renvoie une erreur, le traitement échoue. Cela représente un changement de comportement. Dans les versions antérieures du [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lot continuerait à s’exécuter.  
 
## <a name="xp_cmdshell-proxy-account"></a>Compte proxy xp_cmdshell  
 Lorsqu’il est appelé par un utilisateur qui n’est pas membre du rôle serveur fixe **sysadmin** , **xp_cmdshell** se connecte à Windows en utilisant le nom de compte et le mot de passe stockés dans les informations d’identification nommées **# #xp_cmdshell_proxy_account # #**. Si ces informations d’identification de proxy n’existent pas, **xp_cmdshell** échoue.  
  
 Les informations d’identification du compte proxy peuvent être créées en exécutant **sp_xp_cmdshell_proxy_account**. Cette procédure stockée accepte comme arguments un nom d'utilisateur et un mot de passe Windows. Par exemple, la commande ci-dessous crée des informations d'identification de proxy pour l'utilisateur de domaine Windows `SHIPPING\KobeR` qui possède le mot de passe Windows `sdfh%dkc93vcMt0`.  
  
```  
EXEC sp_xp_cmdshell_proxy_account 'SHIPPING\KobeR','sdfh%dkc93vcMt0';  
```  
  
 Pour plus d’informations, consultez [sp_xp_cmdshell_proxy_account &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Étant donné que les utilisateurs malveillants essaient parfois d’élever leurs privilèges à l’aide de **xp_cmdshell**, **xp_cmdshell** est désactivé par défaut. Utilisez **sp_configure** ou la **gestion basée** sur des stratégies pour l’activer. Pour plus d’informations, consultez [xp_cmdshell (option de configuration de serveur)](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md).  
  
 Lorsque vous activez pour la première fois, **xp_cmdshell** nécessite l’autorisation Control Server pour s’exécuter et le processus Windows créé par **xp_cmdshell** a le même contexte de sécurité que le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service a souvent plus d’autorisations que nécessaire pour le travail effectué par le processus créé par **xp_cmdshell**. Pour améliorer la sécurité, l’accès à **xp_cmdshell** doit être limité aux utilisateurs disposant de privilèges élevés.  
  
 Pour permettre aux non-administrateurs d’utiliser **xp_cmdshell** et autoriser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à créer des processus enfants avec le jeton de sécurité d’un compte avec des privilèges moindres, procédez comme suit :  
  
1.  Créez et personnalisez un compte d'utilisateur local Windows ou un compte de domaine avec les privilèges minimaux requis par vos processus.  
  
2.  Utilisez la procédure système **sp_xp_cmdshell_proxy_account** pour configurer **xp_cmdshell** afin d’utiliser ce compte doté de privilèges minimum.  
  
    > [!NOTE]  
    >  Vous pouvez également configurer ce compte proxy à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en cliquant avec le bouton droit sur les **Propriétés** du nom de votre serveur dans l’Explorateur d’objets, puis en consultant l’onglet **sécurité** de la section **compte proxy du serveur** .  
  
3.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , à l’aide de la base de données Master, exécutez l' `GRANT exec ON xp_cmdshell TO N'<some_user>';` instruction pour accorder à des utilisateurs spécifiques non-**sysadmin** la possibilité d’exécuter des **xp_cmdshell**. L’utilisateur spécifié doit exister dans la base de données Master.  
  
 Désormais, les utilisateurs non-administrateurs peuvent lancer des processus du système d’exploitation avec **xp_cmdshell** et ces processus s’exécutent avec les autorisations du compte proxy que vous avez configuré. Les utilisateurs disposant de l’autorisation CONTROL SERVER (membres du rôle serveur fixe **sysadmin** ) continuent de recevoir les autorisations du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compte de service pour les processus enfants lancés par **xp_cmdshell**.  
  
 Pour déterminer le compte Windows utilisé par **xp_cmdshell** lors du lancement des processus du système d’exploitation, exécutez l’instruction suivante :  
  
```  
xp_cmdshell 'whoami.exe'  
  
```  
  
 Pour déterminer le contexte de sécurité pour une autre connexion, exécutez ce qui suit :  
  
```  
EXECUTE AS LOGIN = '<other_login>' ;  
GO  
xp_cmdshell 'whoami.exe' ;  
REVERT ;  
  
```  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-returning-a-list-of-executable-files"></a>R. Retour d'une liste de fichiers exécutables  
 L'exemple ci-dessous montre la procédure stockée étendue `xp_cmdshell` exécutant une commande de répertoire.  
  
```  
EXEC master..xp_cmdshell 'dir *.exe'  
```  
  
### <a name="b-returning-no-output"></a>B. Aucun renvoi d'information en sortie  
 L'exemple ci-dessous utilise `xp_cmdshell` pour exécuter une chaîne de commande sans retourner les informations en sortie au client.  
  
```  
USE master;  
  
EXEC xp_cmdshell 'copy c:\SQLbcks\AdvWorks.bck  
    \\server2\backups\SQLbcks', NO_OUTPUT;  
GO  
```  
  
### <a name="c-using-return-status"></a>C. Utilisation de l'état de retour  
 Dans l’exemple suivant, la `xp_cmdshell` procédure stockée étendue suggère également l’état de retour. La valeur du code de retour est stockée dans la variable `@result`.  
  
```  
DECLARE @result int;  
EXEC @result = xp_cmdshell 'dir *.exe';  
IF (@result = 0)  
   PRINT 'Success'  
ELSE  
   PRINT 'Failure';  
```  
  
### <a name="d-writing-variable-contents-to-a-file"></a>D. Écriture du contenu des variables dans un fichier  
 Dans l'exemple ci-dessous, le contenu de la variable `@var` est écrit dans un fichier nommé `var_out.txt` dans le répertoire actif sur le serveur.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'Hello world';  
SET @cmd = 'echo ' + @var + ' > var_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
### <a name="e-capturing-the-result-of-a-command-to-a-file"></a>E. Capture du résultat d’une commande dans un fichier  
 Dans l'exemple ci-dessous, le contenu du répertoire actif est écrit dans un fichier nommé `dir_out.txt` dans le répertoire actif du serveur.  
  
```  
DECLARE @cmd sysname, @var sysname;  
SET @var = 'dir/p';  
SET @cmd = @var + ' > dir_out.txt';  
EXEC master..xp_cmdshell @cmd;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées étendues générales &#40;&#41;Transact-SQL ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_cmdshell (option de configuration de serveur)](../../database-engine/configure-windows/xp-cmdshell-server-configuration-option.md)   
 [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)   
 [sp_xp_cmdshell_proxy_account &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-xp-cmdshell-proxy-account-transact-sql.md)  
  
  
