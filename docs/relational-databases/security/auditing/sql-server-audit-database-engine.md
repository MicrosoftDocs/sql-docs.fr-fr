---
title: SQL Server Audit (moteur de base de données) | Microsoft Docs
description: En savoir plus sur les audits de serveur pour le Moteur de base de données SQL Server ou une base de données individuelle. Les audits de serveurs contiennent des spécifications de l’audit du serveur et de la base de données.
ms.custom: ''
ms.date: 01/01/2020
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- audit
helpviewer_keywords:
- SQL Server Audit
- audits [SQL Server], SQL Server Audit
ms.assetid: 0c1fca2e-f22b-4fe8-806f-c87806664f00
author: davidtrigano
ms.author: datrigan
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 7c45a7bfb3f7dace1fb3d8b56d22977ec7768769
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076587"
---
# <a name="sql-server-audit-database-engine"></a>SQL Server Audit (moteur de base de données)
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  L'*audit* d'une instance de [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)] ou d'une base de données individuelle implique le suivi et la journalisation des événements qui se produisent sur le [!INCLUDE[ssDE](../../../includes/ssde-md.md)]. L'audit[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vous permet de créer des audits de serveur, qui peuvent contenir des spécifications d'audit de serveur pour les événements de niveau serveur, ainsi que des spécifications d'audit de base de données pour les événements de niveau base de données. Les événements audités peuvent être écrits dans les journaux d’événements ou les fichiers d’audit.  
  
[!INCLUDE[ssMIlimitation](../../../includes/sql-db-mi-limitation.md)]
  
 Il existe plusieurs niveaux d'audit pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], selon les spécifications de normes pour votre installation. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit fournit les outils et processus nécessaires pour activer, stocker et afficher les audits sur différents objets de serveur et de base de données.  
  
 Vous pouvez enregistrer des groupes d'actions d'audit du serveur par instance, et des groupes d'actions d'audit de base de données ou des actions d'audit de base de données par base de données. L'événement d'audit se produit chaque fois que l'action pouvant être auditée est rencontrée.  
  
 Toutes les éditions de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prennent en charge les audits de niveau serveur. Toutes les éditions prennent en charge les audits de niveau base de données à partir de la version [!INCLUDE[ssSQL15_md](../../../includes/sssql16-md.md)] SP1. Avant cette version, les audits de niveau base de données sont limités aux éditions Enterprise, Developer et Evaluation. Pour plus d’informations, consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
> [!NOTE]  
>  Cette rubrique s’applique à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  Pour [!INCLUDE[ssSDS](../../../includes/sssds-md.md)], voir [Prise en main de l’audit de base de données SQL](/azure/azure-sql/database/auditing-overview).  
  
## <a name="sql-server-audit-components"></a>Composants de SQL Server Audit  
 Un *audit* correspond à la combinaison de plusieurs éléments au sein d'un package unique pour un groupe spécifique d'actions de serveur ou d'actions de base de données. Les composants d'audit de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se combinent de façon à produire une sortie appelée audit, tout comme une définition de rapport combinée à des graphiques et des éléments de données produit un rapport.  
  
 L’audit de[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise des *événements étendus* pour aider à créer un audit. Pour plus d'informations sur les événements étendus, consultez [événements étendus](../../../relational-databases/extended-events/extended-events.md).  
  
### <a name="sql-server-audit"></a>SQL Server Audit  
 L’objet *Audit SQL Server* recueille une seule instance des actions et des groupes d’actions au niveau du serveur ou de la base de données à surveiller. L'audit s'effectue au niveau de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Vous pouvez exécuter plusieurs audits par instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Lorsque vous définissez un audit, vous spécifiez l'emplacement de sortie des résultats. Il s'agit de la destination de l'audit. L'audit est créé dans un état *désactivé* et ne s'exécute pas automatiquement pour contrôler les actions. Les données d'audit sont transmises vers la destination de l'audit lorsque ce dernier est activé.  
  
### <a name="server-audit-specification"></a>Spécification de l'audit du serveur  
 L'objet *Spécification de l'audit du serveur* appartient à un audit. Vous pouvez créer une spécification d'audit de serveur par audit, car tous deux sont créés au niveau de la portée de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 La spécification de l'audit du serveur recueille de nombreux groupes d'actions au niveau du serveur déclenchées par la fonctionnalité Événements étendus. Vous pouvez inclure des *groupes d'actions d'audit* dans une spécification de l'audit du serveur. Les groupes d'actions d'audit constituent des groupes d'actions prédéfinis, qui sont les événements atomiques qui se produisent dans le [!INCLUDE[ssDE](../../../includes/ssde-md.md)]. Ces actions sont envoyées à l'audit, qui les enregistre dans la cible.  
  
 Les groupes d’actions d’audit au niveau du serveur sont décrits dans la rubrique [Actions et groupes d’actions SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
### <a name="database-audit-specification"></a>Spécification de l'audit de la base de données  
 L'objet *Spécification de l'audit de la base de données* appartient également à un audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Vous pouvez créer une spécification de l'audit de la base de données par base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] par audit.  
  
 La spécification de l'audit de la base de données recueille des actions d'audit au niveau de la base de données déclenchées par la fonctionnalité Événements étendus. Vous pouvez ajouter des groupes d’actions d’audit ou des événements d’audit à une spécification de l’audit de la base de données. *Les événements d’audit* sont les opérations atomiques qui peuvent être auditées par le moteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Les *groupes d'actions d'audit* sont des groupes d'actions prédéfinis. Tous deux sont à la portée de la base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Ces actions sont envoyées à l'audit, qui les enregistre dans la cible. N'incluez pas d'objets dans l'étendue du serveur, tels que les vues système, dans une spécification d'audit de base de données utilisateur.  
  
 Les actions d’audit et les groupes d’actions d’audit au niveau de la base de données sont décrits dans la rubrique [Actions et groupes d’actions SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
### <a name="target"></a>Cible  
 Les résultats d'un audit sont envoyés à une cible, qui peut être un fichier, le journal des événements de sécurité de Windows ou le journal des événements d'applications de Windows. Les journaux doivent être examinés et archivés périodiquement afin de s'assurer que la cible dispose d'un espace suffisant pour écrire des enregistrements supplémentaires.  
  
> [!IMPORTANT]  
>  Tout utilisateur authentifié peut lire et écrire dans le journal des événements d'applications de Windows. Celui-ci requiert des autorisations inférieures au journal des événements de sécurité de Windows et il est moins sécurisé.  
  
 L'écriture dans le journal de sécurité de Windows requiert l'ajout du compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à la stratégie **Générer des audits de sécurité** . Par défaut, les comptes Système Local, Service local et Service réseau font partie de cette stratégie. Ce paramètre peut être configuré à l'aide du composant logiciel enfichable de stratégie de sécurité (secpol.msc). En outre, la stratégie de sécurité **Auditer l'accès aux objets** doit être activée pour **Succès** et **Échec**. Ce paramètre peut être configuré à l'aide du composant logiciel enfichable de stratégie de sécurité (secpol.msc). Dans [!INCLUDE[wiprlhext](../../../includes/wiprlhext-md.md)] ou Windows Server 2008, vous pouvez définir une stratégie **générée par une application** plus précise à partir de la ligne de commande à l’aide du programme de stratégie d’audit (**AuditPol.exe)** . Pour plus d’informations sur les étapes permettant d’activer l’écriture dans le journal de sécurité de Windows, consultez [Écrire des événements d’audit SQL Server dans le journal de sécurité](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md). Pour en savoir plus sur le programme Auditpol.exe, consultez l’article 921469 de la Base de connaissances : [Comment faire pour utiliser la stratégie de groupe pour configurer des paramètres d'audit de sécurité détaillés](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/921469). Les journaux des événements de Windows sont communs à l'ensemble du système d'exploitation Windows. Pour plus d'informations sur les journaux des événements de Windows, consultez [Vue d'ensemble de l'observateur d'événements](/previous-versions/windows/it-pro/windows-server-2003/cc737015(v=ws.10)). Si vous avez besoin d'autorisations plus précises sur l'audit, utilisez la cible de fichier binaire.  
  
 Lorsque vous enregistrez des informations d'audit dans un fichier, pour éviter toute falsification, vous pouvez limiter l'accès à l'emplacement du fichier des façons suivantes :  
  
-   Le compte de service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit posséder des autorisations d'accès en lecture et en écriture.  
  
-   Les administrateurs d'audit ont généralement besoin des autorisations d'accès en lecture et en écriture. Cela suppose que ces administrateurs sont des comptes Windows pour l'administration des fichiers d'audit, par exemple leur copie sur des partages différents, leur sauvegarde, entre autres.  
  
-   Les lecteurs d'audit qui sont autorisés à lire les fichiers d'audit doivent disposer de l'autorisation d'accès en lecture.  
  
 Même lorsque le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] écrit dans un fichier, d'autres utilisateurs Windows peuvent lire le fichier d'audit s'ils en ont l'autorisation. Le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] n'applique pas de verrou exclusif qui empêche les opérations de lecture.  
  
 Dans la mesure où le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] peut accéder au fichier, les connexions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui possèdent l'autorisation CONTROL SERVER peuvent utiliser le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] pour accéder aux fichiers d'audit. Pour enregistrer tout utilisateur qui lit le fichier d'audit, définissez un audit sur master.sys.fn_get_audit_file. Vous enregistrez ainsi les connexions avec l'autorisation CONTROL SERVER qui ont accédé au fichier d'audit par le biais de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Si un administrateur d'audit copie le fichier à un autre emplacement (entre autres, à des fins d'archivage), les listes de contrôle d'accès du nouvel emplacement doivent disposer uniquement des autorisations suivantes :  
  
-   Administrateur d’audit - Lecture/Écriture  
  
-   Lecteur d’audit - Lecture  
  
 Nous conseillons de générer des rapports d'audit à partir d'une instance distincte de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], telle une instance de [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)], accessible uniquement aux administrateurs ou lecteurs d'audit. En utilisant une instance distincte du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] pour la création de rapports, vous pouvez mieux empêcher les utilisateurs non autorisés d'obtenir un accès à l'enregistrement d'audit.  
  
 Vous pouvez offrir une protection supplémentaire contre tout accès non autorisé en chiffrant le dossier dans lequel le fichier d'audit est stocké à l'aide du chiffrement de lecteur BitLocker Windows ou du système de fichiers EFS Windows.  
  
 Pour plus d'informations sur les enregistrements d'audit qui sont écrits dans la cible, consultez [SQL Server Audit Records](../../../relational-databases/security/auditing/sql-server-audit-records.md).  
  
