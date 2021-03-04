---
title: Sécurité Distributed Replay
description: Cet article décrit les étapes de configuration de la sécurité pour SQL Server Distributed Replay et des considérations importantes concernant les étapes de protection et de suppression des données.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 907fbe327e712c4e9a6aa989a33822baa6122a07
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836876"
---
# <a name="distributed-replay-security"></a>Sécurité Distributed Replay

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Avant d’installer et d’utiliser la fonctionnalité [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay, vous devez passer en revue les informations de sécurité importantes de cette rubrique. Cette rubrique décrit les étapes de configuration de la sécurité consécutives à l'installation requises pour pouvoir utiliser Distributed Replay. Elle inclut également des considérations importantes en ce qui concerne la protection des données et les étapes de suppression importantes.  
  
## <a name="user-and-service-accounts"></a>Comptes d'utilisateur et de service  
 Le tableau suivant décrit les comptes utilisés pour Distributed Replay. Après l'installation de Distributed Replay, vous devez affecter les principaux de sécurité qui seront exécutés sous les comptes de service du contrôleur et du client. Par conséquent, il est recommandé de configurer les comptes d'utilisateur de domaine correspondants avant d'installer les fonctionnalités de Distributed Replay.  
  
|Compte d'utilisateur|Spécifications|  
|------------------|------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay|Peut être un compte d'utilisateur de domaine ou un compte d'utilisateur local. Si vous utilisez un compte d'utilisateur local, l'outil d'administration, le contrôleur et le client doivent tous s'exécuter sur le même ordinateur.<br /><br /> **\*\* Remarque relative à la sécurité \*\*** Il est préférable que le compte ne soit pas membre du groupe Administrateurs local dans Windows.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay|Peut être un compte d'utilisateur de domaine ou un compte d'utilisateur local. Si vous utilisez un compte d'utilisateur local, le contrôleur, le clien, et la cible SQL Server doivent tous exécuter sur le même ordinateur.<br /><br /> **\*\* Remarque relative à la sécurité \*\*** Il est préférable que le compte ne soit pas membre du groupe Administrateurs local dans Windows.|  
|Compte d'utilisateur interactif qui est utilisé pour exécuter l'outil d'administration de Distributed Replay|Peut être un utilisateur local ou un compte d'utilisateur de domaine. Pour utiliser un compte d'utilisateur local, l'outil d'administration et le contrôleur doivent s'exécuter sur le même ordinateur.|  
  
 **Important !** lorsque vous configurez le contrôleur Distributed Replay, vous pouvez spécifier un ou plusieurs comptes d'utilisateurs qui seront utilisés pour exécuter les services client Distributed Replay. Vous trouverez ci-dessous la liste des comptes pris en charge :  
  
-   Compte d’utilisateur de domaine  
  
-   Compte d'utilisateur local créé par l'utilisateur  
  
-   Administrateur  
  
-   Compte virtuel et Compte de service administré (MSA)  
  
-   Services réseau, Services locaux et Système  
  
 Les comptes de groupe (locaux ou de domaine) et autres comptes intégrés (comme Tout le monde) ne sont pas acceptés.  
  
 Pour définir les comptes de service ou les mots de passe après avoir installé Distributed Replay, vous pouvez utiliser l'outil Services Windows. Pour modifier les comptes de service associés aux services Distributed Replay du contrôleur ou du client, procédez comme suit :  
  
1.  Effectuez l'une des procédures suivantes, en fonction de votre système d'exploitation :  
  
    -   Cliquez sur **Démarrer**, tapez **services.msc** dans la zone **Rechercher** , puis appuyez sur Entrée.  
  
    -   Cliquez sur **Démarrer**, puis sur **Exécuter**, tapez **services.msc** et appuyez sur Entrée.  
  
2.  Dans la boîte de dialogue **Services** , cliquez avec le bouton droit sur le service à configurer, puis cliquez sur **Propriétés**.  
  
3.  Sur l'onglet **Ouvrir une session** , cliquez sur **Ce compte**.  
  
4.  Configurer le compte d'utilisateur que vous souhaitez utiliser.  
  
## <a name="file-and-folder-permissions"></a>Autorisations de fichier et de dossier  
 Une fois que les comptes de service ont été spécifiés, vous devez leur accorder les autorisations de fichier et de dossier nécessaires. Configurez les autorisations de fichiers et de dossiers conformément au tableau suivant :  
  
|Compte|Autorisations d'accès au dossier|  
|-------------|------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay|`<Controller_Installation_Path>\DReplayController` (lecture, écriture, suppression)<br /><br /> `DReplayServer.xml` fichier (lecture, écriture)|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay|`<Client_Installation_Path>\DReplayClient` (lecture, écriture, suppression)<br /><br /> `DReplayClient.xml` fichier (lecture, écriture)<br /><br /> Répertoires d'exécution et de résultat, comme spécifié dans le fichier de configuration du client par les éléments `WorkingDirectory` et `ResultDirectory` , respectivement. (Lecture, écriture)|  
  
## <a name="dcom-permissions"></a>Autorisations DCOM  
 DCOM est utilisé pour la communication d'appel de procédure distante (RPC) entre le contrôleur et l'outil d'administration, et entre le contrôleur et tous les clients. Vous devez configurer les autorisations DCOM au niveau de l'ordinateur et spécifiques à l'application sur le contrôleur une fois que les fonctionnalités Distributed Replay ont été installées.  
  
 Procédez comme suit pour configurer les autorisations DCOM du contrôleur :  
  
1.  **Ouvrez dcomcnfg.exe, le composant logiciel enfichable des services de composants** : C’est l’outil utilisé pour configurer les autorisations DCOM.  
  
    1.  Sur l'ordinateur du contrôleur, cliquez sur **Démarrer**.  
  
    2.  Tapez **dcomcnfg.exe** dans la zone **Rechercher** .  
  
    3.  Appuyez sur Entrée.  
  
2.  **Configurez les autorisations DCOM au niveau de l’ordinateur** : accordez les autorisations DCOM au niveau de l’ordinateur pour chaque compte répertorié dans le tableau suivant. Pour plus d’informations sur la définition des autorisations au niveau de l’ordinateur, consultez [Liste de vérification : gérer des applications DCOM](/windows/win32/com/setting-machine-wide-security-using-dcomcnfg).  
  
3.  **Configurer les autorisations DCOM spécifiques à l’application** : accordez les autorisations DCOM au spécifiques à l’application pour chaque compte répertorié dans le tableau suivant. Le nom d'application DCOM pour le service du contrôleur est **DReplayController**. Pour plus d’informations sur la définition des autorisations spécifiques à l’application, consultez [Liste de vérification : gérer des applications DCOM](/windows/win32/com/setting-processwide-security-using-dcomcnfg).  
  
 Le tableau suivant décrit les autorisations DCOM requises pour le compte d'utilisateur interactif de l'outil d'administration et les comptes de service du client :  
  
|Fonctionnalité|Compte|Autorisations DCOM requises sur le contrôleur|  
|-------------|-------------|---------------------------------------------|  
|Outil d'administration Distributed Replay|Compte d'utilisateur interactif|Accès local<br /><br /> Accès à distance<br /><br /> Lancement local<br /><br /> Lancement distant<br /><br /> Activation locale<br /><br /> Activation distante|  
|Distributed Replay Client|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay|Accès local<br /><br /> Accès à distance<br /><br /> Lancement local<br /><br /> Lancement distant<br /><br /> Activation locale<br /><br /> Activation distante|  
  
> [!IMPORTANT]  
>  Pour protéger le système contre les requêtes malveillantes ou les attaques par déni de service, veillez à d'utiliser uniquement un compte d'utilisateur approuvé pour le compte de service du client. Ce compte pourra se connecter et relire les charges de travail sur l'instance cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="sql-server-permissions"></a>Autorisations SQL Server  
 Les comptes de service du client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay sont utilisés pour la connexion à l'instance cible de la charge de travail de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Seul le mode d'authentification Windows est pris en charge pour ces connexions.  
  
 Après avoir installé le service client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay sur un ensemble d’ordinateurs, le principal de sécurité utilisé pour ces comptes de service doit recevoir le rôle de serveur sysadmin sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle vous avez l’intention de relire la charge de travail de la trace. Cette étape n'est pas effectuée automatiquement pendant l'installation de Distributed Replay.  
  
## <a name="data-protection"></a>Protection des données  
 Dans l'environnement Distributed Replay, les comptes d'utilisateur suivants ont entièrement accès à l'instance de serveur cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aux données de trace d'entrée et aux fichiers de trace de résultats :  
  
-   Compte d'utilisateur interactif qui est utilisé pour exécuter l'outil d'administration.  
  
-   Compte de service du contrôleur.  
  
-   Compte de service du client.  
  
-   Membres du groupe Administrateurs local sur le contrôleur.  
  
-   Membres du groupe Administrateurs local sur les clients.  
  
    > [!IMPORTANT]  
    >  Ces comptes ont entièrement accès à toutes les informations d'identification personnelle (PII) ou informations sensibles contenues dans la trace, l'intermédiaire, la distribution ou les fichiers de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisés par Distributed Replay.  
  
 Nous vous recommandons d'appliquer les précautions de sécurité suivantes :  
  
-   Stockez les données de la trace d'entrée, les résultats de la trace de sortie et les fichiers de base de données dans un emplacement qui utilise le système de fichiers NTFS et appliquez les listes de contrôle d'accès (ACL) appropriées. Si nécessaire, chiffrez les données stockées sur l'ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sachez que les listes de contrôle d'accès (ACL) ne sont pas appliquées aux fichiers de trace, et qu'il n'y a pas de masque de données ni de codage. Vous devez supprimer ces fichiers rapidement après utilisation.  
  
-   Appliquez les listes de contrôle d'accès et la stratégie de rétention appropriées à tous les fichiers intermédiaires et de distribution générés par Distributed Replay.  
  
-   Utilisez le protocole TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer), pour contribuer à sécuriser le transport réseau.  
  
## <a name="important-removal-steps"></a>Étapes de suppression importantes  
 Nous vous recommandons d'utiliser Distributed Replay uniquement dans un environnement de test. Après avoir terminé le test, et avant de mettre en service ces ordinateurs pour une tâche différente, assurez-vous de procéder comme suit :  
  
-   Désinstallez les fonctionnalités Distributed Replay et supprimez les fichiers de configuration associés du contrôleur et de tous les clients.  
  
-   Supprimez tous les fichiers de trace, intermédiaires, de distribution et de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisés pour le test. Les fichiers intermédiaires et de distribution sont stockés dans le répertoire de travail sur le contrôleur et le client, respectivement.  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Install Distributed Replay - Présentation](../../tools/distributed-replay/install-distributed-replay-overview.md)  
  
