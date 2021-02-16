---
title: SQL Server Express LocalDB | Microsoft Docs
description: Familiarisez-vous avec la base de données locale SQL Server Express. Les développeurs peuvent utiliser ce Moteur de base de données léger pour écrire et tester du code Transact-SQL.
ms.custom: ''
ms.date: 04/17/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- user instances
- LocalDB, described
- local database runtime
- file database
- LocalDB
ms.assetid: 5a641a46-7cfb-4d7b-a90d-6e4625719d74
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b7f57127bd8d1bd6b289ba82291ba9c1dd87424
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340434"
---
# <a name="sql-server-express-localdb"></a>Base de données locale SQL Server Express

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Microsoft SQL Server Express LocalDB est une fonctionnalité de [SQL Server Express](../../sql-server/editions-and-components-of-sql-server-version-15.md) destinée aux développeurs. Elle est disponible dans Microsoft SQL Server Express with Advanced Services.

L'installation de LocalDB copie l'ensemble minimal des fichiers nécessaires pour démarrer le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Une fois LocalDB installé, vous pouvez lancer une connexion à l’aide d’une chaîne de connexion particulière. Lors de la connexion, l’infrastructure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessaire est créée et démarrée automatiquement, ce qui permet à l’application d’utiliser la base de données sans tâches de configuration complexes. Les outils de développement incluent un [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] qui permet aux développeurs d'écrire et de tester le code [!INCLUDE[tsql](../../includes/tsql-md.md)] sans devoir gérer une instance de serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]complète. 

## <a name="installation-media"></a>Support d’installation 

LocalDB est une fonctionnalité que vous sélectionnez lors de l’installation de SQL Server Express et qui est disponible quand vous téléchargez le média. Si vous téléchargez le média, choisissez le package **Express Advanced** ou LocalDB. 

