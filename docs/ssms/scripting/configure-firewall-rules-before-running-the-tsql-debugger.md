---
title: Configurer des règles de pare-feu avant d’exécuter le débogueur T-SQL
description: Découvrez comment configurer les règles du pare-feu Windows avant d’exécuter le débogueur Transact-SQL (T-SQL) quand vous vous connectez à SQL Server dans SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.error.sqlde_accessdenied
- vs.debug.error.sqlde_register_failed
- vs.debug.error.sqlde_firewall.remotemachines
helpviewer_keywords:
- Transact-SQL debugger, remote connections
- Windows Firewall [Database Engine], Transact-SQL debugger
- Transact-SQL debugger, Windows Firewall
- Transact-SQL debugger, configuring
- ports [SQL Server], Transact-SQL debugger
- TCP/IP [SQL Server], port numbers
ms.assetid: f50e0b0d-eaf0-4f4a-be83-96f5be63e7ea
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 10/20/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a55de0f9f6a6b8ae80161adaa0252b3d5c4a774
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354692"
---
# <a name="configure-firewall-rules-before-running-the-tsql-debugger"></a>Configurer des règles de pare-feu avant d’exécuter le débogueur TSQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Vous devez configurer des règles de Pare-feu Windows pour permettre le débogage [!INCLUDE[tsql](../../includes/tsql-md.md)] en cas de connexion à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui s'exécute sur un autre ordinateur que l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] .

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="configuring-the-transact-sql-debugger"></a>Configuration du débogueur Transact-SQL

Le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] inclut des composants côté serveur et côté client. Les composants du débogueur côté serveur sont installés avec chaque instance du moteur de base de données à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) ou version ultérieure. Les composants côté client du débogueur sont inclus :

- Lorsque vous utilisez les outils côté client de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou version ultérieure.

- Lorsque vous installez Microsoft Visual Studio 2010 ou version ultérieure.

- Lorsque vous installez [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] à partir d'un téléchargement Web.

Aucune configuration particulière n'est requise pour exécuter le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] lorsque [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] s'exécute sur le même ordinateur que l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Toutefois, pour exécuter le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] lorsqu'il est connecté à une instance distante du [!INCLUDE[ssDE](../../includes/ssde-md.md)], vous devez activer des règles de programme et de port dans le Pare-feu Windows sur les deux ordinateurs. Ces règles peuvent être créées par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous obtenez des erreurs lors de la tentative d'ouverture d'une session de débogage distant, assurez-vous que les règles suivantes de pare-feu sont définis sur votre ordinateur.

Utilisez l'application **Pare-feu Windows avec fonctions avancées de sécurité** pour gérer les règles de pare-feu. Dans [!INCLUDE[win7](../../includes/win7-md.md)] et [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)], ouvrez **Panneau de configuration**, **Pare-feu Windows**, puis sélectionnez **Paramètres avancés**. Dans [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)] vous pouvez également ouvrir **Gestionnaire de services**, développer **Configuration** dans le volet gauche, puis développer **Pare-feu Windows avec fonctions avancées de sécurité**.

> [!CAUTION]
>  L'activation des règles dans le Pare-feu Windows peut exposer votre ordinateur à des atteintes à la sécurité que le pare-feu est conçu pour bloquer. L'activation des règles pour le débogage distant débloque les ports et les programmes répertoriés dans cette rubrique.

## <a name="firewall-rules-on-the-server"></a>Règles de pare-feu sur le serveur
Sur l'ordinateur qui exécute l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], utilisez le **Pare-feu Windows avec fonctions avancées de sécurité** pour spécifier les informations suivantes :

