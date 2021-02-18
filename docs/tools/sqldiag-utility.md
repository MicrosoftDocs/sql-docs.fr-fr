---
title: SQLdiag (utilitaire)
description: Utilisez l’utilitaire SQLdiag pour collecter des fichiers journaux et des fichiers de données à partir de SQL Server et d’autres types de serveurs, et pour analyser les serveurs dans le temps ou résoudre les problèmes.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], SQLdiag
- stopping diagnostic collection
- storing diagnostic information
- performance [SQL Server], diagnostic collection
- diagnostic records [SQL Server]
- scripts [SQL Server], diagnostic collection
- logs [SQL Server], diagnostic collection
- starting diagnostic collection
- clustered instance of SQL Server
- monitoring performance [SQL Server], diagnostic collection
- security [SQL Server], diagnostic collection
- SQLDIAG service
- space [SQL Server], diagnostic collection
- SQLdiag utility
- disk space [SQL Server], diagnostic collection
- configuration files [SQL Server]
- automatic diagnostic collection
- clusters [SQL Server], diagnostic collection
ms.assetid: 45ba1307-33d1-431e-872c-a6e4556f5ff2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1c9f3528ac4f377e379afd26e6583df3fe63fb2b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353332"
---
# <a name="sqldiag-utility"></a>SQLdiag (utilitaire)
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  L’utilitaire **SQLdiag** est un utilitaire de collecte de données de diagnostic, conçu pour un usage général. Il est possible de l’exécuter en tant qu’application console ou service. Vous pouvez utiliser **SQLdiag** pour collecter des fichiers journaux et des fichiers de données à partir de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et depuis d’autres types de serveurs, mais aussi analyser vos serveurs au fil des jours ou trouver des solutions à des problèmes spécifiques les concernant. **SQLdiag** a été conçu pour accélérer et simplifier la collecte d’informations de diagnostic pour les services d’assistance de [!INCLUDE[msCoName](../includes/msconame-md.md)] .  
  
> [!NOTE]  
>  Cet utilitaire peut être amené à évoluer par la suite, ainsi les applications ou les scripts élaborés à partir de son comportement ou de ses arguments de ligne de commande peuvent ne pas fonctionner correctement dans les versions ultérieures qui seront développées.  
  
 **SQLdiag** peut recueillir les types suivants d’informations de diagnostic :  
  
-   Journaux de performances Windows  
  
-   Journaux des événements Windows  
  
-   [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] traces  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] informations de blocage  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] informations de configuration  
  
 Vous pouvez spécifier les types d’informations que vous souhaitez collecter par **SQLdiag** en modifiant le fichier de configuration SQLDiag.xml, décrit dans une section suivante.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sqldiag   
     { [/?] }  
     |  
     { [/I configuration_file]  
       [/O output_folder_path]  
       [/P support_folder_path]  
       [/N output_folder_management_option]  
       [/M machine1 [ machine2 machineN]| @machinelistfile]  
       [/C file_compression_type]  
       [/B [+]start_time]  
       [/E [+]stop_time]  
       [/A SQLdiag_application_name]  
       [/T { tcp [ ,port ] | np | lpc } ]  
       [/Q] [/G] [/R] [/U] [/L] [/X] }  
     |  
     { [START | STOP | STOP_ABORT] }  
     |  
     { [START | STOP | STOP_ABORT] /A SQLdiag_application_name }  
