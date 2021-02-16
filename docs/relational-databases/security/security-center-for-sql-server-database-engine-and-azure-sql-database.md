---
title: Documentation sur la sécurité pour SQL Server et Azure SQL Database
description: Référence du contenu relatif à la sécurité et à la protection pour SQL Server et Azure SQL Database.
ms.custom: seo-lt-2019
ms.date: 09/27/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- Security [SQL Server]
helpviewer_keywords:
- SQL Server, security
- security [SQL Server]
- database security [SQL Server]
- databases [SQL Server], security
ms.assetid: dfb39d16-722a-4734-94bb-98e61e014ee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 22df18f14ddb3eccee24944fa00822cb3176bcfa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340195"
---
# <a name="security-center-for-sql-server-database-engine-and-azure-sql-database"></a>Centre de sécurité pour le moteur de base de données SQL Server et Azure SQL Database
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cette page fournit des liens qui vous aideront à localiser les informations dont vous avez besoin sur la sécurité et la protection dans le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et la [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)].  
  
 **Légende**  
  
 ![Capture d’écran de la légende qui décrit les icônes de disponibilité des fonctionnalités.](../performance/media/security-center-legend.PNG "security-center-legend")  
  
##  <a name="authentication-who-are-you"></a><a name="Who"></a> Authentification : qui êtes-vous ?  
  
|||  
|-|-|  
|**Qui effectue l'authentification ?**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Authentification Windows<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png":::  Azure Active Directory|Qui effectue l'authentification ? (Windows ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])<br /><br /> [Choisir un mode d’authentification](../../relational-databases/security/choose-an-authentication-mode.md)<br /><br /> [Connexion à SQL Database avec l’authentification Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview)|  
|**Où est effectuée l’authentification ?**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Auprès de la base de données MASTER : connexions et utilisateurs de base de données<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Auprès de la base de données utilisateur : utilisateurs de base de données autonome|S’authentifier auprès de la base de données MASTER (connexions et utilisateurs de base de données)<br /><br /> [Créer une connexion SQL Server](../../relational-databases/security/authentication-access/create-a-login.md)<br /><br /> [Gestion des bases de données et des connexions dans Azure SQL Database](/previous-versions/azure/ee336235(v=azure.100))<br /><br /> [Créer un utilisateur de base de données](../../relational-databases/security/authentication-access/create-a-database-user.md)<br /><br /> <br /><br /> S'authentifier auprès d'une base de données utilisateur<br /><br /> [Utilisateurs de base de données autonome - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md)|  
|**Utilisation d'autres identités**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Informations d'identification<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Exécuter en tant qu’autre connexion<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Exécuter en tant qu’autre utilisateur de base de données|[Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)<br /><br /> [Exécuter en tant qu'autre connexion](../../t-sql/statements/execute-as-transact-sql.md)<br /><br /> [Exécuter en tant qu'autre utilisateur de base de données](../../t-sql/statements/execute-as-transact-sql.md)|  
  
##  <a name="authorization-what-can-you-do"></a><a name="What"></a> Autorisation : que pouvez-vous faire ?  
  
|||  
|-|-|  
|**Octroi, révocation et refus d'autorisations**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Classes sécurisables<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Autorisations de serveur précises<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Autorisations de base de données précises|[Hiérarchie des autorisations &#40;moteur de base de données&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)<br /><br /> [autorisations](../../relational-databases/security/permissions-database-engine.md)<br /><br /> [Éléments sécurisables](../../relational-databases/security/securables.md)<br /><br /> [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)|  
|**Sécurité par rôles**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Rôles de niveau serveur<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Rôles de niveau base de données|[Rôles de niveau serveur](../../relational-databases/security/authentication-access/server-level-roles.md)<br /><br /> [Rôles au niveau de la base de données](../../relational-databases/security/authentication-access/database-level-roles.md)|  
|**Restriction de l'accès aux données aux éléments de données sélectionnés**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Limiter l’accès aux données à l’aide de vues/procédures<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Sécurité au niveau des lignes<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Dynamic Data Masking<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Objets signés|Limiter l'accès aux données à l'aide de [Vues](../../relational-databases/views/views.md) et de [Procédures](../../relational-databases/stored-procedures/stored-procedures-database-engine.md)<br /><br /> [Sécurité au niveau des lignes (SQL Server)](../../relational-databases/security/row-level-security.md)<br /><br /> [sécurité au niveau de la ligne (base de données SQL Azure)](./row-level-security.md)<br /><br /> [Masquage dynamique des données (SQL Server)](../../relational-databases/security/dynamic-data-masking.md)<br /><br /> [Masquage dynamique des données (base de données SQL Azure)](/azure/azure-sql/database/dynamic-data-masking-overview)<br /><br /> [Objets signés](../../t-sql/statements/add-signature-transact-sql.md)|  
  
