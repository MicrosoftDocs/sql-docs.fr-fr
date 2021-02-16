---
title: Actions et groupes d’actions SQL Server Audit | Microsoft Docs
description: En savoir plus sur les groupes d’actions au niveau du serveur, au niveau de la base de données et au niveau de l’audit, ainsi que les actions individuelles dans SQL Server Audit.
ms.custom: ''
ms.date: 07/13/2020
ms.prod: sql
ms.prod_service: security
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
f1_keywords:
- audit
helpviewer_keywords:
- audit actions [SQL Server]
- audits [SQL Server], groups
- server-level audit actions [SQL Server]
- SQL Server Audit
- audit-level audit actions [SQL Server]
- database-level audit actions [SQL Server]
- audit action groups [SQL Server]
- audits [SQL Server], actions
ms.assetid: b7422911-7524-4bcd-9ab9-e460d5897b3d
author: DavidTrigano
ms.author: datrigan
ms.openlocfilehash: 0ab7abaaa4473bdc1d4a309ce3ccf0103a33a54a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "100344424"
---
# <a name="sql-server-audit-action-groups-and-actions"></a>Actions et groupes d’actions SQL Server Audit
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  La fonctionnalité Audit de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] vous permet d’effectuer l’audit d’événements et de groupes d’événements au niveau du serveur et au niveau de la base de données. Pour plus d’informations, consultez [SQL Server Audit &#40;moteur de base de données&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont constitués de zéro ou plusieurs éléments d'action d'audit. Ces éléments d'action d'audit peuvent être un groupe d'actions, tel que Server_Object_Change_Group, ou des actions individuelles telles que des opérations SELECT sur une table.  
  
> [!NOTE]  
>  Server_Object_Change_Group inclut CREATE, ALTER et DROP pour tout objet de serveur (Base de données ou Point de terminaison).  
  
 Les audits peuvent inclure les catégories suivantes d'actions :  
  
-   Niveau serveur. Ces actions incluent des opérations de serveur, telles que des modifications de gestion et des opérations d'ouverture de session et de fermeture de session.  
  
-   Niveau base de données. Ces actions englobent les opérations DML (Data Manipulation Languages) et DDL (Data Definition Language).  
  
-   Niveau audit. Ces actions incluent des actions appartenant au processus d'audit.  
  
 Certaines actions effectuées sur des composants d'audit [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont auditées intrinsèquement dans un audit spécifique, et dans ces cas-là des événements d'audit se produisent automatiquement car l'événement s'est produit sur l'objet parent.  
  
 Les actions suivantes sont auditées intrinsèquement :  
  
-   Modification de l'état d'audit du serveur (définition de l'État sur ON ou OFF)  
  
 Les événements suivants ne sont pas audités intrinsèquement :  
  
-   CREATE SERVER AUDIT SPECIFICATION  
  
-   ALTER SERVER AUDIT SPECIFICATION  
  
-   DROP SERVER AUDIT SPECIFICATION  
  
-   CREATE DATABASE AUDIT SPECIFICATION  
  
-   ALTER DATABASE AUDIT SPECIFICATION  
  
-   DROP DATABASE AUDIT SPECIFICATION  
  
 Tous les audits sont désactivés lors de leur création initiale.  
  
## <a name="server-level-audit-action-groups"></a>Groupes d'actions d'audit de niveau serveur  
 Les groupes d’actions d’audit de niveau serveur sont des actions semblables aux classes d’événements d’audit de sécurité [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d'informations, consultez [Référence de classe d'événements SQL Server](../../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
 Le tableau suivant décrit les groupes d'actions d'audit de niveau serveur et fournit la classe d'événements SQL Server équivalente, le cas échéant.  
  
|Nom du groupe d’actions|Description|  
|-----------------------|-----------------|  
|APPLICATION_ROLE_CHANGE_PASSWORD_GROUP|Cet événement est déclenché chaque fois qu'un mot de passe est modifié pour un rôle d'application. Équivaut à la [classe d’événements Audit App Role Change Password](../../../relational-databases/event-classes/audit-app-role-change-password-event-class.md).|  
|AUDIT_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'un audit est créé, modifié ou supprimé. Cet événement est déclenché chaque fois qu'une spécification d'audit est créée, modifiée ou supprimée. Toute modification apportée à un audit est auditée dans cet audit. Équivaut à la [classe d’événements Audit Change Audit](../../../relational-databases/event-classes/audit-change-audit-event-class.md).|  
|BACKUP_RESTORE_GROUP|Cet événement est déclenché chaque fois qu'une commande de sauvegarde ou de restauration est émise. Équivaut à la [Audit Backup and Restore Event Class](../../../relational-databases/event-classes/audit-backup-and-restore-event-class.md).|  
|BATCH_COMPLETED_GROUP|Cet événement se déclenche chaque fois que l’exécution d’une opération de gestion de transaction, de texte de lot ou de procédure stockée se termine. Il se déclenche à la fin du lot et audite l’ensemble du texte du lot ou de la procédure stockée, tel qu’il est envoyé par le client, y compris le résultat. **Ajouté dans SQL Server 2019.**|  
|BATCH_STARTED_GROUP|Cet événement se déclenche chaque fois qu’une opération de gestion de transaction, de texte de lot ou de procédure stockée commence à s’exécuter. Il se déclenche avant l’exécution et audite l’ensemble du texte de lot ou de procédure stockée, tel qu’il est envoyé par le client. **Ajouté dans SQL Server 2019.**|  
|BROKER_LOGIN_GROUP|Cet événement est déclenché pour signaler des messages d'audit relatifs à la sécurité du transport Service Broker. Équivaut à la [classe d’événements Audit Broker Login](../../../relational-databases/event-classes/audit-broker-login-event-class.md).|  
|DATABASE_CHANGE_GROUP|Cet événement est déclenché lors de la création, de la modification ou de la suppression d'une base de données. Cet événement est déclenché chaque fois qu'une base de données est créée, modifiée ou supprimée. Équivaut à la [classe d’événements Audit Database Management](../../../relational-databases/event-classes/audit-database-management-event-class.md).|  
|DATABASE_LOGOUT_GROUP|Cet événement est déclenché quand un utilisateur de base de données autonome se déconnecte d’une base de données. Équivaut à la [classe d’événements Audit Logout](../../../relational-databases/event-classes/audit-logout-event-class.md).|  
|DATABASE_MIRRORING_LOGIN_GROUP|Cet événement est déclenché pour signaler des messages d'audit relatifs à la sécurité du transport de la mise en miroir de bases de données. Équivaut à la [classe d’événements Audit Database Mirroring Login](../../../relational-databases/event-classes/audit-database-mirroring-login-event-class.md).|  
|DATABASE_OBJECT_ACCESS_GROUP|Cet événement est déclenché à chaque accès à des objets de base de données tels qu'un type de message, un assembly ou un contrat. Cet événement est déclenché pour tout accès à toute base de données. Remarque : Cela peut générer des enregistrements d’audit volumineux.<br /><br /> Équivaut à la [classe d’événements Audit Database Object Access](../../../relational-databases/event-classes/audit-database-object-access-event-class.md).|  
|DATABASE_OBJECT_CHANGE_GROUP|Cet événement est déclenché lorsqu'une instruction CREATE, ALTER ou DROP est exécutée sur des objets de base de données, tels que des schémas. Cet événement est déclenché chaque fois qu'un objet de base de données est créé, modifié ou supprimé. Remarque : Cela peut générer une très grande quantité d’enregistrements d’audit.<br /><br /> Équivaut à la [classe d’événements Audit Database Object Management](../../../relational-databases/event-classes/audit-database-object-management-event-class.md).|  
|DATABASE_OBJECT_OWNERSHIP_CHANGE_GROUP|Cet événement est déclenché lorsqu'une modification de propriétaire pour des objets dans l'étendue de la base de données a lieu. Cet événement est déclenché pour toute modification de propriétaire d'objet dans une base de données sur le serveur. Équivaut à la [classe d’événements Audit Database Object Take Ownership](../../../relational-databases/event-classes/audit-database-object-take-ownership-event-class.md).|  
|DATABASE_OBJECT_PERMISSION_CHANGE_GROUP|Cet événement est déclenché lorsqu'une instruction GRANT, REVOKE ou DENY a été émise pour des objets de base de données, tels que des assemblys et des schémas. Cet événement est déclenché pour toute modification d'autorisation d'objet pour une base de données sur le serveur. Équivaut à la [classe d’événements Audit Database Object GDR](../../../relational-databases/event-classes/audit-database-object-gdr-event-class.md).|  
|DATABASE_OPERATION_GROUP|Cet événement est déclenché lorsque des opérations dans une base de données, telles qu'un point de contrôle ou une notification de requête d'abonnement, se produisent. Cet événement est déclenché sur toute opération de base de données sur toute base de données. Équivaut à la [classe d’événements Audit Database Operation](../../../relational-databases/event-classes/audit-database-operation-event-class.md).|  
|DATABASE_OWNERSHIP_CHANGE_GROUP|Cet événement est déclenché lorsque vous utilisez l'instruction ALTER AUTHORIZATION pour changer le propriétaire d'une base de données et que les autorisations requises à cet effet sont activées. Cet événement est déclenché pour toute modification du propriétaire de la base de données sur une base de données sur le serveur. Équivaut à la [classe d’événements Audit Change Database Owner](../../../relational-databases/event-classes/audit-change-database-owner-event-class.md).|  
|DATABASE_PERMISSION_CHANGE_GROUP|Cet événement est déclenché chaque fois qu’une instruction GRANT, REVOKE ou DENY est émise pour une autorisation d’instruction par un principal dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (cela s’applique aux événements de base de données uniquement, tels que l’accord d’autorisations sur une base de données).<br /><br /> Cet événement est déclenché pour toute modification d'autorisation de base de données pour une base de données dans le serveur. Équivaut à la [classe d’événements Audit Database Scope GDR](../../../relational-databases/event-classes/audit-database-scope-gdr-event-class.md).|  
|DATABASE_PRINCIPAL_CHANGE_GROUP|Cet événement est déclenché lorsque des principaux, tels que des utilisateurs, sont créés, modifiés ou supprimés d'une base de données. Équivaut à la [classe d’événements Audit Database Principal Management](../../../relational-databases/event-classes/audit-database-principal-management-event-class.md). (Également équivalent à la Classe d'événements d'audit Add DB Principal, qui se produit sur les procédures stockées déconseillées sp_grantdbaccess, sp_revokedbaccess, sp_addPrincipal et sp_dropPrincipal.)<br /><br /> Cet événement est déclenché chaque fois qu'un rôle de base de données est ajouté ou supprimé à l'aide des procédures stockées sp_addrole et sp_droprole. Cet événement est déclenché lorsque des principaux de base de données sont créés, modifiés ou supprimés d'une base de données. Équivaut à la [classe d’événements Audit Add Role](../../../relational-databases/event-classes/audit-add-role-event-class.md).|  
|DATABASE_PRINCIPAL_IMPERSONATION_GROUP|Cet événement est déclenché en cas d’opération d’emprunt d’identité dans la portée de la base de données, telle que EXECUTE AS \<principal>ou SETPRINCIPAL. Cet événement est déclenché pour les emprunts d'identité effectués dans une base de données. Équivaut à la [classe d’événements Audit Database Principal Impersonation](../../../relational-databases/event-classes/audit-database-principal-impersonation-event-class.md).|  
|DATABASE_ROLE_MEMBER_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'une connexion est ajoutée à un rôle de base de données ou en est supprimée. Cette classe d'événements est déclenchée pour les procédures stockées sp_addrolemember, sp_changegroup et sp_droprolemember. Cet événement est déclenché en cas de modification d'un membre de rôle Base de données dans toute base de données. Équivaut à la [classe d’événements Audit Add Member to DB Role](../../../relational-databases/event-classes/audit-add-member-to-db-role-event-class.md).|  
|DBCC_GROUP|Cet événement est déclenché chaque fois qu'un principal émet une commande DBCC. Équivaut à la [classe d’événements Audit DBCC](../../../relational-databases/event-classes/audit-dbcc-event-class.md).|  
|FAILED_DATABASE_AUTHENTICATION_GROUP|Indique qu'un principal a tenté de se connecter à une base de données autonome et a échoué. Les événements de cette classe sont déclenchés par de nouvelles connexions ou par des connexions réutilisées depuis un groupement de connexions. Équivaut à la [classe d’événements Audit Login Failed](../../../relational-databases/event-classes/audit-login-failed-event-class.md).|    
|FAILED_LOGIN_GROUP|Indique qu'un principal a essayé de se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et a échoué. Les événements de cette classe sont déclenchés par de nouvelles connexions ou par des connexions réutilisées depuis un groupement de connexions. Équivaut à la [classe d’événements Audit Login Failed](../../../relational-databases/event-classes/audit-login-failed-event-class.md). Cet audit ne s’applique pas à Azure SQL Database.| 
|FULLTEXT_GROUP|Indique qu'un événement de texte intégral s'est produit. Équivaut à la [classe d’événements Audit Fulltext](../../../relational-databases/event-classes/audit-fulltext-event-class.md).|  
|LOGIN_CHANGE_PASSWORD_GROUP|Cet événement est déclenché chaque fois qu'un mot de passe de connexion est modifié via une instruction ALTER LOGIN ou une procédure stockée sp_password. Équivaut à la [classe d’événements Audit Login Change Password](../../../relational-databases/event-classes/audit-login-change-password-event-class.md).|  
|LOGOUT_GROUP|Indique qu'un principal s'est déconnecté de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les événements de cette classe sont déclenchés par de nouvelles connexions ou par des connexions réutilisées depuis un groupement de connexions. Équivaut à la [classe d’événements Audit Logout](../../../relational-databases/event-classes/audit-logout-event-class.md).|  
|SCHEMA_OBJECT_ACCESS_GROUP|Cet événement est déclenché chaque fois qu'une autorisation d'objet a été utilisée dans le schéma. Équivaut à la [classe d’événements Audit Schema Object Access](../../../relational-databases/event-classes/audit-schema-object-access-event-class.md).|  
|SCHEMA_OBJECT_CHANGE_GROUP|Cet événement est déclenché lorsqu'une opération CREATE, ALTER ou DROP est effectuée sur un schéma. Équivaut à la [classe d’événements Audit Schema Object Management](../../../relational-databases/event-classes/audit-schema-object-management-event-class.md).<br /><br /> Cet événement est déclenché sur les objets de schéma. Équivaut à la [classe d’événements Audit Object Derived Permission](../../../relational-databases/event-classes/audit-object-derived-permission-event-class.md).<br /><br /> Cet événement est déclenché chaque fois qu'un schéma d'une base de données est modifié. Équivaut à la [classe d’événements Audit Statement Permission](../../../relational-databases/event-classes/audit-statement-permission-event-class.md).|  
|SCHEMA_OBJECT_OWNERSHIP_CHANGE_GROUP|Cet événement est déclenché lorsque les autorisations nécessaires pour modifier le propriétaire d'un objet de schéma (tel qu'une table, procédure ou fonction) sont activées. Cela intervient lorsque l'instruction ALTER AUTHORIZATION est utilisée pour affecter un propriétaire à un objet. Cet événement est déclenché pour toute modification de propriétaire de schéma pour une base de données sur le serveur. Équivaut à la [classe d’événements Audit Schema Object Take Ownership](../../../relational-databases/event-classes/audit-schema-object-take-ownership-event-class.md).|  
|SCHEMA_OBJECT_PERMISSION_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'une opération d'accord, de refus ou de révocation est effectuée contre un objet de schéma. Équivaut à la [classe d’événements Audit Schema Object GDR](../../../relational-databases/event-classes/audit-schema-object-gdr-event-class.md).|  
|SERVER_OBJECT_CHANGE_GROUP|Cet événement est déclenché pour les opérations CREATE, ALTER ou DROP sur des objets de serveur. Équivaut à la [classe d’événements Audit Server Object Management](../../../relational-databases/event-classes/audit-server-object-management-event-class.md).|  
|SERVER_OBJECT_OWNERSHIP_CHANGE_GROUP|Cet événement est déclenché lors de la modification du propriétaire par des objets dans la portée du serveur. Équivaut à la [classe d’événements Audit Server Object Take Ownership](../../../relational-databases/event-classes/audit-server-object-take-ownership-event-class.md).|  
|SERVER_OBJECT_PERMISSION_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'une instruction GRANT, REVOKE ou DENY est émise pour une autorisation d'objet de serveur par un principal dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Équivaut à la [classe d’événements Audit Server Object GDR](../../../relational-databases/event-classes/audit-server-object-gdr-event-class.md).|  
|SERVER_OPERATION_GROUP|Cet événement est déclenché lorsque des opérations d'audit de sécurité, telles que la modification de paramètres, de ressources, d'accès externe ou d'autorisation sont utilisées. Équivaut à la [classe d’événements Audit Server Operation](../../../relational-databases/event-classes/audit-server-operation-event-class.md).|  
|SERVER_PERMISSION_CHANGE_GROUP|Cet événement est déclenché quand une instruction GRANT, REVOKE ou DENY est émise pour les autorisations dans la portée du serveur. Équivaut à la [classe d’événements Audit Server Scope GDR](../../../relational-databases/event-classes/audit-server-scope-gdr-event-class.md).|  
|SERVER_PRINCIPAL_CHANGE_GROUP|Cet événement est déclenché lorsque des principaux de serveur sont créés, modifiés ou supprimés. Équivaut à la [classe d’événements Audit Server Principal Management](../../../relational-databases/event-classes/audit-server-principal-management-event-class.md).<br /><br /> Cet événement est déclenché lorsqu'un principal émet les procédures stockées sp_defaultdb ou sp_defaultlanguage ou des instructions ALTER LOGIN. Équivaut à la [classe d’événements Audit Addlogin](../../../relational-databases/event-classes/audit-addlogin-event-class.md).<br /><br /> Cet événement est déclenché sur les procédures stockées sp_addlogin et sp_droplogin. Équivaut également à la [classe d’événements Audit Login Change Property](../../../relational-databases/event-classes/audit-login-change-property-event-class.md).<br /><br /> Cet événement est déclenché pour les procédures stockées sp_grantlogin ou sp_revokelogin. Équivaut à la [classe d’événements Audit Login GDR](../../../relational-databases/event-classes/audit-login-gdr-event-class.md).|  
|SERVER_PRINCIPAL_IMPERSONATION_GROUP|Cet événement est déclenché en cas d’opération d’emprunt d’identité dans la portée du serveur, telle que EXECUTE AS \<login>. Équivaut à la [classe d’événements Audit Server Principal Impersonation](../../../relational-databases/event-classes/audit-server-principal-impersonation-event-class.md).|  
|SERVER_ROLE_MEMBER_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'une connexion est ajoutée ou supprimée d'un rôle serveur fixe. Cet événement est déclenché pour les procédures stockées sp_addsrvrolemember et sp_dropsrvrolemember. Équivaut à la [classe d’événements Audit Add Login to Server Role](../../../relational-databases/event-classes/audit-add-login-to-server-role-event-class.md).|  
|SERVER_STATE_CHANGE_GROUP|Cet événement est déclenché lorsque l'état du service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est modifié. Équivaut à la [classe d’événements Audit Server Starts and Stops](../../../relational-databases/event-classes/audit-server-starts-and-stops-event-class.md).|  
|SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP|Indique qu'un principal s'est connecté à une base de données autonome.|  
|SUCCESSFUL_LOGIN_GROUP|Indique qu'un principal s'est connecté à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Les événements de cette classe sont déclenchés par de nouvelles connexions ou par des connexions réutilisées depuis un groupement de connexions. Équivaut à la [classe d’événements Audit Login](../../../relational-databases/event-classes/audit-login-event-class.md).|  
|TRACE_CHANGE_GROUP|Cet événement est déclenché pour toutes les instructions qui vérifient l'autorisation ALTER TRACE. Équivaut à la [classe d’événements Audit Server Alter Trace](../../../relational-databases/event-classes/audit-server-alter-trace-event-class.md).|  
|TRANSACTION_GROUP|Cet événement est déclenché pour les opérations BEGIN TRANSACTION et COMMIT TRANSACTION ROLLBACK TRANSACTION, à la fois pour les appels explicites à ces instructions et pour les opérations de transaction implicite. Cet événement est également déclenché pour les opérations UNDO des instructions causées par la restauration d’une transaction.|  
|USER_CHANGE_PASSWORD_GROUP|Cet événement est déclenché chaque fois que le mot de passe d'un utilisateur de base de données autonome est modifié à l'aide de l'instruction ALTER USER.|  
|USER_DEFINED_AUDIT_GROUP|Ce groupe surveille les événements déclenchés à l’aide de [sp_audit_write &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md). En règle générale, les déclencheurs ou procédures stockées incluent des appels à **sp_audit_write** pour activer l’audit d’événements importants.|  
  
### <a name="considerations"></a>Considérations  
 Les groupes d'actions de niveau serveur couvrent les actions sur toute une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Par exemple, toute vérification d'accès à un objet de schéma dans une base de données est enregistrée si le groupe d'actions approprié est ajouté à une spécification de l'audit du serveur. Dans une spécification d'audit de la base de données, seuls les accès aux objets de schéma dans cette base de données sont enregistrés.  
  
 Les actions de niveau serveur ne permettent pas un filtrage détaillé des actions au niveau de la base de données. Un audit de niveau base de données, tel que l'audit d'actions SELECT sur la table Customers pour les connexions dans le groupe Employee est requis pour implémenter le filtrage d'action détaillé. N'incluez pas d'objets dans l'étendue du serveur, tels que les vues système, dans une spécification d'audit de base de données utilisateur.  

 > [!NOTE]
 > En raison de la surcharge qu’implique l’activation de l’audit au niveau des transactions, dans [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] SP2 CU3 et [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)] CU4 et versions ultérieures, l’audit au niveau des transactions est désactivé par défaut, sauf si la conformité des critères communs est activée.  Si la conformité des critères communs est désactivée, vous pourrez toujours ajouter une action à partir de TRANSACTION_GROUP dans une spécification d’audit. Toutefois, aucune action de transaction ne sera collectée.  Si vous envisagez de configurer toutes les actions d’audit de TRANSACTION_GROUP, activez l’infrastructure d’audit au niveau des transactions en activant la conformité des critères communs (ceci est valable pour [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)] SP2 CU3 et [!INCLUDE[ssSQL17](../../../includes/sssql17-md.md)] CU4 et versions ultérieures).  Notez que dans [!INCLUDE[sssql16-md](../../../includes/sssql16-md.md)], l’audit au niveau des transactions peut également être désactivé avec l’indicateur de trace 3427 (à partir de la version SP1 CU2).
  
