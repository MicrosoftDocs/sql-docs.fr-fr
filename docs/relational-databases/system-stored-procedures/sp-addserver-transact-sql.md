---
description: sp_addserver (Transact-SQL)
title: sp_addserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addserver
- sp_addserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addserver
- renaming servers
- machine names [SQL Server]
- computer names
ms.assetid: 160a6b29-5e80-44ab-80ec-77d4280f627c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fabe72f253eda808131d876fd180ddb2f7a67ad1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207100"
---
# <a name="sp_addserver-transact-sql"></a>sp_addserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit le nom de l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque l’ordinateur qui héberge [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est renommé, utilisez **sp_addserver** pour informer l’instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] du nouveau nom d’ordinateur. Cette procédure doit être exécutée sur toutes les instances du [!INCLUDE[ssDE](../../includes/ssde-md.md)] moteur de base de données hébergé sur l'ordinateur. Le nom de l'instance du moteur de base de données [!INCLUDE[ssDE](../../includes/ssde-md.md)] ne peut pas être modifié. Pour modifier le nom d'une instance nommée, installez une nouvelle instance portant le nom souhaité, détachez les fichiers de base de données de l'ancienne instance, attachez les bases de données à la nouvelle instance, puis supprimez l'ancienne instance. Vous pouvez également créer un nom d'alias client sur l'ordinateur client, en redirigeant la connexion vers un autre nom de serveur et d'instance ou la combinaison **serveur:port** sans modifier le nom de l'instance sur l'ordinateur serveur.

 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Syntaxe

```

sp_addserver [ @server = ] 'server' ,
     [ @local = ] 'local' 
     [ , [ @duplicate_ok = ] 'duplicate_OK' ]
```

## <a name="arguments"></a>Arguments
`[ @server = ] 'server'` Nom du serveur. Les noms de serveurs doivent être uniques et suivre les règles de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows concernant les noms des ordinateurs, bien que l'utilisation d'espaces ne soit pas autorisée. *server* est de type **sysname** et n'a pas de valeur par défaut.

 Lorsque plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont installées sur un ordinateur, chacune fonctionne comme si elle était sur un serveur distinct. Spécifiez une instance nommée en faisant référence au *serveur* en tant que *NomServeur\NomInstance*.

`[ @local = ] 'LOCAL'` Spécifie que le serveur est ajouté en tant que serveur local. **\@ local** est de type **varchar (10)**, avec NULL comme valeur par défaut. Si vous spécifiez **\@ local** As **local** , le **\@ serveur** est défini comme le nom du serveur local et la @SERVERNAME fonction @ retourne la valeur de *Server*.

 Le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte à cette variable le nom de l'ordinateur. Par défaut, le nom de l'ordinateur est la manière dont les utilisateurs se connectent à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans configuration complémentaire.

 La définition locale entre uniquement en vigueur une fois le moteur de base de données [!INCLUDE[ssDE](../../includes/ssde-md.md)] redémarré. Un seul serveur local peut être défini dans chaque instance du moteur de base de données [!INCLUDE[ssDE](../../includes/ssde-md.md)].

`[ @duplicate_ok = ] 'duplicate_OK'` Spécifie si un nom de serveur en double est autorisé. **\@ duplicate_OK** est de type **varchar (13)**, avec NULL comme valeur par défaut. **\@ duplicate_OK** peut uniquement avoir la valeur **duplicate_OK** ou null. Si **duplicate_OK** est spécifié et que le nom du serveur en cours d’ajout existe déjà, aucune erreur n’est générée. Si les paramètres nommés ne sont pas utilisés, **\@ local** doit être spécifié.

## <a name="return-code-values"></a>Codet de retour
 0 (réussite) ou 1 (échec)

## <a name="remarks"></a>Notes
 Pour définir ou supprimer des options de serveur, utilisez **sp_serveroption**.

 **sp_addserver** ne peut pas être utilisé dans une transaction définie par l’utilisateur.

 L’utilisation de **sp_addserver** pour ajouter un serveur distant n’est plus disponible.  Utilisez de préférence [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).

## <a name="permissions"></a>Autorisations
 Nécessite l'appartenance au rôle serveur fixe **setupadmin** .

## <a name="examples"></a>Exemples
 L'exemple suivant remplace [!INCLUDE[ssDE](../../includes/ssde-md.md)] l'entrée du moteur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le nom de l'ordinateur qui héberge  par `ACCOUNTS`.

```
sp_addserver 'ACCOUNTS', 'local';
```

## <a name="see-also"></a>Voir aussi
 [Renommez un ordinateur qui héberge une instance de Stand-Alone de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md) [sp_addlinkedserver &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) SP_DROPSERVER &#40;[Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropserver-transact-sql.md) sp_helpserver &#40;[procédures stockées système](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) [Transact-SQL](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)&#41;&#40;procédures stockées de sécurité Transact-SQL&#41;&#40;[Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)


