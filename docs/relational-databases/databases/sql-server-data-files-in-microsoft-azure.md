---
title: Fichiers de données SQL Server dans Microsoft Azure | Microsoft Docs
description: Découvrez les concepts et les considérations essentiels au stockage des fichiers de données SQL Server dans le stockage Microsoft Azure et certains avantages à utiliser le stockage Azure.
ms.custom: ''
ms.date: 12/04/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: 38ffd9c2-18a5-43d2-b674-e425addec4e4
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 952e8d393733c62f60c5b6792b422d22417ddede
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837935"
---
# <a name="sql-server-data-files-in-microsoft-azure"></a>Fichiers de données SQL Server dans Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  ![Fichiers de données sur Azure](../../relational-databases/databases/media/data-files-on-azure.png "Fichiers de données sur Azure")  
  
Les fichiers de données SQL Server dans Microsoft Azure permettent une prise en charge native des fichiers de base de données SQL Server stockés en tant qu’objets blob. Cela vous permet de créer une base de données dans SQL Server s’exécutant localement ou sur une machine virtuelle dans Microsoft Azure, avec un emplacement de stockage dédié pour vos données dans le service Stockage Blob Azure de Microsoft. Cela simplifie également le processus de déplacement des bases de données entre les machines. Vous pouvez détacher des bases de données d’une machine pour les attacher à une autre machine. En outre, elle fournit un autre emplacement de stockage pour les fichiers de sauvegarde de base de données, ce qui permet de restaurer ces fichiers depuis ou vers le service Microsoft Azure Storage. Par conséquent, elle permet plusieurs solutions hybrides en offrant différents avantages en matière de virtualisation des données, de déplacement des données, de sécurité et de disponibilité, le tout à des coûts et une maintenance réduits pour une mise à l'échelle élastique et une haute disponibilité.
 