- Ajoutez une règle de programme entrante pour sqlservr.exe. Vous devez avoir une règle pour chaque instance qui doit prendre en charge les sessions de débogage distant.

   1. Dans **Pare-feu Windows avec fonctions avancées de sécurité**, dans le volet gauche, cliquez avec le bouton droit sur **Règles de trafic entrant**, puis sélectionnez **Nouvelle règle** dans le volet Action.

   2. Dans la boîte de dialogue **Type de règle** , sélectionnez **Programme**, puis cliquez sur **Suivant**.

   3. Dans la boîte de dialogue **Programme** , sélectionnez **Ce chemin d'accès du programme :** , puis entrez le chemin d'accès complet à sqlservr.exe pour cette instance. Par défaut, sqlservr.exe est installé dans C:\Program Files\Microsoft SQL Server\MSSQL13.*NomInstance*\MSSQL\Binn, où *NomInstance* représente MSSQLSERVER pour l’instance par défaut et le nom de l’instance pour toute instance nommée.

   4. Dans la boîte de dialogue **Action** , sélectionnez **Autoriser la connexion**, puis cliquez sur **Suivant**.

   5. Dans la boîte de dialogue **Profil** , sélectionnez des profils qui décrivent l'environnement de connexion de l'ordinateur lorsque vous souhaitez ouvrir une session de débogage distant avec l'instance, puis cliquez sur **Suivant**.

   6. Dans la boîte de dialogue **Nom** , tapez un nom et une description pour cette règle, puis cliquez sur **Terminer**.

   7. Dans la liste **Règles de trafic entrant** , cliquez avec le bouton droit sur la règle que vous avez créée, puis sélectionnez **Propriétés** dans le volet Action.

   8. Sélectionnez l'onglet **Protocoles et ports** .

   9. Sélectionnez **TCP** dans la zone **Type de protocole :** , sélectionnez **Ports dynamiques RPC** dans la zone **Port local :** , cliquez sur **Appliquer**, puis sur **OK**.

- Ajoutez une règle de programme entrante pour svchost.exe pour activer les communications DCOM des sessions de débogage distant.

   1. Dans **Pare-feu Windows avec fonctions avancées de sécurité**, dans le volet gauche, cliquez avec le bouton droit sur **Règles de trafic entrant**, puis sélectionnez **Nouvelle règle** dans le volet Action.

   2. Dans la boîte de dialogue **Type de règle** , sélectionnez **Programme**, puis cliquez sur **Suivant**.

   3. Dans la boîte de dialogue **Programme** , sélectionnez **Ce chemin d'accès du programme :** , puis entrez le chemin d'accès complet à scvhost.exe. Par défaut, svchost.exe est installé dans %systemroot%\System32\svchost.exe.

   4. Dans la boîte de dialogue **Action** , sélectionnez **Autoriser la connexion**, puis cliquez sur **Suivant**.

   5. Dans la boîte de dialogue **Profil** , sélectionnez des profils qui décrivent l'environnement de connexion de l'ordinateur lorsque vous souhaitez ouvrir une session de débogage distant avec l'instance, puis cliquez sur **Suivant**.

   6. Dans la boîte de dialogue **Nom** , tapez un nom et une description pour cette règle, puis cliquez sur **Terminer**.

   7. Dans la liste **Règles de trafic entrant** , cliquez avec le bouton droit sur la règle que vous avez créée, puis sélectionnez **Propriétés** dans le volet Action.

   8. Sélectionnez l'onglet **Protocoles et ports** .

   9. Sélectionnez **TCP** dans la zone **Type de protocole :** , sélectionnez **Mappeur de point de terminaison RPC** dans la zone **Port local :** , cliquez sur **Appliquer**, puis sur **OK**.

- Si la stratégie de domaine exige que les communications réseau s'effectuent par le biais du protocole IPsec, vous devez également ajouter des règles entrantes ouvrant les ports UDP 4500 et UDP 500.

## <a name="firewall-rules-on-the-client"></a>Règles de pare-feu sur le client

Sur l'ordinateur qui exécute l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] , le programme d'installation de SQL Server ou le programme d'installation de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] a peut-être configuré le Pare-feu Windows de façon à autoriser le débogage distant.

Si vous obtenez des erreurs lors de la tentative d'ouverture d'une session de débogage distant, vous pouvez configurer manuellement les exceptions de port et programme à l'aide du **Pare-feu Windows avec fonctions avancées de sécurité** pour configurer des règles de pare-feu :

- Ajouter une entrée de programme pour svchost :

   1. Dans **Pare-feu Windows avec fonctions avancées de sécurité**, dans le volet gauche, cliquez avec le bouton droit sur **Règles de trafic entrant**, puis sélectionnez **Nouvelle règle** dans le volet Action.

   2. Dans la boîte de dialogue **Type de règle** , sélectionnez **Programme**, puis cliquez sur **Suivant**.

   3. Dans la boîte de dialogue **Programme** , sélectionnez **Ce chemin d'accès du programme :** , puis entrez le chemin d'accès complet à scvhost.exe. Par défaut, svchost.exe est installé dans %systemroot%\System32\svchost.exe.

   4. Dans la boîte de dialogue **Action** , sélectionnez **Autoriser la connexion**, puis cliquez sur **Suivant**.

   5. Dans la boîte de dialogue **Profil** , sélectionnez des profils qui décrivent l'environnement de connexion de l'ordinateur lorsque vous souhaitez ouvrir une session de débogage distant avec l'instance, puis cliquez sur **Suivant**.

   6. Dans la boîte de dialogue **Nom** , tapez un nom et une description pour cette règle, puis cliquez sur **Terminer**.

   7. Dans la liste **Règles de trafic entrant** , cliquez avec le bouton droit sur la règle que vous avez créée, puis sélectionnez **Propriétés** dans le volet Action.

   8. Sélectionnez l'onglet **Protocoles et ports** .

   9. Sélectionnez **TCP** dans la zone **Type de protocole :** , sélectionnez **Mappeur de point de terminaison RPC** dans la zone **Port local :** , cliquez sur **Appliquer**, puis sur **OK**.

