---
description: MSSQLSERVER_18483
title: MSSQLSERVER_18483
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18483 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 82b12abb1121ed8f9523a267b5dbe98464c736f5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345540"
---
# <a name="mssqlserver_18483"></a>MSSQLSERVER_18483
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|18483|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|REMLOGIN_INVALID_USER|
|Texte du message|Impossible de se connecter au serveur '%.ls', car '%. ls' n'est pas défini comme ouverture de session distante sur le serveur. Vérifiez que vous avez spécifié le bon nom d'ouverture de session. %.*ls.|
||

## <a name="explanation"></a>Explication

Cette erreur se produit lorsque vous tentez de configurer un serveur de distribution de réplication sur un système qui a été restauré à l’aide de l’image de disque dur d’un autre ordinateur sur lequel l’instance SQL a été installée à l’origine. Un message d’erreur semblable au suivant est signalé à l’utilisateur :

> SQL Server Management Studio n’a pas pu configurer « \<Server>\<Instance> » en tant que serveur de distribution pour « \<Server>\<Instance> ». Erreur 18483 : Impossible de se connecter au serveur « \<Server>\<Instance> », car « distributor_admin » n’est pas défini en tant que connexion distante sur le serveur. Vérifiez que vous avez spécifié le bon nom d'ouverture de session. %.*ls.

## <a name="cause"></a>Cause

Lorsque vous déployez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d’une image de disque dur d’un autre ordinateur sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé, le nom réseau de l’ordinateur de l’image est conservé dans la nouvelle installation. Le nom de réseau incorrect entraîne l’échec de la configuration du serveur de distribution de réplication. Le même problème se produit si vous renommez l’ordinateur après l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="user-action"></a>Action requise

Pour contourner ce problème, remplacez le nom de serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le nom de réseau correct de l’ordinateur. Pour ce faire, procédez comme suit :

1. Connectez-vous à l’ordinateur sur lequel vous avez déployé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de l’image de disque, puis exécutez l’instruction Transact-SQL suivante dans SSMS :

    ```sql
    -- Use the Master database
    USE master
    GO

    -- Declare local variables
    DECLARE @serverproperty_servername varchar(100),
    @servername varchar(100);

    -- Get the value returned by the SERVERPROPERTY system function
    SELECT @serverproperty_servername = CONVERT(varchar(100), SERVERPROPERTY('ServerName'));

    -- Get the value returned by @@SERVERNAME global variable
    SELECT @servername = CONVERT(varchar(100), @@SERVERNAME);

    -- Drop the server with incorrect name
    EXEC sp_dropserver @server=@servername;

    -- Add the correct server as a local server
    EXEC sp_addserver @server=@serverproperty_servername, @local='local';
    ```

2. Redémarrez l’ordinateur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Pour vérifier que le nom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le nom réseau de l’ordinateur sont identiques, exécutez l’instruction Transact-SQL suivante :

    ```sql
    SELECT @@SERVERNAME, SERVERPROPERTY('ServerName');
    ```

## <a name="more-information"></a>Informations complémentaires

Vous pouvez utiliser la variable globale `@@SERVERNAME` ou la fonction `SERVERPROPERTY`('ServerName') dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour rechercher le nom réseau de l’ordinateur exécutant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La propriété ServerName de la fonction `SERVERPROPERTY` signale automatiquement la modification du nom réseau de l’ordinateur lorsque vous redémarrez l’ordinateur et le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La variable globale `@@SERVERNAME` conserve le nom de l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’origine jusqu’à ce que le nom [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] soit réinitialisé manuellement.

### <a name="steps-to-reproduce-the-problem"></a>Étapes pour reproduire le problème

Sur l’ordinateur où vous avez déployé [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d’une image de disque, procédez comme suit :

1. Démarrez [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].
2. Dans l’**Explorateur d’objets**, développez le nom de votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
3. Cliquez avec le bouton droit sur le dossier **Réplication** puis cliquez sur **Configurer la réplication de distribution**, puis sur **Configurer la publication, les abonnés et la distribution**.
4. Dans la boîte de dialogue de l’assistant **Configurer la distribution**, cliquez **Suivant**.
5. Dans la page boîte de dialogue **Serveur de distribution**, cliquez pour sélectionner la '\<**Server**>\<**Instance**>' qui agit comme son propre serveur de distribution ; SQL Server crée une base de données de distribution et un bouton radio de journal. Cliquez alors sur **Suivant**.
6. Dans la boîte de dialogue **Démarrage de SQL Server Agent**, cliquez sur **Suivant**.
7. Dans la boîte de dialogue **Dossier d’instantanés**, cliquez sur **Suivant**.

    > [!NOTE]
    > Si vous recevez un message pour confirmer le chemin d’accès au dossier d’instantanés, cliquez sur **Oui**.
8. Dans la boîte de dialogue **Base de données de distribution**, cliquez sur **Suivant**.
9. Dans la boîte de dialogue **Serveurs de publication**, cliquez sur **Suivant**.
10. Dans la boîte de dialogue **Actions de l’assistant**, cliquez sur **Suivant**.
11. Dans la boîte de dialogue **Terminer l’assistant**, cliquez sur **Terminer**.

## <a name="see-also"></a>Voir aussi

- [Renommer un ordinateur qui héberge une instance autonome de SQL Server](../../database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server.md)
- [@@SERVERNAME (Transact-SQL)](../../t-sql/functions/servername-transact-sql.md)
- [SERVERPROPERTY (Transact-SQL)](../../t-sql/functions/serverproperty-transact-sql.md)
- [sp_addserver (Transact-SQL)](../system-stored-procedures/sp-addserver-transact-sql.md)