> [!IMPORTANT]  
>  Le stockage de bases de données système dans Stockage Blob Azure n’est pas recommandé et n’est pas pris en charge. 

 Cette rubrique présente les concepts et les considérations essentiels au stockage des fichiers de données SQL Server dans le service de Stockage Microsoft Azure.  
  
 Pour une présentation pratique de l’utilisation de cette nouvelle fonctionnalité, consultez [Didacticiel : Utiliser le service Stockage Microsoft Azure Blob avec SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
## <a name="why-use-sql-server-data-files-in-microsoft-azure"></a>Pourquoi utiliser des fichiers de données SQL Server dans Microsoft Azure ? 

- **Migration facile et rapide :** cette fonctionnalité simplifie le processus de migration en déplaçant une base de données à la fois entre les ordinateurs locaux et entre les environnements locaux et le cloud, sans aucune modification de l’application. Par conséquent, elle prend en charge une migration incrémentielle tout en conservant l'infrastructure locale existante. En outre, avoir accès à un stockage de données centralisé simplifie la logique d'application lorsqu'une application doit s'exécuter à plusieurs emplacements dans un environnement local. Dans certains cas, vous devrez peut-être configurer rapidement des centres informatiques dans des emplacements géographiquement dispersés, qui collectent des données provenant de nombreuses sources différentes. Grâce à cette nouvelle amélioration, au lieu de déplacer des données d'un emplacement à un autre, vous pouvez stocker un grand nombre de bases de données en tant qu'objets blob Microsoft Azure, puis exécuter des scripts Transact-SQL pour créer des bases de données sur des ordinateurs locaux ou virtuels.

- **Coûts réduits et stockage illimité :** Cette fonctionnalité vous permet de bénéficier d’un stockage hors site illimité dans Microsoft Azure, tout en tirant parti des ressources de calcul locales. Lorsque vous utilisez Microsoft Azure comme emplacement de stockage, vous pouvez aisément vous concentrer sur la logique d'application sans surcharge pour la gestion du matériel. Si vous perdez un nœud de calcul local, vous pouvez en configurer un autre sans aucun déplacement des données.

- **Haute disponibilité et récupération d’urgence :** La fonctionnalité Fichiers de données SQL Server dans Microsoft Azure peut simplifier les solutions de haute disponibilité et de récupération d’urgence. Par exemple, en cas de plantage d’une machine virtuelle dans Microsoft Azure ou d’une instance de SQL Server, vous pouvez recréer vos bases de données dans une nouvelle instance de SQL Server en rétablissant simplement les liens vers les objets blob.

- **Sécurité :** cette nouvelle amélioration vous permet de séparer une instance de calcul d’une instance de stockage. Vous pouvez avoir une base de données entièrement chiffrée où le déchiffrement ne concerne que l'instance de calcul, mais pas l'instance de stockage. En d'autres termes, cette nouvelle amélioration vous permet de chiffrer toutes les données dans un cloud public à l'aide de certificats TDE (Transparent Data Encryption), lesquels sont physiquement séparés des données. Les clés TDE peuvent être stockées dans la base de données master, qui est stockée localement sur votre ordinateur physiquement sécurisé et sauvegardée localement. Vous pouvez utiliser ces clés locales pour chiffrer les données, qui résident dans Microsoft Azure Storage. Si les informations d'identification du compte de stockage en nuage (cloud) sont dérobées, vos données restent sécurisées dans la mesure où les certificats TDE résident toujours localement.

- **Sauvegarde d’instantané :**  Cette fonctionnalité vous permet d’utiliser les instantanés Azure pour obtenir des sauvegardes quasi instantanées et des restaurations plus rapides pour les fichiers de base de données stockés avec le service de Stockage Blob Azure. Cette fonctionnalité vous permet de simplifier vos stratégies de sauvegarde et de restauration. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  

## <a name="concepts-and-requirements"></a>Concepts et configuration requise  
  
Les disques Azure sont compatibles avec les solutions de continuité de l’activité et de reprise d’activité après sinistre à l’échelle de l’entreprise. Si vous stockez vos bases de données directement sur des objets blob ou dans Azure Premium Files, les données ne sont pas automatiquement associées à votre machine virtuelle pour l’infrastructure, la gestion et la supervision.

Le placement des fichiers de base de données dans des objets blob de pages est une fonctionnalité plus avancée que l’utilisation des disques Azure, qui sont simples et conviviaux.

L’aide de base repose sur l’utilisation de Disques Azure, sauf dans un scénario où vous devez absolument éviter de créer une copie des données pour les sauvegardes ou d’effectuer une restauration sous forme d’opération impactant la taille des données. Pour une haute disponibilité et une reprise d’activité après sinistre, l’utilisation d’une sauvegarde régulière vers une URL ou d’une sauvegarde managée vers le Stockage Blob est également beaucoup plus utile que les sauvegardes de captures instantanées de fichiers. En effet, vous bénéficiez de la gestion du cycle de vie, de la prise en charge multirégion, de la suppression réversible et de toutes les autres fonctionnalités du Stockage Blob pour vos sauvegardes.

### <a name="azure-storage-concepts"></a>Concepts liés au stockage Azure  
Lorsque vous utilisez la fonctionnalité Fichiers de données SQL Server dans Azure, vous devez créer un compte de stockage et un conteneur dans Azure. Ensuite, vous devez créer des informations d'identification SQL Server, qui comportent des informations sur la stratégie du conteneur ainsi qu'une signature d'accès partagé qui est nécessaire pour accéder au conteneur.  

Dans [Microsoft Azure](https://azure.microsoft.com), un compte de [stockage Azure](https://azure.microsoft.com/services/storage/) représente le niveau le plus élevé de l’espace de noms pour accéder aux objets blob. Un compte de stockage peut contenir un nombre illimité de conteneurs, tant que leur taille totale ne dépasse pas la limite de stockage. Pour les informations les plus récentes sur les limites de stockage, consultez [Abonnement Azure et limites, quotas et contraintes du service](/azure/azure-subscription-service-limits). Un conteneur regroupe un ensemble d’objets [blobs](/azure/storage/common/storage-introduction#blob-storage). Tous les objets blob doivent figurer dans un conteneur. Un compte peut contenir un nombre illimité de conteneurs. De même, un conteneur peut stocker un nombre illimité d’objets blob. Il existe deux types d’objets blob qui peuvent être enregistrés dans un stockage Azure : les objets blob de blocs et les objets blob de pages. Cette nouvelle fonctionnalité utilise des objets blob de pages, qui sont plus efficaces quand les plages d’octets d’un fichier sont fréquemment modifiées. Vous pouvez accéder aux objets blob à l’aide du format URL suivant : `https://storageaccount.blob.core.windows.net/<container>/<blob>`.  

### <a name="azure-billing-considerations"></a>Considérations sur la facturation Azure  

 L’estimation du coût d’utilisation des services Azure est une question importante dans le processus de prise de décision et de planification. Lorsque vous stockez des fichiers de données SQL Server dans le stockage Azure, vous devez payer les coûts associés au stockage et aux transactions. De plus, l’implémentation de la fonctionnalité Fichiers de données SQL Server dans le stockage Azure nécessite implicitement un renouvellement du bail d’objets blob toutes les 45 à 60 secondes. Cela entraîne également des coûts de transaction par fichier de base de données (.mdf ou .ldf, par exemple). Utilisez les informations de la page [Tarification Azure](https://azure.microsoft.com/pricing/) pour estimer les coûts mensuels associés à l’utilisation de Stockage Azure et de Machines virtuelles Azure.  
  
### <a name="sql-server-concepts"></a>Concepts de SQL Server  

Si vous utilisez cette nouvelle amélioration, assurez-vous de procéder comme suit :

- Créez une stratégie sur un conteneur et générez également une clé de signature d’accès partagé (SAS).

- Pour chaque conteneur utilisé par un fichier de données ou un fichier journal, créez des informations d’identification SQL Server dont le nom correspond au chemin du conteneur.

- Stockez les informations relatives au conteneur de stockage Azure, le nom de stratégie associé et la clé SAS dans le magasin d’informations d’identification de SQL Server.

L’exemple suivant suppose qu’un conteneur de stockage Azure a été créé, et qu’une stratégie a également été créée avec des droits d’accès en lecture, écriture et listage. La création d’une stratégie sur un conteneur génère une clé SAS, qui peut être conservée non chiffrée en mémoire, et dont SQL Server a besoin pour accéder aux fichiers d’objets blob du conteneur. 

Dans l’extrait de code suivant, remplacez `'<your SAS key>'` par la clé SAS. La clé SAS ressemble à ce qui suit `'sr=c&si=<MYPOLICYNAME>&sig=<THESHAREDACCESSSIGNATURE>'`.

```sql
CREATE CREDENTIAL [https://testdb.blob.core.windows.net/data]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '<your SAS key>'  
  
CREATE DATABASE testdb   
ON  
( NAME = testdb_dat,  
    FILENAME = 'https://testdb.blob.core.windows.net/data/TestData.mdf' )  
 LOG ON  
( NAME = testdb_log,  
    FILENAME =  'https://testdb.blob.core.windows.net/data/TestLog.ldf')  
```  

>[!IMPORTANT]
>s’il existe des références actives aux fichiers de données dans un conteneur, toutes les tentatives de suppression des informations d’identification SQL Server correspondantes échouent.

Pour plus d'informations, consultez [Gérer l'accès aux ressources Azure Storage](/azure/storage/blobs/storage-manage-access-to-resources).  

### <a name="security"></a>Sécurité  
 Vous trouverez ci-dessous les considérations de sécurité et la configuration requise pour le stockage de fichiers de données SQL Server dans le stockage Azure.

- Lorsque vous créez un conteneur pour le service de stockage d’objets blob Azure, nous vous recommandons de définir l’accès sur Privé. Lorsque vous définissez l’accès privé, le conteneur et les données blob ne peuvent être lus que par le propriétaire du compte Azure.

- Lors de l’enregistrement des fichiers de base de données SQL Server dans le stockage Azure, vous devez utiliser une signature d’accès partagé, un URI qui octroie des droits d’accès restreints aux conteneurs, aux objets blob, aux files d’attente et aux tables. À l’aide d’une signature d’accès partagé, vous pouvez autoriser SQL Server à accéder aux ressources dans votre compte de stockage sans partager votre clé de compte de stockage Azure.

- De plus, nous vous recommandons de continuer de suivre les pratiques de sécurité locales habituelles pour vos bases de données.  
  
### <a name="installation-prerequisites"></a>Prérequis pour l’installation  
 Vous trouverez ci-dessous les conditions préalables à l’installation pour le stockage de fichiers de données SQL Server dans le stockage Azure.

- **SQL Server local :** SQL Server 2016 et versions ultérieures comprend cette fonctionnalité. Pour savoir comment télécharger la dernière version de SQL Server, consultez [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads).

- SQL Server s’exécutant sur une machine virtuelle Azure : Si vous installez [SQL Server sur Windows Azure Virtual Machine](https://azuremarketplace.microsoft.com/marketplace/apps?search=sql%20server&page=1), installez SQL Server 2016 ou mettez à jour votre instance. De la même manière, vous pouvez aussi créer une nouvelle machine virtuelle dans Azure à l’aide de l’image de la plateforme SQL Server 2016.

  
###  <a name="limitations"></a><a name="bkmk_Limitations"></a> Limitations  
  
- En raison des caractéristiques de performances des charges de travail SQL Server, les fichiers de données SQL Server sont implémentés en tant qu’objets blob de pages dans le Stockage Blob Azure. D’autres types de stockage d’objets blob, tels que les objets blob de blocs ou [Azure Data Lake Storage](/azure/storage/blobs/data-lake-storage-introduction), ne sont pas pris en charge.

- Dans la version actuelle de cette fonctionnalité, l’enregistrement de données **FileStream** dans le stockage Azure n’est pas pris en charge. Vous pouvez stocker des données **FileStream** dans une base de données qui contient également des fichiers stockés dans le stockage Azure, mais tous les fichiers de données FileStream doivent se trouver sur le stockage local.  Comme les données FileStream doivent résider sur un stockage local, il est impossible de les déplacer entre des ordinateurs à l’aide du stockage Azure. Nous vous recommandons donc d’utiliser les [techniques traditionnelles](../../relational-databases/blob/move-a-filestream-enabled-database.md) pour déplacer les données associées à FileStream entre différents ordinateurs.  
  
- Actuellement, cette nouvelle amélioration ne prend pas en charge plusieurs instances SQL Server qui accèdent en même temps aux mêmes fichiers de base de données dans Stockage Azure. Si un serveur A est en ligne avec un fichier de base de données actif et si un serveur B démarre par erreur, et qu’il comporte également une base de données qui pointe vers le même fichier de données, le deuxième serveur ne peut pas démarrer la base de données. Un code d’erreur **5120 Impossible d’ouvrir le fichier physique « %.\*ls » est généré. Erreur %d du système d’exploitation : « %ls »** .  
  
- Seuls les fichiers .mdf, .ldf et .ndf peuvent être enregistrés dans le Stockage Azure à l’aide de la fonctionnalité Fichiers de données SQL Server dans Azure.  
  
- En cas d’utilisation de la fonctionnalité Fichiers de données SQL Server dans Azure, la géoréplication de votre compte de stockage n’est pas prise en charge. Si un compte de stockage est géorépliqué et qu'un géobasculement a lieu, la base de données risque d'être endommagée.  
  
- Pour les limites de capacité, consultez [Présentation du Stockage Blob](/azure/storage/blobs/storage-blobs-introduction).  
  
- Il est impossible de conserver des données OLTP en mémoire dans le Stockage Blob à l’aide de la fonctionnalité Fichiers de données SQL Server dans le stockage Azure. Cela s’explique par le fait que l’OLTP en mémoire compte une dépendance sur **FileStream** et que, dans la version actuelle de cette fonctionnalité, l’enregistrement de données **FileStream** dans Azure Storage n’est pas pris en charge.  
  
- Quand vous utilisez la fonctionnalité Fichiers de données SQL Server dans Azure, SQL Server effectue toutes les comparaisons d’URL ou de chemin de fichier à l’aide du classement défini dans la base de données **MASTER**.  
  
- Les **groupes de disponibilité AlwaysOn** sont pris en charge tant que vous n’ajoutez pas de nouveaux fichiers de base de données à la base de données primaire. Si une opération de base de données nécessite la création d’un fichier dans la base de données primaire, désactivez d’abord les groupes de disponibilité dans le nœud secondaire. Puis, effectuez l'opération de base de données dans la base de données primaire et sauvegardez la base de données dans le nœud principal. Restaurez ensuite la base de données sur le nœud secondaire. Une fois que vous avez fini, réactivez les groupes de disponibilité Always On dans le nœud secondaire. 

   >[!NOTE]
   >Les instances de cluster de basculement Always On ne sont pas prises en charge en cas d’utilisation de la fonctionnalité Fichiers de données SQL Server dans Azure.
  
- En mode de fonctionnement normal, SQL Server utilise des baux temporaires afin de réserver des objets blob pour le stockage avec un renouvellement de bail de chaque objet blob toutes les 45 à 60 secondes. Si un serveur plante et si une autre instance de SQL Server configurée pour utiliser les mêmes objets blob démarre, la nouvelle instance attend jusqu’à 60 secondes, le temps que le bail existant sur l’objet blob expire. Si vous souhaitez attacher la base de données à une autre instance et si vous ne pouvez pas attendre l’expiration du bail pendant 60 secondes, vous pouvez libérer explicitement le bail sur l’objet blob.
  
## <a name="tools-and-programming-reference-support"></a>Prise en charge des bibliothèques de référence de programmation et des outils  
 Cette section décrit les bibliothèques de référence de programmation et les outils qui peuvent être utilisés lors du stockage de fichiers de données SQL Server dans le stockage Azure.  
  
### <a name="powershell-support"></a>Prise en charge de PowerShell  
 Utilisez les applets de commande PowerShell pour stocker des fichiers de données SQL Server dans le service Stockage Blob en référençant une URL vers Stockage Blob à la place d’un chemin de fichier. Accédez aux objets blob à l’aide du format URL suivant : `https://storageaccount.blob.core.windows.net/<container>/<blob>`.  
  
### <a name="sql-server-object-and-performance-counters-support"></a>Prise en charge des compteurs de performances et des objets SQL Server  
 Depuis SQL Server 2014, un nouvel objet SQL Server a été ajouté pour être utilisé avec la fonctionnalité Fichiers de données SQL Server dans le stockage Azure. Le nouvel objet SQL Server est appelé [SQL Server, HTTP_STORAGE_OBJECT](../../relational-databases/performance-monitor/sql-server-http-storage-object.md) et il peut être utilisé par le Moniteur système pour surveiller l’activité lors de l’exécution de SQL Server avec le Stockage Azure.  
  
### <a name="sql-server-management-studio-support"></a>Prise en charge de SQL Server Management Studio  
 SQL Server Management Studio vous permet d'utiliser cette fonctionnalité via plusieurs fenêtres de dialogue. Par exemple, `https://teststorageaccnt.blob.core.windows.net/testcontainer/` représente l’URL d’un conteneur de stockage.
 
 sous la forme d’un **Chemin d’accès** dans plusieurs fenêtres de boîte de dialogue, comme **Nouvelle base de données**, **Attacher la base de données**, et **Restaurer la base de données**. Pour plus d’informations, consultez [Didacticiel : Utiliser le service Stockage Microsoft Azure Blob avec SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
### <a name="sql-server-management-objects-smo-support"></a>Prise en charge d’objets SMO (SQL Server Management Objects)  
 Quand vous utilisez la fonctionnalité Fichiers de données SQL Server dans Azure, tous les objets SMO (SQL Server Management Objects) sont pris en charge. Si un objet SMO nécessite un chemin d'accès de fichier, utilisez le format d'URL BLOB à la place d'un chemin d'accès de fichier local, tel que `https://teststorageaccnt.blob.core.windows.net/testcontainer/`. Pour plus d’informations sur les objets SMO, consultez [Guide de programmation SMO &#40;SQL Server Management Objects&#41;](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) dans la documentation en ligne de SQL Server.  
  
### <a name="transact-sql-support"></a>Prise en charge de Transact-SQL  
 Cette nouvelle fonctionnalité a apporté le changement suivant dans la surface d'exposition de Transact-SQL :

- Une nouvelle colonne **int** , **credential_id**, dans la vue système **sys.master_files** . La colonne **credential_id** permet aux fichiers de données du stockage Azure d’être référencés dans `sys.credentials` en ce qui concerne les informations d’identification créées à leur intention. Vous pouvez vous en servir pour résoudre des problèmes, par exemple quand des informations d’identification ne peuvent pas être supprimées, car elles sont utilisées par un fichier de base de données.  
  
##  <a name="troubleshooting-for-sql-server-data-files-in-microsoft-azure"></a><a name="bkmk_Troubleshooting"></a> Dépannage pour les fichiers de données SQL Server dans Microsoft Azure  
 Pour éviter des erreurs attribuables à des limitations ou à des fonctionnalités non prises en charge, passez d'abord en revue [Limitations](../../relational-databases/databases/sql-server-data-files-in-microsoft-azure.md#bkmk_Limitations).  
  
 Voici la liste des erreurs possibles lorsque vous utilisez la fonctionnalité Fichiers de données SQL Server dans le stockage Azure.  
  
 **Erreurs d'authentification**  
  
- *Impossible de supprimer les informations d’identification « %.\*ls » parce qu’elles sont utilisées par un fichier de base de données actif.*    
    Résolution : Cette erreur peut s’afficher lorsque vous tentez de supprimer des informations d’identification utilisées par un fichier de base de données actif dans le Stockage Azure. Pour supprimer les informations d'identification, vous devez d'abord supprimer l'objet blob associé qui comporte ce fichier de base de données. Pour supprimer un objet blob dont le bail est actif, vous devez d’abord libérer le bail.  
  
- *La signature d'accès partagé n'a pas été créée correctement sur le conteneur.*    
     Résolution : vérifiez que vous avez créé correctement une signature d’accès partagé sur le conteneur. Consultez les instructions fournies dans la leçon 2 du [Didacticiel : Utiliser le service Stockage Microsoft Azure Blob avec SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature).  
  
- *Les informations d'identification de SQL Server n'ont pas été créées correctement.*    
    Résolution : Vérifiez que vous avez utilisé une « signature d’accès partagé » pour le champ **Identité** et que vous avez créé un secret correctement. Consultez les instructions données dans la leçon 3 du [Didacticiel : Utiliser le service Stockage Microsoft Azure Blob avec SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#3---database-backup-to-url).  
  
 **Erreurs de bail d'un objet blob :**  
  
- Erreur lors de la tentative de démarrage de SQL Server après la défaillance d'une autre instance utilisant les mêmes fichiers d'objets blob. Résolution : En mode de fonctionnement normal, SQL Server utilise des baux temporaires afin de réserver des objets blob pour le stockage avec un renouvellement de bail de chaque objet blob toutes les 45 à 60 secondes. Si un serveur plante et si une autre instance de SQL Server configurée pour utiliser les mêmes objets blob démarre, la nouvelle instance attend jusqu’à 60 secondes, le temps que le bail existant sur l’objet blob expire. Si vous souhaitez attacher la base de données à une autre instance et si vous ne pouvez pas attendre l’expiration du bail pendant 60 secondes, vous pouvez libérer explicitement le bail sur l’objet blob pour éviter les erreurs dans les opérations d’attachement.  
  
 **Erreurs de base de données**  
  
Résolution des **erreurs liées à la création d’une base de données** : Consultez les instructions fournies dans la leçon 4 du [Didacticiel : Utiliser le service Stockage Microsoft Azure Blob avec SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#4----restore-database-to-virtual-machine-from-url).  
  
**Erreurs lors de l'exécution de l'instruction Alter** Résolution : veillez à exécuter l'instruction Alter Database lorsque la base de données est en ligne. Lors de la copie des fichiers de données vers le stockage Azure, créez toujours un objet blob de pages, et non un objet blob de blocs. Sinon, la modification de la base de données avec l'instruction ALTER échouera. Consultez les instructions fournies dans la leçon 7 du [Didacticiel : Utiliser le service Stockage Microsoft Azure Blob avec SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
**Code d’erreur 5120 - Impossible d’ouvrir le fichier physique « %.\*ls ». Erreur %d du système d'exploitation : « %ls »**   

Résolution : Actuellement, cette nouvelle amélioration ne prend pas en charge l’accès simultané de plusieurs instances SQL Server aux mêmes fichiers de base de données dans le Stockage Azure. Si un serveur A est en ligne avec un fichier de base de données actif et si un serveur B démarre par erreur, et qu’il comporte également une base de données qui pointe vers le même fichier de données, le deuxième serveur ne peut pas démarrer la base de données. Un code d’erreur *5120 Impossible d’ouvrir le fichier physique « %.\*ls » est généré. Erreur %d du système d’exploitation : « %ls »* .  
  
Pour résoudre ce problème, déterminez d’abord si vous avez besoin du serveur A pour accéder au fichier de base de données dans le stockage Azure. Sinon, supprimez les connexions entre le serveur A et les fichiers de base de données dans le stockage Azure. Pour ce faire, procédez comme suit :  

1.  Définissez le chemin d'accès du serveur A sur un dossier local à l'aide de l'instruction ALTER Database.  

2.  Mettez la base de données hors connexion sur le serveur A.  

3.  Puis, copiez les fichiers de base de données du stockage Azure dans le dossier local du serveur A. Cela garantit que le serveur A dispose toujours d’une copie de la base de données localement.  

4.  Mettez la base de données en ligne.

**Code d’erreur 833 - L’exécution des demandes d’E/S dure plus de 15 secondes** 
   
   Cette erreur indique que le système de stockage n’est pas en mesure de répondre aux demandes de la charge de travail SQL Server. Diminuez l’activité d’E/S de la couche d’application ou augmentez la capacité de débit sur la couche de stockage. Pour en savoir plus, consultez [Erreur 833](../errors-events/mssqlserver-833-database-engine-error.md). Si les problèmes de performances persistent, envisagez de déplacer les fichiers vers un autre niveau de stockage, par exemple Premium ou UltraSSD. Pour SQL Server sur les machines virtuelles Azure, consultez [Optimisation des performances de stockage](/azure/virtual-machines/premium-storage-performance).


## <a name="next-steps"></a>Étapes suivantes  
  
[Créer une base de données](create-a-database.md)