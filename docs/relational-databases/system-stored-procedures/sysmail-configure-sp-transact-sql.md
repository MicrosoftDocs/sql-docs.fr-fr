---
description: sysmail_configure_sp (Transact-SQL)
title: sysmail_configure_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_configure_sp_TSQL
- sysmail_configure_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_configure_sp
ms.assetid: 73b33c56-2bff-446a-b495-ae198ad74db1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c150f6707a5f0ec537613617182b9da9889d85a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188659"
---
# <a name="sysmail_configure_sp-transact-sql"></a>sysmail_configure_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les paramètres de configuration de la messagerie de base de données. Les paramètres de configuration spécifiés avec **sysmail_configure_sp** s’appliquent à l’ensemble de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_configure_sp [ [ @parameter_name = ] 'parameter_name' ]  
    [ , [ @parameter_value = ] 'parameter_value' ]  
    [ , [ @description = ] 'description' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@parameter_name** =] **'**_parameter_name_**'**  
 Nom du paramètre à modifier.  
  
 [ **@parameter_value** =] **'**_parameter_value_**'**  
 Nouvelle valeur du paramètre.  
  
 [ **@description** =] **'**_Description_**'**  
 Description du paramètre.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 None  
  
## <a name="remarks"></a>Notes  
 La messagerie de base de données utilise les paramètres suivants :  
  
| Nom du paramètre | Description | Valeur par défaut |
| -------------- | ----------- | ------------- |
|*AccountRetryAttempts*|Nombre de fois où le processus de messagerie externe tente d'envoyer le message électronique à l'aide de chaque compte présent dans le profil spécifié.|**1**|  
|*AccountRetryDelay*|Quantité de temps, en secondes, d'attente du processus de messagerie externe entre chaque tentative d'envoi d'un message.|**5000**|  
|*DatabaseMailExeMinimumLifeTime*|Durée minimale (en secondes) pendant laquelle le processus de messagerie externe reste actif. Lorsque la messagerie de base de données envoie un nombre important de messages, augmentez cette valeur afin de conserver cette messagerie de base de données active et d'éviter la charge de gestion engendrée par des démarrages et arrêts fréquents.|**600**|  
|*DefaultAttachmentEncoding*|Encodage par défaut pour les pièces jointes de messagerie électronique.|MIME|  
|*MaxFileSize*|Taille maximale d'une pièce jointe, en octets.|**1 million**|  
|*ProhibitedExtensions*|Liste séparée par des virgules des extensions qui ne peuvent pas être envoyées en pièces jointes dans un message électronique.|**exe,dll,vbs,js**|  
|*LoggingLevel*|Spécifiez quels messages sont enregistrés dans le journal de la messagerie de base de données. Une des valeurs numériques suivantes :<br /><br /> 1 - Mode normal. Seules les erreurs sont consignées dans le journal.<br /><br /> 2 - Mode étendu. Les erreurs, les avertissements et les messages à contenu informatif sont consignés dans le journal.<br /><br /> 3 - Mode documenté. Les erreurs, les avertissements, les messages informatifs et de réussite ainsi que d'autres messages internes sont consignés dans le journal. Utilisez ce mode lorsque vous tentez de résoudre des problèmes.|**2**|  
  
 La procédure stockée **sysmail_configure_sp** se trouve dans la base de données **msdb** et appartient au schéma **dbo** . La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour cette procédure sont octroyées par défaut aux membres du rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 **A. Paramétrage de la messagerie de base de données pour réessayer chaque compte 10 fois**  
  
 L'exemple suivant illustre le paramétrage de la base de données de messagerie afin de réessayer chaque compte à dix reprises, avant de considérer que le compte est injoignable.  
  
```  
EXECUTE msdb.dbo.sysmail_configure_sp  
    'AccountRetryAttempts', '10' ;  
```  
  
 **B. Paramétrage de la taille de pièce jointe maximale à deux mégaoctets**  
  
 L'exemple suivant illustre le paramétrage de la taille de pièce jointe maximale à 2 mégaoctets.  
  
```  
EXECUTE msdb.dbo.sysmail_configure_sp  
    'MaxFileSize', '2097152' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [sysmail_help_configure_sp &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sysmail-help-configure-sp-transact-sql.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