- [SQL Server Express 2019](https://go.microsoft.com/fwlink/?LinkID=866658)
- [SQL Server Express 2017](https://go.microsoft.com/fwlink/?LinkID=853017)
- [SQL Server Express 2016](https://go.microsoft.com/fwlink/?LinkID=799012)

Vous pouvez également installer la base de données locale à l’aide de [Visual Studio Installer](https://visualstudio.microsoft.com/downloads/), dans le cadre de la charge de travail **Stockage et traitement des données**, la charge de travail **Développement web et ASP.NET**, ou en tant que composant individuel.


## <a name="install-localdb"></a>Installer LocalDB

Installez LocalDB par le biais de l’Assistant installation ou à l’aide du programme SqlLocalDB.msi. LocalDB est une option proposée quand vous installez SQL Server Express LocalDB. 
 
Sélectionnez LocalDB dans la page **Sélection de fonctionnalités/Fonctionnalités partagées** lors de l’installation. Il ne peut y avoir qu'une seule installation des fichiers binaires LocalDB pour chaque version majeure de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Plusieurs processus [!INCLUDE[ssDE](../../includes/ssde-md.md)] peuvent être démarrés et utiliseront tous les mêmes fichiers binaires. Une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] démarrée comme LocalDB a les mêmes limitations que [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].

Une instance de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB est gérée à l’aide de l’utilitaire `SqlLocalDB.exe`. [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB doit être utilisé à la place de la fonctionnalité d’instance utilisateur [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] qui est déconseillée.

## <a name="description"></a>Description

Le programme d’installation de LocalDB utilise le programme `SqlLocalDB.msi` pour installer les fichiers nécessaires sur l’ordinateur. Une fois installé, LocalDB est une instance de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] qui peut créer et ouvrir des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les fichiers de base de données système pour la base de données sont stockés dans le chemin d'accès AppData local, qui est normalement masqué. Par exemple : `C:\Users\<user>\AppData\Local\Microsoft\Microsoft SQL Server Local DB\Instances\LocalDBApp1\`. Les fichiers de la base de données utilisateur sont stockés à l'emplacement indiqué par l'utilisateur, en général dans le dossier `C:\Users\<user>\Documents\`.

Pour plus d’informations sur l’inclusion de LocalDB dans une application, consultez [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] [Vue d’ensemble des données locales](/previous-versions/visualstudio/visual-studio-2012/ms233817(v=vs.110)), [Créer une base de données et ajouter des tables dans Visual Studio](/visualstudio/data-tools/create-a-sql-database-by-using-a-designer).

Pour plus d’informations sur l’API LocalDB, consultez [Référence SQL Server Express LocalDB](../../relational-databases/sql-server-express-localdb-reference.md).

L’utilitaire `SqlLocalDb` peut créer des instances de LocalDB, démarrer et arrêter une instance de LocalDB et inclut des options permettant de gérer LocalDB. Pour plus d’informations sur l’utilitaire `SqlLocalDb`, consultez [Utilitaire SqlLocalDB](../../tools/sqllocaldb-utility.md).

Le classement d'instance pour LocalDB a la valeur `SQL_Latin1_General_CP1_CI_AS` et ne peut pas être modifié. Les classements au niveau des bases de données, des colonnes ou des expressions sont pris en charge normalement. Les bases de données autonomes suivent les règles de classement des métadonnées et de `tempdb` définies par [Classements de base de données autonome](../../relational-databases/databases/contained-database-collations.md).

### <a name="restrictions"></a>Restrictions

- La Base de données locale ne peut pas être corrigée au-delà des Service Packs. Ni les mises à jour cumulatives ni les mises à jour de sécurité ne peuvent être appliquées manuellement. Elles ne seront par ailleurs appliquées ni par Windows Update, ni par Windows Update pour Entreprise, ni par d’autres méthodes.

- La Base de données locale ne peut pas être gérée à distance avec SQL Server Management Studio.

- LocalDB ne peut pas être un abonné de réplication de fusion.

- LocalDB ne prend pas en charge FILESTREAM.

- LocalDB n'autorise que les files d'attente locales pour Service Broker.

- Une instance de LocalDB détenue par les comptes intégrés tels que `NT AUTHORITY\SYSTEM` peut présenter des problèmes de gestion dus à la redirection du système de fichiers Windows. À la place, utilisez un compte Windows normal comme propriétaire.

### <a name="automatic-and-named-instances"></a>Instances automatiques et nommées

LocalDB prend en charge deux types d'instances : les instances automatiques et les instances nommées.

- Les instances automatiques de LocalDB sont publiques. Elles sont créées et gérées automatiquement pour l'utilisateur et peuvent être utilisées par n'importe quelle application. Il existe une instance automatique de LocalDB pour chaque version de LocalDB installée sur l’ordinateur de l’utilisateur. Les instances automatiques de LocalDB fournissent la gestion transparente d'instances. Il n'est pas nécessaire de créer l'instance ; elle fonctionne tout simplement. Cette fonctionnalité permet une installation et une migration faciles de l'application vers un autre ordinateur. Si la version spécifiée de LocalDB est installée sur l'ordinateur cible, l'instance automatique de LocalDB pour cette version est disponible sur l'ordinateur cible. Les instances automatiques de LocalDB ont un modèle particulier pour le nom de l'instance qui appartient à un espace de noms réservé. Les instances automatiques empêchent les conflits de nom avec les instances nommées de LocalDB. Le nom de l'instance automatique est **MSSQLLocalDB**.

- Les instances nommées de LocalDB sont privées. Elles appartiennent à une seule application qui est chargée de créer et de gérer l'instance. Les instances nommées fournissent l'isolement d'autres instances et peuvent améliorer les performances en réduisant les contentions de ressources avec d'autres utilisateurs de base de données. Les instances nommées doivent être créées explicitement par l’utilisateur par le biais de l’API de gestion LocalDB ou implicitement par le biais du fichier app.config pour une application managée (bien que l’application managée puisse également utiliser l’API, si vous le souhaitez). Chaque instance nommée de LocalDB possède une version associée de LocalDB qui désigne le jeu respectif de binaires de LocalDB. Le nom d'instance d'une LocalDB est un type de données **sysname** et peut comporter jusqu'à 128 caractères. (Ceci diffère des instances nommées normales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], qui limitent les noms aux noms NetBIOS standard de 16 caractères ASCII.) Le nom d’une instance de LocalDB peut contenir des caractères Unicode qui sont valides dans un nom de fichier. Une instance nommée qui utilise le nom d’une instance automatique devient une instance automatique.

Les différents utilisateurs d'un ordinateur peuvent avoir des instances de même nom. Chaque instance correspond à différents processus qui s'exécutent sous un utilisateur différent.

## <a name="shared-instances-of-localdb"></a>Instances partagées de LocalDB

Pour prendre en charge les scénarios dans lesquels plusieurs utilisateurs de l'ordinateur doivent se connecter à une instance unique de LocalDB, LocalDB prend en charge le partage d'instance. Un propriétaire d'instance peut choisir d'autoriser les autres utilisateurs sur l'ordinateur à se connecter à l’instance. Les instances automatiques et nommées de LocalDB peuvent être partagées. Pour partager une instance de LocalDB, un utilisateur sélectionne un nom partagé (alias) pour celle-ci. Étant donné que le nom partagé est visible par tous les utilisateurs de l'ordinateur, ce nom partagé doit être unique sur l'ordinateur. Le nom partagé pour une instance de LocalDB présente le même format que l'instance nommée de LocalDB.

Seul un administrateur sur l'ordinateur peut créer une instance partagée de LocalDB. Le partage d'une instance de LocalDB peut être annulé par un administrateur ou par le propriétaire de l'instance partagée de LocalDB. Pour partager et annuler le partage d’une instance de LocalDB, utilisez les méthodes `LocalDBShareInstance` et `LocalDBUnShareInstance` de l’API LocalDB, ou les options de partage et non partagées de l’utilitaire `SqlLocalDb`.

## <a name="start-localdb-and-connect-to-localdb"></a>Démarrer LocalDB et se connecter à LocalDB

### <a name="connect-to-the-automatic-instance"></a>Se connecter à l'instance automatique

La façon la plus simple d'utiliser LocalDB est de se connecter à l'instance automatique possédée par l'utilisateur actuel en utilisant la chaîne de connexion `Server=(localdb)\MSSQLLocalDB;Integrated Security=true`. Pour vous connecter à une base de données spécifique en utilisant le nom de fichier, connectez-vous à l'aide d'une chaîne de connexion semblable à `Server=(LocalDB)\MSSQLLocalDB;Integrated Security=true;AttachDbFileName=D:\Data\MyDB1.mdf`.

La convention d’affectation de noms et la chaîne de connexion pour le format de la base de données locale ont changé dans SQL Server 2014. Auparavant, le nom de l’instance était un seul caractère v suivi de la base de données locale et du numéro de version. À compter de SQL Server 2014, ce format de nom d’instance n’est plus pris en charge, et la chaîne de connexion mentionnée précédemment doit être utilisée à la place.  

>[!NOTE]
> - La première fois qu'un utilisateur tentera de se connecter à LocalDB, l'instance automatique devra être créée et démarrée. Le temps supplémentaire requis pour créer l'instance peut entraîner l'échec de la tentative de connexion et l'affichage d'un message de délai de connexion dépassé. Dans ce cas, attendez quelques secondes la fin du processus de création, puis connectez-vous à nouveau.


### <a name="create-and-connect-to-a-named-instance"></a>Créer une instance nommée et s’y connecter

En plus de l'instance automatique, LocalDB prend également en charge les instances nommées. Utilisez le programme SqlLocalDB.exe pour créer, démarrer et arrêter une instance nommée de LocalDB. Pour plus d’informations sur SqlLocalDB.exe, consultez [Utilitaire SqlLocalDB](../../tools/sqllocaldb-utility.md).

```console
REM Create an instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" create LocalDBApp1
REM Start the instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" start LocalDBApp1
REM Gather information about the instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" info LocalDBApp1
```

 La dernière ligne ci-dessus retourne des informations semblables aux suivantes.

|Category|Valeur|
|-|-|
|Nom|`LocalDBApp1`|
|Version|\<Current Version>|
|Nom partagé|""|
|Propriétaire|"\<Your Windows User>"|
|Création automatique|Non|
|State|exécution en cours|
|Dernière heure de début|\<Date and Time>|
|Nom du canal de l'instance|np:\\\\.\pipe\LOCALDB#F365A78E\tsql\query|

>[!NOTE]
>Si votre application utilise une version de .NET antérieure à 4.0.2, vous devez vous connecter directement au canal nommé de LocalDB. La valeur Nom du canal de l’instance est le canal nommé sur lequel l’instance de LocalDB écoute. La partie du nom du canal de l’instance après LOCALDB# change chaque fois que l’instance de LocalDB est démarrée. Pour se connecter à l’instance de LocalDB à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], entrez le nom du canal de l’instance dans la zone **Nom du serveur** de la boîte de dialogue **Se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)]** . Depuis votre programme personnalisé, vous pouvez établir la connexion à l'instance de LocalDB à l'aide d'une chaîne de connexion semblable à `SqlConnection conn = new SqlConnection(@"Server=np:\\.\pipe\LOCALDB#F365A78E\tsql\query");`

### <a name="connect-to-a-shared-instance-of-localdb"></a>Se connecter à une instance partagée de LocalDB

Pour vous connecter à une instance partagée de LocalDB, ajoutez `\.\` (barre oblique inverse + point + barre oblique inverse) à la chaîne de connexion pour faire référence à l’espace de noms réservé aux instances partagées. Par exemple, pour vous connecter à une instance partagée de LocalDB nommée `AppData`, utilisez une chaîne de connexion telle que `(localdb)\.\AppData` dans la chaîne de connexion. Un utilisateur se connectant à une instance partagée de LocalDB dont il n'est pas propriétaire doit obtenir une connexion via l'authentification Windows ou l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="troubleshooting"></a>Dépannage

Pour plus d'informations sur la résolution des problèmes liés à LocalDB, consultez [Dépannage de SQL Server 2012 Express LocalDB](https://social.technet.microsoft.com/wiki/contents/articles/4609.aspx).

## <a name="permissions"></a>Autorisations

Une instance SQL Server Express LocalDB est une instance créée par un utilisateur pour son propre usage. Tout utilisateur peut créer sur l'ordinateur une base de données à l'aide d'une instance de LocalDB, stocker les fichiers sous son profil utilisateur et exécuter le processus à l'aide de ses informations d'identification. Par défaut, l'accès à l'instance de LocalDB est limité à son propriétaire. Les données contenues dans LocalDB sont protégées via l'accès au système de fichiers de base de données. Si les fichiers de base de données utilisateur sont stockés dans un emplacement partagé, la base de données peut être ouverte par toute personne ayant accès au système de fichiers à cet emplacement en utilisant une instance de LocalDB dont elle est propriétaire. Si les fichiers de base de données sont dans un emplacement protégé, tel que le dossier des données d'utilisateurs, seul cet utilisateur, et tous les administrateurs ayant accès à ce dossier, peuvent ouvrir la base de données. Les fichiers LocalDB ne peuvent être ouverts que par une instance de LocalDB à la fois.

>[!NOTE]
>LocalDB s’exécute toujours dans le contexte de sécurité des utilisateurs. Autrement dit, LocalDB ne s’exécute jamais avec les informations d’identification du groupe des administrateurs local. Cela signifie que tous les fichiers de base de données utilisés par une instance LocalDB doivent être accessibles à l’aide du compte Windows de l’utilisateur propriétaire, sans prendre en compte l’appartenance au groupe des administrateurs local.

## <a name="see-also"></a>Voir aussi

[Utilitaire SqlLocalDB](../../tools/sqllocaldb-utility.md)