##  <a name="encryption-storing-secret-data"></a><a name="Encrypt"></a> Chiffrement : stockage de données secrètes  
  
|||  
|-|-|  
|**Chiffrement de fichiers**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Chiffrement BitLocker (au niveau du lecteur)<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Chiffrement NTFS (au niveau du dossier)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Transparent Data Encryption (au niveau du fichier)<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Chiffrement de la sauvegarde (au niveau du fichier)|[BitLocker (au niveau du lecteur)](https://support.microsoft.com/kb/2855131)<br /><br /> [Chiffrement NTFS (au niveau du dossier)](/previous-versions/tn-archive/dd163562(v=technet.10))<br /><br /> [Chiffrement transparent des données (au niveau du fichier)](../../relational-databases/security/encryption/transparent-data-encryption.md)<br /><br /> [Chiffrement de la sauvegarde (au niveau du fichier)](../../relational-databases/backup-restore/backup-encryption.md)|  
|**Chiffrement des sources**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Module Gestion de clés extensible<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Clés stockées dans Azure Key Vault<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Always Encrypted|[Module Gestion de clés extensible](../../relational-databases/security/encryption/extensible-key-management-ekm.md)<br /><br /> [Clés stockées dans le coffre de clés Azure](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)<br /><br /> [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)|  
|**Colonne, données et chiffrement à clé**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Chiffrer par certificat<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Chiffrer par clé symétrique<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Chiffrer par clé asymétrique<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Chiffrer par phrase secrète|[Chiffrer par certificat](../../t-sql/functions/encryptbycert-transact-sql.md)<br /><br /> [Chiffrer par clé asymétrique](../../t-sql/functions/encryptbyasymkey-transact-sql.md)<br /><br /> [Chiffrer par clé symétrique](../../t-sql/functions/encryptbykey-transact-sql.md)<br /><br /> [Chiffrer par mot de passe](../../t-sql/functions/encryptbypassphrase-transact-sql.md)<br /><br /> [Chiffrer une colonne de données](../../relational-databases/security/encryption/encrypt-a-column-of-data.md)|  
  
##  <a name="connection-security-restricting-and-securing"></a><a name="Connect"></a> Sécurité de la connexion : restriction et sécurisation  
  
|||  
|-|-|  
|**Protection par pare-feu**<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Paramètres du Pare-feu Windows<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Paramètres de pare-feu du service Azure<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Paramètres de pare-feu de la base de données|[Configurer un pare-feu Windows pour accéder au moteur de base de données](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)<br /><br /> [Paramètres de pare-feu Azure SQL Database](../../relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database.md)<br /><br /> [Paramètres de pare-feu du service Azure](../../relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database.md)|  
|**Chiffrement des données en transit**<br /><br /> :::image type="icon" source="../performance/media/security-center-both.png"::: Connexions SSL forcées<br /><br /> :::image type="icon" source="../performance/media/security-center-sqlserver.png"::: Connexions SSL facultatives|[Activer les connexions chiffrées dans le moteur de base de données](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)<br /><br /> [Activer les connexions chiffrées dans le moteur de base de données](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md), [Sécurité réseau](/azure/sql-database/sql-database-security-best-practice#network-security) <br /><br /> [Prise en charge de TLS 1.2 pour Microsoft SQL Server](https://support.microsoft.com/kb/3135244)|  
  
##  <a name="auditing-recording-access"></a><a name="Audit"></a> Audit : enregistrement de l’accès  
  
|||  
|-|-|  
|**Audit automatisé**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: Audit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (au niveau du serveur et de la base de données)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Audit [!INCLUDE[ssSDS](../../includes/sssds-md.md)] (au niveau de la base de données)<br /><br /> :::image type="icon" source="../../relational-databases/security/media/security-center-sqldb.png"::: Détecter les menaces| <br /><br /> [SQL Server Audit &#40moteur de base de données&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)<br /><br /> [Audit SQL Database](/azure/azure-sql/database/auditing-overview)<br /><br /> [Bien démarrer avec SQL Database Advanced Threat Protection](/azure/azure-sql/database/threat-detection-configure) <br /><br /> [SQL Database Vulnerability Assessment](/azure/sql-database/sql-vulnerability-assessment) |  
|**Audit personnalisé**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: Déclencheurs|Implémentation d’un audit personnalisé : Création de [DDL Triggers](../../relational-databases/triggers/ddl-triggers.md) et de [DML Triggers](../../relational-databases/triggers/dml-triggers.md)|  
|**Conformité**<br /><br /> :::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: Conformité|SQL Server :<br />                        [Critères courants](https://go.microsoft.com/fwlink/?LinkId=616319)<br /><br /> Base de données SQL :<br />                        [Centre de confidentialité Microsoft Azure : conformité par fonctionnalité](https://azure.microsoft.com/support/trust-center/services/)|  
  
##  <a name="sql-injection"></a><a name="SQLInjection"></a> Injection SQL  
 Une attaque par injection SQL consiste à insérer un code malveillant dans les chaînes transmises ultérieurement à [!INCLUDE[ssDE](../../includes/ssde-md.md)] en vue de leur analyse et de leur exécution. Les procédures qui permettent de créer des instructions SQL doivent être vérifiées et analysées à la recherche d'éventuelles failles autorisant cette injection, car [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] exécute toutes les requêtes syntaxiquement correctes qu'il reçoit. Tous les systèmes de base de données peuvent être la cible d’une attaque par injection SQL, et la plupart des vulnérabilités sont introduites dans l’application qui interroge [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Vous pouvez déjouer les attaques par injection SQL en utilisant des procédures stockées et des commandes paramétrées, en évitant les instructions SQL dynamiques et en limitant les autorisations de l’ensemble des utilisateurs.  Pour plus d’informations, consultez [SQL Injection](../../relational-databases/security/sql-injection.md).  
  
 Liens supplémentaires pour les programmeurs d’applications :  
  
-   [Scénarios de sécurité des applications dans SQL Server](/dotnet/framework/data/adonet/sql/application-security-scenarios-in-sql-server)  
  
-   [Écriture d’une instruction SQL dynamique sécurisée dans SQL Server](/dotnet/framework/data/adonet/sql/writing-secure-dynamic-sql-in-sql-server)  
  
-   [Guide pratique pour se protéger contre l’injection de code SQL dans ASP.NET](/previous-versions/msp-n-p/ff648339(v=pandp.10))  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main des autorisations du moteur de base de données](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)   
 [Sécurisation de SQL Server](../../relational-databases/security/securing-sql-server.md)   
 [Principaux &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Certificats et clés asymétriques SQL Server](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md)   
 [Chiffrement SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
 [Configuration de la surface d'exposition](../../relational-databases/security/surface-area-configuration.md)   
 [Mots de passe forts](../../relational-databases/security/strong-passwords.md)   
 [Propriété de base de données TRUSTWORTHY](../../relational-databases/security/trustworthy-database-property.md)   
 [Fonctionnalités et tâches du moteur de base de données](../../sql-server/what-s-new-in-sql-server-ver15.md)  
 [Protection de votre propriété intellectuelle SQL Server](../../relational-databases/security/protecting-your-sql-server-intellectual-property.md)   
  
[!INCLUDE[get-help-security](../../includes/get-help-security.md)]