---
description: MSsubscription_properties (Transact-SQL)
title: MSsubscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscription_properties
- MSsubscription_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_properties system table
ms.assetid: f96fc1ae-b798-4b05-82a7-564ae6ef23b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9927dd1e15f04a073d0fb3cf7f95e13b1f425e0c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100489"
---
# <a name="mssubscription_properties-transact-sql"></a>MSsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La table **MSsubscription_properties** contient des lignes pour les informations de paramètres requises pour exécuter des agents de réplication sur l’abonné. Cette table est stockée dans la base de données d'abonnés sur l'Abonné pour un abonnement extrait ou dans la base de données de distribution sur le serveur de distribution pour un abonnement envoyé.  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nom du serveur de publication.|  
|**publisher_db**|**sysname**|Nom de la base de données du serveur de publication.|  
|**édition**|**sysname**|Nom de la publication.|  
|**publication_type**|**int**|Type de publication :<br /><br /> **0** = transactionnel.<br /><br /> **2** = fusion.|  
|**publisher_login**|**sysname**|ID de connexion utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_password**|**nvarchar (524)**|Mot de passe (chiffré) utilisé côté serveur de publication pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_security_mode**|**int**|Mode de sécurité implémenté sur le serveur de publication :<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification SQL Server.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] authentification Windows.<br /><br /> **2** = les déclencheurs de synchronisation utilisent une entrée **sysservers** statique pour effectuer un appel de procédure distante (RPC), et le serveur de *publication* doit être défini dans la table **sysservers** en tant que serveur distant ou serveur lié.|  
|**conseiller**|**sysname**|Nom du serveur de distribution.|  
|**distributor_login**|**sysname**|ID de connexion utilisé sur le serveur de distribution pour l'authentification SQL Server.|  
|**distributor_password**|**nvarchar (524)**|Mot de passe (chiffré) utilisé côté serveur de distribution pour l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_security_mode**|**int**|Mode de sécurité implémenté sur le serveur de distribution :<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.<br /><br /> **1** = authentification Windows.|  
|**ftp_address**|**sysname**|Adresse réseau du service FTP (File Transfer Protocol) du serveur de distribution.|  
|**ftp_port**|**int**|Numéro de port du service FTP du serveur de distribution.|  
|**ftp_login**|**sysname**|Nom d'utilisateur, utilisé pour la connexion au service FTP.|  
|**ftp_password**|**nvarchar (524)**|Mot de passe utilisateur, utilisé pour la connexion au service FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Indique l'emplacement du dossier de remplacement pour l'instantané.|  
|**working_directory**|**nvarchar(255)**|Nom du répertoire de travail utilisé pour stocker les fichiers de données et de schéma.|  
|**use_ftp**|**bit**|Spécifie l’utilisation de FTP au lieu du protocole normal pour récupérer des instantanés. Si la **1** est utilisée, FTP est utilisé.|  
|**dts_package_name**|**sysname**|Spécifie le nom du package DTS (Data Transformation Services).|  
|**dts_package_password**|**nvarchar (524)**|Spécifie le mot de passe du package.|  
|**dts_package_location**|**int**|Emplacement de stockage du package DTS.|  
|**enabled_for_syncmgr**|**bit**|Spécifie s'il est possible de synchroniser l'abonnement à l'aide du Gestionnaire de synchronisation [!INCLUDE[msCoName](../../includes/msconame-md.md)].<br /><br /> **0** = l’abonnement n’est pas inscrit auprès du gestionnaire de synchronisation.<br /><br /> **1** = l’abonnement est inscrit auprès du gestionnaire de synchronisation et peut être synchronisé sans démarrage [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**offload_agent**|**bit**|Spécifie si l'Agent peut ou non être activé à distance. Si la **valeur est 0**, l’agent ne peut pas être activé à distance.|  
|**offload_server**|**sysname**|Indique le nom de réseau du serveur utilisé pour l'activation à distance.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Spécifie le chemin d'accès au dossier où les fichiers d'instantané sont enregistrés.|  
|**use_web_sync**|**bit**|Spécifie si l'abonnement peut ou non être synchronisé via HTTP. La valeur **1** signifie que cette fonctionnalité est activée.|  
|**internet_url**|**nvarchar(260)**|Adresse URL de l'emplacement de l'écouteur de réplication pour la synchronisation Web.|  
|**internet_login**|**sysname**|Connexion que le Agent de fusion utilise lors de la connexion au serveur Web qui héberge la synchronisation Web à l’aide de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.|  
|**internet_password**|**nvarchar (524)**|Mot de passe de la connexion que le Agent de fusion utilise lors de la connexion au serveur Web qui héberge la synchronisation Web à l’aide de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] authentification.|  
|**internet_security_mode**|**int**|Mode d’authentification utilisé lors de la connexion au serveur Web qui héberge la synchronisation Web, où la valeur **1** correspond à l’authentification Windows et la valeur **0** à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification.|  
|**internet_timeout**|**int**|Durée (en secondes) avant l'expiration d'une demande de synchronisation Web.|  
|**hostname**|**sysname**|Spécifie la valeur de **HOST_NAME** lorsque cette fonction est utilisée dans la clause **Where** d’un filtre de jointure ou d’une relation d’enregistrement logique.|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
