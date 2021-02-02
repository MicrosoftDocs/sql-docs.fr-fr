---
title: Installer SQL Server avec le stockage de partage de fichiers SMB | Microsoft Docs
description: Dans SQL Server, les bases de données système et les bases de données utilisateur du moteur de base de données peuvent être installées avec le serveur de fichiers SMB (Server Message Block) comme option de stockage.
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 8b7810b2-637e-46a3-9fe1-d055898ba639
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 229735b49a8b12411fcace1273d5035d95c631a2
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077037"
---
# <a name="install-sql-server-with-smb-fileshare-storage"></a>Installer SQL Server avec le stockage de partage de fichiers SMB

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Depuis [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], les bases de données système (Master, Model, MSDB et TempDB) et les bases de données utilisateur [!INCLUDE[ssDE](../../includes/ssde-md.md)] peuvent être installées avec le serveur de fichiers SMB (Server Message Block) comme option de stockage. Cela s'applique à la fois aux installations autonomes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux installations de cluster de basculement (FCI) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Le flux de fichier n'est pas pris en charge actuellement sur un partage de fichiers SMB.  
  
## <a name="installation-considerations"></a>Considérations relatives à l’installation  
  
### <a name="smb-fileshare-formats"></a>Formats de partage de fichiers SMB :  
 Lors de la spécification du partage de fichiers SMB, ce qui suit correspond à des formats de chemin d'accès UNC (Universal Naming Convention) pris en charge pour les bases de données autonomes et FCI :  
  
-   \\\NomServeur\NomPartage\  
  
-   \\\NomServeur\NomPartage  
  
 Pour plus d’informations sur la convention de nommage (UNC), consultez [UNC](/openspecs/windows_protocols/ms-dtyp/62e862f4-2a51-452e-8eeb-dc4ff5ee33cc).  
  
 Le chemin d'accès UNC de bouclage (chemin d'accès UNC dont le nom du serveur est localhost, 127.0.0.1 ou le nom d'ordinateur local) n'est pas pris en charge. Exemple de cas particulier : [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisant le cluster du serveur de fichiers qui est hébergé sur le même nœud sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution n'est pas non plus pris en charge. Pour éviter cette situation, il est recommandé de créer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un cluster de serveur de fichiers sur des clusters Windows distincts.  
  
 Les formats de chemin UNC ci-dessous ne sont pas pris en charge :  
  
-   Chemin de bouclage, par exemple \\\localhost\\..\ ou \\\127.0.0.1\\...\  
  
-   Partages administratifs, par exemple, \\\nom_serveur\x$  
  
-   Autres formats de chemin UNC tels que \\\\?\x:\  
  
-   Lecteurs réseau mappés.  
  
### <a name="supported-data-definition-language-ddl-statements"></a>Instructions DDL (Data Definition Language) prises en charge  
 Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] DDL et les procédures stockées du moteur de base de données suivantes prennent en charge les partages de fichiers SMB :  
  
1.  [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md)  
  
