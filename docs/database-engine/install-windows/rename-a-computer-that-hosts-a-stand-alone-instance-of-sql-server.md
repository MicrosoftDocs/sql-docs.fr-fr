---
title: Renommer l’instance d’hébergement d’un ordinateur
description: Lorsque vous renommez un ordinateur qui héberge une instance de SQL Server, mettez à jour les métadonnées système stockées dans sys.servers.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- remote login errors [SQL Server]
- standalone computer names [SQL Server]
- names [SQL Server], standalone instances of SQL Server
- renaming standalone instances of SQL Server
- sysservers system table
- removing remote logins
- deleting remote logins
- dropping remote logins
ms.assetid: bbaf1445-b8a2-4ebf-babe-17d8cf20b037
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b84f700ab19b940d8aa742be94dc2c3ab3726507
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353046"
---
# <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server"></a>Renommer un ordinateur qui héberge une instance autonome de SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Lorsque vous modifiez le nom de l'ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le nouveau nom est reconnu au démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous n'avez pas besoin de réexécuter le programme d'installation pour réinitialiser le nom d'ordinateur. À la place, utilisez la procédure suivante pour mettre à jour les métadonnées système qui sont stockées dans sys.servers et signalées par la fonction système @@SERVERNAME . Mettez à jour les métadonnées système pour refléter les modifications opérées dans les noms d’ordinateurs pour les connexions à distance et les applications qui utilisent @@SERVERNAME, ou qui interrogent le nom du serveur à partir de sys.servers.  
  
Les procédures suivantes ne vous permettent pas de renommer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elles ne peuvent être utilisées que pour renommer la partie du nom de l'instance qui correspond au nom de l'ordinateur. Par exemple, vous pouvez remplacer le nom d'un ordinateur nommé MB1 qui héberge une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommée Instance1 par un autre nom, tel que MB2. Cependant, la partie d'instance du nom, Instance1, restera inchangée. Dans cet exemple, la partie \\\\*Nom_ordinateur*\\*Nom_instance* nommée \\\MB1\Instance1 sera remplacée par \\\MB2\Instance1.  
  
 **Avant de commencer**  
  
 Avant d'entamer la procédure consistant à attribuer un nouveau nom, prenez connaissance des informations suivantes :  
  
