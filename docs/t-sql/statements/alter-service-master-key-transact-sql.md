---
description: ALTER SERVICE MASTER KEY (Transact-SQL)
title: ALTER SERVICE MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SERVICE_MASTER_KEY_TSQL
- ALTER SERVICE MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- REGENERATE phrase
- FORCE option
- ALTER SERVICE MASTER KEY statement
- cryptography [SQL Server], Service Master Key
- modifying Service Master Key
- decryption [SQL Server], Service Master Key
- encryption [SQL Server], Service Master Key
- service master key [SQL Server], modifying
ms.assetid: a1e9be0e-4115-47d8-9d3a-3316d876a35e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d83d71f8640575319817f3610b1055e2808140ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182217"
---
# <a name="alter-service-master-key-transact-sql"></a>ALTER SERVICE MASTER KEY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permet de modifier la clé principale de service d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
ALTER SERVICE MASTER KEY   
    [ { <regenerate_option> | <recover_option> } ] [;]  
  
<regenerate_option> ::=  
    [ FORCE ] REGENERATE  
  
<recover_option> ::=  
    { WITH OLD_ACCOUNT = 'account_name' , OLD_PASSWORD = 'password' }  
    |      
    { WITH NEW_ACCOUNT = 'account_name' , NEW_PASSWORD = 'password' }  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 FORCE  
 Indique que la clé principale de service doit être générée de nouveau, même au risque de perdre des données. Pour plus d’informations, consultez [Modification du compte de service SQL Server](#_changing), plus loin dans cette rubrique.  
  
 REGENERATE  
 Indique que la clé principale de service doit être générée de nouveau.  
  
 OLD_ACCOUNT **='** _account_name_*_'_*  
 Spécifie le nom de l'ancien compte de service Windows.  
  
> [!WARNING]  
>  Cette option est obsolète. Ne pas utiliser. Utilisez à la place le Gestionnaire de configuration d'[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 OLD_PASSWORD **='** _password_*_'_*  
 Spécifie le mot de passe de l'ancien compte de service Windows.  
  
> [!WARNING]  
>  Cette option est obsolète. Ne pas utiliser. Utilisez à la place le Gestionnaire de configuration d'[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 NEW_ACCOUNT **='** _account_name_*_'_*  
 Spécifie le nom du nouveau compte de service Windows.  
  
> [!WARNING]  
>  Cette option est obsolète. Ne pas utiliser. Utilisez à la place le Gestionnaire de configuration d'[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 NEW_PASSWORD **='** _password_*_'_*  
 Spécifie le mot de passe du nouveau compte de service Windows.  
  
> [!WARNING]  
>  Cette option est obsolète. Ne pas utiliser. Utilisez à la place le Gestionnaire de configuration d'[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Notes  
 La clé principale de service est générée automatiquement la première fois qu'elle est requise pour chiffrer un mot de passe, des informations d'identification ou la clé principale de base de données d'un serveur lié. La clé principale de service est chiffrée à l'aide de la clé de l'ordinateur local ou de l'API de protection des données Windows. Cette API utilise une clé dérivée des informations d'identification Windows du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] utilise l’algorithme de chiffrement AES pour protéger la clé principale du service (SMK) et la clé principale de base de données (DMK). AES est un algorithme de chiffrement plus récent que 3DES, qui était utilisé dans les versions antérieures. Au terme de la mise à niveau d'une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] vers [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , les clés SMK et DMK doivent être régénérées pour mettre à niveau les clés principales vers AES. Pour plus d’informations sur la régénération de la clé DMK, consultez [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md).  
  
##  <a name="changing-the-sql-server-service-account"></a><a name="_changing"></a> Modification du compte de service SQL Server  
 Pour modifier le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour gérer une modification du compte de service, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stocke une copie redondante de la clé principale de service protégée par le compte d’ordinateur qui dispose des autorisations nécessaires accordées au groupe de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si l'ordinateur fait l'objet d'une reconstruction, le même utilisateur de domaine utilisé précédemment par le compte de service peut récupérer la clé principale de service. Cela ne fonctionne pas avec les comptes locaux, ni avec le compte système local, le compte de service local ou le compte de service réseau. Quand vous déplacez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers un autre ordinateur, migrez la clé principale de service à l’aide des fonctionnalités de sauvegarde et de restauration.  
  
 L'expression REGENERATE permet de régénérer la clé principale de service. Lorsque la clé principale de service est régénérée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] déchiffre toutes les clés qu'elle a permis de chiffrer, puis les chiffre au moyen de la nouvelle clé principale de service. Cette opération consomme beaucoup de ressources. Par conséquent, vous devez planifier cette opération au cours d'une période de faible demande, à moins que la clé ait été compromise. Si l'un des déchiffrements échoue, l'instruction entière échoue.  
  
 Lorsque l'option FORCE est spécifiée, le processus de régénération de clé continue même s'il ne parvient pas à récupérer la clé principale actuelle ou s'il ne peut pas déchiffrer toutes les clés privées qu'elle a permis de chiffrer. Utilisez l’option FORCE seulement si la régénération échoue et que vous ne pouvez pas restaurer la clé principale de service à l’aide de l’instruction [RESTORE SERVICE MASTER KEY](../../t-sql/statements/restore-service-master-key-transact-sql.md).  
  
> [!CAUTION]  
>  La clé principale de service représente la racine de la hiérarchie de chiffrement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La clé principale de service protège de manière directe ou indirecte toutes les autres clés et tous les secrets de l'arborescence. Si une clé dépendante ne peut pas être déchiffrée au cours d'une régénération forcée, les données sécurisées par cette clé sont perdues.  
  
 Si vous déplacez SQL sur un autre ordinateur, vous devez utiliser le même compte de service pour déchiffrer la clé SMK. SQL Server résoudra automatiquement le chiffrement du compte d’ordinateur.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL SERVER sur le serveur.  
  
## <a name="examples"></a>Exemples  
 Dans l'exemple ci-dessous, la clé principale de service est régénérée.  
  
```sql  
ALTER SERVICE MASTER KEY REGENERATE;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [RESTORE SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-service-master-key-transact-sql.md)   
 [BACKUP SERVICE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/backup-service-master-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
