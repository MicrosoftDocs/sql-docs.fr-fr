---
title: Sauvegarde SQL Server vers une URL | Microsoft Docs
description: Découvrez les concepts, les exigences et les composants nécessaires à SQL Server pour utiliser le service Stockage Blob Microsoft Azure comme destination de sauvegarde.
ms.custom: ''
ms.date: 03/25/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 11be89e9-ff2a-4a94-ab5d-27d8edf9167d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 75f74fcfca0e039b6f11bc36fd6cf694dfb756b3
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765574"
---
# <a name="sql-server-backup-to-url"></a>Sauvegarde SQL Server sur URL

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Cette rubrique présente les concepts, les exigences et les composants nécessaires pour utiliser le service Stockage Blob Microsoft Azure comme destination de sauvegarde. Les fonctionnalités de sauvegarde et de restauration sont identiques ou similaires à l'utilisation de l'option DISK ou TAPE, à quelques différences près. Ces différences et quelques exemples de code sont inclus dans cette rubrique.  
  
## <a name="overview"></a>Vue d’ensemble

Il est important de comprendre les composants et leur interaction pour effectuer une sauvegarde ou une restauration à l’aide du service Stockage Blob Microsoft Azure.  
  
 La création d’un compte de stockage Azure dans votre abonnement Azure est la première étape de ce processus. Ce compte de stockage est un compte d’administrateur disposant des autorisations administratives complètes sur tous les conteneurs et objets créés avec le compte de stockage. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut soit utiliser le nom du compte de stockage Azure et la valeur de sa clé d’accès pour s’authentifier et écrire/lire des objets blob dans le service Stockage Blob Microsoft Azure, soit utiliser un jeton de signature d’accès partagé généré sur des conteneurs spécifiques lui octroyant des droits de lecture et d’écriture. Pour plus d’informations sur les comptes de stockage Azure, consultez [À propos des comptes de stockage Azure](/azure/storage/common/storage-account-create) et, pour plus d’informations sur les signatures d’accès partagé, consultez [Signatures d’accès partagé, partie 1 : présentation du modèle SAP](/azure/storage/common/storage-sas-overview). Les informations d'identification de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stockent ces informations et sont utilisées lors des opérations de sauvegarde ou de restauration.  
  
###  <a name="backup-to-block-blob-vs-page-blob"></a><a name="blockbloborpageblob"></a> Sauvegarde sur un objet blob de blocs vs un objet blob de pages

Deux types d’objets blob peuvent être stockés dans le service Stockage Blob Microsoft Azure : les objets blob de blocs et les objets blob de pages. Pour la version 2016 et les versions ultérieures de SQL Server, l’objet blob de blocs est préféré.

si la clé de stockage est utilisée dans les informations d’identification, l’objet blob de pages sera utilisé ; si la signature d’accès partagé est utilisée, l’objet blob de blocs sera utilisé.

La sauvegarde sur un objet blob de blocs est uniquement disponible dans SQL Server 2016 ou version ultérieure. Sauvegardez sur un objet blob de blocs au lieu d’un objet blob de pages si vous exécutez SQL Server 2016 ou version ultérieure.

Les principales raisons sont les suivantes :