-   Lorsqu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fait partie d'un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le processus permettant de renommer l'ordinateur diffère du processus permettant de renommer un ordinateur qui héberge une instance autonome.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge la modification de nom des ordinateurs impliqués dans la réplication, excepté lorsque vous utilisez la copie des journaux de transaction avec la réplication. L'ordinateur secondaire pour la copie des journaux de transaction peut être renommé si l'ordinateur principal est définitivement perdu. Pour plus d’informations, consultez [Copie des journaux de transaction et réplication &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   Lorsque vous renommez un ordinateur configuré pour utiliser [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] peut ne pas être disponible après la modification du nom d'ordinateur. Pour plus d’informations, consultez [Changement de nom d’un ordinateur serveur de rapports](../../reporting-services/report-server/rename-a-report-server-computer.md).  
  
-   Lorsque vous renommez un ordinateur configuré pour utiliser la mise en miroir de bases de données, vous devez désactiver cette fonction avant de modifier le nom. Ensuite, vous devez la réactiver avec le nouveau nom de l'ordinateur. Les métadonnées de la mise en miroir de la base de données ne seront pas mises à jour automatiquement de façon à refléter le nouveau nom de l'ordinateur. Procédez comme suit pour mettre à jour les métadonnées système :  
  
-   Les utilisateurs qui se connectent à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le biais d'un groupe Windows utilisant une référence codée en dur au nom de l'ordinateur risquent de ne pas pouvoir se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela peut se produire après l'attribution du nouveau nom si le groupe Windows spécifie l'ancien nom d'ordinateur. Pour vous assurer que ces groupes Windows bénéficient de la connectivité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] après l'opération de changement de nom, mettez à jour le groupe Windows pour spécifier le nouveau nom de l'ordinateur.  
  
 Vous pouvez vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide du nouveau nom d'ordinateur après avoir redémarré [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour vous assurer que @@SERVERNAME retourne le nom mis à jour de l’instance de serveur local, vous devez exécuter manuellement la procédure suivante qui s’applique à votre scénario. La procédure à utiliser varie selon que vous mettez à jour un ordinateur qui héberge une instance par défaut ou une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-ssnoversion"></a>Renommer un ordinateur qui héberge une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Dans le cas d'un ordinateur renommé qui héberge une instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez les procédures ci-dessous :  
  
    ```sql
    EXEC sp_dropserver '<old_name>';  
    GO  
    EXEC sp_addserver '<new_name>', local;  
    GO  
    ```  
  
     Redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Dans le cas d'un ordinateur renommé qui héberge une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], exécutez les procédures ci-dessous :  
  
    ```sql
    EXEC sp_dropserver '<old_name\instancename>';  
    GO  
    EXEC sp_addserver '<new_name\instancename>', local;  
    GO  
    ```  
  
     Redémarrez l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="after-the-renaming-operation"></a>Après l'opération d'attribution d'un nouveau nom  
 Une fois l'ordinateur redémarré, toutes les connexions qui utilisaient l'ancien nom d'ordinateur doivent se connecter à l'aide du nouveau nom.  
  
## <a name="verify-renaming-operation"></a>Vérifier l’opération de renommage  
  
-   Sélectionnez des informations à partir de @@SERVERNAME ou sys.servers. La fonction @@SERVERNAME renvoie le nouveau nom et la table sys.servers affiche le nouveau nom. L’exemple suivant illustre l’utilisation de @@SERVERNAME .  
  
    ```  
    SELECT @@SERVERNAME AS 'Server Name';  
    ```  
  
## <a name="additional-considerations"></a>Considérations supplémentaires  
 **Ouvertures de sessions distantes** - Si des sessions à distance sont ouvertes sur l’ordinateur, l’exécution de **sp_dropserver** risque de générer une erreur semblable à la suivante :  
  
 `Server: Msg 15190, Level 16, State 1, Procedure sp_dropserver, Line 44 There are still remote logins for the server 'SERVER1'.`  
  
 Pour résoudre l'erreur, vous devez supprimer les ouvertures de session à distance du serveur.  
  
### <a name="drop-remote-logins"></a>Supprimer les ouvertures de session à distance  
  
-   Dans le cas d'une instance par défaut, suivez la procédure ci-dessous :  
  
    ```sql
    EXEC sp_dropremotelogin old_name;  
    GO  
    ```  
  
-   Dans le cas d'une instance nommée, suivez la procédure ci-dessous :  
  
    ```sql
    EXEC sp_dropremotelogin old_name\instancename;  
    GO  
    ```  
  
 **Configurations de serveur lié** - Les configurations de serveur lié seront affectées par l’opération d’attribution d’un nouveau nom à l’ordinateur. Utilisez **sp_addlinkedserver** ou **sp_setnetname** pour mettre à jour les références de nom d’ordinateur. Pour plus d’informations, consultez [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) ou [sp_setnetname &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md).  
  
 **Noms d’alias client** - Les alias client qui utilisent des canaux nommés seront affectés par l’opération d’attribution d’un nouveau nom à l’ordinateur. Par exemple, si un alias « PROD_SRVR » a été créé pour désigner SRVR1 et utilise le protocole de canaux nommés, le nom de canal ressemblera à `\\SRVR1\pipe\sql\query`. Après avoir renommé l'ordinateur, le chemin d'accès du canal nommé ne sera plus valide. Pour plus d’informations sur les canaux nommés, consultez [Création d’une chaîne de connexion valide à l’aide de canaux nommés](/previous-versions/sql/sql-server-2008/ms189307(v=sql.100)).  
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server](../../database-engine/install-windows/install-sql-server.md)  
  
