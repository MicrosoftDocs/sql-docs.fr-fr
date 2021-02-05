---
title: Configuration UDL (Universal Data Link) | Microsoft Docs
description: Découvrez comment utiliser l’onglet Connexion pour spécifier la méthode de connexion à vos données à l’aide de OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.author: v-beaziz
author: bazizi
ms.openlocfilehash: 52089c92af76611fe8ab60b53dc7cf2848fb31c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195303"
---
# <a name="universal-data-link-udl-configuration"></a>Configuration UDL (Universal Data Link)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="connection-tab"></a>Onglet Connexions
Utilisez l’onglet Connexion pour spécifier la méthode de connexion à vos données à l’aide de Microsoft OLE DB Driver pour SQL Server.

![Capture d’écran des pages de liaison de données OLE DB - Onglet Connexion](../media/data-link-pages-connection-tab.png)

L’onglet Connexion est propre à chaque fournisseur et affiche uniquement les propriétés de connexion requises par Microsoft OLE DB Driver for SQL Server.

|Option|Description|
|---   |---        |
|Sélectionnez ou entrez un nom de serveur.|Sélectionnez le nom d'un serveur dans la liste déroulante ou tapez l'emplacement du serveur sur lequel se trouve la base de données à laquelle vous souhaitez accéder. La sélection de la base de données sur le serveur est une action distincte. Mettez à jour la liste en cliquant sur « Actualiser ».
|Entrez des informations pour vous connecter au serveur|Vous pouvez sélectionner les options d’authentification suivantes dans cette liste déroulante : <ul><li>`Windows Authentication:` Authentification à SQL Server à l’aide des informations d’identification du compte Windows de l’utilisateur connecté.</li><li>`SQL Server Authentication:` Authentification à l’aide de l’ID de connexion et du mot de passe.</li><li>`Active Directory - Integrated:` Authentification intégrée avec une identité Azure Active Directory. Ce mode peut également être utilisé pour l’authentification Windows à SQL Server.</li><li>`Active Directory - Password:` Authentification par ID d’utilisateur et mot de passe avec une identité Azure Active Directory.</li><li>`Active Directory - Universal with MFA support:` Authentification interactive avec une identité Azure Active Directory. Ce mode prend en charge Azure Multi-Factor Authentication (MFA).</li><li>Authentification `Active Directory - Service Principal:` avec un principal de service Azure Active Directory. **Le nom d’utilisateur** doit être défini sur l’ID de l’application (client). **Le mot de passe** doit être défini sur le secret de l’application (client).</li></ul>|
|SPN du serveur|Si vous utilisez une connexion approuvée, vous pouvez spécifier un nom de principal de service (SPN) pour le serveur.|
|Nom d'utilisateur|Saisissez l’ID d’utilisateur à utiliser pour l’authentification lorsque vous vous connectez à la source de données.|
|Mot de passe|Saisissez le mot de passe à utiliser pour l’authentification lorsque vous vous connectez à la source de données.|
|Mot de passe vide|Lorsque cette option est activée, permet au fournisseur spécifié d’utiliser un mot de passe vide dans la chaîne de connexion.|
|Autoriser l'enregistrement du mot de passe|Lorsque cette option est activée, le mot de passe peut être enregistré avec la chaîne de connexion. Le fait que le mot de passe soit inclus ou non dans la chaîne de connexion dépend de la fonctionnalité de l'application appelante. <br/><br/>**REMARQUE :** s’il est enregistré, le mot de passe est retourné puis enregistré comme non masqué et non chiffré.|
|Utiliser le chiffrement renforcé pour les données|Lorsque cette option est activée, les données transmises via la connexion sont chiffrées.|
|Faire confiance au certificat de serveur|Lorsque cette option est activée, le certificat du serveur est validé. Le certificat du serveur doit avoir le nom d’hôte de serveur correct et être émis par une autorité de certification approuvée.|
|Sélectionnez la base de données|Sélectionnez ou saisissez le nom de la base de données à laquelle vous souhaitez accéder.|
|Joindre un fichier de base de données en tant que nom de base de données|Indique le nom du fichier primaire d'une base de données joignable. Cette base de données est jointe et utilisée comme base de données par défaut pour la source de données. Dans la première zone de texte de cette section, saisissez le nom de la base de données à utiliser pour le fichier de base de données joint.<br/><br/>Saisissez le chemin d’accès complet au fichier de base de données à joindre dans la zone de texte intitulée `Using the filename`, ou cliquez sur le bouton `...` pour rechercher le fichier de base de données.|
|Modification du mot de passe|Affiche la boîte de dialogue Changer de mot de passe SQL Server. |
|Tester la connexion|Cliquez pour essayer d'établir une connexion à la source de données spécifiée. Si la connexion échoue, vérifiez que les paramètres sont corrects. Par exemple, les fautes d'orthographe ou de respect de la casse peuvent empêcher les connexions.|

## <a name="advanced-tab"></a>Onglet Avancé
Utilisez l’onglet Avancé pour afficher et définir des propriétés d’initialisation supplémentaires.

![Capture d’écran des pages de liaison de données OLE DB - Onglet Avancé](../media/data-link-pages-advanced-tab.png)

|Option|Description|
|---   |---        |
| Connect timeout | Spécifie le délai (en secondes) d'attente de la fin de l'initialisation par Microsoft OLE DB Driver pour SQL Server. Si l'initialisation dépasse le délai imparti, une erreur est renvoyée et la connexion n'est pas créée.|


> [!NOTE]  
>  Pour plus d’informations sur la connexion aux liaisons de données générales, consultez la [Vue d’ensemble de l’API de liaison de données](/previous-versions/windows/desktop/ms718102(v=vs.85)).

## <a name="next-steps"></a>Étapes suivantes
- [S’authentifier auprès de Azure Active Directory](../features/using-azure-active-directory.md) à l’aide du pilote OLE DB.

- [Inviter l’utilisateur à fournir des informations d’authentification](../help-topics/sql-server-login-dialog.md) à l’aide du pilote OLE DB.