## <a name="overview-of-using-sql-server-audit"></a>Vue d'ensemble de l'utilisation de SQL Server Audit  
 Vous pouvez utiliser [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour définir un audit. Une fois l'audit créé et activé, la cible reçoit des entrées.  
  
 Vous pouvez lire les journaux des événements de Windows à l'aide de l'utilitaire **Observateur d'événements** de Windows. Si la cible est un fichier, vous pouvez utiliser la **Visionneuse du fichier journal** dans [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou la fonction [fn_get_audit_file](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md) pour lire le fichier journal.  
  
 Voici le processus général employé pour créer et utiliser un audit.  
  
1.  Créez un audit et définissez la cible.  
  
2.  Créez une spécification de l'audit du serveur ou une spécification de l'audit de la base de données mappée à l'audit. Activez la spécification d'audit.  
  
3.  Activez l'audit.  
  
4.  Lisez les événements d’audit à l’aide de l’ **Observateur d’événements** Windows, de la **Visionneuse du fichier journal** ou de la fonction fn_get_audit_file.  

 Pour plus d'informations, consultez [Créer un audit du serveur et une spécification d’audit du serveur](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md) et [Créer une spécification de l’audit du serveur et de la base de données](../../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md).  
  
## <a name="considerations"></a>Considérations  
 En cas d'échec pendant le lancement de l'audit, le serveur ne démarre pas. Dans ce cas, vous pouvez démarrer le serveur en saisissant l’option **-f** sur la ligne de commande.  
  
 Lorsqu'un échec de l'audit provoque l'arrêt ou empêche le démarrage du serveur car l'instruction ON_FAILURE=SHUTDOWN est spécifiée pour l'audit, l'événement MSG_AUDIT_FORCED_SHUTDOWN est écrit dans le journal. Étant donné que l'arrêt se produit lors de la première rencontre de ce paramètre, l'événement est écrit une seule fois. Cet événement est écrit après le message d'échec d'audit qui provoque l'arrêt. Un administrateur peut contourner les arrêts provoqués par l’audit en démarrant [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en mode mono-utilisateur, à l’aide de l’indicateur **-m**. Un démarrage en mode mono-utilisateur rétrograde les audits pour lesquels ON_FAILURE=SHUTDOWN est spécifié pour s'exécuter dans cette session comme ON_FAILURE=CONTINUE. Quand [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est démarré à l’aide de l’indicateur **-m**, le message MSG_AUDIT_SHUTDOWN_BYPASSED est écrit dans le journal des erreurs.  
  
 Pour plus d’informations sur les options de démarrage de service, consultez [Options de démarrage du service moteur de base de données](../../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
### <a name="attaching-a-database-with-an-audit-defined"></a>Attachement d'une base de données avec un audit défini  
 L'attachement d'une base de données qui a une spécification d'audit et spécifie un GUID qui n'existe pas sur le serveur génère une spécification d'audit *orpheline* . Étant donné qu'il n'existe pas d'audit avec un GUID correspondant sur l'instance de serveur, aucun événement d'audit n'est enregistré. Pour remédier à cette situation, utilisez la commande ALTER DATABASE AUDIT SPECIFICATION pour connecter la spécification d'audit orpheline à un audit du serveur existant. Ou utilisez la commande CREATE SERVER AUDIT pour créer un nouvel audit de serveur avec le GUID spécifié.  
  
 Vous pouvez attacher une base de données pour laquelle une spécification d'audit est définie à une autre édition de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui ne prend pas en charge l'audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , telle que [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)] , mais les événements d'audit ne seront pas enregistrés.  
  
### <a name="database-mirroring-and-sql-server-audit"></a>Mise en miroir de bases de données et SQL Server Audit  
 Une base de données qui possède une spécification d'audit définie et qui utilise la mise en miroir de bases de données inclut la spécification de l'audit de la base de données. Pour fonctionner correctement sur l'instance SQL en miroir, les éléments suivants doivent être configurés :  
  
-   Le serveur miroir doit avoir un audit avec le même GUID afin de permettre à la spécification de l'audit de la base de données d'écrire des enregistrements d'audit. La commande CREATE AUDIT WITH GUID **=** _\<GUID from source Server Audit_> permet d’obtenir cette configuration.  
  
-   Si la cible est un fichier binaire, le compte de service de serveur miroir doit avoir des autorisations appropriées pour l'emplacement où le journal d'audit est écrit.  
  
-   Si la cible est le journal des événements Windows, la stratégie de sécurité sur l'ordinateur où le serveur miroir se trouve doit autoriser l'accès du compte de service au journal des événements de sécurité ou des applications.  
  
### <a name="auditing-administrators"></a>Administrateurs d'audit  
 Les membres du rôle serveur fixe **sysadmin** sont identifiés comme utilisateur **dbo** dans toutes les bases de données. Pour auditer les actions des administrateurs, auditez les actions de l'utilisateur **dbo** .  
  
## <a name="creating-and-managing-audits-with-transact-sql"></a>Création et gestion d'audits avec Transact-SQL  
 Vous pouvez utiliser des instructions DDL, des vues et fonctions de gestion dynamique et des affichages catalogue pour implémenter tous les aspects de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit.  
  
### <a name="data-definition-language-statements"></a>Instructions DDL (Data Definition Language)  
 Vous pouvez utiliser les instructions DDL suivantes pour créer, modifier et supprimer des spécifications d'audit :  
  
|Instructions DDL|Description| 
|-|-|  
|[ALTER AUTHORIZATION](../../../t-sql/statements/alter-authorization-transact-sql.md)|Change la propriété d'un élément sécurisable.|  
|[ALTER DATABASE AUDIT SPECIFICATION](../../../t-sql/statements/alter-database-audit-specification-transact-sql.md)|Modifie un objet de spécification d’audit de base de données à l’aide de la fonctionnalité SQL Server Audit.|  
|[ALTER SERVER AUDIT](../../../t-sql/statements/alter-server-audit-transact-sql.md)|Modifie un objet d’audit du serveur à l’aide de la fonctionnalité SQL Server Audit.|  
|[ALTER SERVER AUDIT SPECIFICATION](../../../t-sql/statements/alter-server-audit-specification-transact-sql.md)|Modifie un objet de spécification d’audit de serveur à l’aide de la fonctionnalité SQL Server Audit.|  
|[CREATE DATABASE AUDIT SPECIFICATION](../../../t-sql/statements/create-database-audit-specification-transact-sql.md)|Crée un objet de spécification d’audit de base de données à l’aide de la fonctionnalité SQL Server Audit.|  
|[CREATE SERVER AUDIT](../../../t-sql/statements/create-server-audit-transact-sql.md)|Crée un objet d’audit de serveur à l’aide de SQL Server Audit.|  
|[CREATE SERVER AUDIT SPECIFICATION](../../../t-sql/statements/create-server-audit-specification-transact-sql.md)|Crée un objet de spécification d’audit de serveur à l’aide de la fonctionnalité SQL Server Audit.|  
|[DROP DATABASE AUDIT SPECIFICATION](../../../t-sql/statements/drop-database-audit-specification-transact-sql.md)|Supprime un objet de spécification d’audit de base de données à l’aide de la fonctionnalité SQL Server Audit.|  
|[DROP SERVER AUDIT](../../../t-sql/statements/drop-server-audit-transact-sql.md)|Supprime un objet d'audit du serveur à l'aide de la fonctionnalité SQL Server Audit.|  
|[DROP SERVER AUDIT SPECIFICATION](../../../t-sql/statements/drop-server-audit-specification-transact-sql.md)|Supprime un objet de spécification d’audit de serveur à l’aide de la fonctionnalité SQL Server Audit.|  
  
### <a name="dynamic-views-and-functions"></a>Fonctions et vues dynamiques  
 Le tableau suivant répertorie les vues et fonctions dynamiques que vous pouvez utiliser pour l'audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
|Fonctions et vues dynamiques|Description|  
|---------------------------------|-----------------|  
|[sys.dm_audit_actions](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)|Retourne une ligne pour chaque action d'audit qui peut être signalée dans le journal d'audit et chaque groupe d'actions d'audit qui peut être configuré dans le cadre de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit.|  
|[sys.dm_server_audit_status](../../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)|Fournit des informations sur l'état actuel de l'audit.|  
|[sys.dm_audit_class_type_map](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)|Retourne une table qui mappe le champ class_type dans le journal d'audit au champ class_desc dans sys.dm_audit_actions.|  
|[fn_get_audit_file](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)|Retourne des informations à partir d'un fichier d'audit créé par un audit du serveur.|  
  
### <a name="catalog-views"></a>Affichages catalogue  
 Le tableau suivant répertorie les affichages catalogue que vous pouvez utiliser pour l'audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
|Affichages catalogue|Description|  
|-------------------|-----------------|  
|[sys.database_audit_specifications](../../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)|Contient des informations sur les spécifications de l'audit de la base de données dans un audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur une instance de serveur.|  
|[sys.database_audit_specification_details](../../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)|Contient des informations sur les spécifications de l’audit de la base de données dans un audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur une instance de serveur, pour toutes les bases de données.|  
|[sys.server_audits](../../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)|Contient une ligne pour chaque audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] dans une instance de serveur.|  
|[sys.server_audit_specifications](../../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)|Contient des informations à propos des spécifications de l'audit du serveur dans un audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur une instance de serveur.|  
|[sys.server_audit_specifications_details](../../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)|Contient des informations sur les détails (actions) d’une spécification de l’audit du serveur dans un audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur une instance de serveur.|  
|[sys.server_file_audits](../../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)|Contient les informations détaillées des magasins à propos du type d'audit de fichier dans un audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur une instance de serveur.|  
  
## <a name="permissions"></a>Autorisations  
 Chaque fonctionnalité et commande pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit a des spécifications d'autorisation individuelles.  
  
 Pour créer, modifier ou supprimer un audit du serveur ou une spécification de l'audit du serveur, les principaux du serveur requièrent l'autorisation ALTER ANY SERVER AUDIT ou CONTROL SERVER. Pour créer, modifier ou supprimer une spécification de l'audit de la base de données, les principaux de la base de données requièrent l'autorisation ALTER ANY DATABASE AUDIT, ou l'autorisation ALTER ou CONTROL sur la base de données. De plus, les principaux doivent soit avoir l'autorisation de se connecter à la base de données, soit disposer des autorisations ALTER ANY SERVER AUDIT ou CONTROL SERVER.  
  
 L’autorisation VIEW ANY DEFINITION fournit l’accès permettant d’afficher les vues d’audit au niveau du serveur ; l’autorisation VIEW DEFINITION fournit un accès permettant d’afficher les vues d’audit au niveau de la base de données. Si ces autorisations sont refusées, il n’est plus possible d’afficher les vues de catalogue, même si le principal dispose de l’autorisation ALTER ANY SERVER AUDIT ou ALTER ANY DATABASE AUDIT.  
  
 Pour plus d’informations sur l’octroi de droits et d’autorisations, consultez [GRANT &#40;Transact-SQL&#41;](../../../t-sql/statements/grant-transact-sql.md).  
  
> [!CAUTION]  
>  Les principaux dans le rôle sysadmin peuvent falsifier tout composant d'audit et ceux dans le rôle db_owner peuvent falsifier les spécifications d'audit dans une base de données. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Audit s'assure qu'une ouverture de session qui crée ou modifie une spécification d'audit possède au moins l'autorisation ALTER ANY DATABASE AUDIT. Toutefois, aucune validation n'est effectuée lorsque vous attachez une base de données. Vous devez supposer que toutes les spécifications de l'audit de la base de données sont aussi dignes de confiance que les principaux dans le rôle sysadmin ou db_owner.  
  
## <a name="related-tasks"></a>Tâches associées  
 [Créer un audit du serveur et une spécification d’audit du serveur](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
 [Créer une spécification de l’audit du serveur et de la base de données](../../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md)  
  
 [Afficher un journal d’audit SQL Server](../../../relational-databases/security/auditing/view-a-sql-server-audit-log.md)  
  
 [Écrire des événements d’audit SQL Server dans le journal de sécurité](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md)  
  
## <a name="topics-closely-related-to-auditing"></a>Rubriques étroitement associées à l'audit  
 [Propriétés du serveur &#40;page Sécurité&#41;](../../../database-engine/configure-windows/server-properties-security-page.md)  
 Explique comment activer l'audit de connexion pour [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les enregistrements d'audit sont stockés dans le journal des applications Windows.  
  
 [Mode d’audit C2 (option de configuration de serveur)](../../../database-engine/configure-windows/c2-audit-mode-server-configuration-option.md)  
 Explique le mode d'audit de compatibilité de sécurité C2 dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Catégorie d’événements d’audit de sécurité &#40;SQL Server Profiler&#41;](../../../relational-databases/event-classes/security-audit-event-category-sql-server-profiler.md)  
 Explique les événements d'audit que vous pouvez utiliser dans [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)]. Pour en savoir plus, voir [SQL Server Profiler](../../../tools/sql-server-profiler/sql-server-profiler.md).  
  
 [Trace SQL](../../../relational-databases/sql-trace/sql-trace.md)  
 Explique comment utiliser Trace SQL à partir de vos propres applications pour créer des traces manuellement au lieu d’utiliser [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Profiler.  
  
 [Déclencheurs DDL](../../../relational-databases/triggers/ddl-triggers.md)  
 Explique comment utiliser des déclencheurs DDL (Data Definition Language) pour effectuer le suivi des modifications de vos bases de données.  
  
 [Microsoft TechNet - TechCenter SQL Server : Sécurité et protection SQL Server 2005](../../../sql-server/index.yml)  
 Fournit des informations à jour sur la sécurité de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>Voir aussi  
 [Actions et groupes d’actions SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)   
 [Enregistrements SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-records.md)  
  