- Ajoutez une entrée de programme pour l'application qui héberge l'Éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Si vous devez ouvrir des sessions de débogage distant depuis [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] sur le même ordinateur, vous devez ajouter une règle de programme pour les deux :

   1. Dans **Pare-feu Windows avec fonctions avancées de sécurité**, dans le volet gauche, cliquez avec le bouton droit sur **Règles de trafic entrant**, puis sélectionnez **Nouvelle règle** dans le volet Action.

   2. Dans la boîte de dialogue **Type de règle** , sélectionnez **Programme**, puis cliquez sur **Suivant**.

   3. Dans la boîte de dialogue **Programme** , sélectionnez **Ce chemin d'accès du programme :** , puis entrez une de ces trois valeurs.

       - Pour [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], entrez le chemin d'accès complet à ssms.exe. Par défaut, ssms.exe est installé dans C:\Program Files (x86)\Microsoft SQL Server\130\Tools\Binn\Management Studio.

       - Pour [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] , entrez le chemin d'accès complet à devenv.exe :

            1. L'emplacement par défaut du fichier devenv.exe pour Visual Studio 2010 est C:\Program Files (x86)\Microsoft Visual Studio 10.0\Common7\IDE.

            2. L'emplacement par défaut du fichier devenv.exe pour Visual Studio 2012 est C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE.

            3. Vous pouvez trouver le chemin d'accès à ssms.exe dans le raccourci utilisé pour lancer [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez trouver le chemin d'accès à devenv.exe dans le raccourci utilisé pour lancer [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Cliquez avec le bouton droit sur le raccourci et choisissez **Propriétés**. L'exécutable et le chemin d'accès sont répertoriés dans la zone **Cible** .

   4. Dans la boîte de dialogue **Action** , sélectionnez **Autoriser la connexion**, puis cliquez sur **Suivant**.

   5. Dans la boîte de dialogue **Profil** , sélectionnez des profils qui décrivent l'environnement de connexion de l'ordinateur lorsque vous souhaitez ouvrir une session de débogage distant avec l'instance, puis cliquez sur **Suivant**.

   6. Dans la boîte de dialogue **Nom** , tapez un nom et une description pour cette règle, puis cliquez sur **Terminer**.

   7. Dans la liste **Règles de trafic entrant** , cliquez avec le bouton droit sur la règle que vous avez créée, puis sélectionnez **Propriétés** dans le volet Action.

   8. Sélectionnez l'onglet **Protocoles et ports** .

   9. Sélectionnez **TCP** dans la zone **Type de protocole :** , sélectionnez **Ports dynamiques RPC** dans la zone **Port local :** , cliquez sur **Appliquer**, puis sur **OK**.

## <a name="requirements-for-starting-the-debugger"></a>Configuration requise pour le démarrage du débogueur

Toute tentative de démarrer le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] doit également respecter les conditions suivantes :

- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] doit s'exécuter sous un compte Windows qui est membre du rôle serveur fixe sysadmin.

- La fenêtre de l’éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit être connectée à l’aide d’une connexion via l’authentification Windows ou l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est membre du rôle serveur fixe sysadmin.

- La fenêtre de l'éditeur de requête du [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit être connectée à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) ou version ultérieure. Vous ne pouvez pas exécuter le débogueur lorsque la fenêtre de l'éditeur de requête est connectée à une instance en mode mono-utilisateur.

- Le serveur doit communiquer avec le client par le biais de RPC. Le compte sous lequel le service SQL Server est exécuté doit avoir des autorisations d’accès authentifiées au client

## <a name="see-also"></a>Voir aussi

- [Débogueur Transact-SQL](./transact-sql-debugger.md)
- [Exécuter le débogueur Transact-SQL](./run-the-transact-sql-debugger.md)
- [Exécuter pas à pas du code Transact-SQL](./step-through-transact-sql-code.md)
- [Informations du débogueur Transact-SQL](./transact-sql-debugger-information.md)
- [Éditeur de requête du moteur de base de données &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)