2.  [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
  
3.  [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
4.  [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)  
  
### <a name="installation-options"></a>Options d'installation  
  
-   Dans la page « Configuration du moteur de base de données » de l’interface utilisateur de l’installation, sous l’onglet « Répertoires de données », affectez au paramètre « Répertoire de données racine » la valeur « \\\fileserver1\share1\" ».  
  
-   Dans l’installation à partir de l’invite de commandes, affectez à « /INSTALLSQLDATADIR » la valeur « \\\fileserver1\share1\" ».  
  
     Voici l'exemple de syntaxe qui permet d'installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un serveur autonome à l'aide de l'option de partage de fichiers SMB :  
  
    ```  
    Setup.exe /q /ACTION=Install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="<StrongPassword>" /INSTALLSQLDATADIR="\\FileServer\Share1\" /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     Pour installer une instance du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à nœud unique avec l'instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]:  
  
    ```  
    setup.exe /q /ACTION=InstallFailoverCluster /InstanceName=MSSQLSERVER /INDICATEPROGRESS /ASSYSADMINACCOUNTS="<DomainName\UserName>" /ASDATADIR=<Drive>:\OLAP\Data /ASLOGDIR=<Drive>:\OLAP\Log /ASBACKUPDIR=<Drive>:\OLAP\Backup /ASCONFIGDIR=<Drive>:\OLAP\Config /ASTEMPDIR=<Drive>:\OLAP\Temp /FAILOVERCLUSTERDISKS="<Cluster Disk Resource Name - for example, 'Disk S:'" /FAILOVERCLUSTERNETWORKNAME="<Insert Network Name>" /FAILOVERCLUSTERIPADDRESSES="IPv4;xx.xxx.xx.xx;Cluster Network;xxx.xxx.xxx.x" /FAILOVERCLUSTERGROUP="MSSQLSERVER" /Features=AS,SQL /ASSVCACCOUNT="<DomainName\UserName>" /ASSVCPASSWORD="xxxxxxxxxxx" /AGTSVCACCOUNT="<DomainName\UserName>" /AGTSVCPASSWORD="xxxxxxxxxxx" /INSTALLSQLDATADIR="\\FileServer\Share1\" /SQLCOLLATION="SQL_Latin1_General_CP1_CS_AS" /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="xxxxxxxxxxx" /SQLSYSADMINACCOUNTS="<DomainName\UserName> /IACCEPTSQLSERVERLICENSETERMS  
    ```  
  
     Pour plus d’informations sur l’utilisation des différentes options de paramètre de ligne de commande dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], consultez [Installer SQL Server 2016 à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md).  
  
## <a name="operating-system-considerations-smb-protocol-vs-ssnoversion"></a>Considérations relatives au système d’exploitation (protocole SMB et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])  
 Les différents systèmes d'exploitation Windows disposent de différentes versions du protocole SMB, et la version du protocole SMB est transparente pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez connaître les avantages des différentes versions de protocole SMB pour [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
|Système d’exploitation|Version de protocole SMB2|Avantages de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|----------------------|---------------------------|-------------------------------------------|  
|[!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] SP 2|2|Performances améliorées par rapport aux versions SMB précédentes.<br /><br /> Durabilité, ce qui aide à la récupération lors de problèmes temporaires du réseau.|  
|[!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] SP 1, comprenant Server Core|2.1|Prise en charge de MTU volumineuses, ce qui profite aux transferts de données volumineux, par exemple lors d'opérations de sauvegarde et de restauration SQL. Cette fonction doit être activée par l'utilisateur. Pour plus d’informations sur l’activation de cette fonction, consultez [Nouveautés du protocole SMB](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff625695(v=ws.10)) (https://go.microsoft.com/fwlink/?LinkID=237319).<br /><br /> Améliorations significatives des performances, en particulier pour les charges de travail de style SQL OLTP. Ces améliorations de performances requièrent l'application d'un correctif. Pour plus d’informations sur le correctif logiciel, consultez [cet article](https://go.microsoft.com/fwlink/?LinkId=237320) (https://mskb.pkisolutions.com/kb/2536493).|  
|[!INCLUDE[win8srv](../../includes/win8srv-md.md)], comprenant Server Core|3.0|Prise en charge du basculement transparent des partages de fichiers ce qui évite tout temps d'arrêt, sans intervention nécessaire de l'administrateur de la base de données SQL ou l'administrateur du serveur de fichiers dans les configurations de cluster de serveurs de fichiers.<br /><br /> Prise en charge d'E/S dans plusieurs interfaces réseau simultanément, ainsi que tolérance à la défaillance de l'interface réseau.<br /><br /> Prise en charge des interfaces réseau avec fonctions RDMA.<br /><br /> Pour plus d’informations sur ces fonctionnalités et le protocole SMB, consultez [Vue d’ensemble du protocole SMB](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=253174).<br /><br /> Prise en charge de Scale Out File Server (SoFS) avec disponibilité continue.|  
|[!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2, comprenant Server Core|3.2|Prise en charge du basculement transparent des partages de fichiers ce qui évite tout temps d'arrêt, sans intervention nécessaire de l'administrateur de la base de données SQL ou l'administrateur du serveur de fichiers dans les configurations de cluster de serveurs de fichiers.<br /><br /> Prise en charge d'E/S dans plusieurs interfaces réseau simultanément, ainsi que tolérance à la défaillance de l'interface réseau, à l'aide de SMB Multichannel.<br /><br /> Prise en charge des interfaces réseau avec fonctions RDMA à l'aide de SMB Direct.<br /><br /> Pour plus d’informations sur ces fonctionnalités et le protocole SMB, consultez [Vue d’ensemble du protocole SMB](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831795(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=253174).<br /><br /> Prise en charge de Scale Out File Server (SoFS) avec disponibilité continue.<br /><br /> Optimisé pour les E/S de lecture/écriture de petite taille courantes sur OLTP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> MTU (Maximum Transmission Unit) est activé par défaut, ce qui améliore grandement les performances lors des transferts séquentiels de grande taille comme pour l'entrepôt de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et la sauvegarde et la restauration de base de données.|  
  
## <a name="security-considerations"></a>Considérations relatives à la sécurité  
  
-   Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le compte de service de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent disposer des autorisations de partage FULL CONTROL et des autorisations NTFS sur les dossiers de partage SMB. Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être un compte de domaine ou un compte système si un serveur de fichiers SMB est utilisé. Pour plus d’informations sur les autorisations de partage et NTFS, consultez [Autorisations de partage et NTFS sur un serveur de fichiers](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754178(v=ws.11)) (https://go.microsoft.com/fwlink/?LinkId=245535).  
  
    > [!NOTE]  
    >  Les autorisations de partage FULL CONTROL et les autorisations NTFS sur les dossiers de partage SMB doivent être limitées : au compte de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , au compte de service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et aux utilisateurs Windows avec des rôles de serveur admin.  
  
     Il est recommandé d'utiliser le compte de domaine en tant que compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le compte système est utilisé comme compte de service, accordez les autorisations pour le compte d’ordinateur au format suivant : \<*domain_name*>\\<*nom_ordinateur*>\*$*.  
  
    > [!NOTE]  
    >  Pendant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez spécifier le compte de domaine en tant que compte de service si le partage de fichiers SMB est indiqué comme option de stockage. Avec le partage de fichiers SMB, le compte système peut être spécifié comme compte de service après l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
    >   
    >  Les comptes virtuels ne peuvent pas être authentifiés sur un emplacement distant. Tous les comptes virtuels utilisent l'autorisation de compte d'ordinateur. Provisionnez le compte d’ordinateur au format \<*domain_name*>\\<*nom_ordinateur*>\*$*.  
  
-   Le compte utilisé pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir des autorisations FULL CONTROL sur le dossier de partage de fichiers SMB utilisé comme répertoire de données, ou tous les autres dossiers de données (répertoire de base de données utilisateur, répertoire du journal de la base de données utilisateur, répertoire TempDB, répertoire du journal TempDB, répertoire de sauvegarde) pendant la configuration des clusters.  
  
-   Des privilèges SeSecurityPrivilege sur le serveur de fichiers SMB doivent être accordés au compte utilisé pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour accorder ce privilège, utilisez la console de stratégie de sécurité locale sur le serveur de fichiers pour ajouter le compte utilisé pour l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la stratégie Gérer le journal d'audit et de la sécurité. Ce paramètre est disponible dans la section Attribution des droits utilisateur sous Stratégies locales dans la console Stratégie de sécurité locale.  
  
## <a name="known-issues"></a>Problèmes connus  
  
-   Après avoir détaché une base de données [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] qui réside sur le stockage attaché au réseau, vous pouvez rencontrer un problème d'autorisations sur la base de données lorsque vous essaierez de lier à nouveau la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations, consultez [Erreur 5120](../../relational-databases/errors-events/mssqlserver-5120-database-engine-error.md).
  
-   Si un partage de fichiers SMB est utilisé comme option de stockage pour une instance cluster de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par défaut le journal de diagnostic du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas être écrit dans le partage de fichiers car la DLL Resource de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne dispose pas des autorisations de lecture/écriture sur le partage de fichiers. Pour résoudre ce problème, essayez l'une des opérations suivantes :  
  
    1.  Accordez des autorisations de lecture/écriture sur le partage de fichiers à tous les objets ordinateur dans le cluster.  
  
    2.  Définissez l'emplacement des journaux de diagnostics dans un chemin d'accès local. Voir l’exemple suivant :  
  
        ```sql  
        ALTER SERVER CONFIGURATION  
        SET DIAGNOSTICS LOG PATH = 'C:\logs';  
        ```  
  
## <a name="see-also"></a>Voir aussi  
 [Planification d'une installation SQL Server](../../sql-server/install/planning-a-sql-server-installation.md)   
 [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
  
