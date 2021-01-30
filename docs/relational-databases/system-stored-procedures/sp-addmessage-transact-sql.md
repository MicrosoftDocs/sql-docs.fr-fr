---
description: sp_addmessage (Transact-SQL)
title: sp_addmessage (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addmessage
- sp_addmessage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addmessage
ms.assetid: 54746d30-f944-40e5-a707-f2d9be0fb9eb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9bdbbac913643bd0d44ba7e45b07c8bc3e09789
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209377"
---
# <a name="sp_addmessage-transact-sql"></a>sp_addmessage (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Stocke un nouveau message d’erreur défini par l’utilisateur dans une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . Les messages stockés à l’aide de **sp_addmessage** peuvent être affichés à l’aide de l’affichage catalogue **sys. messages** .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addmessage [ @msgnum= ] msg_id , [ @severity= ] severity , [ @msgtext= ] 'msg'   
     [ , [ @lang= ] 'language' ]   
     [ , [ @with_log= ] { 'TRUE' | 'FALSE' } ]   
     [ , [ @replace= ] 'replace' ]   
```  
  
## <a name="arguments"></a>Arguments  
`[ @msgnum = ] msg_id` ID du message. *msg_id* est de **type int** avec NULL comme valeur par défaut. *msg_id* pour les messages d’erreur définis par l’utilisateur peut être un entier compris entre 50 001 et 2 147 483 647. La combinaison des *msg_id* et de la *langue* doit être unique ; une erreur est retournée si l’ID existe déjà pour la langue spécifiée.  
  
`[ @severity = ]severity` Niveau de gravité de l’erreur. la *gravité* est de type **smallint** avec NULL comme valeur par défaut. Les niveaux valides vont de 1 à 25. Pour plus d’informations sur les niveaux de gravité, consultez [Niveaux de gravité des erreurs du moteur de base de données](../../relational-databases/errors-events/database-engine-error-severities.md).  
  
`[ @msgtext = ] 'msg'` Texte du message d’erreur. *MSG* est de type **nvarchar (255),** avec NULL comme valeur par défaut.  
  
`[ @lang = ] 'language'` Langue de ce message. *Language* est de **type sysname** , avec NULL comme valeur par défaut. Étant donné que plusieurs langues peuvent être installées sur le même serveur, *Language* spécifie la langue dans laquelle chaque message est écrit. Lorsque la *langue* est omise, la langue est la langue par défaut de la session.  
  
`[ @with_log = ] { 'TRUE' | 'FALSE' }` Indique si le message doit être écrit dans le journal des applications Windows lorsqu’il se produit. **\@ WITH_LOG** est de type **varchar (5),** avec false comme valeur par défaut. Si sa valeur est TRUE, l'erreur est automatiquement écrite dans le journal des applications Windows. Si sa valeur est FALSE, l'erreur n'est pas automatiquement écrite dans le journal des applications Windows ; c'est la façon dont elle a été déclenchée qui détermine si l'erreur sera ou non écrite dans le journal. Seuls les membres du rôle de serveur **sysadmin** peuvent utiliser cette option.  
  
> [!NOTE]  
>  Si un message est écrit dans le journal des applications Windows, il est également écrit dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fichier journal des erreurs.  
  
`[ @replace = ] 'replace'` S’il est spécifié comme chaîne de *remplacement*, un message d’erreur existant est remplacé par un nouveau texte de message et un niveau de gravité. *Replace* est de type **varchar (7)** avec NULL comme valeur par défaut. Cette option doit être spécifiée si *msg_id* existe déjà. Si vous remplacez un message en anglais des États-Unis, le niveau de gravité est remplacé pour tous les messages dans toutes les autres langues qui ont le même *msg_id*.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 Pour les versions non anglaises de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la version anglaise d'un message doit exister préalablement à l'ajout d'un message dans une autre langue. Le degré de gravité des deux versions du message doit correspondre.  
  
 Lors de la localisation des messages qui contiennent des paramètres, utilisez les numéros de paramètres correspondant aux paramètres des messages d'origine. Insérez un point d'exclamation (!) après chaque numéro de paramètre.  
  
|Message d'origine|Message localisé|  
|----------------------|-----------------------|  
|''Message d'origine param 1 : %s,<br /><br /> param 2 : %d'|''Message localisé param 1 : %1!,<br /><br /> param 2 : %2!'|  
  
 La syntaxe pouvant différer d'une langue à l'autre, il se peut que les numéros de paramètres apparaissent dans un ordre différent par rapport au message d'origine.  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance aux rôles serveur fixes **sysadmin** ou **ServerAdmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-defining-a-custom-message"></a>R. Définition d'un message personnalisé  
 L’exemple suivant ajoute un message personnalisé à **sys. messages**.  
  
```  
USE master;  
GO  
EXEC sp_addmessage 50001, 16,   
   N'Percentage expects a value between 20 and 100.   
   Please reexecute with a more appropriate value.';  
GO  
```  
  
### <a name="b-adding-a-message-in-two-languages"></a>B. Ajout d'un message en deux langues  
 L'exemple suivant ajoute d'abord un message en anglais, puis ajoute ce même message en français`.`  
  
```  
USE master;  
GO  
EXEC sp_addmessage @msgnum = 60000, @severity = 16,   
   @msgtext = N'The item named %s already exists in %s.',   
   @lang = 'us_english';  
  
EXEC sp_addmessage @msgnum = 60000, @severity = 16,   
   @msgtext = N'L''élément nommé %1! existe déjà dans %2!',   
   @lang = 'French';  
GO  
```  
  
### <a name="c-changing-the-order-of-parameters"></a>C. Modification de l'ordre des paramètres  
 L'exemple suivant ajoute d'abord un message en anglais, puis un message localisé dans lequel l'ordre des paramètres est modifié.  
  
```  
USE master;  
GO  
  
EXEC sp_addmessage   
    @msgnum = 60000,   
    @severity = 16,  
    @msgtext =   
        N'This is a test message with one numeric  
        parameter (%d), one string parameter (%s),   
        and another string parameter (%s).',  
    @lang = 'us_english';  
  
EXEC sp_addmessage   
    @msgnum = 60000,   
    @severity = 16,  
    @msgtext =   
        -- In the localized version of the message,  
        -- the parameter order has changed. The   
        -- string parameters are first and second  
        -- place in the message, and the numeric   
        -- parameter is third place.  
        N'Dies ist eine Testmeldung mit einem   
        Zeichenfolgenparameter (%3!),  
        einem weiteren Zeichenfolgenparameter (%2!),   
        und einem numerischen Parameter (%1!).',  
    @lang = 'German';  
GO    
  
-- Changing the session language to use the U.S. English  
-- version of the error message.  
SET LANGUAGE us_english;  
GO  
  
RAISERROR(60000,1,1,15,'param1','param2') -- error, severity, state,  
GO                                       -- parameters.  
  
-- Changing the session language to use the German  
-- version of the error message.  
SET LANGUAGE German;  
GO  
  
RAISERROR(60000,1,1,15,'param1','param2'); -- error, severity, state,   
GO                                       -- parameters.  
```  
  
## <a name="see-also"></a>Voir aussi  
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_altermessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-altermessage-transact-sql.md)   
 [sp_dropmessage &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