## <a name="database-level-audit-action-groups"></a>Groupes d'actions d'audit de niveau base de données  
 Les groupes d’actions d’audit de niveau base de données sont des actions semblables aux classes d’événements d’audit de sécurité [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour plus d'informations sur les classes d'événements, consultez [Référence de classe d'événements SQL Server](../../../relational-databases/event-classes/sql-server-event-class-reference.md).  
  
 Le tableau suivant décrit les groupes d'actions d'audit de niveau base de données et fournit leur Classe d'événements SQL Server équivalente le cas échéant.  
  
|Nom du groupe d’actions|Description|  
|-----------------------|-----------------|  
|APPLICATION_ROLE_CHANGE_PASSWORD_GROUP|Cet événement est déclenché chaque fois qu'un mot de passe est modifié pour un rôle d'application. Équivaut à la [classe d’événements Audit App Role Change Password](../../../relational-databases/event-classes/audit-app-role-change-password-event-class.md).|  
|AUDIT_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'un audit est créé, modifié ou supprimé. Cet événement est déclenché chaque fois qu'une spécification d'audit est créée, modifiée ou supprimée. Toute modification apportée à un audit est auditée dans cet audit. Équivaut à la [classe d’événements Audit Change Audit](../../../relational-databases/event-classes/audit-change-audit-event-class.md).|  
|BACKUP_RESTORE_GROUP|Cet événement est déclenché chaque fois qu'une commande de sauvegarde ou de restauration est émise. Équivaut à la [Audit Backup and Restore Event Class](../../../relational-databases/event-classes/audit-backup-and-restore-event-class.md).|  
|BATCH_COMPLETED_GROUP|Cet événement se déclenche chaque fois que l’exécution d’une opération de gestion de transaction, de texte de lot ou de procédure stockée se termine. Il se déclenche à la fin du lot et audite l’ensemble du texte du lot ou de la procédure stockée, tel qu’il est envoyé par le client, y compris le résultat.|  
|BATCH_STARTED_GROUP|Cet événement se déclenche chaque fois qu’une opération de gestion de transaction, de texte de lot ou de procédure stockée commence à s’exécuter. Il se déclenche avant l’exécution et audite l’ensemble du texte de lot ou de procédure stockée, tel qu’il est envoyé par le client.|  
|DATABASE_CHANGE_GROUP|Cet événement est déclenché lors de la création, de la modification ou de la suppression d'une base de données. Équivaut à la [classe d’événements Audit Database Management](../../../relational-databases/event-classes/audit-database-management-event-class.md).|  
|DATABASE_LOGOUT_GROUP|Cet événement est déclenché lorsqu'un utilisateur de base de données autonome se déconnecte d'une base de données.|  
|DATABASE_OBJECT_ACCESS_GROUP|Cet événement est déclenché en cas d'accès à des objets de base de données tels que des certificats et des clés asymétriques. Équivaut à la [classe d’événements Audit Database Object Access](../../../relational-databases/event-classes/audit-database-object-access-event-class.md).|  
|DATABASE_OBJECT_CHANGE_GROUP|Cet événement est déclenché lorsqu'une instruction CREATE, ALTER ou DROP est exécutée sur des objets de base de données, tels que des schémas. Équivaut à la [classe d’événements Audit Database Object Management](../../../relational-databases/event-classes/audit-database-object-management-event-class.md).|  
|DATABASE_OBJECT_OWNERSHIP_CHANGE_GROUP|Cet événement est déclenché lorsqu'une modification de propriétaire pour des objets dans la portée de la base de données a lieu. Équivaut à la [classe d’événements Audit Database Object Take Ownership](../../../relational-databases/event-classes/audit-database-object-take-ownership-event-class.md).|  
|DATABASE_OBJECT_PERMISSION_CHANGE_GROUP|Cet événement est déclenché lorsqu'une instruction GRANT, REVOKE ou DENY a été émise pour des objets de base de données, tels que des assemblys et des schémas. Équivaut à la [classe d’événements Audit Database Object GDR](../../../relational-databases/event-classes/audit-database-object-gdr-event-class.md).|  
|DATABASE_OPERATION_GROUP|Cet événement est déclenché lorsque des opérations dans une base de données, telles qu'un point de contrôle ou une notification de requête d'abonnement, se produisent. Équivaut à la [classe d’événements Audit Database Operation](../../../relational-databases/event-classes/audit-database-operation-event-class.md).|  
|DATABASE_OWNERSHIP_CHANGE_GROUP|Cet événement est déclenché lorsque vous utilisez l'instruction ALTER AUTHORIZATION pour changer le propriétaire d'une base de données et que les autorisations requises à cet effet sont activées. Équivaut à la [classe d’événements Audit Change Database Owner](../../../relational-databases/event-classes/audit-change-database-owner-event-class.md).|  
|DATABASE_PERMISSION_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'une instruction GRANT, REVOKE ou DENY est émise pour une autorisation d'instruction par tout utilisateur dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour les événements de base de données uniquement, tels que l'accord d'autorisations sur une base de données. Équivaut à la [classe d’événements Audit Database Scope GDR](../../../relational-databases/event-classes/audit-database-scope-gdr-event-class.md).|  
|DATABASE_PRINCIPAL_CHANGE_GROUP|Cet événement est déclenché lorsque des principaux, tels que des utilisateurs, sont créés, modifiés ou supprimés d'une base de données. Équivaut à la [classe d’événements Audit Database Principal Management](../../../relational-databases/event-classes/audit-database-principal-management-event-class.md). Équivaut également à la [classe d’événements Audit Add DB User](../../../relational-databases/event-classes/audit-add-db-user-event-class.md), qui se produit sur les procédures stockées déconseillées sp_grantdbaccess, sp_revokedbaccess, sp_adduser et sp_dropuser.<br /><br /> Cet événement est déclenché chaque fois qu'un rôle de base de données est ajouté ou supprimé à l'aide des procédures stockées sp_addrole et sp_droprole déconseillées. Équivaut à la [classe d’événements Audit Add Role](../../../relational-databases/event-classes/audit-add-role-event-class.md).|  
|DATABASE_PRINCIPAL_IMPERSONATION_GROUP|Cet événement est déclenché en cas d’emprunt d’identité dans la portée de la base de données, par exemple EXECUTE AS \<user>. Équivaut à la [classe d’événements Audit Database Principal Impersonation](../../../relational-databases/event-classes/audit-database-principal-impersonation-event-class.md).|  
|DATABASE_ROLE_MEMBER_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'une connexion est ajoutée à un rôle de base de données ou en est supprimée. Cette classe d’événements est utilisée avec les procédures stockées sp_addrolemember, sp_changegroup et sp_droprolemember. Équivaut à la [classe d’événements Audit Add Member to DB Role](../../../relational-databases/event-classes/audit-add-member-to-db-role-event-class.md)|  
|DBCC_GROUP|Cet événement est déclenché chaque fois qu'un principal émet une commande DBCC. Équivaut à la [classe d’événements Audit DBCC](../../../relational-databases/event-classes/audit-dbcc-event-class.md).|  
|FAILED_DATABASE_AUTHENTICATION_GROUP|Indique qu'un principal a tenté de se connecter à une base de données autonome et a échoué. Les événements de cette classe sont déclenchés par de nouvelles connexions ou par des connexions réutilisées depuis un groupement de connexions. Cet événement est déclenché.|  
|SCHEMA_OBJECT_ACCESS_GROUP|Cet événement est déclenché chaque fois qu'une autorisation d'objet a été utilisée dans le schéma. Équivaut à la [classe d’événements Audit Schema Object Access](../../../relational-databases/event-classes/audit-schema-object-access-event-class.md).|  
|SCHEMA_OBJECT_CHANGE_GROUP|Cet événement est déclenché lorsqu'une opération CREATE, ALTER ou DROP est effectuée sur un schéma. Équivaut à la [classe d’événements Audit Schema Object Management](../../../relational-databases/event-classes/audit-schema-object-management-event-class.md).<br /><br /> Cet événement est déclenché sur les objets de schéma. Équivaut à la [classe d’événements Audit Object Derived Permission](../../../relational-databases/event-classes/audit-object-derived-permission-event-class.md). Équivaut également à la [classe d’événements Audit Statement Permission](../../../relational-databases/event-classes/audit-statement-permission-event-class.md).|  
|SCHEMA_OBJECT_OWNERSHIP_CHANGE_GROUP|Cet événement est déclenché lorsque les autorisations nécessaires pour modifier le propriétaire d'un objet de schéma, tel qu'une table, procédure ou fonction, sont activées. Cela intervient lorsque l'instruction ALTER AUTHORIZATION est utilisée pour affecter un propriétaire à un objet. Équivaut à la [classe d’événements Audit Schema Object Take Ownership](../../../relational-databases/event-classes/audit-schema-object-take-ownership-event-class.md).|  
|SCHEMA_OBJECT_PERMISSION_CHANGE_GROUP|Cet événement est déclenché chaque fois qu'une opération d'accord, de refus ou de révocation est émise pour un objet de schéma. Équivaut à la [classe d’événements Audit Schema Object GDR](../../../relational-databases/event-classes/audit-schema-object-gdr-event-class.md).|  
|SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP|Indique qu'un principal s'est connecté à une base de données autonome.|  
|USER_CHANGE_PASSWORD_GROUP|Cet événement est déclenché chaque fois que le mot de passe d'un utilisateur de base de données autonome est modifié à l'aide de l'instruction ALTER USER.|  
|USER_DEFINED_AUDIT_GROUP|Ce groupe surveille les événements déclenchés à l’aide de [sp_audit_write &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md).|  
  
## <a name="database-level-audit-actions"></a>Actions d'audit de niveau base de données  
 Les actions de niveau base de données prennent en charge l'audit d'actions spécifiques directement sur le schéma de base de données et sur les objets de schéma, tels que les tables, les vues, les procédures stockées, les fonctions, les procédures stockées étendues, les files d'attente, les synonymes. Les types, les collections de schémas XML, les bases de données et les schémas ne sont pas audités. L'audit d'objets de schéma peut être configuré sur le schéma et la base de données, ce qui signifie que les événements de tous les objets de schéma contenus dans le schéma ou la base de données spécifié seront audités. Le tableau suivant décrit les actions d'audit de niveau base de données.  
  
|Action|Description|  
|------------|-----------------|  
|SELECT|Cet événement est déclenché chaque fois qu'une instruction SELECT est exécutée.|  
|UPDATE|Cet événement est déclenché chaque fois qu'une instruction UPDATE est exécutée.|  
|INSERT|Cet événement est déclenché chaque fois qu'une instruction INSERT est exécutée.|  
|Suppression|Cet événement est déclenché chaque fois qu'une instruction DELETE est exécutée.|  
|Exécutez|Cet événement est déclenché chaque fois qu'une instruction EXECUTE est exécutée.|  
|RECEIVE|Cet événement est déclenché chaque fois qu'une instruction RECEIVE est exécutée.|  
|REFERENCES|Cet événement est déclenché chaque fois qu'une autorisation REFERENCES est vérifiée.|  
  
### <a name="considerations"></a>Considérations  
*  Les actions d'audit de niveau base de données ne s'appliquent pas aux colonnes.  
  
*  Lorsque le processeur de requêtes paramètre la requête, le paramètre peut apparaître dans le journal des événements d'audit au lieu des valeurs de colonnes de la requête. 
 
*  Les instructions RPC ne sont pas enregistrées. 
  
## <a name="audit-level-audit-action-groups"></a>Groupes d'actions d'audit de niveau audit  
 Vous pouvez également auditer les actions dans le processus d'audit. Ce peut être dans la portée du serveur ou dans la portée de la base de données. Dans la portée de la base de données, cela se produit seulement pour les spécifications d'audit de la base de données. Le tableau suivant décrit les groupes d'actions d'audit de niveau audit.  
  
|Nom du groupe d’actions|Description|  
|-----------------------|-----------------|  
|AUDIT_CHANGE_GROUP|Cet événement est déclenché chaque fois que l'une des commandes suivantes est exécutée :<br /><br /> CREATE SERVER AUDIT<br /><br /> ALTER SERVER AUDIT<br /><br /> DROP SERVER AUDIT<br /><br /> CREATE SERVER AUDIT SPECIFICATION<br /><br /> ALTER SERVER AUDIT SPECIFICATION<br /><br /> DROP SERVER AUDIT SPECIFICATION<br /><br /> CREATE DATABASE AUDIT SPECIFICATION<br /><br /> ALTER DATABASE AUDIT SPECIFICATION<br /><br /> DROP DATABASE AUDIT SPECIFICATION|  
  
## <a name="related-content"></a>Contenu associé  
 [Créer un audit du serveur et une spécification d’audit du serveur](../../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
 [Créer une spécification de l’audit du serveur et de la base de données](../../../relational-databases/security/auditing/create-a-server-audit-and-database-audit-specification.md)  
  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md)  
  
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-audit-transact-sql.md)  
  
 [DROP SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-server-audit-transact-sql.md)  
  
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-specification-transact-sql.md)  
  
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-server-audit-specification-transact-sql.md)  
  
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-server-audit-specification-transact-sql.md)  
  
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-audit-specification-transact-sql.md)  
  
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-audit-specification-transact-sql.md)  
  
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-database-audit-specification-transact-sql.md)  
  
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-authorization-transact-sql.md)  
  
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)  
  
 [sys.server_audits &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)  
  
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)  
  
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)  
  
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)  
  
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)  
  
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)  
  
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)  
  
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)  
  
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)  
  
  