- La signature d’accès partagé est un moyen plus sûr que la clé de stockage pour autoriser l’accès aux objets blob.
- Vous pouvez sauvegarder sur plusieurs objets blob de blocs pour obtenir une meilleure sauvegarde, restaurer les performances et prendre en charge la sauvegarde de bases de données plus volumineuses.
- Un [objet blob de blocs](https://azure.microsoft.com/pricing/details/storage/blobs/) est moins cher qu’un [objet blob de pages](https://azure.microsoft.com/pricing/details/storage/page-blobs/).
- Les clients qui doivent effectuer une sauvegarde vers des objets blob de pages via un serveur proxy devront utiliser backuptourl.exe.

La sauvegarde d’une base de données volumineuse dans Stockage Blob est soumise aux limitations listées dans [Différences, limitations et problèmes connus du langage T-SQL pour les instances managées](/azure/sql-database/sql-database-managed-instance-transact-sql-information#backup).

Si la base de données est trop grande, vous pouvez :

- Utiliser la compression de la sauvegarde ou
- Sauvegarder sur plusieurs objets blob de blocs

#### <a name="support-on-linux-containers-and-azure-arc-enabled-sql-managed-instance"></a>Prise en charge sur Linux, les conteneurs et SQL Managed Instance compatible Azure Arc

Si l’instance SQL Server est hébergée sur Linux, notamment :

- Système d’exploitation autonome
- Conteneurs
- SQL Managed Instance avec Azure Arc
- Tout autre environnement Linux

Le seul modèle de sauvegarde sur URL pris en charge est celle qui s’effectue dans des objets blob de blocs, à l’aide de la Signature d’accès partagé.

###  <a name="microsoft-azure-blob-storage-service"></a><a name="Blob"></a> Service Stockage Blob Microsoft Azure  

**Compte de stockage :** Le compte de stockage constitue le point de départ de tous les services de stockage. Pour accéder au service Stockage Blob Microsoft Azure, commencez par créer un compte de stockage Azure. Pour plus d’informations, voir [Créez un compte de stockage](/azure/storage/common/storage-account-create).  
  
**Conteneur :** un conteneur permet de regrouper un ensemble d’objets blob, et peut stocker un nombre illimité d’objets blob. Pour écrire une sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le service Stockage Blob Microsoft Azure, au moins le conteneur racine doit être créé. Vous pouvez générer un jeton de signature d’accès partagé sur un conteneur, et accorder l’accès aux objets uniquement sur un conteneur spécifique.  
  
**Objet blob :** Fichier de tout type et de toute taille. Deux types d’objets blob peuvent être stockés dans le service Stockage Blob Microsoft Azure : les objets blob de blocs et les objets blob de pages. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] La sauvegarde peut utiliser les deux types d’objets blob en fonction de la syntaxe Transact-SQL utilisée. Les objets blob sont adressables à l’aide du format d’URL suivant : https://\<storage account>.blob.core.windows.net/\<container>/\<blob>. Pour plus d’informations sur le service Stockage Blob Microsoft Azure, consultez le [guide pratique pour utiliser le Stockage Blob à l’aide de .NET](https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/). Pour plus d’informations sur les objets blob de pages et de blocs, voir [Présentation des objets blob de blocs, des objets blob d’ajout et des objets blob de pages](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs).  
  
![Stockage Blob Azure](../../relational-databases/backup-restore/media/backuptocloud-blobarchitecture.gif "Stockage Blob Azure")  
  
**Capture instantanée Azure :** capture instantanée d’objet blob Azure effectuée à un point dans le temps. Pour plus d’informations, consultez [Création d’un instantané d’objet blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob). La sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend désormais en charge les sauvegardes de captures instantanées Azure de fichiers de base de données stockés dans le service Stockage Blob Microsoft Azure. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
###  <a name="ssnoversion-components"></a><a name="sqlserver"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Composants  

**URL :** Une URL spécifie un URI (Uniform Resource Identifier) pour un fichier de sauvegarde unique. L'URL est utilisée pour indiquer l'emplacement et le nom du fichier de sauvegarde de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . L’URL doit pointer vers un objet blob réel et pas un simple conteneur. Si l’objet blob n’existe pas, il est créé. Si un objet blob existant est spécifié, la sauvegarde échoue, sauf si l’option « WITH FORMAT » est spécifiée pour remplacer le fichier de sauvegarde existant dans l’objet blob.  
  
 Voici un exemple de valeur d’URL : http[s]://ACCOUNTNAME.blob.core.windows.net/\<CONTAINER>/\<FILENAME.bak>. HTTPS n'est pas obligatoire, mais est recommandé.  
  
**Informations d’identification :** Les informations d'identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont des objets utilisés pour stocker les informations d'authentification requises pour la connexion à une ressource en dehors de SQL Server. Ici, les processus de sauvegarde et de restauration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisent des informations d’identification pour s’authentifier auprès du service Stockage Blob Microsoft Azure, de son conteneur et de ses objets blob. Les informations d’identification contiennent soit le nom du compte de stockage et les valeurs de **clé d’accès** de celui-ci, soit l’URL du conteneur et son jeton de signature d’accès partagé. Une fois les informations d’identification créées, la syntaxe des instructions BACKUP/RESTORE détermine le type d’objet blob et les informations d’identification requises.  
  
Pour savoir comment créer une signature d’accès partagé, consultez les exemples [Création d’une signature d’accès partagé](../../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS) plus loin dans cette rubrique. Pour savoir comment créer des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez les exemples [Création d’informations d’identification](../../relational-databases/backup-restore/sql-server-backup-to-url.md#credential) plus loin dans cette rubrique.  
  
Pour plus d’informations sur les informations d’identification en général, consultez [Informations d’identification](../security/authentication-access/credentials-database-engine.md).  
  
Pour des informations sur d’autres exemples d’utilisation des informations d’identification, voir [Créer un proxy de SQL Server Agent](../../ssms/agent/create-a-sql-server-agent-proxy.md).  
  
##  <a name="security"></a><a name="security"></a> Sécurité  

Vous trouverez ci-dessous les considérations de sécurité et conditions requises pour la sauvegarde et la restauration à l’aide du service Stockage Blob Microsoft Azure.  
  
- Quand vous créez un conteneur pour le service Stockage Blob Microsoft Azure, nous vous recommandons de définir un accès **privé**. La définition d’un accès privé limite l’accès aux seuls utilisateurs ou comptes capables de fournir les informations nécessaires pour s’authentifier auprès du compte Azure.  
  
    > [!IMPORTANT]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert que, soit un nom de compte Azure et une authentification par clé d’accès, soit une signature d’accès partagé (SAP) et un jeton d’accès soient stockés dans les informations d’identification de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces informations sont utilisées pour l’authentification auprès du compte Azure lors de l’exécution d’opérations de sauvegarde ou de restauration.  
  
- Le compte d’utilisateur utilisé pour émettre les commandes BACKUP (sauvegarder) ou RESTORE (restaurer) doit figurer dans le rôle de base de données **db_backup operator** avec les autorisations **Modifier des informations d’identification** .   

##  <a name="limitations"></a><a name="limitations"></a> Limitations  
  
- SQL Server limite la taille de sauvegarde maximale prise en charge en utilisant un objet blob de pages à 1 To. La taille de sauvegarde prise en charge avec des objets blob de blocs est limitée à environ 200 Go (50 000 blocs * 4 Mo de MAXTRANSFERSIZE). Les objets blob de blocs prennent en charge l’entrelacement pour prendre en charge des tailles de sauvegarde sensiblement plus importantes.  
  
    > [!IMPORTANT]  
    >  Bien que la taille de sauvegarde maximale prise en charge par un seul objet blob de blocs soit de 200 Go, il est possible pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’écrire dans des tailles de bloc plus petites, ce qui peut amener [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à atteindre la limite de 50 000 blocs avant le transfert de l’intégralité de la sauvegarde. Divisez les sauvegardes en plusieurs fichiers (même si elles sont inférieures à 200 Go) pour éviter la limite du nombre de blocs, en particulier si vous utilisez des sauvegardes différentielles ou non compressées.

- Vous pouvez émettre des instructions de sauvegarde ou de restauration à l’aide de TSQL, SMO, des applets de commande PowerShell, ou de l’Assistant Restauration ou Sauvegarde de SQL Server Management Studio.   
  
- La création d'un nom d'unité logique n'est pas prise en charge. Par conséquent, l'ajout d'une URL comme unité de sauvegarde à l'aide de sp_dumpdevice ou de SQL Server Management Studio n'est pas pris en charge.  
  
- L'ajout d'objets blob de sauvegarde existants n'est pas pris en charge. Des sauvegardes vers un objet blob existant peuvent être remplacées uniquement à l’aide de l’option **WITH FORMAT** . Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument **WITH FORMAT** n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde file-snapshot d’origine.  
  
- La sauvegarde vers plusieurs objets blob en une seule opération est prise en charge uniquement en utilisant un jeton de signature d’accès partagé (SAP) au lieu de la clé du compte de stockage pour les informations d’identification SQL.  
  
- La spécification de **BLOCKSIZE** n’est pas prise en charge pour les objets blob de pages. 
  
- La spécification de **MAXTRANSFERSIZE** n’est pas prise en charge pour les objets blob de pages. 
  
- La spécification des options du jeu de sauvegarde **RETAINDAYS** et **EXPIREDATE** n’est pas prise en charge.  
  
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est soumis à une limite de 259 caractères pour le nom d'une unité de sauvegarde. BACKUP TO URL utilise 36 caractères pour les éléments requis utilisés pour spécifier l’URL « https://.blob.core.windows.net//.bak  », ce qui laisse 223 caractères pour les noms de compte, de conteneur et d’objet blob réunis.  

- Si votre serveur accède à Azure via un serveur proxy, vous devez utiliser l’indicateur de trace 1819, puis définir la configuration du proxy WinHTTP via l’une des méthodes suivantes :
   - L’utilitaire [proxycfg.exe](/windows/win32/winhttp/proxycfg-exe--a-proxy-configuration-tool) sur Windows XP ou Windows Server 2003 et versions antérieures. 
   - L’utilitaire [netsh.exe](/windows/win32/winsock/netsh-exe) sur Windows Vista et Windows Server 2008 ou versions ultérieures. 

- Le [stockage immuable pour Stockage Blob Azure](/azure/storage/blobs/storage-blob-immutable-storage) n’est pas pris en charge. Affectez la valeur false à la stratégie **Stockage immuable**. 
  
## <a name="supported-arguments--statements"></a>Instructions et arguments pris en charge

###  <a name="support-for-backuprestore-statements"></a><a name="Support"></a> Prise en charge des instructions de sauvegarde/restauration  
  
|Instruction de sauvegarde/restauration|Prise en charge|Exceptions|Commentaires|
|-|-|-|-|
|BACKUP|O|BLOCKSIZE et MAXTRANSFERSIZE sont pris en charge pour les objets blob de blocs. Ils ne sont pas pris en charge pour les objets blob de pages. | BACKUP sur un objet blob de blocs nécessite une Signature d’accès partagé enregistrée dans des informations d’identification SQL Server. BACKUP sur un objet blob de pages nécessite la clé de compte de stockage enregistrée dans des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et nécessite de spécifier l’argument WITH CREDENTIAL.|  
|RESTORE|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE FILELISTONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE HEADERONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE LABELONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE VERIFYONLY|O||Requiert que les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soient définies, et que l’argument WITH CREDENTIAL soit spécifié si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies en utilisant la clé du compte de stockage comme clé secrète.|  
|RESTORE REWINDONLY|-|||  
  
 Pour plus d’informations sur la syntaxe et les instructions de sauvegarde, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
 Pour plus d’informations sur la syntaxe et les instructions de restauration, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
### <a name="support-for-backup-arguments"></a>Prise en charge des arguments de sauvegarde  

|Argument|Prise en charge|Exception|Commentaires|  
|-|-|-|-|  
|DATABASE|O|||  
|LOG|O|||  
||  
|TO (URL)|O|Contrairement à DISK et TAPE, URL ne prend pas en charge la spécification ou la création d'un nom logique.|Cet argument est utilisé pour spécifier le chemin d'accès de l'URL du fichier de sauvegarde.|  
|MIRROR TO|O|||  
|**WITH OPTIONS :**||||  
|CREDENTIAL|O||L’argument WITH CREDENTIAL est pris en charge uniquement en cas d’utilisation de l’option BACKUP TO URL pour sauvegarder dans le service Stockage Blob Microsoft Azure, et uniquement si les informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont définies avec comme secret la clé du compte de stockage.|  
|FILE_SNAPSHOT|O|||  
|ENCRYPTION|O||Quand l’argument **WITH ENCRYPTION** est spécifié, la sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifie que la base de données entière a été chiffrée par TDE (Transparent Data Encryption) avant d’effectuer la sauvegarde et, dans ce cas, chiffre le fichier de sauvegarde de capture instantanée de fichier en utilisant l’algorithme spécifié pour TDE sur la base de données. Si des données de la base de données ne sont pas chiffrées (par exemple, si le processus de chiffrement n’est pas encore terminé), la sauvegarde échoue.|  
|DIFFERENTIAL|O|||  
|COPY_ONLY|O|||  
|COMPRESSION&#124;NO_COMPRESSION|O|Non prise en charge pour une sauvegarde de capture instantanée de fichier.||  
|Description|O|||  
|NAME|O|||  
|EXPIREDATE &#124; RETAINDAYS|-|||  
|NOINIT &#124; INIT|-||L'ajout aux objets blob n'est pas possible. Pour remplacer une sauvegarde, utilisez l’argument **WITH FORMAT** . Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument **WITH FORMAT** n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde d’origine.|  
|NOSKIP &#124; SKIP|-|||  
|NOFORMAT &#124; FORMAT|O||Une sauvegarde effectuée vers un objet blob existant échoue à moins que l’option **WITH FORMAT** soit spécifiée. Quand l’option **WITH FORMAT** est spécifiée, l’objet blob existant est remplacé. Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument FORMAT n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde file-snapshot d’origine. Toutefois, lors de l’utilisation de sauvegardes de capture instantanée de fichier (avec l’argument **WITH FILE_SNAPSHOT** ), l’argument **WITH FORMAT** n’est pas autorisé, pour éviter de laisser orphelines des captures instantanées de fichier qui ont été créées avec la sauvegarde d’origine.|  
|MEDIADESCRIPTION|O|||  
|MEDIANAME|O|||  
|BLOCKSIZE|O|Non pris en charge pour l’objet blob de pages. Prise en charge pour l’objet blob de blocs.| Nous recommandons BLOCKSIZE=65536 pour optimiser l’utilisation des 50 000 blocs autorisés dans un objet blob de blocs. |  
|BUFFERCOUNT|O|||  
|MAXTRANSFERSIZE|O|Non pris en charge pour l’objet blob de pages. Prise en charge pour l’objet blob de blocs.| La valeur par défaut est 1 048 576. La valeur peut aller jusqu’à 4 Mo, par incréments de 65 536 octets.</br> Nous recommandons MAXTRANSFERSIZE=4194304 pour optimiser l’utilisation des 50 000 blocs autorisés dans un objet blob de blocs. |  
|NO_CHECKSUM &#124; CHECKSUM|O|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|O|||  
|STATS|O|||  
|REWIND &#124; NOREWIND|-|||  
|UNLOAD &#124; NOUNLOAD|-|||  
|NORECOVERY &#124; STANDBY|O|||  
|NO_TRUNCATE|O|||  
  
 Pour plus d’informations sur les arguments de Backup, consultez [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  
  
### <a name="support-for-restore-arguments"></a>Prise en charge des arguments de restauration  
  
|Argument|Prise en charge|Exceptions|Commentaires|  
|-|-|-|-|  
|DATABASE|O|||  
|LOG|O|||  
|FROM (URL)|O||L'argument FROM URL est utilisé pour spécifier le chemin d'accès de l'URL du fichier de sauvegarde.|  
|**WITH Options:**||||  
|CREDENTIAL|O||L’argument WITH CREDENTIAL est pris en charge uniquement en cas d’utilisation de l’option RESTORE FROM URL pour une restauration à partir du service Stockage Blob Microsoft Azure.|  
|PARTIAL|O|||  
|RECOVERY &#124; NORECOVERY &#124; STANDBY|O|||  
|LOADHISTORY|O|||  
|MOVE|O|||  
|REPLACE|O|||  
|RESTART|O|||  
|RESTRICTED_USER|O|||  
|FILE|-|||  
|PASSWORD|O|||  
|MEDIANAME|O|||  
|MEDIAPASSWORD|O|||  
|BLOCKSIZE|O|||  
|BUFFERCOUNT|-|||  
|MAXTRANSFERSIZE|-|||  
|CHECKSUM &#124; NO_CHECKSUM|O|||  
|STOP_ON_ERROR &#124; CONTINUE_AFTER_ERROR|O|||  
|FILESTREAM|O|Non prise en charge pour la sauvegarde de capture instantanée.||  
|STATS|O|||  
|REWIND &#124; NOREWIND|-|||  
|UNLOAD &#124; NOUNLOAD|-|||  
|KEEP_REPLICATION|O|||  
|KEEP_CDC|O|||  
|ENABLE_BROKER &#124; ERROR_BROKER_CONVERSATIONS &#124; NEW_BROKER|O|||  
|STOPAT &#124; STOPATMARK &#124; STOPBEFOREMARK|O|||  
  
 Pour plus d’informations sur les arguments de Restore, consultez [Arguments RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md).  
  
##  <a name="back-up-with-ssms"></a><a name="BackupTaskSSMS"></a> Sauvegarder avec SSMS  
Vous pouvez sauvegarder une base de données vers une URL par le biais de la tâche de sauvegarde dans SQL Server Management Studio à l’aide des informations d’identification SQL Server.  
  
> [!NOTE]  
>  Pour créer une sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ou pour remplacer un support de sauvegarde existant, vous devez utiliser Transact-SQL, Powershell ou C# plutôt que la tâche de sauvegarde dans SQL Server Management Studio.  
  
 Les étapes suivantes décrivent les modifications apportées à la tâche Sauvegarder la base de données dans SQL Server Management Studio pour autoriser la sauvegarde vers le stockage Azure :  
  
1.  Dans l’ **Explorateur d’objets**, connectez-vous à une instance du moteur de base de données SQL Server et développez-la.

2.  Développez **Bases de données**, cliquez avec le bouton droit sur la base de données souhaitée, pointez sur **Tâches**, puis cliquez sur **Sauvegarder**.
  
3.  Dans la page **Général** de la section **Destination** , l’option **URL** est disponible dans la liste déroulante **Sauvegarde sur** .  L’option **URL** sert à créer une sauvegarde dans le stockage Microsoft Azure. Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner la destination de la sauvegarde** .
    1.  **Conteneur de stockage Azure :** Nom du conteneur de stockage Microsoft Azure pour stocker les fichiers de sauvegarde.  Sélectionnez un conteneur existant dans la liste déroulante ou entrez manuellement le conteneur. 
  
    2.  **Stratégie d’accès partagé :** Entrez la signature d’accès partagé pour un conteneur entré manuellement.  Ce champ n’est pas disponible si un conteneur existant a été choisi. 
  
    3.  **Fichier de sauvegarde :** Nom du fichier de sauvegarde.
    
    4.  **Nouveau conteneur :** Permet d’enregistrer un conteneur existant pour lequel vous n’avez pas de signature d’accès partagé.  Consultez [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).

> [!NOTE] 
>  **Ajouter** prend en charge plusieurs fichiers de sauvegarde et conteneurs de stockage pour un seul support de sauvegarde.

Quand vous sélectionnez **URL** comme destination, certaines options de la page **Options de support** sont désactivées.  Les rubriques suivantes contiennent d'autres informations sur la boîte de dialogue Sauvegarder la base de données :  
  
 [Sauvegarder la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)  
  
 [Sauvegarder la base de données &#40;page Options de support&#41;](../../relational-databases/backup-restore/back-up-database-media-options-page.md)  
  
 [Sauvegarder la base de données &#40;page Options de sauvegarde&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)  
  
 [Créer des informations d’identification - Authentification Azure Storage](../../relational-databases/backup-restore/create-credential-authenticate-to-azure-storage.md)  
  
##  <a name="back-up-with-maintenance-plan"></a><a name="MaintenanceWiz"></a> Sauvegarder avec un plan de maintenance  
 Comme pour la tâche de sauvegarde décrite précédemment, l’Assistant Plan de maintenance dans SQL Server Management Studio inclut l’option **URL** parmi les options de destination, ainsi que d’autres objets de prise en charge requis pour sauvegarder vers un stockage Azure, telles les informations d’identification SQL. Pour plus d’informations, voir la section **Définir les tâches de sauvegarde** dans [Using Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md#SSMSProcedure).  
  
> [!NOTE]  
>  Pour créer un jeu de sauvegarde distribué, une sauvegarde de capture instantanée de fichier [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou des informations d’identification SQL utilisant un jeton d’accès partagé, vous devez utiliser Transact-SQL, Powershell ou C# plutôt que la tâche Sauvegarder dans l’Assistant Plan de Maintenance.  
  
##  <a name="restore-with-ssms"></a><a name="RestoreSSMS"></a> Restauration avec SSMS 
La tâche Restaurer la base de données propose **URL** comme unité à partir de laquelle effectuer la restauration.  Les étapes suivantes décrivent l’utilisation de la tâche Restaurer pour une restauration à partir du service Stockage Blob Microsoft Azure : 
  
1. Cliquez avec le bouton droit sur **Bases de données** et sélectionnez **Restaurer la base de données**. 
  
2. Dans la page **Général** , sélectionnez **Unité** dans la section **Source** .
  
3. Cliquez sur le bouton Parcourir (...) pour ouvrir la boîte de dialogue **Sélectionner les unités de sauvegarde** . 

4. Sélectionnez **URL** dans la liste déroulante **Type de support de sauvegarde** .  Cliquez sur **Ajouter** pour ouvrir la boîte de dialogue **Sélectionner un emplacement de fichier de sauvegarde** .

    1. **Conteneur de stockage Azure :** nom qualifié complet du conteneur de stockage Microsoft Azure qui contient les fichiers de sauvegarde.  Sélectionnez un conteneur existant dans la liste déroulante ou entrez manuellement le nom qualifié complet du conteneur.
      
    2. **Signature d’accès partagé :**  permet d’entrer la signature d’accès partagé pour le conteneur spécifié.
      
    3. **Ajouter :**  Permet d’enregistrer un conteneur existant pour lequel vous n’avez pas de signature d’accès partagé.  Consultez [Se connecter à un abonnement Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md).
      
    4. **OK :**    SQL Server se connecte au stockage Microsoft Azure en utilisant les informations d’identification SQL spécifiées et ouvre la boîte de dialogue **Localiser le fichier de sauvegarde dans Microsoft Azure**. Les fichiers de sauvegarde résidant dans le conteneur de stockage s’affichent dans cette page. Sélectionnez le fichier à utiliser pour la restauration, puis cliquez sur **OK**. Vous revenez dans la boîte de dialogue **Sélectionner les unités de sauvegarde** et, quand vous cliquez sur **OK** , vous revenez dans la boîte de dialogue principale **Restaurer** où vous pourrez effectuer la restauration. 
  
     [Restaurer la base de données &#40;page Général&#41;](../../relational-databases/backup-restore/restore-database-general-page.md)  
  
     [Restaurer la base de données &#40;page Fichiers&#41;](../../relational-databases/backup-restore/restore-database-files-page.md)  
  
     [Restaurer la base de données &#40;page Options&#41;](../../relational-databases/backup-restore/restore-database-options-page.md)  
  
##  <a name="code-examples"></a><a name="Examples"></a> Exemples de code  

Cette section contient les exemples suivants :  
  
- [Création d’informations d’identification](#credential)  
  
- [Sauvegarde d'une base de données complète](#complete)  

- [Restauration jusqu'à une date et heure en utilisant STOPAT](#PITR)  
  
> [!NOTE]  
> Pour obtenir un tutoriel sur l’utilisation de SQL Server 2016 avec le service Stockage Blob Microsoft Azure, consultez [Tutoriel : Utilisation du service Stockage Blob Microsoft Azure avec des bases de données SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)  
  
### <a name="create-a-shared-access-signature"></a><a name="SAS"></a> Créer une signature d’accès partagé

L’exemple suivant crée des signatures d’accès partagé utilisables pour créer des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un conteneur nouvellement créé. Le script crée une signature d’accès partagé associée à une stratégie d’accès stockée. Pour plus d’informations, consultez [Signatures d’accès partagé, partie 1 : Présentation du modèle SAP](/azure/storage/common/storage-sas-overview). Le script écrit également la commande T-SQL requise pour créer les informations d’identification sur SQL Server. 

> [!NOTE]
> L’exemple nécessite Microsoft Azure Powershell. Pour plus d’informations sur l’installation et l’utilisation d’Azure Powershell, consultez [Installation et configuration d’Azure PowerShell](/powershell/azure/).  
> Ces scripts ont été vérifiés à l’aide d’Azure PowerShell 5.1.15063. 

**Signature d’accès partagé associée à une stratégie d’accès stockée**
  
```Powershell  
# Define global variables for the script  
$prefixName = '<a prefix name>'  # used as the prefix for the name for various objects  
$subscriptionName='<your subscription name>'   # the name of subscription name you will use  
$locationName = '<a data center location>'  # the data center region you will use  
$storageAccountName= $prefixName + 'storage' # the storage account name you will create or use  
$containerName= $prefixName + 'container'  # the storage container name to which you will attach the SAS policy with its SAS token  
$policyName = $prefixName + 'policy' # the name of the SAS policy  

# Set a variable for the name of the resource group you will create or use  
$resourceGroupName=$prefixName + 'rg'

# adds an authenticated Azure account for use in the session
Connect-AzAccount

# set the tenant, subscription and environment for use in the rest of
Set-AzContext -SubscriptionName $subscriptionName

# create a new resource group - comment out this line to use an existing resource group  
New-AzResourceGroup -Name $resourceGroupName -Location $locationName

# Create a new ARM storage account - comment out this line to use an existing ARM storage account  
New-AzStorageAccount -Name $storageAccountName -ResourceGroupName $resourceGroupName -Type Standard_RAGRS -Location $locationName   

# Get the access keys for the ARM storage account  
$accountKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName  

# Create a new storage account context using an ARM storage account  
$storageContext = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $accountKeys[0].value 

# Creates a new container in blob storage  
$container = New-AzStorageContainer -Context $storageContext -Name $containerName  
$cbc = $container.CloudBlobContainer  

# Sets up a Stored Access Policy and a Shared Access Signature for the new container  
$policy = New-AzStorageContainerStoredAccessPolicy -Container $containerName -Policy $policyName -Context $storageContext -ExpiryTime $(Get-Date).ToUniversalTime().AddYears(10) -Permission "rwld"
$sas = New-AzStorageContainerSASToken -Policy $policyName -Context $storageContext -Container $containerName
Write-Host 'Shared Access Signature= '$($sas.Substring(1))''  

# Outputs the Transact SQL to the clipboard and to the screen to create the credential using the Shared Access Signature  
Write-Host 'Credential T-SQL'  
$tSql = "CREATE CREDENTIAL [{0}] WITH IDENTITY='Shared Access Signature', SECRET='{1}'" -f $cbc.Uri,$sas.Substring(1)   
$tSql | clip  
Write-Host $tSql  
```  

Après avoir exécuté le script, copiez la commande `CREATE CREDENTIAL` dans un outil de requête, connectez-vous à une instance de SQL Server et exécutez la commande pour créer les informations d’identification avec la signature d’accès partagé. 

###  <a name="create-a-credential"></a><a name="credential"></a> Création d’informations d’identification

Les exemples suivants créent des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l’authentification auprès du service Stockage Blob Microsoft Azure. Procédez de l'une des manières suivantes :
  
1. **Utilisation d’une signature d’accès partagé**  

   Si vous avez exécuté le script pour créer la signature d’accès partagé ci-dessus, copiez `CREATE CREDENTIAL` dans un éditeur de requête connecté à votre instance de SQL Server et exécutez la commande. 

   Le code T-SQL suivant est un exemple qui crée les informations d’identification pour utiliser une signature d’accès partagé. 

   ```sql  
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = 'https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>')  
   CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE',  
      SECRET = '<SAS_TOKEN>';  
   ```  
  
2. **Utilisation d’une identité de compte de stockage et d’une clé d’accès**  
  
   ```sql 
   IF NOT EXISTS  
   (SELECT * FROM sys.credentials   
   WHERE name = '<mycredentialname>')  
   CREATE CREDENTIAL [<mycredentialname>] WITH IDENTITY = '<mystorageaccountname>'  
   ,SECRET = '<mystorageaccountaccesskey>';  
   ```  
  
### <a name="perform-a-full-database-backup"></a><a name="complete"></a> Effectuer une sauvegarde complète de la base de données  

Les exemples suivants effectuent une sauvegarde complète de la base de données AdventureWorks2016 dans le service Stockage Blob Microsoft Azure. Effectuez l’une des actions suivantes :
  
1. **Vers une URL en utilisant une signature d’accès partagé**  
  
   ```sql  
   BACKUP DATABASE AdventureWorks2016   
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak';  
   GO   
   ```  

1. **Vers une URL en utilisant une identité de compte de stockage et une clé d’accès**  
  
   ```sql
   BACKUP DATABASE AdventureWorks2016  
   TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016.bak'   
         WITH CREDENTIAL = '<mycredentialname>'   
        ,COMPRESSION  
        ,STATS = 5;  
   GO
   ```  

###  <a name="restoring-to-a-point-in-time-using-stopat"></a><a name="PITR"></a> Restauration jusqu'à une date et heure en utilisant STOPAT  

L’exemple suivant restaure l’exemple de base de données AdventureWorks2016 dans l’état où elle était à un moment donné et montre une opération de restauration.  
  
**À partir d’une URL en utilisant une signature d’accès partagé**  
  
   ```sql
   RESTORE DATABASE AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_16_00_00.bak'   
   WITH MOVE 'AdventureWorks2016_data' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.mdf'  
   ,MOVE 'AdventureWorks2016_log' to 'C:\Program Files\Microsoft SQL Server\<myinstancename>\MSSQL\DATA\AdventureWorks2016.ldf'  
   ,NORECOVERY  
   ,REPLACE  
   ,STATS = 5;  
   GO   
  
   RESTORE LOG AdventureWorks2016 FROM URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/AdventureWorks2016_2015_05_18_18_00_00.trn'   
   WITH   
   RECOVERY   
   ,STOPAT = 'May 18, 2015 5:35 PM'   
   GO  
   ```  
  
## <a name="see-also"></a>Voir aussi  

- [Bonnes pratiques et résolution des problèmes liés à la sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)
- [Sauvegarde et restauration des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)
- [Tutoriel : Utilisation du service Stockage Blob Microsoft Azure avec des bases de données SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)
