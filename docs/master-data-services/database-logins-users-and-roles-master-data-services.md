---
title: Connexions, utilisateurs et rôles de base de données
description: Master Data Services comprend des connexions, des utilisateurs et des rôles installés sur l’instance SQL Server Moteur de base de données qui héberge la base de données Master Data Services.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- security [Master Data Services], database roles
- database [Master Data Services], users
- security [Master Data Services], database users
- database [Master Data Services], roles
- database [Master Data Services], logins
- security [Master Data Services], database logins
ms.assetid: 72ee383e-a619-461b-9f9d-1cac162ab0c5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e909388212b30942be2d11d53991f7e8d8756b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339073"
---
# <a name="database-logins-users-and-roles-master-data-services"></a>Connexions, utilisateurs et rôles de base de données (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] comprend des connexions, des utilisateurs et des rôles installés automatiquement sur l'instance de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] qui héberge la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Ces connexions, utilisateurs et rôles ne doivent pas être modifiés.  
  
## <a name="logins"></a>Connexions  
  
|Connexion|Description|  
|-----------|-----------------|  
|**mds_dlp_login**|Autorise la création d'assemblys UNSAFE. Pour plus d’informations, consultez [Creating an Assembly](../relational-databases/clr-integration/assemblies/creating-an-assembly.md).<br /><br /> - Connexion désactivée avec mot de passe généré aléatoirement.<br /><br /> - Mappé à dbo pour la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .<br /><br /> - Pour msdb, mds_clr_user mappe à cette connexion.|  
|**mds_email_login**|Connexion active utilisée pour les notifications.<br /><br /> Pour msdb et la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , mds_email_user mappe à cette connexion.|  
  
## <a name="msdb-users"></a>Utilisateurs de msdb  
  
|Utilisateur|Description|  
|----------|-----------------|  
|**mds_clr_user**|Non utilisé. Mappe à mds_dlp_login.|  
|**mds_email_user**|Utilisé pour les notifications.<br /><br /> - Mappe à mds_email_login.<br /><br /> - Est un membre du rôle : DatabaseMailUserRole.|  
  
## <a name="master-data-services-database-users"></a>Utilisateurs de base de données Master Data Services  
  
|Utilisateur|Description|  
|----------|-----------------|  
|**mds_email_user**|Utilisé pour les notifications.<br /><br /> - A l’autorisation SELECT pour le schéma mdm.<br /><br /> - A l’autorisation EXECUTE pour le type de table défini par l’utilisateur mdm.MemberGetCriteria.<br /><br /> - A l’autorisation EXECUTE pour la procédure stockée mdm.udpNotificationQueueActivate.|  
|**mds_schema_user**|Possède les schémas mdq et mdm. Le schéma par défaut est mdm.<br /><br /> Il n'est mappé à aucune connexion.|  
|**mds_ssb_user**|Utilisé pour exécuter des tâches Service Broker.<br /><br /> - A les autorisations DELETE, INSERT, REFERENCES, SELECT et UPDATE pour tous les schémas.<br /><br /> - N’est mappé à aucune connexion.|  
  
## <a name="master-data-services-database-role"></a>Rôle de base de données Master Data Services  
  
|Role|Description|Autorisations|  
|----------|-----------------|-----------------|  
|**mds_exec**|Ce rôle contient le compte que vous désignez dans [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] lorsque vous créez une application Web [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] et désignez un compte pour le pool d'applications.|L'autorisation EXECUTE sur tous les schémas.<br /><br /> <br /><br /> Les autorisations ALTER, INSERT et SELECT sur ces tables :<br /><br /> mdm.tblStgMember<br /><br /> mdm.tblStgMemberAttribute<br /><br /> mdm.tbleStgRelationship<br /><br /> <br /><br /> L'autorisation SELECT sur ces tables :<br /><br /> mdm.tblUser<br /><br /> mdm.tblUserGroup<br /><br /> mdm.tblUserPreference<br /><br /> <br /><br /> L'autorisation SELECT sur ces vues :<br /><br /> mdm.viw_SYSTEM_SECURITY_NAVIGATION<br /><br /> mdm.viw_SYSTEM_SECURITY_ROLE_ACCCESSCONTROL<br /><br /> mdm.viw_SYSTEM_SECURITY_ROLE_ACCCESSCONTROL_MEMBER<br /><br /> mdm.viw_SYSTEM_SECURITY_USER_MODEL|  
  
## <a name="schemas"></a>Schémas  
  
|Role|Description|  
|----------|-----------------|  
|**Mobile**|Contient tous les objets de la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] et Service Broker autres que les fonctions contenues dans le schéma mdq.|  
|**mdq**|Contient les fonctions de la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] relatives aux résultats du filtrage de membres selon des expressions régulières ou des ressemblances, et pour la mise en forme de courriers électroniques de notification.|  
|**stg.**|Contient des tables de base de données, des procédures stockées et des vues [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] liées au processus de site. Ne supprimez pas l'un de ces objets. Pour plus d’informations sur le processus de mise en lots, consultez [Vue d’ensemble : importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Sécurité de l’objet de base de données &#40;Master Data Services&#41;](../master-data-services/database-object-security-master-data-services.md)  
  
  
