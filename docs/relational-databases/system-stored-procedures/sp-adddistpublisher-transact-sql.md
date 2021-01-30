---
description: sp_adddistpublisher (Transact-SQL)
title: sp_adddistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistpublisher
- sp_adddistpublisher_TSQL
helpviewer_keywords:
- sp_adddistpublisher
ms.assetid: 04e15011-a902-4074-b38c-3ec2fc73b838
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 3ad2edff83e6600ab77e422d2d18516ecd8a2064
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182820"
---
# <a name="sp_adddistpublisher-transact-sql"></a>sp_adddistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Configure un serveur de publication pour qu'il utilise une base de données de distribution spécifique. Cette procédure stockée est exécutée sur n’importe quelle base de données du serveur de distribution. Notez que les procédures stockées [sp_adddistributor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) et [sp_adddistributiondb &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md) doivent avoir été exécutées avant l’utilisation de cette procédure stockée.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_adddistpublisher [ @publisher= ] 'publisher'   
        , [ @distribution_db= ] 'distribution_db'   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @working_directory= ] 'working_directory' ]   
    [ , [ @storage_connection_string= ] 'storage_connection_string']
    [ , [ @trusted= ] 'trusted' ]   
    [ , [ @encrypted_password= ] encrypted_password ]   
    [ , [ @thirdparty_flag = ] thirdparty_flag ]  
    [ , [ @publisher_type = ] 'publisher_type' ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publisher = ] 'publisher'` Nom du serveur de publication. *Publisher* est de **type sysname**, sans valeur par défaut.  

> [!NOTE]
> Le nom du serveur peut être spécifié sous la forme `<Hostname>,<PortNumber>` . Vous devrez peut-être spécifier le numéro de port de votre connexion lorsque SQL Server est déployé sur Linux ou Windows avec un port personnalisé, et que le service Browser est désactivé.
  
`[ @distribution_db = ] 'distribution_db'` Nom de la base de données de distribution. *distributor_db* est de **type sysname**, sans valeur par défaut. Il est utilisé par les agents de réplication pour se connecter au serveur de publication.  
  
`[ @security_mode = ] security_mode` Est le mode de sécurité implémenté. Ce paramètre est utilisé uniquement par les agents de réplication pour se connecter au serveur de publication pour les abonnements de mise à jour en attente ou avec un serveur de publication non- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *security_mode* est de **type int** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0**|Les Agents de réplication situés sur le serveur de distribution utilisent l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour se connecter au serveur de publication.|  
|**1** (par défaut)|Les agents de réplication situés sur le serveur de distribution utilisent l'authentification Windows pour se connecter au serveur de publication.|  
  
`[ @login = ] 'login'` Nom de la connexion. Ce paramètre est obligatoire si *security_mode* a la **valeur 0**. *login* est de type **sysname**, avec NULL comme valeur par défaut. Il est utilisé par les agents de réplication pour se connecter au serveur de publication.  
  
`[ @password = ] 'password']` Est le mot de passe. *Password* est de **type sysname**, avec NULL comme valeur par défaut. Il est utilisé par les agents de réplication pour se connecter au serveur de publication.  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort.  
  
`[ @working_directory = ] 'working_directory'` Nom du répertoire de travail utilisé pour stocker les fichiers de données et de schéma de la publication. *working_directory* est de type **nvarchar (255)**, et par défaut le dossier repldata pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , par exemple `C:\Program Files\Microsoft SQL Server\MSSQL\MSSQ.1\ReplData` . Le nom doit être indiqué au format UNC.  

 Pour Azure SQL Database, utilisez `\\<storage_account>.file.core.windows.net\<share>` .

`[ @storage_connection_string = ] 'storage_connection_string'` Est requis pour SQL Database. Utilisez la clé d’accès du portail Azure sous paramètres de > de stockage.

 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]

`[ @trusted = ] 'trusted'` Ce paramètre est déconseillé et n’est fourni qu’à des fins de compatibilité descendante. *Trusted* est de type **nvarchar (5)** et son affectation à la **valeur false** génère une erreur.  
  
`[ @encrypted_password = ] encrypted_password` La définition de *encrypted_password* n’est plus prise en charge. Si vous tentez de définir ce paramètre de **bit** sur **1** , une erreur se produit.  
  
`[ @thirdparty_flag = ] thirdparty_flag` Est lorsque le serveur de publication est [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *thirdparty_flag* est de **bits** et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**0** (valeur par défaut)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Database.|  
|**1**|Base de données autre que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
`[ @publisher_type = ] 'publisher_type'` Spécifie le type de serveur de publication lorsque le serveur de publication n’est pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *publisher_type* est de type sysname et peut prendre l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**MSSQLSERVER**<br /><br /> (par défaut)|Spécifie un serveur de publication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**SOLUTION**|Spécifie un serveur de publication Oracle standard.|  
|**ORACLE GATEWAY**|Spécifie un serveur de publication Oracle Gateway.|  
  
 Pour plus d’informations sur les différences entre un serveur de publication Oracle et un serveur de publication Oracle Gateway, consultez [configurer un serveur de publication Oracle](../../relational-databases/replication/non-sql/configure-an-oracle-publisher.md).  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_adddistpublisher** est utilisé par la réplication d’instantané, la réplication transactionnelle et la réplication de fusion.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistpublisher-tran_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_adddistpublisher**.  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurer la distribution](../../relational-databases/replication/configure-distribution.md)  
  
  
