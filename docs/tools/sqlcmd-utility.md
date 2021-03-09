---
title: Utilitaire sqlcmd
description: L’utilitaire sqlcmd vous permet d’entrer des instructions Transact-SQL, des procédures système et des fichiers de script à l’aide de différents modes, et utilise ODBC pour exécuter les lots Transact-SQL.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- sqlcmd commands
- ED command
- sqlcmd utility
- command prompt utilities [SQL Server], sqlcmd
- '!! command'
- stored procedures [SQL Server], command prompt
- system stored procedures [SQL Server], command prompt
- sqlcmd utility, about sqlcmd utility
- scripts [SQL Server], command prompt
- RESET command
- GO command
ms.assetid: e1728707-5215-4c04-8320-e36f161b834a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/24/2021
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 928936a4b4ad6524e4d0817b94696353cb8427e9
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186596"
---
# <a name="sqlcmd-utility"></a>Utilitaire sqlcmd

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> Pour SQL Server 2014 et les versions antérieure, consultez [sqlcmd Utility](/previous-versions/sql/2014/tools/sqlcmd-utility?view=sql-server-2014&preserve-view=true).
>
> Pour utiliser sqlcmd sur Linux, consultez [Installer sqlcmd et bcp sur Linux](../linux/sql-server-linux-setup-tools.md).

 L’utilitaire **sqlcmd** vous permet d’entrer des instructions Transact-SQL, des procédures système et des fichiers de script dans des modes très variables :

- À l'invite de commandes.
- Dans l’**l'éditeur de requête** en mode SQLCMD.
- Dans un fichier de script Windows.
- Dans une étape de travail de système d’exploitation (cmd.exe) d’un travail SQL Server Agent.

L’utilitaire utilise ODBC pour exécuter des lots Transact-SQL.

## <a name="download-the-latest-version-of-sqlcmd-utility"></a>Télécharger la dernière version de l’utilitaire sqlcmd