```  
  
## <a name="arguments"></a>Arguments  
 **/?**  
 Affiche des informations sur l’utilisation.  
  
 **/I** _configuration_file_  
 Définit le fichier de configuration que **SQLdiag** doit utiliser. Par défaut, **/I** a la valeur SQLDiag.Xml.  
  
 **/O** _output_folder_path_  
 Redirige la sortie de **SQLdiag** vers le dossier spécifié. Si l’option **/O** n’est pas spécifiée, la sortie de **SQLdiag** est écrite dans un sous-dossier nommé SQLDIAG, sous le dossier de démarrage **SQLdiag** . Si le dossier SQLDIAG n’existe pas, **SQLdiag** tente de le créer.  
  
> [!NOTE]  
>  L’emplacement du dossier de sortie varie en fonction de l’emplacement du dossier de support qui peut être spécifié à l’aide de l’option **/P**. Si vous voulez définir un emplacement complètement différent pour le dossier de sortie, spécifiez le chemin complet du répertoire pour **/O**.  
  
 **/P** _support_folder_path_  
 Définit le chemin d'accès au dossier de support. Par défaut, l’option **/P** a comme valeur le dossier qui contient le fichier exécutable **SQLdiag** . Le dossier de support contient les fichiers de prise en charge de **SQLdiag** , par exemple le fichier de configuration XML, les scripts Transact-SQL ainsi que d’autres fichiers dont l’utilitaire a besoin quand il collecte des diagnostics. Si vous utilisez cette option pour définir un autre chemin d’accès aux fichiers de prise en charge, **SQLdiag** copie automatiquement les fichiers de prise en charge indispensables dans le dossier spécifié s’ils ne s’y trouvent pas déjà.  
  
> [!NOTE]  
>  Pour définir votre dossier actif comme chemin de support, spécifiez **%cd%** sur la ligne de commande comme suit :  
>   
>  **SQLDIAG /P %cd%**  
  
 **/N** _output_folder_management_option_  
 Indique si **SQLdiag** remplace ou renomme le dossier de sortie à son démarrage. Options disponibles :  
  
 1 = Remplace le dossier de sortie (par défaut)  
  
 2 = Quand **SQLdiag** démarre, il renomme le dossier de sortie en SQLDIAG_00001, SQLDIAG_00002, etc. Une fois le dossier de sortie actif renommé, **SQLdiag** enregistre la nouvelle sortie dans le dossier de sortie par défaut, SQLDIAG.  
  
> [!NOTE]  
>  **SQLdiag** n’ajoute pas la sortie au dossier de sortie actif quand il démarre. Il peut uniquement remplacer le dossier de sortie par défaut (option 1) ou renommer le dossier (option 2), puis enregistrer la sortie dans le nouveau dossier de sortie par défaut nommé SQLDIAG.  
  
 **/M** _machine1_ [ *machine2* *machineN*] | *\@machinelistfile*  
 Remplace les ordinateurs spécifiés dans le fichier de configuration. Par défaut, le fichier de configuration est SQLDiag.Xml ou est défini avec le paramètre **/I** . Lorsque vous spécifiez plusieurs ordinateurs, séparez chaque nom d'ordinateur avec un espace.  
  
 Le paramètre *\@machinelistfile* spécifie un nom de fichier de liste d'ordinateurs à stocker dans le fichier de configuration.  
  
 **/C** _file_compression_type_  
 Définit le type de compression de fichiers utilisé sur les fichiers du dossier de sortie de **SQLdiag** . Options disponibles :  
  
 0 = aucune (par défaut)  
  
 1 = utilise la compression NTFS  
  
 **/B** [ **+** ]*heure_début*  
 Spécifie la date et l'heure de début de la collecte des données de diagnostics dans le format suivant :  
  
 AAAAMMJJ_HH:MM:SS  
  
 L'heure est spécifiée en utilisant une notation de 24 heures. Par exemple, 2:00 P.M. doit être spécifié comme **14:00:00**.  
  
 Utilisez **+** sans la date (HH:MM:SS seulement) pour spécifier une heure relative à la date et l’heure actuelle. Par exemple, si vous spécifiez **/B +02:00:00**, **SQLdiag** attend deux heures avant de démarrer la collecte d’informations.  
  
 N’insérez pas d’espace entre **+** et la valeur *date_début* spécifiée.  
  
 Si vous spécifiez une heure de début dans le passé, **SQLdiag** modifie cette date de façon à ce que la date et l’heure de début se situent dans le futur. Par exemple, si vous spécifiez **/B 01:00:00** et si l’heure actuelle est 08:00:00, **SQLdiag** remplace la date de début par la date du lendemain.  
  
 Notez que **SQLdiag** utilise l’heure locale sur l’ordinateur sur lequel l’utilitaire s’exécute.  
  
 **/E** [ **+** ]*heure_fin*  
 Spécifie la date et l'heure d'arrêt de la collecte des données de diagnostics dans le format suivant :  
  
 AAAAMMJJ_HH:MM:SS  
  
 L'heure est spécifiée en utilisant une notation de 24 heures. Par exemple, 2:00 P.M. doit être spécifié comme **14:00:00**.  
  
 Utilisez **+** sans la date (HH:MM:SS seulement) pour spécifier une heure relative à la date et l’heure actuelle. Par exemple si vous spécifiez une heure de début et une heure de fin avec **/B +02:00:00 /E +03:00:00**, **SQLdiag** attend deux heures avant de démarrer la collecte d’informations, puis collecte des informations pendant trois heures avant de s’arrêter et de quitter. Si **/B** n’est pas spécifié, **SQLdiag** débute la collecte de diagnostics immédiatement et l’arrête à la date et l’heure spécifiées par **/E**.  
  
 N’insérez pas d’espace entre **+** et la valeur *heure_début* ou *heure_fin* spécifié.  
  
 Notez que **SQLdiag** utilise l’heure locale sur l’ordinateur sur lequel l’utilitaire s’exécute.  
  
 **/A**  _SQLdiag_application_name_  
 Permet d’exécuter plusieurs instances de l’utilitaire **SQLdiag[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur la même instance** .  
  
 Chaque *nom_application_SQLdiag* identifie une instance différente de **SQLdiag**. Il n’existe aucune relation entre une instance *nom_application_SQLdiag* et un nom d’instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Vous pouvez utiliser *nom_application_SQLdiag* pour démarrer ou arrêter une instance spécifique du service **SQLdiag** .  
  
 Par exemple :  
  
 **SQLDIAG START /A**  _SQLdiag_application_name_  
  
 Vous pouvez aussi l’utiliser avec l’option **/R** pour enregistrer une instance spécifique de **SQLdiag** en tant que service. Par exemple :  
  
 **SQLDIAG /R /A** _SQLdiag_application_name_  
  
> [!NOTE]  
>  **SQLdiag** ajoute automatiquement le préfixe DIAG$ au nom de l’instance spécifié pour *nom_application_SQLdiag*. Vous obtenez ainsi un nom de service pratique si vous inscrivez **SQLdiag** comme service.  
  
 /T { tcp [ ,*port* ] | np | lpc }  
 Permet de se connecter à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant le protocole spécifié.  
  
 tcp [,*port*]  
 Transmission Control Protocol/Internet Protocol (TCP/IP). Vous pouvez éventuellement spécifier un numéro de port pour la connexion.  
  
 np  
 Canaux nommés. Par défaut, l'instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] écoute le canal nommé `\\.\pipe\sql\query` et `\\.\pipe\MSSQL$<instancename>\sql\query` pour une instance nommée. Vous ne pouvez pas vous connecter à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant un autre nom de canal.  
  
 lpc  
 Appel de procédure locale. Ce protocole de mémoire partagée est disponible si le client se connecte à une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur le même ordinateur.  
  
 **/Q**  
 Exécute **SQLdiag** en mode silencieux. **/Q** supprime toutes les invites, telles que les invites de mot de passe.  
  
 **/G**  
 Exécute **SQLdiag** en mode générique. Quand l’option **/G** est spécifiée, **SQLdiag** n’effectue pas les contrôles de connectivité de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] au démarrage. Il ne vérifie pas non plus l’adhésion de l’utilisateur au rôle de serveur fixe **sysadmin** . **SQLdiag** confie plutôt à Windows le soin de déterminer si un utilisateur dispose des droits appropriés pour collecter chacun des diagnostics demandés.  
  
 Si l’option **/G** n’est pas spécifiée, **SQLdiag** détermine si l’utilisateur adhère au groupe **Administrateurs** de Windows et il ne collecte aucun diagnostic [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s’il n’est pas membre du groupe **Administrateurs** .  
  
 **/R**  
 Inscrit **SQLdiag** en tant que service. Tous les arguments de ligne de commande qui sont spécifiés à l’inscription de **SQLdiag** en tant que service sont conservés pour de futures exécutions du service.  
  
 Quand **SQLdiag** est inscrit comme service, le nom du service par défaut est SQLDIAG. Vous pouvez modifiez ce nom au moyen de l’argument **/A** .  
  
 Utilisez l'argument de ligne de commande **START** pour démarrer le service.  
  
 **SQLDIAG START**  
  
 Vous pouvez également utiliser la commande **net start** pour démarrer le service.  
  
 **net  start SQLDIAG**  
  
 **/U**  
 Annule l’inscription de **SQLdiag** en tant que service.  
  
 Utilisez également l’argument **/A** si vous annulez l’inscription d’une instance **SQLdiag** nommée.  
  
 **/L**  
 Exécute **SQLdiag** en mode continu quand une heure de début ou de fin est également indiquée respectivement par le biais des arguments **/B** ou **/E** . **SQLdiag** redémarre automatiquement après l’arrêt de la collecte des diagnostics en raison d’un arrêt programmé. Par exemple, en utilisant les arguments **/E** ou **/X** .  
  
> [!NOTE]  
>  **SQLdiag** ignore l’argument **/L** si aucune heure de début ni de fin n’est spécifiée à l’aide des arguments de ligne de commande **/B** et **/E** .  
  
 L’utilisation de **/L** n’implique aucunement le mode service. Pour utiliser **/L** quand vous exécutez **SQLdiag** en tant que service, spécifiez cet argument sur la ligne de commande au moment de l’inscription du service.  
  
 **/X**  
 Exécute **SQLdiag** en mode d’instantané. **SQLdiag** effectue un instantané de tous les diagnostics configurés, puis s’arrête automatiquement.  
  
 **START** | **STOP** | **STOP_ABORT**  
 Démarre ou arrête le service **SQLdiag** . **STOP_ABORT** oblige le service à s’arrêter le plus rapidement possible, sans terminer la collecte de diagnostics en cours.  
  
 Lorsqu'ils sont utilisés ces arguments destinés au contrôle du service doivent être placés en premier sur les lignes de commande. Par exemple :  
  
 **SQLDIAG START**  
  
 Seul l’argument **/A** , qui spécifie une instance nommée de **SQLdiag**, peut être utilisé avec **START**, **STOP** ou **STOP_ABORT** pour prendre le contrôle d’une instance spécifique du service **SQLdiag** . Par exemple :  
  
 **SQLDIAG START /A** _SQLdiag_application_name_  
  
## <a name="security-requirements"></a>Spécifications de sécurité  
 Sauf si **SQLdiag** est exécuté en mode générique (avec l’argument de ligne de commande **/G** ), l’utilisateur qui exécute **SQLdiag** doit être membre du groupe **Administrateurs** de Windows et membre du rôle serveur fixe [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] **sysadmin**. Par défaut, **SQLdiag** établit la connexion à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant l’authentification Windows, mais il prend également en charge l’authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="performance-considerations"></a>Considérations relatives aux performances  
 Les effets sur les performances de l’exécution de **SQLdiag** varient selon le type de données de diagnostics dont vous avez configuré la collecte. Par exemple, si la configuration de **SQLdiag** prévoit la collecte d’informations de suivi de trace de [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , les performances du serveur se trouveront affectées proportionnellement au nombre de classes d’événements à tracer.  
  
 L’impact sur les performances d’exécution de **SQLdiag** équivaut approximativement à la somme des coûts de collecte des diagnostics configurés séparément. Par exemple, la collecte d’une trace avec **SQLdiag** implique le même coût de performances que sa collecte avec [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]. L’impact sur les performances d’utilisation de **SQLdiag** est négligeable.  
  
## <a name="required-disk-space"></a>Espace disque requis  
 Comme **SQLdiag** peut recueillir différents types d’informations de diagnostics, l’espace disque libre requis pour exécuter **SQLdiag** est variable. La quantité d'informations de diagnostics recueillies dépend de la nature et du volume de la charge de travail que le serveur traite et peut être comprise entre quelques mégaoctets et plusieurs gigaoctets.  
  
## <a name="configuration-files"></a>Fichiers de configuration  
 Au démarrage, **SQLdiag** lit le fichier de configuration et les arguments de ligne de commande qui ont été spécifiés. Vous spécifiez les types d’informations de diagnostics que **SQLdiag** collecte dans le fichier de configuration. Par défaut, **SQLdiag** utilise le fichier de configuration SQLDiag.Xml ; ce fichier, extrait chaque fois que l’outil s’exécute, se trouve dans le dossier de démarrage de l’utilitaire **SQLdiag** . Le fichier de configuration utilise le schéma XML, SQLDiag_schema.xsd, qui est également extrait à partir du fichier exécutable dans le répertoire de démarrage de l’utilitaire chaque fois que **SQLdiag** s’exécute.  
  
### <a name="editing-the-configuration-files"></a>Modification des fichiers de configuration  
 Vous pouvez copier et modifier SQLDiag.Xml pour changer les types de données de diagnostics collectées par **SQLdiag** . Lors de la modification du fichier de configuration, utilisez toujours un éditeur XML pouvant valider le fichier de configuration par rapport à son schéma XML, par exemple [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)]. Ne modifiez pas SQLDiag.Xml directement. Effectuez plutôt une copie de SQLDiag.Xml dans le même dossier et attribuez-lui un nouveau nom. Ensuite, modifiez le nouveau fichier et utilisez l’argument **/I** pour le passer à **SQLdiag**.  
  
#### <a name="editing-the-configuration-file-when-sqldiag-runs-as-a-service"></a>Modification du fichier de configuration lors de l'exécution de SQLdiag en tant que service  
 Si vous avez déjà exécuté **SQLdiag** en tant que service et que vous devez modifier le fichier de configuration, annulez l’inscription du service SQLDIAG en spécifiant l’argument de ligne de commande **/U** , puis réinscrivez le service avec l’argument de ligne de commande **/R** . L'annulation de l'inscription du service puis sa réinscription supprime les anciennes informations de configuration mises en mémoire cache dans le Registre Windows.  
  
## <a name="output-folder"></a>Dossier de sortie  
 Si vous ne spécifiez pas de dossier de sortie avec l’argument **/O** , **SQLdiag** crée un sous-dossier nommé SQLDIAG sous le dossier de démarrage de **SQLdiag** . Pour la collecte d'informations de diagnostics impliquant un suivi à grand volume, tel que [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] , assurez-vous que le dossier de sortie se trouve sur un lecteur local offrant suffisamment d'espace pour stocker la sortie de diagnostics demandée.  
  
 Quand il est redémarré, **SQLdiag** remplace le contenu du dossier de sortie. Pour éviter cela, spécifiez **/N 2** sur la ligne de commande.  
  
## <a name="data-collection-process"></a>Processus de collecte de données  
 Quand **SQLdiag** démarre, il effectue les contrôles d’initialisation nécessaires pour collecter les données de diagnostics qui ont été spécifiées dans SQLDiag.Xml. Ce processus peut prendre plusieurs secondes. Une fois que **SQLdiag** a démarré la collecte de données de diagnostics quand il est exécuté en tant qu’application de console, un message s’affiche pour vous signaler que la collecte de **SQLdiag** a commencé et que vous pouvez appuyer sur Ctrl+C pour l’arrêter. Quand **SQLdiag** est exécuté en tant que service, un message similaire est écrit dans le journal d’événements de Windows.  
  
 Si vous utilisez **SQLdiag** pour diagnostiquer un problème que vous pouvez reproduire, attendez la réception de ce message avant de reproduire le problème sur votre serveur.  
  
 **SQLdiag** collecte la plupart des données de diagnostics en parallèle. Toutes les informations de diagnostics sont collectées en se connectant à des outils, tels que l’utilitaire [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] **sqlcmd** ou le processeur de commandes Windows, sauf quand des informations sont recueillies à partir des journaux de performances et des journaux d’événements de Windows. **SQLdiag** utilise un thread de travail par ordinateur pour contrôler la collecte de données de diagnostics de ces autres outils, attendant souvent simultanément la fin de l’exécution de plusieurs outils. Pendant le processus de collecte, **SQLdiag** achemine les résultats de chaque diagnostic dans le dossier de sortie.  
  
## <a name="stopping-data-collection"></a>Arrêt de la collecte de données  
 Une fois que **SQLdiag** a démarré la collecte de données de diagnostics, il poursuit celle-ci sauf si vous l’arrêtez ou s’il est configuré pour s’arrêter à une heure spécifiée. Vous pouvez configurer **SQLdiag** pour s’arrêter à une heure spécifique avec l’argument **/E** , qui permet de spécifier une heure d’arrêt ou avec l’argument **/X** , qui force l’exécution de **SQLdiag** en mode d’instantané.  
  
 Quand **SQLdiag** s’arrête, il arrête tous les diagnostics qu’il a commencés. Par exemple, il arrête les traces du [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] dont il assurait la collecte, interrompt également les scripts [!INCLUDE[tsql](../includes/tsql-md.md)] en cours d'exécution ainsi que tous les sous-processus qu'il a générés pendant la collecte des données. Une fois la collecte des données de diagnostic terminée, **SQLdiag** prend fin.  
  
> [!NOTE]  
>  L’interruption du service **SQLdiag** n’est pas prise en charge. Si vous tentez de suspendre le service **SQLdiag** , il s’arrête seulement après avoir fini la collecte des diagnostics qui était en cours au moment où vous l’avez interrompu. Si vous redémarrez **SQLdiag** suite à une interruption, l’application remplace le dossier de sortie en se lançant. Pour éviter le remplacement de ce dossier, spécifiez **/N 2** sur la ligne de commande.  
  
 **Pour arrêter SQLdiag lorsqu'il s'exécute en tant qu'application console**  
  
 Si vous exécutez **SQLdiag** en tant qu’application console, appuyez sur Ctrl+C dans la fenêtre de console où **SQLdiag** s’exécute pour l’arrêter. Une fois que vous avez appuyé sur Ctrl+C, un message s’affiche dans la fenêtre de console pour vous signaler que la collecte de données **SQLDiag** se termine et que vous devez attendre l’arrêt du processus, ce qui peut prendre plusieurs minutes.  
  
 Appuyez deux fois sur les touches Ctrl+C pour mettre fin à tous les processus de diagnostics enfants et arrêter immédiatement l'application.  
  
 **Pour arrêter SQLdiag lorsqu'il s'exécute en tant que service**  
  
 Si vous exécutez **SQLdiag** en tant que service, exécutez **SQLDiag STOP** dans le dossier de démarrage **SQLdiag** pour l’arrêter.  
  
 Si vous exécutez plusieurs instances de **SQLdiag** sur le même ordinateur, vous pouvez également mentionner le nom de l’instance **SQLdiag** sur la ligne de commande quand vous suspendez le service. Utilisez, par exemple, la syntaxe suivante pour arrêter une instance **SQLdiag** nommée Instance1 :  
  
```  
SQLDIAG STOP /A Instance1  
```  
  
> [!NOTE]  
>  **/A** est le seul argument de ligne de commande pouvant être utilisé avec **START**, **STOP** ou **STOP_ABORT**. Si vous devez spécifier une instance nommée de **SQLdiag** avec l’un des verbes de contrôle du service, placez **/A** après le verbe de contrôle sur la ligne de commande, comme illustré dans l’exemple de syntaxe précédent. Lorsqu'ils sont utilisés, les verbes de contrôle doivent impérativement occuper la place du premier argument sur une ligne de commande.  
  
 Pour arrêter le service le plus rapidement possible, exécutez **SQLDIAG STOP_ABORT** dans le dossier de démarrage de l’utilitaire. Cette commande interrompt toute collecte de diagnostics en cours d'exécution sans attendre que cette opération se termine.  
  
> [!NOTE]  
>  Utilisez **SQLDiag STOP** ou **SQLDIAG STOP_ABORT** pour arrêter le service **SQLdiag** . N’utilisez pas la console Services Windows pour arrêter **SQLdiag** ou d’autres services [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="automatically-starting-and-stopping-sqldiag"></a>Démarrage et arrêt automatiques de SQLdiag  
 Pour démarrer et arrêter automatiquement la collecte de données de diagnostics à une heure spécifiée, utilisez les arguments **/B**_heure\_début_ et **/E**_heure\_fin_, en vous servant d’une notation horaire sur 24 heures. Par exemple, si vous dépannez un problème qui apparaît constamment vers 02:00:00, vous pouvez configurer **SQLdiag** pour qu’il démarre automatiquement la collecte de données de diagnostics à 01:00 et l’arrête à 03:00:00. Utilisez les arguments **/B** et **/E** pour spécifier les heures de début et de fin. Utilisez la notation 24 heures pour spécifier des dates et heures précises de début et de fin au format AAAAMMJJ_HH:MM:SS. Pour spécifier des heures de début et de fin relatives, utilisez le préfixe **+** devant ces heures et omettez la partie de la date (AAAAMMJJ_) comme dans l’exemple suivant, qui demande à **SQLdiag** d’attendre une heure avant de commencer la collecte d’informations, d’exécuter celle-ci pendant trois heures, puis de s’arrêter et de quitter :  
  
```  
sqldiag /B +01:00:00 /E +03:00:00  
```  
  
 Quand vous spécifiez une valeur *heure_début* relative, **SQLdiag** commence à une heure qui est cohérente par rapport à la date et l’heure actuelles. Quand vous spécifiez une valeur *heure_fin* relative, **SQLdiag** se termine à une heure qui est chronologiquement cohérente par rapport à la valeur *heure_début* spécifiée. Si les dates ou heures de démarrage et d’arrêt que vous spécifiez se situent dans le passé, **SQLdiag** change la date de démarrage afin que la date et l’heure de démarrage se situent dans le futur.  
  
 Ceci présente des implications importantes sur les dates de démarrage et d'arrêt que vous choisissez. Prenons l’exemple suivant :  
  
```  
sqldiag /B +01:00:00 /E 08:30:00  
```  
  
 Si l'heure actuelle est 08:00, l'heure de fin arrive avant que ne commence la collecte du diagnostic. Comme **SQLDiag** règle automatiquement les dates de début et de fin au jour suivant quand elles se produisent dans le passé, dans cet exemple la collecte des diagnostics commence à 09:00 aujourd’hui (une heure de début relative a été spécifiée avec **+** ) et se poursuit jusqu’à 08:30 le lendemain matin.  
  
### <a name="stopping-and-restarting-sqldiag-to-collect-daily-diagnostics"></a>Arrêt et redémarrage de SQLdiag pour collecter des diagnostics quotidiennement  
 Pour collecter un ensemble de diagnostics sur une base quotidienne sans devoir démarrer et arrêter manuellement **SQLdiag**, utilisez l’argument **/L** . L’argument **/L** force **SQLdiag** à s’exécuter de façon continue en redémarrant automatiquement après un arrêt programmé. Quand vous spécifiez l’option **/L** et que **SQLdiag** s’arrête car il a atteint l’heure de fin définie avec l’argument **/E** , ou qu’il s’arrête car il s’exécute en mode d’instantané avec l’argument **/X** , **SQLdiag** redémarre au lieu de se fermer.  
  
 L’exemple suivant spécifie que **SQLdiag** s’exécute en mode continu pour redémarrer automatiquement après une collecte de données de diagnostics effectuée entre 03:00:00 et 05:00:00.  
  
```  
sqldiag /B 03:00:00 /E 05:00:00 /L  
```  
  
 L’exemple suivant spécifie que **SQLdiag** s’exécute en mode continu pour redémarrer automatiquement après un instantané de données de diagnostics à 03:00:00.  
  
```  
sqldiag /B 03:00:00 /X /L  
```  
  
## <a name="running-sqldiag-as-a-service"></a>Exécution de SQLdiag en tant que service  
 Quand vous souhaitez utiliser **SQLdiag** pour collecter des données de diagnostics pendant de longues périodes durant lesquelles vous pourriez devoir vous déconnecter de l’ordinateur sur lequel s’exécute **SQLdiag** , vous pouvez l’exécuter en tant que service.  
  
 **Pour inscrire SQLDiag afin qu'il s'exécute en tant que service**  
  
 Vous pouvez inscrire **SQLdiag** pour qu’il s’exécute en tant que service en spécifiant l’argument **/R** sur la ligne de commande. Cet argument inscrit **SQLdiag** afin qu’il s’exécute en tant que service. Le nom du service **SQLdiag** est SQLDIAG. Tous les autres arguments que vous spécifiez sur la ligne de commande quand vous inscrivez **SQLDiag** en tant que service sont préservés et réutilisés au démarrage du service.  
  
 Si vous voulez modifier le nom de service SQLDIAG par défaut, utilisez l’argument de ligne de commande **/A** pour spécifier un autre nom. **SQLdiag** ajoute automatiquement le préfixe DIAG$ à tout nom d’instance **SQLdiag** spécifié à l’aide l’argument **/A** pour créer des noms de services explicites et pratiques d’utilisation.  
  
 **Pour annuler l'instruction du service SQLDIAG**  
  
 Pour annuler l’inscription du service, spécifiez l’argument **/U** . L’annulation de l’inscription de **SQLdiag** en tant que service supprime également les clés de Registre Windows associées au service SQLDIAG.  
  
 **Pour démarrer ou redémarrer le service SQLDIAG**  
  
 Pour démarrer ou redémarrer le service SQLDIAG, exécutez **SQLDiag START** à partir de la ligne de commande.  
  
 Si vous exécutez plusieurs instances de **SQLdiag** par le biais de l’argument **/A** , vous pouvez également mentionner le nom de l’instance **SQLdiag** sur la ligne de commande quand vous démarrez le service. Utilisez, par exemple, la syntaxe suivante pour démarrer une instance **SQLdiag** nommée Instance1 :  
  
```  
SQLDIAG START /A Instance1  
```  
  
 Vous pouvez également utiliser la commande **net start** pour démarrer le service SQLDIAG.  
  
 Quand il est redémarré, **SQLdiag** remplace le contenu du dossier de sortie actif. Pour éviter cela, spécifiez **/N 2** sur la ligne de commande pour renommer le dossier de sortie au redémarrage de l’utilitaire.  
  
 L’interruption du service **SQLdiag** n’est pas prise en charge.  
  
## <a name="running-multiple-instances-of-sqldiag"></a>Exécution de plusieurs instances de SQLdiag  
 Exécutez plusieurs instances de **SQLdiag** sur le même ordinateur en spécifiant **/A**_SQLdiag\_nom\_application_ sur la ligne de commande. Ceci se révèle utile pour collecter différents jeux de diagnostics simultanément à partir de la même instance [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Vous pouvez, par exemple, configurer une instance nommée de **SQLdiag** pour qu’elle assure en continu la collecte de données peu volumineuses. De cette façon, si un problème particulier survient sur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous avez la possibilité d’exécuter l’instance **SQLdiag** par défaut pour collecter des diagnostics sur le problème ou réunir un jeu de diagnostics que les services d’assistance de [!INCLUDE[msCoName](../includes/msconame-md.md)] demanderont en vue d’émettre un diagnostic.  
  
## <a name="collecting-diagnostic-data-from-clustered-sql-server-instances"></a>Collecte de données de diagnostics à partir d'instances cluster de SQL Server  
 **SQLdiag** prend en charge la collecte des données de diagnostics à partir d’instances cluster [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour recueillir des diagnostics à partir d’instances de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en cluster, vérifiez que **« . »** est spécifié pour l’attribut **name** de l’élément **\<Machine>** dans le fichier de configuration de SQLDiag.Xml et ne spécifiez pas l’argument **/G** sur la ligne de commande. Par défaut, **« . »** est spécifié pour l’attribut **name** dans le fichier de configuration et l’argument **/G** est désactivé. Il n'est généralement pas nécessaire de modifier le fichier de configuration ni les arguments de ligne de commande lors d'une collecte effectuée à partir d'une instance cluster [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 Quand **« . »** est spécifié comme nom d’ordinateur, **SQLdiag** détecte qu’il fonctionne sur un cluster et récupère simultanément les informations des diagnostics à partir de toutes les instances virtuelles de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] installées sur ce cluster. Pour collecter des informations de diagnostics à partir d’une seule instance virtuelle de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] s’exécutant sur un ordinateur, spécifiez cette instance virtuelle de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour l’attribut **name** de l’élément **\<Machine>** dans SQLDiag.Xml.  
  
> [!NOTE]  
>  Pour collecter des informations de traces de [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] à partir d'instances cluster [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , les partages administratifs (ADMIN$) doivent être activés sur le cluster.  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’utilitaire d’invite de commandes &#40;moteur de base de données&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