**[![Télécharger sqlcmd pour x64](../ssdt/media/download.png) Télécharger les utilitaires de ligne de commande Microsoft 15 pour SQL Server (x64) (2,6 Mo)](https://go.microsoft.com/fwlink/?linkid=2142258)**
<br>**[![Télécharger sqlcmd pour x86](../ssdt/media/download.png) Télécharger les utilitaires de ligne de commande Microsoft 15 pour SQL Server (x86) (2,3 Mo)](https://go.microsoft.com/fwlink/?linkid=2142257)**

Les outils en ligne de commande sont en disponibilité générale (GA), mais ils sont publiés avec le package d’installation de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

**Informations sur la version**

Numéro de version : 15.0.2<br>
Numéro de build : 15.0.2000.5<br>
Date de publication : 11 septembre 2020

La nouvelle version de SQLCMD prend en charge l’authentification Azure AD, y compris la prise en charge de MFA (Multi-Factor Authentication) pour les fonctionnalités SQL Database, Azure Synapse Analytics et Always Encrypted.
Le nouveau BCP prend en charge l’authentification Azure AD, notamment la prise en charge de MFA pour SQL Database et Azure Synapse Analytics.

**Configuration requise** Windows 10 , Windows 7, Windows 8, Windows 8.1, Windows Server 2008 - 2019.

Ce composant nécessite [Windows Installer 4.5](https://windows-installer.soft32.com/) et [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md).
 
Pour vérifier la version de SQLCMD, exécutez la commande `sqlcmd -?` et vérifiez que la version 15.0.2000.5 ou une version ultérieure est utilisée.

> [!NOTE]
> La version 13.1 ou une version ultérieure est requise pour prendre en charge les authentifications Always Encrypted (`-g`) et Azure Active Directory (`-G`). (Plusieurs versions de sqlcmd.exe peuvent être installées sur votre ordinateur. Assurez-vous d’utiliser la version correcte. Pour déterminer la version, exécutez `sqlcmd -?`.)

Vous pouvez essayer l’utilitaire sqlcmd à partir d’Azure Cloud Shell, car il est préinstallé par défaut : [![Lancer Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Lancer Cloud Shell")](https://shell.azure.com)

Pour exécuter des instructions sqlcmd dans SSMS, sélectionnez le Mode SQLCMD à partir de la liste déroulante du menu Requête.  

> [!IMPORTANT]
> [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] (SSMS) utilise le client Microsoft SQL [!INCLUDE[dnprdnshort_md](../includes/dnprdnshort-md.md)] pour l’exécution en mode régulier et SQLCMD dans **l’Éditeur de requête**. Lorsque **sqlcmd** est exécuté à partir de la ligne de commande, **sqlcmd** utilise le pilote ODBC. Dans la mesure où différentes options par défaut peuvent s’appliquer, vous pouvez constater un comportement différent lorsque vous exécutez la même requête dans [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] en mode SQLCMD et dans l’utilitaire **sqlcmd** .  
>

Actuellement, **sqlcmd** ne requiert pas d’espace entre l’option de ligne de commande et la valeur. Toutefois, dans une version ultérieure, un espace peut être requis entre l'option de ligne de commande et la valeur.  

 Autres rubriques : 

- [Démarrer l’utilitaire sqlcmd](../ssms/scripting/sqlcmd-start-the-utility.md)
- [Utiliser l'utilitaire sqlcmd](../ssms/scripting/sqlcmd-use-the-utility.md)
  
## <a name="syntax"></a>Syntaxe

```cmd
sqlcmd
   -a packet_size
   -A (dedicated administrator connection)
   -b (terminate batch job if there is an error)
   -c batch_terminator
   -C (trust the server certificate)
   -d db_name
   -D
   -e (echo input)
   -E (use trusted connection)
   -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage]
   -g (enable column encryption)
   -G (use Azure Active Directory for authentication)
   -h rows_per_header
   -H workstation_name
   -i input_file
   -I (enable quoted identifiers)
   -j (Print raw error messages)
   -k[1 | 2] (remove or replace control characters)  
   -K application_intent  
   -l login_timeout  
   -L[c] (list servers, optional clean output)  
   -m error_level  
   -M multisubnet_failover  
   -N (encrypt connection)  
   -o output_file  
   -p[1] (print statistics, optional colon format)  
   -P password  
   -q "cmdline query"  
   -Q "cmdline query" (and exit)  
   -r[0 | 1] (msgs to stderr)  
   -R (use client regional settings)  
   -s col_separator  
   -S [protocol:]server[instance_name][,port]  
   -t query_timeout  
   -u (unicode output file)  
   -U login_id  
   -v var = "value"
   -V error_severity_level
   -w column_width
   -W (remove trailing spaces)
   -x (disable variable substitution)
   -X[1] (disable commands, startup script, environment variables, optional exit)
   -y variable_length_type_display_width
   -Y fixed_length_type_display_width
   -z new_password
   -Z new_password (and exit)
   -? (usage)
```
  
## <a name="command-line-options"></a>Options de ligne de commande

**Options relatives à la connexion**

**-A**  
Se connecte à SQL Server avec une connexion administrateur dédiée (DAC, Dedicated Administrator Connection). Ce type de connexion est utilisé pour dépanner un serveur. Cette connexion ne fonctionne qu'avec les serveurs prenant en charge DAC. Si DAC n’est pas disponible, **sqlcmd** génère un message d’erreur et se termine. Pour plus d’informations sur DAC, consultez [Connexion de diagnostic pour les administrateurs de base de données](../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md). L’option -A n’est pas prise en charge avec l’option -G. Quand vous vous connectez à SQL Database à l’aide de -A, vous devez être administrateur SQL Server. La DAC n’est pas disponible pour un administrateur Azure Active Directory.

**-C**  
Ce commutateur est utilisé par le client pour le configurer afin d'approuver implicitement le certificat de serveur sans validation. Cette option est équivalente à l'option ADO.NET `TRUSTSERVERCERTIFICATE = true`.  

**-d** _db_name_  
Émet une instruction `USE` *db_name* quand vous démarrez **sqlcmd**. Cette option définit la variable de script **sqlcmd** SQLCMDDBNAME. Ce paramètre spécifie la base de données initiale. La valeur par défaut est la propriété de base de données par défaut de votre connexion. Si la base de données n’existe pas, un message d’erreur est généré et **sqlcmd** se termine.  

**-D**  
Interprète le nom du serveur fourni à -S comme un nom de source de données plutôt qu’un nom d’hôte. Pour plus d’informations, consultez _Prise en charge du nom de source de données dans sqlcmd et bcp_ dans [Connexion avec sqlcmd](../connect/odbc/linux-mac/connecting-with-sqlcmd.md).

> [!NOTE]
> L’option -D est uniquement disponible sur les clients Linux et MacOS. Sur les clients Windows, elle faisait précédemment référence à une option désormais obsolète qui a été supprimée et est ignorée.

**-l** _login_timeout_  
Spécifie le nombre de secondes au terme duquel une connexion **sqlcmd** au pilote ODBC expire quand vous tentez d’établir une connexion à un serveur. Cette option définit la variable de script **sqlcmd** SQLCMDLOGINTIMEOUT. Le délai d’attente par défaut pour la connexion à **sqlcmd** est de huit secondes. Quand vous utilisez l’option **-G** pour vous connecter à SQL Database ou à Azure Synapse Analytics et vous authentifier à l’aide d’Azure Active Directory, il est recommandé d’indiquer un délai d’attente d’au moins 30 secondes. Le délai d'attente de la connexion doit être un nombre compris entre 0 et 65534. Si la valeur fournie n’est pas numérique ou n’est pas comprise dans cette plage, **sqlcmd** génère un message d’erreur. Une valeur de 0 spécifie un délai d'attente infini.

**-E**  
Utilise une connexion approuvée au lieu d’un nom d’utilisateur et d’un mot de passe pour se connecter à SQL Server. Par défaut, si **-E** n’est pas spécifié, **sqlcmd** utilise l’option de connexion approuvée.  

L’option **-E** ignore les éventuels paramètres de variables d’environnement de nom d’utilisateur et de mot de passe, tels que SQLCMDPASSWORD. Si l’option **-E** est utilisée avec l’option **-U** ou **-P** , un message d’erreur est généré.  

**-g**  
Définissez le paramètre de chiffrement de colonne sur `Enabled`. Pour plus d’informations, consultez [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md). Uniquement les clés principales stockées dans le magasin de certificats Windows sont prises en charge. Le commutateur -g nécessite au moins **sqlcmd** version [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Pour déterminer votre version, exécutez `sqlcmd -?`.

**-G**  
Ce commutateur est utilisé par le client durant la connexion à SQL Database ou à Azure Synapse Analytics pour faire en sorte que l’utilisateur soit authentifié à l’aide de l’authentification Azure Active Directory. Cette option définit la variable de script **sqlcmd** SQLCMDUSEAAD = true. Le commutateur -G nécessite au moins **sqlcmd** version [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Pour déterminer votre version, exécutez `sqlcmd -?`. Pour plus d’informations, consultez [Connexion à SQL Database ou Azure Synapse Analytics à l’aide de l’authentification Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview). L’option -A n’est pas prise en charge avec l’option -G.

> [!IMPORTANT]
> L’option `-G` s’applique uniquement à Azure SQL Database et à Azure Synapse Analytics.
> L’authentification interactive AAD n’est actuellement prise en charge ni sur Linux ni sur macOS. L’authentification intégrée AAD requiert [Microsoft ODBC Driver 17 pour SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) version 17.6.1 ou ultérieure et un [environnement Kerberos correctement configuré](../connect/odbc/linux-mac/using-integrated-authentication.md).


- **Nom d’utilisateur et mot de passe Azure Active Directory :** 

    Lorsque vous souhaitez utiliser un nom d’utilisateur Azure Active Directory et le mot de passe, vous pouvez fournir l’option **- G** et utiliser également le nom d’utilisateur et le mot de passe en fournissant les options **-U** et **-P** .

    ```cmd
    Sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -U bob@contoso.com -P MyAADPassword -G
    ```

    Le paramètre -G génère la chaîne de connexion suivante dans le serveur principal : 

    ```cmd
     SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID= bob@contoso.com;PWD=MyAADPassword;AUTHENTICATION = ActiveDirectoryPassword 
    ```

- **Intégrée à Azure Active Directory**

   Pour l’authentification intégrée à Azure Active Directory, spécifiez l’option **-G** sans nom d’utilisateur ni mot de passe.
   *L’authentification intégrée AAD requiert [Microsoft ODBC Driver 17 pour SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md) version 17.6.1 ou ultérieure et un [environnement Kerberos correctement configuré](../connect/odbc/linux-mac/using-integrated-authentication.md).*

    ```cmd
    Sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G
    ```

    La chaîne de connexion suivante est générée dans le service principal : 

    ```cmd
    SERVER = Target_DB_or_DW.testsrv.database.windows.net Authentication = ActiveDirectoryIntegrated; Trusted_Connection=NO
    ```

    > [!NOTE]
    > L’option `-E` (Trusted_Connection) ne peut pas être utilisée avec l’option `-G`.

- **Azure Active Directory Interactive**

    L’authentification interactive Azure AD pour Azure SQL Database et Azure Synapse Analytics vous permet d’utiliser une méthode interactive qui prend en charge l’authentification multifacteur. Pour plus d’informations, consultez [Authentification interactive Active Directory](../ssdt/azure-active-directory.md#active-directory-interactive-authentication). 

   L’authentification Azure AD interactive requiert **sqlcmd**, [version 15.0.1000.34](#download-the-latest-version-of-sqlcmd-utility) ou version ultérieure, ainsi qu’[ODBC, version 17.2 ou version ultérieure](../connect/odbc/download-odbc-driver-for-sql-server.md).  

   Pour activer l’authentification interactive, fournissez l’option -G avec le nom d’utilisateur (-U) uniquement, sans mot de passe.

   L’exemple suivant exporte des données à l’aide du mode interactif Azure AD indiquant le nom d’utilisateur où l’utilisateur représente un compte AAD. C’est le même exemple que celui utilisé dans la section précédente : *Nom d’utilisateur et mot de passe Azure Active Directory*.  

   Le mode interactif nécessite que vous entriez un mot de passe manuellement. Pour les comptes sur lesquels l’authentification multifacteur est activée, effectuez votre méthode d’authentification MFA configurée.

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U alice@aadtest.onmicrosoft.com
   ```

   La commande précédente génère la chaîne de connexion suivante dans le serveur principal :  

   ```cmd
   SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID=alice@aadtest.onmicrosoft.com; AUTHENTICATION = ActiveDirectoryInteractive   
   ```

   Si un utilisateur Azure AD est un utilisateur de domaine fédéré avec un compte Windows, le nom d’utilisateur requis sur la ligne de commande contient son compte de domaine (par exemple, joe@contoso.com voir ci-dessous) :

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U joe@contoso.com  
   ```

   S’il existe des utilisateurs invités dans un Azure AD spécifique faisant partie d’un groupe qui existe dans SQL Database et qui dispose d’autorisations de base de données pour exécuter la commande sqlcmd, leur alias d’utilisateur invité est utilisé (par exemple, *keith0@adventureworks.com* ).

  >[!IMPORTANT]
  >Il existe un problème connu lors de l’utilisation des options `-G` et `-U` avec SQLCMD, où mettre l’option `-U` avant l’option `-G` peut entraîner l’échec de l’authentification. Commencez toujours par l’option `-G` suivie de l’option `-U`.

**-H** _workstation_name_  
 Nom d'une station de travail. Cette option définit la variable de script **sqlcmd** SQLCMDWORKSTATION. Le nom de la station de travail est indiqué dans la colonne **hostname** de la vue catalogue **sys.sysprocesses** et peut être retourné à l’aide de la procédure stockée **sp_who**. Si cette option n'est pas spécifiée, le nom de l'ordinateur actif est utilisé par défaut. Ce nom peut être utilisé pour identifier différentes 

sessions **sqlcmd**.  

**j -** imprime des messages d’erreur bruts à l’écran.
  
 **-K** _application_intent_  
 Déclare le type de la charge de travail de l'application lors de la connexion à un serveur. La seule valeur actuellement prise en charge est **ReadOnly**. Si **-K** n’est pas spécifié, l’utilitaire sqlcmd ne prend pas en charge la connectivité sur un réplica secondaire dans un groupe de disponibilité AlwaysOn. Pour plus d’informations, consultez [Secondaires actifs : réplica secondaire lisible (groupes de disponibilité AlwaysOn)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)  
  
**-M** _multisubnet_failover_  
 Spécifiez toujours **-M** en cas de connexion à l’écouteur de groupe de disponibilité d’un groupe de disponibilité SQL Server ou d’une instance de cluster de basculement SQL Server. **-M** accélère la détection et la connexion au serveur (actuellement) actif. Si vous ne spécifiez pas l’option **-M** , **-M** est désactivé. Pour plus d’informations sur [Écouteurs, connectivité client et basculement d’application](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Création et configuration des groupes de disponibilité &#40;SQL Server&#41;](../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Clustering de basculement et groupes de disponibilité AlwaysOn (SQL Server)](../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md), et [Secondaires actifs : réplicas secondaires lisibles (groupes de disponibilité AlwaysOn)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
  
 **-N**  
 Ce commutateur est utilisé par le client pour demander une connexion chiffrée.  
  
 **-P** _password_  
 Spécifie le mot de passe pour l'utilisateur. Les mots de passe respectent la casse. Si l’option -U est utilisée sans l’option **-P** et que la variable d’environnement SQLCMDPASSWORD n’a pas été définie, **sqlcmd** demande à l’utilisateur d’entrer un mot de passe. Nous déconseillons d’utiliser le mot de passe Null, mais vous pouvez spécifier le mot de passe Null à l’aide d’une paire de guillemets doubles contigus pour la valeur du paramètre :

- **-P ""**

Nous vous recommandons d’utiliser un mot de passe fort.

#### <a name="use-a-strong-password"></a>[**Utilisez un mot de passe fort !**](../relational-databases/security/strong-passwords.md)

 L’invite de mot de passe s’affiche en imprimant l’invite de commande sur la console, comme suit : `Password:`  
  
 L'entrée de l'utilisateur est masquée, ce qui signifie que rien ne s'affiche et que le curseur reste immobile.  
  
 La variable d'environnement SQLCMDPASSWORD vous permet de définir un mot de passe par défaut pour la session en cours. Par conséquent, les mots de passe n'ont pas besoin d'être codés en dur dans des fichiers de commandes.  
  
 L’exemple suivant définit la variable SQLCMDPASSWORD au niveau de l’invite de commandes et accède ensuite à l’utilitaire **sqlcmd** . À l’invite de commandes, tapez :  
  
 `SET SQLCMDPASSWORD= p@a$$w0rd`  
 À l'invite de commandes suivante, tapez :  
  
 `sqlcmd`  
  
 Si la combinaison de nom d'utilisateur et de mot de passe est incorrecte, un message d'erreur est généré.  
  
**REMARQUE !**  La variable d’environnement OSQLPASSWORD a été conservée pour garantir une compatibilité descendante. La variable d'environnement SQLCMDPASSWORD est prioritaire par rapport à la variable d'environnement OSQLPASSWORD. Maintenant qu’OSQLPASSWORD n’est plus partagé, les utilitaires **sqlcmd** et **osql** peuvent être utilisés ensemble sans interférence. Les anciens scripts continueront à fonctionner.  
  
 Si l’option **-P** est utilisée avec l’option **-E** , un message d’erreur est généré.  
  
 Si l’option **-P** est suivie de plusieurs arguments, un message d’erreur est généré et le programme se termine.  
  
 **-S** [*protocole*:]*serveur*[ **\\** _instance\_nom_][ **,** _port_]  
 Spécifie l’instance de SQL Server à laquelle se connecter. Cette option définit la variable de script **sqlcmd** SQLCMDSERVER.  
  
 Spécifiez *server_name* pour vous connecter à l’instance par défaut de SQL Server sur cet ordinateur serveur. Spécifiez *server_name* [ **\\** _instance\_name_ ] pour vous connecter à une instance nommée de SQL Server sur cet ordinateur serveur. Si aucun ordinateur serveur n’est spécifié, **sqlcmd** se connecte à l’instance par défaut de SQL Server sur l’ordinateur local. Cette option est indispensable lorsque vous exécutez **sqlcmd** à partir d’un ordinateur distant connecté au réseau.  
  
 Le *protocole* peut avoir la valeur **tcp** (TCP/IP), **lpc** (mémoire partagée) ou **np** (canaux nommés).  
  
 Si vous ne spécifiez pas *server_name* [ **\\** _instance\_name_ ] quand vous démarrez **sqlcmd**, SQL Server cherche et utilise la variable d’environnement SQLCMDSERVER.  
  
> [!NOTE]  
>  La variable d'environnement OSQLSERVER a été conservée pour assurer une compatibilité descendante. La variable d’environnement SQLCMDSERVER est prioritaire par rapport à la variable d’environnement OSQLSERVER ; **sqlcmd** et **osql** peuvent donc être utilisés l’un à côté de l’autre sans interférence et les anciens scripts continuent à fonctionner.  
  
 **-U** _login_id_  
 Est le nom de connexion ou le nom d’utilisateur de base de données autonome. Pour les utilisateurs de base de données autonome, vous devez fournir l’option de nom de base de données (-d).  
  
> [!NOTE]  
>  La variable d'environnement OSQLUSER est disponible à des fins de compatibilité descendante. La variable d'environnement SQLCMDUSER est prioritaire par rapport à la variable d'environnement OSQLUSER. Il est donc possible d’utiliser **sqlcmd** et **osql** côte à côte sans interférence. Cela signifie également que les scripts **osql** existants continueront de fonctionner.  
  
 Si ni l’option **-U**, ni l’option **-P** ne sont spécifiées, **sqlcmd** tente de se connecter en utilisant le mode d’authentification Microsoft Windows. L’authentification est basée sur le compte Windows de l’utilisateur exécutant **sqlcmd**.  
  
 Si l’option **-U** est utilisée avec l’option **-E** (décrite plus loin dans cet article), un message d’erreur est généré. Si l’option **-U** est suivie de plusieurs arguments, un message d’erreur est généré et le programme se termine.  
  
 **-z** _new_password_  
 Modifier le mot de passe :  
  
 `sqlcmd -U someuser -P s0mep@ssword -z a_new_p@a$$w0rd`  
  
 **-Z** _new_password_  
 Modifier le mot de passe et quitter :  
  
 `sqlcmd -U someuser -P s0mep@ssword -Z a_new_p@a$$w0rd`  
  
 **Options d’entrée/sortie**  
  **-f** _codepage_ | **i:** _codepage_[ **,o:** _codepage_] | **o:** _codepage_[ **,i:** _codepage_]  
 Spécifie les pages de codes d'entrée et de sortie. Le numéro de pages de codes est une valeur numérique spécifiant une page de codes Windows installée.  
  
 Règles de conversion des pages de code :  
  
- Si aucune page de codes n’est spécifiée, **sqlcmd** utilise la page de codes en cours, à la fois pour le fichier d’entrée et le fichier de sortie, sauf si le fichier d’entrée est un fichier Unicode, auquel cas aucune conversion n’est requise.  
  
- **sqlcmd** reconnaît automatiquement les fichiers d’entrée Unicode Big-endian et Little-endian. Si l’option **-u** est spécifiée, les données de sortie sont toujours de type Unicode Little-endian.  
  
- Si aucun fichier de sortie n'est spécifié, la page de codes de sortie correspond à la page de codes de la console. Cette approche permet aux données de sortie d'être correctement affichées sur la console.  
  
- Plusieurs fichiers d'entrée sont supposés correspondre à une même page de codes. Les fichiers d'entrée Unicode et non Unicode peuvent être mélangés.  
  
 Entrez **chcp** à l’invite de commandes pour vérifier la page de codes de Cmd.exe.  
  
 **-i** _input_file_[ **,** _input\_file2_...]  
 Identifie le fichier contenant un traitement d'instructions SQL ou des procédures stockées. Plusieurs fichiers peuvent être spécifiés, ils sont lus et traités dans l'ordre. N'utilisez pas d'espace entre les noms de fichiers. **sqlcmd** vérifie d’abord que tous les fichiers spécifiés existent. Si un ou plusieurs fichiers n’existent pas, **sqlcmd** se termine. Les options -i et -Q/-q s'excluent mutuellement.  
  
 Exemples de chemins :  

```cmd
-i C:\<filename>  
-i \\<Server>\<Share$>\<filename>  
-i "C:\Some Folder\<file name>"  
```
  
 Les chemins d'accès aux fichiers comportant des espaces doivent être placés entre guillemets.  
  
 Cette option peut être utilisée plusieurs fois : **-i**_input\_file_ **-I**_I input_file._  
  
 **-o** _output_file_  
 Identifie le fichier recevant une sortie de **sqlcmd**.  
  
 Si **-u** est spécifié, le *fichier_sortie* est stocké au format Unicode. Si le nom de fichier n’est pas valide, un message d’erreur est généré et **sqlcmd** se termine. **sqlcmd** ne prend pas en charge l’écriture simultanée de plusieurs processus **sqlcmd** dans le même fichier. La sortie fichier sera endommagée ou incorrecte. Voir le commutateur **-f**, qui s’applique également aux formats de fichiers. Ce fichier est créé s’il n’existe pas. Un fichier portant le même nom qui provient d’une session **sqlcmd** antérieure est remplacé. Le fichier spécifié ici n'est pas le fichier **stdout** . Si un fichier **stdout** est spécifié, ce fichier ne sera pas utilisé.  
  
 Exemples de chemins :  

```cmd
-o C:< filename>  
-o \\<Server>\<Share$>\<filename>  
-o "C:\Some Folder\<file name>"  
 ```
 Les chemins d'accès aux fichiers comportant des espaces doivent être placés entre guillemets.  
  
 **-r**[**0** | **1**]  
 Redirige la sortie des messages d’erreur à l’écran (**stderr**). Si vous n'indiquez aucun paramètre ou si vous spécifiez la valeur **0**, seuls les messages d'erreur dotés d'un degré de gravité égal ou supérieur à 11 sont redirigés. Si vous indiquez la valeur **1**, tous les messages émis, y compris PRINT, sont redirigés. Est sans effet si vous utilisez - o. Par défaut, les messages sont envoyés à **stdout**.  
  
 **-R**  
 Demande à **sqlcmd** de localiser les colonnes numériques, de devise, de date et heure récupérées auprès de SQL Server, en fonction des paramètres régionaux du client. Par défaut, ces colonnes sont affichées à l'aide des paramètres régionaux du serveur.  
  
 **-u**  
 Spécifie le stockage de *fichier_sortie* au format Unicode, quel que soit le format de *fichier_entrée*.  
  
 **Options relatives à l’exécution de requêtes**  
  **-e**  
 Écrit des scripts d’entrée sur l’appareil de sortie standard (**stdout**).  
  
 **-I**  
 Attribue la valeur ON à l'option de connexion SET QUOTED_IDENTIFIER. La valeur OFF est choisie par défaut. Pour plus d’informations, consultez [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](~/t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **-q «**  _requête cmdline_  **»**  
 Exécute une requête au démarrage de **sqlcmd** , mais ne quitte pas **sqlcmd** au terme de l’exécution de la requête. Il est possible d'exécuter des requêtes séparées par plusieurs points-virgules. Placez la requête entre guillemets, comme dans l'exemple suivant.  
  
 À l’invite de commandes, tapez :  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  N'utilisez pas le terminateur GO dans la requête.  
  
 Si l’option **-b** est spécifiée avec cette option, **sqlcmd** se termine avec une erreur. L’option **-b** est traitée plus loin dans cet article.  
  
 **-Q «**   _requête cmdline_   **»**  
 Exécute une requête quand **sqlcmd** démarre, puis quitte immédiatement **sqlcmd**. Il est possible d'exécuter des requêtes séparées par plusieurs points-virgules.  
  
 Placez la requête entre guillemets, comme dans l'exemple suivant.  
  
 À l’invite de commandes, tapez :  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  N'utilisez pas le terminateur GO dans la requête.  
  
 Si l’option **-b** est spécifiée avec cette option, **sqlcmd** se termine avec une erreur. L’option **-b** est traitée plus loin dans cet article.  
  
 **-t** _query_timeout_  
 Spécifie le nombre de secondes accordées pour l'exécution d'une commande (ou une instruction SQL). Cette option définit la variable de script **sqlcmd** SQLCMDSTATTIMEOUT. Si une valeur *délai_expiration_requête* n’est pas spécifiée, la commande n’a pas de délai d’expiration. La valeur *délai_expiration* de la *requête* doit être un nombre compris entre 1 et 65 534. Si la valeur fournie n’est pas numérique ou n’est pas comprise dans cette plage, **sqlcmd** génère un message d’erreur.  
  
> [!NOTE]  
>  La valeur de délai d’expiration réelle peut différer de quelques secondes de la valeur *délai_expiration* .  
  
 **-vvar =**  _value_[ **var =** _value_...]  
 Crée une variable de script **sqlcmd** qui peut être utilisée dans un script **sqlcmd** . Placez la valeur entre guillemets si elle contient des espaces. Vous pouvez spécifier plusieurs valeurs _**var**_= **"** _valeurs_ **"** . Si l’une des valeurs spécifiées comporte des erreurs, **sqlcmd** génère un message d’erreur et se termine.  
  
 `sqlcmd -v MyVar1=something MyVar2="some thing"`  
  
 `sqlcmd -v MyVar1=something -v MyVar2="some thing"`  
  
 **-x**  
 Demande à **sqlcmd** d’ignorer les variables de script. Ce paramètre s’avère utile quand un script contient de nombreuses instructions INSERT pouvant contenir des chaînes dotées du même format que des variables régulières, par exemple $(*nom_variable*).  
  
 **Options relatives à la mise en forme**  
  **-h** _en-têtes_  
 Spécifie le nombre de lignes à imprimer entre les en-têtes de colonne. Par défaut, les en-têtes ne sont imprimés qu'une fois pour chaque jeu de résultats d'une requête. Cette option définit la variable de script **sqlcmd** SQLCMDHEADERS. Utilisez **-1** pour indiquer qu’aucun en-tête ne doit être imprimé. En présence d’une valeur non valide, **sqlcmd** génère un message d’erreur et se termine.  
  
 **-k** [**1** | **2**]  
 Supprime de la sortie tous les caractères de contrôle, par exemple les tabulations et les caractères de nouvelle ligne. Ce paramètre préserve la mise en forme des colonnes lorsque des données sont retournées. Si 1 est spécifié, les caractères de contrôle sont remplacés par un espace. Si 2 est spécifié, les caractères de contrôle sont remplacés par un espace. **-k** est identique à **-k1**.  
  
 **-s** _col_separator_  
 Spécifie le caractère de séparation des colonnes. Le caractère espace est utilisé par défaut. Cette option définit la variable de script **sqlcmd** SQLCMDCOLSEP. Pour utiliser des caractères ayant une signification spéciale pour le système d'exploitation, tels que le « et » commercial (&) ou le point-virgule (;), placez ce caractère entre guillemets ("). Le séparateur des colonnes peut être n'importe quel caractère 8 bits.  
  
 **-w** _column_width_  
 Spécifie la largeur d'écran pour la sortie. Cette option définit la variable de script **sqlcmd** SQLCMDWIDTH. La largeur de colonne doit être un nombre supérieur à 8 et inférieur à 65536. Si la largeur de colonne spécifiée n’est pas comprise dans cette plage, **sqlcmd** génère un message d’erreur. La largeur par défaut est de 80 caractères. Lorsque la longueur d'une ligne de sortie est supérieure à la largeur de colonne spécifiée, elle revient à la ligne suivante.  
  
 **-W**  
 Cette option supprime les espaces à droite d'une colonne. Utilisez cette option avec l’option **-s** lors de la préparation de données à exporter dans une autre application. Elle ne peut pas être utilisée avec les options **-y** ou **-Y** .  
  
 **-y** _variable_length_type_display_width_  
 Définit la variable de script **sqlcmd**`SQLCMDMAXVARTYPEWIDTH`. La valeur par défaut est 256. Elle limite le nombre de caractères retournés pour les types de données de longueur variable importante :  
  
- **varchar(max)**  
  
- **nvarchar(max)**  
  
- **varbinary(max)**  
  
- **xml**  
  
- **UDT (types de données définis par l’utilisateur)**  
  
- **text**  
  
- **ntext**  
  
- **image**  
  
> [!NOTE]
> Les types UDT peuvent être de longueur fixe, selon la mise en œuvre. Si cette longueur d’un type UDT à longueur fixe est inférieure à *largeur_affichage*, la valeur du type UDT retournée n’est pas affectée. Cependant, si la longueur est supérieure à *largeur_affichage*, la sortie est tronquée.  
  
> [!IMPORTANT]
>  Utilisez l’option **-y 0** avec une extrême prudence, car elle peut créer de graves problèmes de performances sur le serveur et le réseau, en fonction de la taille des données retournées.  
  
 **-Y** _fixed_length_type_display_width_  
 Définit la variable de script **sqlcmd**`SQLCMDMAXFIXEDTYPEWIDTH`. La valeur par défaut est 0 (illimitée). Limite le nombre de caractères retournés pour les types de données suivants :  
  
- **char(** _n_ **)** , où 1 <= n <= 8000  
  
- **nchar(n** _n_ **)** , où 1 <= n <= 4000  
  
- **varchar(n** _n_ **)** , où 1 <= n <= 8000  
  
- **nvarchar(n** _n_ **)** , où 1 <= n <= 4000  
  
- **varbinary(n** _n_ **)** , où 1 <= n \<= 4000  
  
- **variant**  
  
 **Options relatives aux rapports d’erreurs**  
  **-b**  
 Spécifie que **sqlcmd** prend fin et retourne une valeur DOS ERRORLEVEL quand une erreur se produit. La valeur qui est retournée à la variable DOS ERRORLEVEL est **1** quand le message d’erreur de SQL Server a un niveau de gravité supérieur à 10 ; sinon, la valeur retournée est **0**. Si l’option **-V** a été définie en complément de **-b**, **sqlcmd** ne signale pas d’erreur si le niveau de gravité est inférieur aux valeurs définies à l’aide de **-V**. Les fichiers de commande peuvent tester la valeur de ERRORLEVEL et traiter l'erreur d'une manière appropriée. **sqlcmd** ne signale pas d’erreurs pour un niveau de gravité 10 (messages d’information).  
  
 Si le script **sqlcmd** contient un commentaire incorrect, une erreur de syntaxe ou si une variable de script est manquante, la valeur ERRORLEVEL retournée est 1.  
  
 **-m** _error_level_  
 Détermine les messages d’erreur envoyés à **stdout**. Les messages assortis d'un niveau de gravité supérieur ou égal à ce niveau sont envoyés. Quand cette valeur est égale à **-1**, tous les messages, notamment les messages d’information, sont envoyés. Les espaces ne sont pas autorisés entre **-m** et **-1**. Par exemple, **-m-1** est valide, mais **-m-1** ne l’est pas.  
  
 Cette option définit également la variable de script **sqlcmd** SQLCMDERRORLEVEL. La valeur par défaut de cette variable est 0.  
  
 **-V** _error_severity_level_  
 Contrôle le niveau de gravité utilisé pour définir la variable ERRORLEVEL. Les messages d'erreur assortis de niveaux de gravité supérieurs ou égaux à cette valeur définissent ERRORLEVEL. Les valeurs inférieures à 0 sont signalées comme étant 0. La valeur de la variable ERRORLEVEL peut être testée au moyen de fichiers de commandes et CMD.  
  
 **Options diverses**  
  **-a** _packet_size_  
 Demande un paquet d'une taille différente. Cette option définit la variable de script **sqlcmd** SQLCMDPACKETSIZE. *taille_paquet* doit être une valeur comprise entre 512 et 32767. La valeur par défaut est de 4096. Une plus grande taille de paquet peut améliorer les performances d'exécution des scripts comportant un grand nombre d'instructions SQL entre des commandes GO. Vous pouvez demander une taille de paquet plus élevée. Cependant, si la requête est refusée, **sqlcmd** adopte la taille par défaut du serveur comme taille de paquet.  
  
 **-c** _batch_terminator_  
 Spécifie le terminateur de traitement. Par défaut, il faut entrer la commande « GO » sur une ligne isolée pour terminer une commande et la soumettre à SQL Server. Quand vous réinitialisez le terminateur du lot, n’utilisez pas de mots clés réservés de Transact-SQL ou des caractères ayant une signification particulière pour le système d’exploitation, même s’ils sont précédés d’une barre oblique inverse.  
  
 **-L**[**c**]  
 Répertorie tous les serveurs configurés localement et le nom des serveurs diffusant sur le réseau. Ce paramètre ne peut pas être utilisé en combinaison avec d'autres paramètres. Le nombre maximal de serveurs pouvant être répertoriés est de 3000. Si la liste de serveurs est tronquée en raison de la taille de la mémoire tampon, un message d'avertissement s'affiche.  
  
> [!NOTE]  
> Compte tenu de la nature de la diffusion sur les réseaux, il est possible que **sqlcmd** ne reçoive pas de réponse de tous les serveurs dans les délais impartis. Par conséquent, la liste des serveurs retournée peut varier à chaque invocation de cette option.  
  
 Si le paramètre optionnel **c** est spécifié, la sortie s'affiche sans les **serveurs** : la ligne d'en-tête et chaque ligne de serveur apparaissent sans espace de début. Cette présentation est qualifiée comme « propre ». Une sortie propre améliore les performances de traitement des langages de script.  
  
 **-p**[**1**]  
 Imprime des statistiques de performances pour chaque jeu de résultats. L'écran suivant illustre le format des statistiques de performances :  
  
 `Network packet size (bytes): n`  
  
 `x xact[s]:`  
  
 `Clock Time (ms.): total       t1  avg       t2 (t3 xacts per sec.)`  
  
 Où :  
  
 `x` = Nombre de transactions traitées par SQL Server.  
  
 `t1` = Durée totale de toutes les transactions.  
  
 `t2` = Durée moyenne d’une transaction spécifique.  
  
 `t3` = Nombre moyen de transactions par seconde.  
  
 Toutes les durées sont exprimées en millisecondes.  
  
 Si le paramètre facultatif **1** est spécifié, les statistiques sont séparées par deux points et peuvent être aisément importées dans une feuille de calcul ou traitées par un script.  
  
 Si le paramètre facultatif correspond à n’importe quelle valeur autre que **1**, une erreur est générée et **sqlcmd** se termine.  
  
 **-X**[**1**]  
 Désactive les commandes pouvant compromettre la sécurité du système quand **sqlcmd** est exécuté à partir d’un fichier de commandes. Les commandes désactivées sont toujours reconnues ; **sqlcmd** émet un message d’avertissement et continue. Si le paramètre facultatif **1** est spécifié, **sqlcmd** génère un message d’erreur, puis il se termine. Les commandes suivantes sont désactivées quand l’option **-X** est utilisée :  
  
- **ED**  
  
- **!!** _command_  
  
 Si l’option **-X** est spécifiée, elle empêche le passage des variables d’environnement à **sqlcmd**. Elle interdit également l'exécution du script de démarrage spécifié au moyen de la variable de script SQLCMDINI. Pour plus d’informations sur les variables de script **sqlcmd** , consultez [Utiliser sqlcmd avec des variables de script](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 **-?**  
 Affiche la version de **sqlcmd** et un résumé de la syntaxe des options de **sqlcmd** .  
  
## <a name="remarks"></a>Notes

Les options ne doivent pas nécessairement être utilisées dans l'ordre indiqué dans la section de la syntaxe.

Lorsque plusieurs résultats sont retournés, **sqlcmd** imprime une ligne vide entre chaque ensemble de résultats dans un traitement. En outre, le message `<x> rows affected` ne s’affiche pas lorsqu’il ne concerne pas l’instruction exécutée.

Pour utiliser **sqlcmd** de façon interactive, tapez **sqlcmd** dans l’invite de commandes avec une ou plusieurs des options décrites plus haut dans cet article. Pour plus d’informations, consultez [Utiliser l’utilitaire sqlcmd](~/relational-databases/scripting/sqlcmd-use-the-utility.md).

> [!NOTE]  
>  Les options **-L**, **-Q**, **-Z** et **-i** entraîne la fermeture de **sqlcmd** après l’exécution.
  
 La longueur totale de la ligne de commande **sqlcmd** dans l’environnement de commande (Cmd.exe), comprenant tous les arguments et les variables développées, est la longueur déterminée par le système d’exploitation pour Cmd.exe.
  
## <a name="variable-precedence-low-to-high"></a>Priorité des variables (faible à élevée)
  
1.  Variables d'environnement au niveau du système.
  
2.  Variables d'environnement au niveau de l'utilisateur.  
  
3.  Environnement d’exécution de commande (**SET** X=Y) défini à l’invite de commandes avant l’exécution de **sqlcmd**.  
  
4.  **sqlcmd-v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Pour afficher les variables d’environnement, dans le **Panneau de configuration**, ouvrez **Système**, puis cliquez sur l’onglet **Avancé** .  
  
## <a name="sqlcmd-scripting-variables"></a>Variables de script sqlcmd  
  
|Variable|Commutateur associé|R/W (Lecture/écriture)|Default|  
|--------------|--------------------|----------|-------------|  
|SQLCMDUSER|-U|R|""|  
|SQLCMDPASSWORD|-P|--|""|  
|SQLCMDSERVER|-S|R|"DefaultLocalInstance"|  
|SQLCMDWORKSTATION|-H|R|"ComputerName"|  
|SQLCMDDBNAME|-d|R|""|  
|SQLCMDLOGINTIMEOUT|-l|R/W (Lecture/écriture)|"8" (secondes)|  
|SQLCMDSTATTIMEOUT|-T|R/W (Lecture/écriture)|"0" = Attendre indéfiniment|  
|SQLCMDHEADERS|-H|R/W (Lecture/écriture)|"0"|  
|SQLCMDCOLSEP|-S|R/W (Lecture/écriture)|" "|  
|SQLCMDCOLWIDTH|-w|R/W (Lecture/écriture)|"0"|  
|SQLCMDPACKETSIZE|-a|R|"4096"|  
|SQLCMDERRORLEVEL|-M|R/W (Lecture/écriture)|0|  
|SQLCMDMAXVARTYPEWIDTH|-y|R/W (Lecture/écriture)|"256"|  
|SQLCMDMAXFIXEDTYPEWIDTH|-y|R/W (Lecture/écriture)|"0" = illimitée|  
|SQLCMDEDITOR||R/W (Lecture/écriture)|"edit.com"|  
|SQLCMDINI||R|""|
|SQLCMDUSEAAD  | -G | R/W (Lecture/écriture) | "" |  
  
 SQLCMDUSER, SQLCMDPASSWORD et SQLCMDSERVER sont définis lorsque la commande **:Connect** est utilisée.  
  
 R indique que la valeur ne peut être définie qu'une seule fois lors de l'initialisation du programme.  
  
 R/W (« Lecture/écriture ») indique que la valeur peut être modifiée à l’aide de la commande **setvar** et que les commandes ultérieures sont tributaires de la nouvelle valeur.  
  
## <a name="sqlcmd-commands"></a>Commandes sqlcmd  
 En complément des instructions Transact-SQL dans **sqlcmd**, vous pouvez également utiliser les commandes suivantes :  
  
|||  
|-|-|  
|**GO** [*count*]|**:List**|  
|[ **:** ] **RESET**|**:Error**|  
|[ **:** ] **ED**|**:Out**|  
|[**:**] **!!**|**:Perftrace**|  
|[ **:** ] **QUIT**|**:Connect**|  
|[ **:** ] **EXIT**|**:On Error**|  
|**:r**|**:Help**|  
|**:ServerList**|**:XML** [**ON** &#124; **OFF**]|  
|**:Setvar**|**:Listvar**|  
  
 Tenez compte des éléments suivants lorsque vous utilisez des commandes **sqlcmd** :  
  
- Toutes les commandes **sqlcmd** , à l’exception de GO, doivent être précédées d’un signe deux-points (:).  
  
    > [!IMPORTANT]  
    >  Pour assurer la compatibilité descendante avec les scripts **osql** existants, certaines commandes sont reconnues sans les deux-points. Elles sont indiquées par [ **:** ].
  
- Les commandes **sqlcmd** ne sont reconnues que si elles apparaissent au début d’une ligne.  
  
- Toutes les commandes **sqlcmd** ne respectent pas la casse.  
  
- Chaque commande doit figurer sur une ligne séparée. Une commande ne peut pas être suivie d’une instruction Transact-SQL ou d’une autre commande.  
  
- Les commandes sont exécutées immédiatement. Elles ne sont pas placées dans le tampon d’exécution comme le sont les instructions Transact-SQL.  
  
 **Commandes d’édition**  
  [ **:** ] **ED**  
 Démarre l'éditeur de texte. Cet éditeur peut être utilisé pour modifier le lot Transact-SQL actif ou le dernier lot exécuté. Pour modifier le dernier traitement exécuté, la commande **ED** doit être tapée immédiatement après la fin de l'exécution du dernier traitement.  
  
 L'éditeur de texte est défini dans la variable d'environnement SQLCMDEDITOR. L'éditeur par défaut est « edit ». Pour modifier l'éditeur, définissez la variable SQLCMDEDITOR. Par exemple, pour choisir l’éditeur Bloc-notes de Microsoft, à l’invite de commandes, tapez :  
  
 `SET SQLCMDEDITOR=notepad`  
  
 [ **:** ] **RESET**  
 Vide le cache d'instruction.  
  
 **:List**  
 Imprime le contenu du cache d'instruction.  
  
 **Variables**  
  **:Setvar** \<**var**> [ **"** _value_ **"** ]  
 Définit les variables de script **sqlcmd** . Les variables de script possèdent le format suivant : `$(VARNAME)`.  
  
 Les noms de variable ne respectent pas la casse.  
  
 Les variables de script peuvent être définies comme suit :  
  
- Implicitement à l'aide d'une option de ligne de commande. Par exemple, l’option **-l** définit la variable **sqlcmd** SQLCMDLOGINTIMEOUT.  
  
- Explicitement à l'aide de la commande **:Setvar** .  
  
- En définissant une variable d’environnement avant d’exécuter **sqlcmd**.  
  
> [!NOTE]  
>  L’option **-X** empêche le passage des variables d’environnement à **sqlcmd**.  
  
 Si une variable définie à l'aide de **:Setvar** et une variable d'environnement possèdent le même nom, la variable définie à l'aide de **:Setvar** est prioritaire.  
  
 Les noms de variable ne doivent pas contenir de caractères espace.  
  
 Les noms de variables ne peuvent pas posséder la même forme qu'une expression variable, telle que $(var).  
  
 Si une valeur de chaîne de la variable de script contient des espaces, placez la valeur entre guillemets. Si aucune valeur n'est spécifiée pour une variable de script, cette dernière est supprimée.  
  
 **:Listvar**  
 Affiche la liste des variables de script actuellement définies.  
  
> [!NOTE]  
>  Seules les variables de script qui sont définies par **sqlcmd** et celles qui sont définies avec la commande **:Setvar** s’affichent.  
  
 **Commandes de sortie**  
  **:Error**   
 _**\<**_  _filename_  **_>|_ STDERR|STDOUT**  
 Redirige l’ensemble de la sortie d’erreur dans le fichier spécifié par *nom_fichier* vers **stderr** ou vers **stdout**. La commande **Error** peut apparaître plusieurs fois dans un script. Par défaut, la sortie d'erreur est envoyée à **stderr**.  
  
 *nom de fichier*  
 Crée et ouvre un fichier destiné à recevoir la sortie. Si le fichier existe déjà, il est tronqué à zéro octet. Si le fichier n'est pas disponible car des autorisations sont requises, ou pour d'autres raisons, la sortie n'est pas modifiée, et elle est envoyée à la dernière destination spécifiée ou à la destination par défaut.  
  
 **STDERR**  
 Dirige la sortie d’erreur vers le flux **stderr** . Si cette destination a été redirigée, la cible de cette redirection reçoit la sortie d'erreur.  
  
 **STDOUT**  
 Fait basculer la sortie d’erreur vers le flux **stdout** . Si cette destination a été redirigée, la cible de cette redirection reçoit la sortie d'erreur.  
  
 **:Out \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Crée et redirige l’ensemble des résultats de requête dans le fichier spécifié par *nom_fichier* vers **stderr** ou vers **stdout**. Par défaut, la sortie est envoyée à **stdout**. Si le fichier existe déjà, il est tronqué à zéro octet. La commande **Out** peut apparaître plusieurs fois dans un script.  
  
 **:Perftrace \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Crée et redirige l’ensemble des informations de traces de performances dans le fichier spécifié par *nom_fichier* vers **stderr** ou vers **stdout**. Par défaut, la sortie de traces de performances est envoyée à **stdout**. Si le fichier existe déjà, il est tronqué à zéro octet. La commande **Perftrace** peut apparaître plusieurs fois dans un script.  
  
 **Commandes de contrôle d’exécution**  
  **:On Error**[ **exit** | **ignore**]  
 Définit l'action à effectuer lorsqu'une erreur se produit en cours de script ou d'exécution d'un traitement.  
  
 Lorsque l’option **exit** est employée, **sqlcmd** se termine avec la valeur d’erreur appropriée.  
  
 Lorsque l’option **ignore** est employée, **sqlcmd** ignore l’erreur et poursuit l’exécution du traitement ou du script. Par défaut, un message d'erreur est imprimé.  
  
 [ **:** ] **QUIT**  
 Entraîne la fermeture de **sqlcmd** .  
  
 [ **:** ] **EXIT**[ **(** _instruction_ **)** ]  
 Vous permet d’utiliser le résultat d’une instruction SELECT comme valeur de retour de **sqlcmd**. S'il est numérique, la première colonne de la dernière ligne de résultats est convertie en un entier de 4 octets (entier long). MS-DOS, Linux et Mac transmettent l'octet faible au processus parent ou au niveau erreur du système d'exploitation. Windows 200x transmet la totalité de l'entier de 4 octets. La syntaxe est :  
  
 `:EXIT(query)`  
  
 Par exemple :  
  
 `:EXIT(SELECT @@ROWCOUNT)`  
  
 Vous pouvez également inclure le paramètre **EXIT** dans un fichier de commandes. Par exemple, à l'invite de commandes, tapez :  
  
 `sqlcmd -Q "EXIT(SELECT COUNT(*) FROM '%1')"`  
  
 L’utilitaire **sqlcmd** envoie tout le contenu entre parenthèses **()** au serveur. Si une procédure stockée système sélectionne un ensemble et retourne une valeur, seule la sélection est retournée. L’instruction EXIT **()** sans information entre parenthèses exécute toutes les commandes qui la précèdent dans le traitement, puis se termine sans valeur de retour.  
  
 Lorsqu’une requête incorrecte est spécifiée, **sqlcmd** se termine sans valeur de retour.  
  
 Liste des formats EXIT :  
  
- :EXIT  
  
 N'exécute pas le traitement, puis se termine immédiatement sans retourner de valeur.  
  
- :EXIT( )  
  
 Exécute le traitement, puis quitte sans retourner de valeur.  
  
- :EXIT(requête)  
  
 Exécute le traitement qui inclut la requête, puis se termine après avoir retourné les résultats de la requête.  
  
 Si RAISERROR est utilisé dans un script **sqlcmd** et qu’une erreur de gravité 127 se produit, l’exécution de **sqlcmd** se termine et l’ID du message est retourné au client. Par exemple :  
  
 `RAISERROR(50001, 10, 127)`  
  
 Cette erreur arrête l’exécution du script **sqlcmd** et envoie au client le message 50001.  
  
 Les valeurs retournées de -1 à -99 sont réservées à SQL Server, et **sqlcmd** définit les valeurs de retour supplémentaires suivantes :  
  
|Valeurs de retour|Description|  
|-------------------|-----------------|  
|-100|Erreur rencontrée avant la sélection d'une valeur retournée.|  
|-101|Aucune ligne trouvée lors de la sélection d'une valeur retournée.|  
|-102|Erreur de conversion survenue lors de la sélection d'une valeur retournée.|  
  
 **GO** [*count*]  
 GO indique la fin d'un traitement et l'exécution des instructions Transact-SQL placées dans le cache. Le lot est exécuté plusieurs fois par lots distincts. Vous ne pouvez pas déclarer une variable plusieurs fois dans un même lot.
  
 **Commandes diverses**  
  **:r \<** _filename_ **>**  
 Analyse les instructions Transact-SQL et les commandes **sqlcmd** supplémentaires du fichier spécifié par **\<**_filename_**>** dans le cache des instructions.  
  
 Si le fichier contient des instructions Transact-SQL qui ne sont pas suivies par **GO**, vous devez entrer **GO** sur la ligne qui suit **:r**.  
  
> [!NOTE]  
>  **\<** _filename_ **>** est lu par rapport au répertoire de démarrage dans lequel **sqlcmd** a été exécuté.  
  
 Le fichier est lu et exécuté après la rencontre d'un terminateur de traitement. Vous pouvez émettre plusieurs commandes **:r** . Le fichier peut inclure une commande **sqlcmd** . Elle inclut le terminateur de traitement **GO**.  
  
> [!NOTE]  
>  Le nombre de lignes affichées en mode interactif augmente de 1 chaque fois qu'une commande `:r` est rencontrée. La commande `:r` apparaît dans la sortie de la commande list.  
  
 **:Serverlist**  
 Répertorie tous les serveurs configurés localement et les noms des serveurs émettant sur le réseau.  
  
 **:Connect**  _server_name_[ **\\** _instance\_name_] [-l *timeout*] [-U *user_name* [-P *password*]]  
 Se connecte à une instance de SQL Server. Ferme également la connexion actuelle.  
  
 Options de délai :  
  
|Valeur|Comportement|  
|-|-|  
|0|attendre indéfiniment|  
|n>0|attendre n secondes|  
  
 La variable de script SQLCMDSERVER reflète la connexion active actuelle.  
  
 Si l'argument *timeout* n'est pas spécifié, la valeur de la variable SQLCMDLOGINTIMEOUT est la valeur par défaut.  
  
 Si seulement *nom_utilisateur* est spécifié (en tant qu’option ou en tant que variable d’environnement), un message invite l’utilisateur à entrer un mot de passe. Ce message n’apparaît pas si la variable d’environnement SQLCMDUSER ou la variable d’environnement SQLCMDPASSWORD a été définie. Si ni les options ni les variables d'environnement ne sont fournies, le mode d'authentification Windows est employé pour se connecter. Par exemple, pour établir une connexion à une instance `instance1` de SQL Server, `myserver`, à l’aide de la sécurité intégrée, utilisez la commande suivante :  
  
 `:connect myserver\instance1`  
  
 Pour établir une connexion à l'instance par défaut de `myserver` en utilisant des variables de script, utilisez ce qui suit :  
  
 `:setvar myusername test`  
  
 `:setvar myservername myserver`  
  
 `:connect $(myservername) $(myusername)`  
  
 [ **:** ] **!!** < *commande*>  
 Exécute des commandes du système d'exploitation. Pour exécuter une commande du système d’exploitation, commencez une ligne par deux points d’exclamation ( **!!** ) suivis de la commande du système d’exploitation. Par exemple :  
  
 `:!! Dir`  
  
> [!NOTE]  
>  La commande est exécutée sur l’ordinateur sur lequel **sqlcmd** s’exécute.  
  
 **:XML** [**ON** | **OFF**]  
 Pour plus d’informations, voir [Format de sortie XML](#OutputXML) et [Format de sortie JSON](#OutputJSON) plus loin dans cet article.  
  
 **:Help**  
 Répertorie les commandes **sqlcmd** avec une brève description de chaque commande.  
  
### <a name="sqlcmd-file-names"></a>Noms de fichiers sqlcmd  
 Les fichiers d’entrée **sqlcmd** peuvent être spécifiés avec l’option **-i** ou la commande **:r** . Les fichiers de sortie peuvent être spécifiés avec l’option **-o** ou les commandes **:Error**, **:Out** et **:Perftrace** . Voici quelques consignes relatives à l'utilisation de ces fichiers :  
  
- **:Error**, **:Out** et **:Perftrace** doivent utiliser un autre **\<**_filename_**>** . Si le même **\<**_filename_**>** est utilisé, les entrées des commandes peuvent être mélangées.  
  
- Si un fichier d’entrée situé sur un serveur distant est appelé à partir de **sqlcmd** sur un ordinateur local et qu’il contient un chemin d’accès de fichier sur un lecteur, comme :Out C:\OutputFile.txt, le fichier de sortie est créé sur l’ordinateur local et non sur le serveur distant.  
  
- Les chemins d’accès de fichier valides sont les suivants : `C:\<filename>`, `\\<Server>\<Share$>\<filename>` et `"C:\Some Folder\<file name>"`. Si le chemin d'accès comporte un espace, utilisez des guillemets.  
  
- Chaque nouvelle session **sqlcmd** remplace les fichiers existants qui ont des noms identiques.  
  
### <a name="informational-messages"></a>Messages d'information

**sqlcmd** imprime les messages d’information envoyés par le serveur. Dans l’exemple suivant, une fois que les instructions Transact-SQL sont exécutées, un message d’information est affiché.
  
Dans l’invite de commandes, tapez la commande :

`sqlcmd`
  
À l’invite sqlcmd, tapez :

`USE AdventureWorks2012;`

`GO`

Lorsque vous appuyez sur ENTRÉE, le message d'information suivant s'affiche : « Le contexte de la base de données a changé ; il est maintenant 'AdventureWorks2012'. »  
  
### <a name="output-format-from-transact-sql-queries"></a>Format de sortie des requêtes Transact-SQL  
 **sqlcmd** imprime tout d’abord un en-tête de colonne qui contient les noms de colonne spécifiés dans la liste de sélection. Les noms de colonne sont séparés avec le caractère SQLCMDCOLSEP. Par défaut, il s'agit d'un espace. Si le nom de colonne est plus court que la largeur de colonne, la sortie est complétée avec des espaces jusqu'à la colonne suivante.  
  
 Cette ligne est suivie d'un séparateur de ligne qui correspond à une série de tirets. La sortie suivante constitue un exemple.  
  
 Démarrer **sqlcmd**. Dans l’invite de commandes **sqlcmd**, tapez la requête :  
  
 `USE AdventureWorks2012;`  
  
 `SELECT TOP (2) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 Lorsque vous appuyez sur ENTRÉE, le jeu de résultats suivant est retourné.  
  
 `BusinessEntityID FirstName    LastName`  
  
 `---------------- ------------ ----------`  
  
 `285              Syed         Abbas`  
  
 `293              Catherine    Abel`  
  
 `(2 row(s) affected)`  
  
 Bien que la colonne `BusinessEntityID` ne soit large que de quatre caractères, elle a été étendue pour contenir les noms de colonne plus longs. Par défaut, la sortie se termine à 80 caractères. Vous pouvez modifier ceci en utilisant l’option **-w** ou en définissant la variable de script SQLCMDCOLWIDTH.  
  
###  <a name="xml-output-format"></a><a name="OutputXML"></a> Format de sortie XML  
 La sortie XML qui est le résultat d'une clause FOR XML est générée, sans mise en forme, dans un flux continu.  
  
 Lorsque vous attendez une sortie XML, utilisez la commande suivante : `:XML ON`.  
  
> [!NOTE]  
>  **sqlcmd** retourne des messages d’erreur au format habituel. Notez que les messages d'erreur sont également une sortie dans le flux de texte XML au format XML. Avec `:XML ON`, **sqlcmd** n’affiche aucun message d’information.  
  
 Pour désactiver le mode XML, utilisez la commande suivante : `:XML OFF`.  
  
 La commande GO ne doit pas apparaître avant la commande XML OFF, car cette dernière remet **sqlcmd** en sortie orientée ligne.  
  
 Les données XML (diffusées en continu) et les données d’ensemble de lignes ne peuvent être mélangées. Si la commande XML ON n’a pas été émise avant l’exécution d’une instruction Transact-SQL qui génère des flux XML, la sortie est incohérente. Une fois la commande XML ON émise, il n’est pas possible d’exécuter des instructions Transact-SQL qui produisent des ensembles de lignes réguliers.  
  
> [!NOTE]  
>  La commande `:XML` ne prend pas en charge l’instruction SET STATISTICS XML.  
  
###  <a name="json-output-format"></a><a name="OutputJSON"></a> Format de sortie JSON  
 Lorsque vous attendez une sortie JSON, utilisez la commande suivante : `:XML ON`. Dans le cas contraire, la sortie inclut le nom de colonne et le texte JSON. Cette option n’est pas valide pour JSON.  
  
 Pour désactiver le mode XML, utilisez la commande suivante : `:XML OFF`.  
  
 Pour plus d’informations, voir [Format de sortie XML](#OutputXML) dans cet article.  

### <a name="using-azure-active-directory-authentication"></a>Utilisation de l’authentification Azure Active Directory

Exemples d’utilisation de l’authentification Azure Active Directory :

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  -l 30
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G -U bob@contoso.com -P MyAADPassword -l 30
```
  
## <a name="sqlcmd-best-practices"></a>Méthodes conseillées sqlcmd

Utilisez les méthodes suivantes pour optimiser la sécurité et l'efficacité.  

- Utilisez la sécurité intégrée.  

- Utilisez **-X[1]** dans des environnements automatisés.

- Protégez les fichiers d'entrée et de sortie à l'aide des autorisations appropriées du système de fichiers NTFS.

- Pour accroître les performances, effectuez autant d’opérations que possible au sein d’une session **sqlcmd** au lieu de recourir à une série de sessions.

- Pour l'exécution de requête ou de traitement, définissez des valeurs de délai supérieures à la durée que vous prévoyez pour l'exécution du traitement ou de la requête.

Utilisez les méthodes suivantes pour améliorer l’exactitude :

- Utilisez **-V16** pour consigner les [messages de gravité de niveau 16](../relational-databases/errors-events/database-engine-error-severities.md#levels-of-severity).  Les messages de gravité 16 indiquent des erreurs générales qui peuvent être corrigées par l'utilisateur.

- Une fois le processus terminé, vérifiez le code de sortie et la variable DOS ERRORLEVEL.  **sqlcmd** retourne normalement la valeur 0 ; sinon, il définit la variable ERRORLEVEL comme configurée par **-V**.  En d’autres termes, ERRORLEVEL ne doit pas afficher la même valeur que le numéro d’erreur renvoyé par SQL Server. Le numéro d’erreur est une valeur spécifique à SQL Server et correspondant à la fonction système [ **@@ERROR**](../t-sql/functions/error-transact-sql.md).  ERRORLEVEL est une valeur spécifique à SQLCMD pour indiquer la raison pour laquelle SQLCMD s’est terminé, et sa valeur est conditionnée par l’argument de ligne de commande **-b**.

L’utilisation de **-V16** conjointement avec la vérification du code de sortie et de la variable DOS ERRORLEVEL peut aider à détecter les erreurs dans les environnements automatisés, en particulier les contrôles de qualité avant une version de production.

## <a name="next-steps"></a>Étapes suivantes

- [Démarrer l’utilitaire sqlcmd](~/relational-databases/scripting/sqlcmd-start-the-utility.md)
- [Exécuter des fichiers de script Transact-SQL à l'aide de sqlcmd](~/relational-databases/scripting/sqlcmd-run-transact-sql-script-files.md)
- [Utiliser l'utilitaire sqlcmd](~/relational-databases/scripting/sqlcmd-use-the-utility.md)
- [Utiliser sqlcmd avec des variables de script](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md)
- [Se connecter au moteur de base de données avec sqlcmd](~/relational-databases/scripting/sqlcmd-connect-to-the-database-engine.md)
- [Modifier des scripts SQLCMD à l’aide de l’Éditeur de requête](~/relational-databases/scripting/edit-sqlcmd-scripts-with-query-editor.md)
- [Gérer les étapes de travail](~/ssms/agent/manage-job-steps.md)   
- [Créer une étape de travail CmdExec](~/ssms/agent/create-a-cmdexec-job-step.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]